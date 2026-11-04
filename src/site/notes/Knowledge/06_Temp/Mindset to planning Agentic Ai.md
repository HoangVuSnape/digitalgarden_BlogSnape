---
{"dg-publish":true,"permalink":"/knowledge/06-temp/mindset-to-planning-agentic-ai/","pinned":"false"}
---


---
![](/img/user/assets/images/Knowledge/06_Temp/IMG-20260909015223638.png)

# Mindset, Lý thuyết nền & Quy trình Planning cho Agentic AI

## 1. Định nghĩa & Cấu trúc nền tảng

*(Tinh hoa từ Chương 6, sách "AI Engineering" — Chip Huyen)*

Agent là một thực thể có khả năng **nhận thức môi trường xung quanh và hành động tương tác** lên môi trường đó. Một hệ thống Agentic AI gồm ba thành phần cốt lõi:

- **Bộ não (Brain — AI Planner):** mô hình nền tảng, đóng vai trung tâm xử lý thông tin, phân rã nhiệm vụ, ra quyết định.
- **Hệ thống Công cụ (Tools — Tool Inventory):** mở rộng giới hạn năng lực của agent, chia làm 3 loại:
    - *Tăng cường tri thức (Knowledge augmentation):* công cụ đọc dữ liệu — retriever, SQL executor.
    - *Mở rộng khả năng (Capability extension):* vượt giới hạn của LLM — code interpreter, calculator.
    - *Hành động ghi (Write actions):* thay đổi trạng thái môi trường — gửi email, đặt bàn, thực hiện giao dịch.
- **Hệ thống Bộ nhớ (Memory):** 3 tầng —
    - Tri thức nội tại (Internal knowledge — từ quá trình huấn luyện)
    - Bộ nhớ ngắn hạn (Short-term — trong ngữ cảnh hội thoại hiện tại)
    - Bộ nhớ dài hạn (Long-term — lưu ngoài, ví dụ Vector DB)

---

## 2. Mindset: Tư duy thiết kế agentic system

Trước khi chạm vào kiến trúc hay code, cần xác định rõ tư duy nền:

- **Agent ≠ Chatbot** — Agent có khả năng **lập kế hoạch**, **gọi tool**, **tự đánh giá**, và **lặp lại** cho đến khi hoàn thành goal.
- **Loop > One-shot** — Hệ thống agentic là một vòng lặp: Plan → Act → Observe → Reflect → (Loop/Terminate).
- **Verifiable end-state** — Chọn task có trạng thái kết thúc rõ ràng, kiểm tra được đúng/sai.
- **Human-in-the-loop** — Xác định từ đầu đâu là điểm cần con người phê duyệt, đặc biệt với hành động irreversible (xóa, thanh toán, gửi email).
- **Observability first** — Ghi log mọi bước (tool gọi, input/output, cost, latency) ngay từ run đầu tiên.

> **Tư duy cốt lõi:** Biết rõ **goal**, **data**, **tool**, **rủi ro** và **cách đo thành công** trước khi vẽ kiến trúc.

---

## 3. Nguyên tắc vàng: Tách biệt Lập kế hoạch & Thực thi (Decoupling)

- **Tránh chạy "vô định" (fruitless execution):** một sai lầm phổ biến là gộp chung lập kế hoạch và thực thi vào một prompt (mô hình vừa suy nghĩ vừa gọi tool cùng lúc). Điều này dễ khiến agent rơi vào vòng lặp vô hạn (ví dụ chạy 1.000 bước sai), gây tốn chi phí API cực lớn.
- **Quy trình 4 bước chuẩn:**
    1. Tạo kế hoạch (Plan generation / Task decomposition)
    2. Phản tư & sửa lỗi kế hoạch (Reflection)
    3. Thực thi (Execution / Function calling)
    4. Phản tư & sửa lỗi kết quả

  Kế hoạch chỉ được thực thi sau khi đã xác thực qua bộ lọc heuristics hoặc AI-judge.
- **Human-in-the-loop:** với write action rủi ro cao (sửa database, chuyển tiền), bắt buộc có cơ chế yêu cầu con người phê duyệt trước khi thực thi.

---

## 4. Cơ chế Phản tư & Sửa lỗi (Reflection & Error Correction)

Chìa khóa giúp agent tăng hiệu suất bằng cách học từ chính sai lầm của nó:

- **ReAct (Reason + Act):** đan xen liên tục suy luận và hành động theo chu kỳ *Thought → Act → Observation*, lặp đến khi hoàn thành nhiệm vụ.
- **Reflexion:** tách hẳn hai mô-đun — evaluator (đánh giá kết quả) và self-reflection (tự phản tư) — để điều chỉnh lộ trình hành động khi thất bại.
- **Đánh đổi lớn nhất:** phản tư sinh ra rất nhiều token trung gian (thoughts, observations) → tăng mạnh chi phí vận hành và độ trễ (latency).

---

## 5. Rủi ro cần biết: Lỗi tích tụ (Compound Mistakes)

Agent thực hiện tác vụ càng nhiều bước, tỷ lệ lỗi càng tăng theo cấp số nhân. Nếu mỗi bước có độ chính xác 95%:

| Số bước | Độ chính xác tổng thể |
|---|---|
| 1 bước | 95% |
| 10 bước | ~60% |
| 100 bước | ~0.6% |

Đây là lý do agent luôn đòi hỏi các mô hình nền tảng mạnh nhất, và tại sao giới hạn số bước (`max_iterations`) là một guardrail bắt buộc, không phải tùy chọn.

---

## 6. Planning: Từ đề bài → thiết kế hệ thống

Khi nhận một đề bài agentic AI, đừng nhảy ngay vào chọn model hay viết code. Coi đây là quá trình **làm rõ yêu cầu → xác định ràng buộc → thiết kế kiến trúc phù hợp**, chia thành 4 nhóm câu hỏi:

1. Hiểu bài toán (goal, task shape, autonomy level)
2. Xác định data & tool (RAG, memory, tool gì, credential ra sao)
3. Chất lượng & ràng buộc (validation, human checkpoint, eval, latency, budget)
4. Operability & governance (log, trace, risk, agent charter)

### 6.1. Hiểu bài toán (Goal & Scope)

| Câu hỏi | Ý nghĩa |
|---|---|
| **Goal cuối cùng là gì?** (output dạng nào: text, action, file, API call?) | Xác định "khi nào thì xong" và success criteria |
| **Task shape: sequential hay branching?** | Sequential → workflow đơn giản; Branching → cần agent tự quyết định nhánh |
| **Reasoning: exploratory hay deterministic?** | Exploratory → cần linh hoạt, nhiều bước; Deterministic → rule-based, constraint chặt |
| **Domain: narrow hay general?** | Narrow → dễ guardrail, ít tool; General → có thể cần multi-agent hoặc nhiều tool hơn |
| **Autonomy level: fully autonomous hay human-in-the-loop?** | Quyết định chỗ nào cần con người phê duyệt, đặc biệt hành động rủi ro cao |

**Mẹo:** nếu chưa trả lời rõ được 5 câu này, chưa nên đi tiếp xuống kiến trúc.

### 6.2. Data & Tools

| Câu hỏi                                                              | Ý nghĩa                                                                                     |
| -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Cần internal/proprietary data không?**                             | Có → cần RAG (vector DB, retrieval); Không → chỉ dùng knowledge trong model                 |
| **Cần persistent state/memory không?**                               | Có (nhớ lịch sử task, state dài hạn) → cần framework như LangGraph; Không → script đơn giản |
| **Có bao nhiêu tool cần dùng?**                                      | >20 tool → cân nhắc split multi-agent hoặc group theo domain; Ít tool → single agent OK     |
| **Tool surface đã define chưa?** (read/write API nào, credential gì) | Liệt kê rõ tool trước khi viết prompt; mỗi tool chỉ nên có quyền tối thiểu cần thiết        |

### 6.3. Chất lượng & Ràng buộc (Quality & Constraints)

| Câu hỏi | Ý nghĩa |
|---|---|
| **Output cần validation loop/quality gate không?** | Có → thêm verifier (rule-based hoặc LLM) trước khi commit action |
| **Human checkpoint ở đâu?** (review plan, research, draft, action) | Xác định các điểm bắt buộc có người duyệt (gửi email, thanh toán, xóa dữ liệu) |
| **Có evaluation data không?** | Có → build automated evals (test set); Không → cần thu thập ví dụ trước khi chạy production |
| **Latency tolerance?** | Tight → ít hop, model nhỏ; Loose → cho phép deep reasoning, multi-agent |
| **Budget per task?** | Low → model rẻ, caching; High → model mạnh, nhiều bước reflection |

### 6.4. Operability & Governance

| Câu hỏi                                                                       | Ý nghĩa                                                                                          |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Observability: logs/traces lưu ở đâu?**                                     | Quyết định công cụ log, trace (Opik, structured logging...) để debug và audit                    |
| **Problem decomposable thành distinct competencies không?**                   | Có → thiết kế multi-agent (mỗi agent một skill); Không → single agent là đủ                      |
| **Risk level?** (criticality, PII, sensitive actions)                         | Phân loại rủi ro → set access control, approval workflow, guardrail chặt hơn                     |
| **Agent charter:** agent được làm gì, phải escalate gì, được access data nào? | Viết 1 trang spec rõ ràng trước khi code: scope, allowed actions, escalated actions, data policy |

---

## 7. Architecture: Từ planning → thiết kế hệ thống

### 7.1. Core Loop (Request → End)

```
Request Intake
  → Context Assembly (RAG / Memory)
  → LLM Reasoning (Plan)
  → Action Validation (Guardrails / Verifier)
  → Sandboxed Execution (Tool Call)
  → Result Processing
  → State Update
  → Loop / Terminate
```

### 7.2. Component Architecture

| Component              | Chức năng                                                                                     | Ghi chú                                                     |
| ---------------------- | --------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Intake**             | Parse & validate input, extract intent, schema validation                                     | Reject malformed input trước khi gọi LLM                    |
| **Planner Agent**      | Nhận goal → sinh execution plan (sequence of steps, tool, expected output, success criterion) | Plan schema validation, max step limit, restricted tool set |
| **Executor Agent**     | Execute từng step trong plan, report outcome về orchestrator                                  | Có thể là single hoặc multi-agent                           |
| **Memory Tier**        | System prompt + retrieved context (vector DB) + dynamic state                                 | Map memory tiers rõ ràng                                    |
| **Tool Layer (MCP)**   | Kết nối external systems qua Model Context Protocol hoặc function calling                     | Narrowest credential per tool                               |
| **Verifier/Guardrail** | Validate plan & action trước khi execute (PII check, sensitive tool, confidence score)        | Human gate cho consequential writes                         |
| **Observability**      | Log: tool, input, output, cost, latency, trace ID                                             | Instrument từ run đầu                                       |

### 7.3. Multi-agent vs Single-agent

| Tiêu chí | Single Agent | Multi-agent |
|---|---|---|
| **Task complexity** | Đơn giản, ít tool | Phức tạp, nhiều domain competency |
| **Decomposability** | Không decomposable rõ | Decomposable thành distinct skills |
| **Cost & latency** | Thấp hơn | Cao hơn (nhiều hop) |
| **Coordination overhead** | Không có | Cần orchestrator, message passing |

---

## 8. Từ Planning → Code: Các bước implementation

**Bước 1 — Define Agent Charter (1-page spec)**
Goal, scope boundary · allowed actions, escalated actions · data access policy.
Output: 1 trang tài liệu mô tả rõ agent được làm gì, không được làm gì, khi nào phải gọi người.

**Bước 2 — Design Context Architecture**
System prompt content · RAG retrieval strategy (vector DB, chunk size, top-k) · dynamic state (conversation history, plan state).
Output: sơ đồ các loại memory — system prompt, retrieved context, conversation state.

**Bước 3 — Build Tool Integrations**
List tool surface (read/write API) · grant narrowest credential · implement via MCP hoặc function calling.
Output: danh sách tool + interface (input schema, output schema, error handling).

**Bước 4 — Implement Reasoning Loop**

```python
# Pseudocode agentic loop
goal = parse_goal(request)
plan = planner.generate(goal)  # LLM call

if not verifier.validate(plan):
    escalate_to_human(plan)

for step in plan.steps:
    action = executor.execute(step.tool, step.input)
    observation = observe(action)

    if not verifier.validate(observation):
        rollback_or_escalate(step)

    update_state(observation)

    if is_goal_achieved(state):
        terminate()
```

Output: code skeleton cho core loop (LangGraph, LangChain, hoặc custom).

**Bước 5 — Add Human Handoff & Approval**
Route consequential writes (payment, delete, customer-visible) qua approval queue · giữ queue nhỏ để review thực sự, không rubber-stamp.
Output: workflow phê duyệt (Slack approval, UI review queue).

**Bước 6 — Deploy Observability & Monitoring**
Log mọi step: tool, input, output, cost, latency · tracing + replay để debug · fallback/escalation policy.
Output: dashboard/log system để theo dõi agent chạy.

**Bước 7 — Evaluate Before Autonomy**
Assemble evaluation set với known-good outcomes · run agent sau mỗi thay đổi · track: task success rate, tool-use accuracy, safety violations, cost, latency.
Output: bộ test + metric để quyết định khi nào được bật autonomy.

---

## 9. Đánh giá Agent (Evaluation)

Thay vì chỉ đánh giá turn-based (từng phản hồi đơn lẻ), agent cần được đánh giá dựa trên **task-based** — tác vụ có hoàn thành hay không — và đo các chỉ số cụ thể:

- Tỷ lệ kế hoạch hợp lệ sinh ra.
- Tần suất gọi sai công cụ hoặc truyền sai tham số.
- **Chỉ số hiệu quả (Efficiency):** số bước trung bình để hoàn thành tác vụ, chi phí trung bình, thời gian thực thi trung bình cho mỗi hành động.

---

## 10. Checklist nhanh (Cheat Sheet)

**Trước khi code:**
- [ ] Goal & success criteria rõ?
- [ ] Task shape: sequential/branching?
- [ ] Cần RAG/memory không?
- [ ] Tool surface đã list & credential narrow chưa?
- [ ] Human checkpoint ở đâu?
- [ ] Evaluation data có chưa?
- [ ] Observability setup (log, trace, cost)?
- [ ] Agent charter (scope, allowed/escalated actions, data access)?

**Trong quá trình build:**
- [ ] Loop: Plan → Act → Observe → Reflect → Loop/Terminate
- [ ] Verifier trước mỗi consequential action
- [ ] Instrument từ run đầu tiên
- [ ] Shadow mode trước khi autonomy (agent làm, human execute official process)

**Trước khi production:**
- [ ] Eval set pass rate đạt threshold?
- [ ] Human approval queue cho irreversible actions?
- [ ] Agent registered: owner, purpose, systems read/write, credentials, autonomy level
- [ ] Governance lifecycle: risk assessment, ethics, regulatory alignment

---

## Nguồn tham khảo

- Chip Huyen, *AI Engineering* — Chương 6 (Agents)
- [louisbouchard.ai — 12 Questions on AI Architecture](https://www.louisbouchard.ai/12-questions-ai-architecture/)
- [getreadyforagents.com — Build an Agentic AI System](https://www.getreadyforagents.com/guides/build-an-agentic-ai-system/)
- [fast.io — Build Agentic AI System](https://fast.io/resources/build-agentic-ai-system/)
- [samta.ai — Agentic AI Engineering Architecture](https://samta.ai/blogs/agentic-ai-engineering-architecture)
