---
{"dg-publish":true,"permalink":"/knowledge/agent/mcp/","pinned":"false"}
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


{ .block-language-dataview}
# Notes last Touches Today


{ .block-language-dataview}

# Today I learned 🧭


# Summary 💬


# Gratitude and Recognition 😍😍