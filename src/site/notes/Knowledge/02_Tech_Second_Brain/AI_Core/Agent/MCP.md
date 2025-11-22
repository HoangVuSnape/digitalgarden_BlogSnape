---
created: 2025-08-27T15:53:30
Updated: 2025-02-21T20:15
dg-publish: true
dg-home:
tags:
dg-pinned: "false"
aliases:
cssclasses:
updated:
  - 2025-08-28T14:04:52+07:00
---
In version v2.11
# Link 
- https://www.jlowin.dev/blog/stop-converting-rest-apis-to-mcp
- https://gofastmcp.com/integrations/openapi



-----
# README — FastMCP + OpenAPI + Orchestrator/Proxy (Tổng hợp nhanh)

> Tài liệu này tổng hợp toàn bộ “kiến thức nãy giờ” để bạn triển khai MCP theo hướng **ít tool – dễ điều phối – độ tin cậy cao**.

---

## 1) Ảnh tổng quan

- **FastMCP**: giúp tạo **MCP server** nhanh, hoặc **tự chuyển OpenAPI → MCP** (mỗi endpoint thành 1 component, mặc định là _tool_).
    
- **Vấn đề thường gặp**: quá nhiều tools/servers làm model **rối lúc chọn tool** (tool selection). Kinh nghiệm cộng đồng: khi tổng số tool ~**50** trở lên, chất lượng chọn giảm rõ. Thực tế nên giữ **≤ 5 MCP servers**, mỗi server ~ 5–10 tools (tham chiếu kinh nghiệm, không phải giới hạn cứng).
    
- **Giải pháp khuyến nghị**:
    
    1. **Orchestrator pattern**: chỉ phơi **1–vài tool “đầu mối”** cho LLM; bên trong orchestrator chủ động gọi tuần tự/có điều kiện các tool của server khác.
        
    2. **Hybrid “router”**: LLM chỉ chọn **intent**; bạn map intent → tool trong **allow-list nhỏ**.
        
    3. **Proxy khi cần**: dùng proxy/mount để cầu nối (bridge) transport hoặc test, **không** phơi đại trà.
        

---

## 2) FastMCP từ OpenAPI (điểm chính)

- **Tạo server từ spec**: `FastMCP.from_openapi(openapi_spec=..., client=..., name=...)`.
    
- **Route mapping** (lọc và gán loại component):
    
    - `MCPType.TOOL` (mặc định), `RESOURCE`, `RESOURCE_TEMPLATE`, `EXCLUDE`.
        
    - Lọc theo **method/pattern/tags** để **allow-list** các route cần, _exclude_ phần nhạy cảm.
        
- **Đặt tên/metadata**:
    
    - Tên mặc định dựa trên `operationId` (slug & unique); có thể override bằng `mcp_names`.
        
    - Tags: từ RouteMap, global tags, và cả OpenAPI tags sẽ có trong `_meta._fastmcp.tags`.
        
- **Chỉnh component sau khi sinh**: `mcp_component_fn(route, component)` (ví dụ sửa description/tags).
    
- **Xử lý tham số**:
    
    - Tự **bỏ qua None/rỗng** cho query.
        
    - Path array → **simple (comma-separated)**; query array → theo OpenAPI `explode`.
        
    - Header tự chuỗi hoá.
        
- **Parser mới (tùy chọn)**: bật `FASTMCP_EXPERIMENTAL_ENABLE_NEW_OPENAPI_PARSER=true` (nhanh & hợp serverless hơn).
    
- **Lưu ý quan trọng**: Dùng converter **để bootstrap**/thử nghiệm. Khi ship production, **curate** endpoint/params → **ít tool, tên rõ, mô tả chuẩn**, tránh “nở tool”.
    

---

## 3) Orchestrator pattern (khuyên dùng)

### 3.1 Ý tưởng

- Phơi ra **1 tool duy nhất** (vd. `execute_workflow`).
    
- Bên trong: orchestrator **gọi tuần tự**/song song các tool ở server khác, **ràng buộc** timeout, retry, kiểm tra dữ liệu, ghép kết quả.
    

### 3.2 Code mẫu siêu gọn

```python
# orchestrator.py
from fastmcp import FastMCP, Client
from pydantic import BaseModel

orchestrator = FastMCP("Orchestrator")

client_a = Client("http://localhost:7001/mcp")
client_b = Client("http://localhost:7002/mcp")
client_c = Client("http://localhost:7003/mcp")

class In(BaseModel):
    input_data: str

class Out(BaseModel):
    result: str

@orchestrator.tool
async def execute_workflow(payload: In) -> Out:
    async with client_a, client_b, client_c:
        s1 = await client_a.call_tool("step1", {"data": payload.input_data}, timeout=5)
        s2 = await client_b.call_tool("step2", {"result": s1}, timeout=5)
        fin = await client_c.call_tool("final", {"input": s2}, timeout=5)
        return Out(result=str(fin))

if __name__ == "__main__":
    import asyncio; asyncio.run(orchestrator.run())
```

### 3.3 Ví dụ domain (tuyển sinh)

1. **A.step1**: phân tích câu hỏi & lấy yêu cầu ngành (điểm sàn, bonus IELTS…).
    
2. **B.step2**: tính _eligible?_ + giải thích (tổng điểm, cộng điểm…).
    
3. **C.final**: tạo câu trả lời tự nhiên cho thí sinh.
    

> Ưu: LLM chỉ thấy 1 tool → ít rối; bạn kiểm soát chặt timeout/retry/luồng phụ thuộc.

---

## 4) Proxy/Chain pattern (khi nào nên dùng)

- **Proxy**: `FastMCP.as_proxy(server, name="...")` hoặc `mount(..., as_proxy=True)` để _lộ_ toàn bộ tool của server đích (bridge remote HTTP/SSE ↔ local stdio, thống nhất transport…).
    
- **Proxy chain**: A proxy → B proxy → C (xây _dependency chain_).
    
- **Cảnh báo**: Proxy **phơi toàn bộ tool** → chỉ sử dụng khi thật cần, dễ gây “tool overload”.
    

**Snippet minh hoạ:**

```python
server_c = FastMCP("ServerC")
server_b = FastMCP.as_proxy(server_c, name="ServerB")
server_a = FastMCP.as_proxy(server_b, name="ServerA")
```

---

## 5) Hybrid “router” (linh hoạt mà vẫn gọn)

- Lộ **1 tool router**; LLM đưa `intent`; router map `intent → tool` trong **allow-list nhỏ**.
    

```python
@orchestrator.tool
async def run(task: str, intent: str):
    async with client_a, client_b, client_c:
        if intent == "eligibility":
            return await client_b.call_tool("check_eligibility", {"q": task})
        if intent == "tuition":
            return await client_c.call_tool("get_tuition", {"q": task})
        return await client_a.call_tool("search_programs", {"q": task})
```

---

## 6) Best-practices (rất quan trọng)

1. **Giới hạn tool hiển thị**: aim tổng **≤ 20–30 tool**; tốt nhất 1–5 tool “nhiệm vụ”.
    
2. **Curate tên & mô tả**: tên ngắn, có động từ, mô tả “khi nào dùng” + ví dụ arg.
    
3. **Allow-list mạnh tay** khi convert OpenAPI: `EXCLUDE` phần admin/internal/thao tác nguy hiểm.
    
4. **Timeout + retry** ở orchestrator; có thể thêm **circuit breaker** cho tool hay lỗi/treo.
    
5. **Kiểm tra dữ liệu giữa các bước** (schema Pydantic) để tránh “rác” đẩy sang bước sau.
    
6. **Auth/Headers**: cấu hình qua `Client(..., headers=...)` (Bearer/API key/…); có thể thêm per-call timeout.
    
7. **Quan sát/trace**: trả về `traces` (tool, args rút gọn, attempt) để debug nhanh.
    
8. **Từng bước một**: bootstrap bằng OpenAPI → đo lường → _chắt lọc_ thành workflow ít tool.
    

---

## 7) Cách chạy nhanh

```bash
pip install fastmcp pydantic
python orchestrator.py
# gắn vào client MCP (Claude Desktop, OpenAI Responses API, v.v.)
```

**Biến môi trường hữu ích**

- `FASTMCP_EXPERIMENTAL_ENABLE_NEW_OPENAPI_PARSER=true` (khi convert OpenAPI).
    
- Tự đặt `REQUEST_TIMEOUT`, `RETRY`, `SERVER_X_URL`, `SERVER_X_TOKEN`… như ví dụ server đầy đủ.
    

---

## 8) FAQ ngắn

**Q: Có cần xác định rõ tool sẽ gọi không?**

- **Orchestrator**: Có (deterministic).
    
- **Open**: Mount/proxy để LLM tự chọn (nhưng dễ rối).
    
- **Hybrid**: LLM chọn _intent_, bạn map _intent → tool_ trong allow-list.
    

**Q: Vì sao không expose hết tools?**

- Nhiều tool làm LLM phân vân → **selection kém**, tốn token/latency, dễ vòng lặp vô ích.
    

**Q: Khi nào dùng proxy chain?**

- Khi cần bridge transport, gom nhiều server “bên dưới” vào một điểm truy cập; tránh lạm dụng khi số tool lớn.
    

---

## 9) Checklist triển khai

-  Chọn **pattern**: Orchestrator / Hybrid / (Proxy chỉ khi cần).
    
-  Nếu dùng **OpenAPI → MCP**: định nghĩa **RouteMap** (allow-list + EXCLUDE).
    
-  Đặt **tên & mô tả** tool rõ ràng; thêm **tags** theo nhóm nhiệm vụ.
    
-  Thêm **timeout/retry**; validate dữ liệu giữa các bước (Pydantic).
    
-  Đo lường số tool hiển thị; **giảm** còn các nhiệm vụ “đầu mối”.
    
-  Viết **health_check** tool để kiểm nhanh kết nối & danh sách tool.
    

---

## 10) Tệp mẫu đề xuất

- `orchestrator.py` (ở trên): **1 tool `execute_workflow`**, gọi A→B→C.
    
- (Tuỳ chọn) `server.py` bản đầy đủ với **env**, **retry helper**, **traces**, **health_check** (mình đã cung cấp; bạn có thể copy sang repo của bạn).
    # Notes created today

- [[DailyNote/2025-11-22 Week 47 Day 6\|2025-11-22 Week 47 Day 6]]
- [[Home\|Home]]
- [[Knowledge/05_Collections/Collections\|Collections]]
- [[Knowledge/01_Projects/NCKH/Analyze data\|Analyze data]]
- [[Knowledge/01_Projects/NCKH/NCKH\|NCKH]]
- [[Knowledge/01_Projects/NCKH/OAI/Analyze data\|Analyze data]]
- [[Knowledge/01_Projects/NCKH/OAI/Improve Model\|Improve Model]]
- [[Knowledge/01_Projects/NCKH/OAI/Tập huấn OAI\|Tập huấn OAI]]
- [[Knowledge/01_Projects/KLTN/Data\|Data]]
- [[Knowledge/01_Projects/KLTN/Report/18.5\|18.5]]
- [[Knowledge/01_Projects/ProjectCNTT/Project Information Technology\|Project Information Technology]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/Docker\|Docker]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/MCP & A2A\|MCP & A2A]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/MCP\|MCP]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/MLoops\|MLoops]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/ModelRetrieval & VLM Localization\|ModelRetrieval & VLM Localization]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Computer_Vision/GAN\|GAN]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Automatic AI/N8n\|N8n]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Chuỗi thời gian - Time series\|Chuỗi thời gian - Time series]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Code with AI\|Code with AI]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Mixture Of Experts\|Mixture Of Experts]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/PreprocessingData_img\|PreprocessingData_img]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/RAG Advance\|RAG Advance]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Random Forest vs XGboost\|Random Forest vs XGboost]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Data enhance, scale in RAG\|Data enhance, scale in RAG]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Data evaluation\|Data evaluation]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Fine tune\|Fine tune]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Kiến trúc và huấn luyện BERT\|Kiến trúc và huấn luyện BERT]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Reinforcement Learning Human Feedback - RLHF\|Reinforcement Learning Human Feedback - RLHF]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Report CNTT\|Report CNTT]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Rerank_EmbeddingModel\|Rerank_EmbeddingModel]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Analyze explainable my system\|Analyze explainable my system]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Explainability for NLP\|Explainability for NLP]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Explainable AI\|Explainable AI]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Graph Rag vs Graph DB\|Graph Rag vs Graph DB]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Knowledge Graph\|Knowledge Graph]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Knowledge Graphs and ASTs\|Knowledge Graphs and ASTs]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Scene graph\|Scene graph]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Sumary paper\|Sumary paper]]
- [[Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/Gitlab - docker\|Gitlab - docker]]
- [[Knowledge/02_Tech_Second_Brain/DevOps_Tools/Jmeter_Testing/Jmeter\|Jmeter]]
- [[Knowledge/03_Career_Center/Soft_Skills/Điểm hay mà mình lưu ý khi thuyết trình\|Điểm hay mà mình lưu ý khi thuyết trình]]

{ .block-language-dataview}
# Notes last Touches Today

- [[DailyNote/2025-11-22 Week 47 Day 6\|2025-11-22 Week 47 Day 6]]
- [[Knowledge/01_Projects/KLTN/Large reason model\|Large reason model]]
- [[Knowledge/01_Projects/KLTN/Review project IT\|Review project IT]]
- [[Knowledge/01_Projects/KLTN/Report/6.6\|6.6]]
- [[Knowledge/01_Projects/KLTN/Sửa lại thesis\|Sửa lại thesis]]
- [[Knowledge/01_Projects/KLTN/Meeting with teacher\|Meeting with teacher]]
- [[Knowledge/01_Projects/KLTN/Tiến độ - Note\|Tiến độ - Note]]
- [[Knowledge/01_Projects/Master/Note tiến độ\|Note tiến độ]]
- [[Knowledge/01_Projects/Master/Phát triển và hướng nghiên cứu tiếp\|Phát triển và hướng nghiên cứu tiếp]]
- [[Knowledge/01_Projects/Master/Chương trình\|Chương trình]]
- [[Knowledge/01_Projects/Innotech0vn/Innocody/Treesister\|Treesister]]
- [[Knowledge/01_Projects/Innotech0vn/Innocody/Rust\|Rust]]
- [[Knowledge/01_Projects/Innotech0vn/Innotech-chatbot/Innotech - chatbot\|Innotech - chatbot]]
- [[Knowledge/01_Projects/Innotech0vn/Report/Note\|Note]]
- [[Knowledge/05_Collections/Collections\|Collections]]
- [[Knowledge/01_Projects/NCKH/OAI/Improve Model\|Improve Model]]
- [[Knowledge/01_Projects/NCKH/NCKH\|NCKH]]
- [[Knowledge/01_Projects/NCKH/OAI/Analyze data\|Analyze data]]
- [[Knowledge/01_Projects/NCKH/OAI/Tập huấn OAI\|Tập huấn OAI]]
- [[Knowledge/01_Projects/NCKH/Analyze data\|Analyze data]]
- [[Knowledge/01_Projects/KLTN/Data\|Data]]
- [[Knowledge/01_Projects/KLTN/Report/18.5\|18.5]]
- [[Knowledge/01_Projects/ProjectCNTT/Project Information Technology\|Project Information Technology]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/Docker\|Docker]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/MCP & A2A\|MCP & A2A]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/MCP\|MCP]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/MLoops\|MLoops]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Agent/ModelRetrieval & VLM Localization\|ModelRetrieval & VLM Localization]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Computer_Vision/GAN\|GAN]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Automatic AI/N8n\|N8n]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Chuỗi thời gian - Time series\|Chuỗi thời gian - Time series]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Code with AI\|Code with AI]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Mixture Of Experts\|Mixture Of Experts]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/PreprocessingData_img\|PreprocessingData_img]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/RAG Advance\|RAG Advance]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/Fundamentals/Random Forest vs XGboost\|Random Forest vs XGboost]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Data enhance, scale in RAG\|Data enhance, scale in RAG]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Data evaluation\|Data evaluation]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Fine tune\|Fine tune]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Kiến trúc và huấn luyện BERT\|Kiến trúc và huấn luyện BERT]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Reinforcement Learning Human Feedback - RLHF\|Reinforcement Learning Human Feedback - RLHF]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Report CNTT\|Report CNTT]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/NLP/Rerank_EmbeddingModel\|Rerank_EmbeddingModel]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Analyze explainable my system\|Analyze explainable my system]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Explainability for NLP\|Explainability for NLP]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Explainable AI\|Explainable AI]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Graph Rag vs Graph DB\|Graph Rag vs Graph DB]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Knowledge Graph\|Knowledge Graph]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Knowledge Graphs and ASTs\|Knowledge Graphs and ASTs]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Scene graph\|Scene graph]]
- [[Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/Sumary paper\|Sumary paper]]
- [[Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/Gitlab - docker\|Gitlab - docker]]
- [[Knowledge/03_Career_Center/Soft_Skills/Điểm hay mà mình lưu ý khi thuyết trình\|Điểm hay mà mình lưu ý khi thuyết trình]]
- [[Knowledge/02_Tech_Second_Brain/DevOps_Tools/Jmeter_Testing/Jmeter\|Jmeter]]
- [[Knowledge/03_Career_Center/Soft_Skills/Marketing - generate image, video\|Marketing - generate image, video]]
- [[Knowledge/03_Career_Center/Soft_Skills/Tìm hiểu về cách nghiên cứu\|Tìm hiểu về cách nghiên cứu]]
- [[Knowledge/03_Career_Center/Interview_Prep/Interview_chuyên môn\|Interview_chuyên môn]]
- [[Knowledge/03_Career_Center/Interview_Prep/Tìm hiểu đọc CV\|Tìm hiểu đọc CV]]
- [[Knowledge/04_Life_Management/Learning_English/Planning\|Planning]]
- [[Knowledge/04_Life_Management/Journey/DỰ tốt nghiệp\|DỰ tốt nghiệp]]
- [[assets/template/Question_everyWeek\|Question_everyWeek]]
- [[Knowledge/04_Life_Management/Self_Reflection/October\|October]]
- [[Knowledge/04_Life_Management/Self_Reflection/Kim chỉ nam\|Kim chỉ nam]]
- [[Knowledge/04_Life_Management/ShortLesson/2. Hippo - Motivation\|2. Hippo - Motivation]]
- [[Knowledge/04_Life_Management/ShortLesson/6. Ma trận eisenhower\|6. Ma trận eisenhower]]
- [[Knowledge/04_Life_Management/ShortLesson/Những câu nói hay 2025\|Những câu nói hay 2025]]
- [[Knowledge/04_Life_Management/ShortLesson/6 điều học sau 7h tối chill\|6 điều học sau 7h tối chill]]
- [[Knowledge/04_Life_Management/Learning_English/Điểm để ý của mình về IELTS\|Điểm để ý của mình về IELTS]]
- [[Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/AWS - Mì AI\|AWS - Mì AI]]
- [[Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/Gate way, proxy, GRPC\|Gate way, proxy, GRPC]]
- [[Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/Hiểu về lộ trình tìm hiểu về chứng chỉ cloud\|Hiểu về lộ trình tìm hiểu về chứng chỉ cloud]]
- [[Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/AWS\|AWS]]
- [[Knowledge/04_Life_Management/Journey/Đà Lạt 2025\|Đà Lạt 2025]]
- [[Home\|Home]]
- [[Knowledge/04_Life_Management/Books/7.5 Writing guaranteed\|7.5 Writing guaranteed]]

{ .block-language-dataview}

# Today I learned 🧭


# Summary 💬


# Gratitude and Recognition 😍😍