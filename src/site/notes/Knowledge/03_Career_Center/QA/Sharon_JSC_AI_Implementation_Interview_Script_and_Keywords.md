---
{"dg-publish":true,"permalink":"/knowledge/03-career-center/qa/sharon-jsc-ai-implementation-interview-script-and-keywords/","pinned":"false","tags":["career","interview-prep","ai-engineer","sharon-jsc"]}
---

# 🎯 SHARON JSC – AI IMPLEMENTATION SPECIALIST
## Complete Interview Playbook & Internal AI System Architecture (200-250 Lines)

---

### 01. BỐI CẢNH SHARON JSC & VỊ THẾ B2B CONTRACT MANUFACTURER
- **Bản chất mô hình:** Sharon không chỉ là tiệm bánh lẻ mà là **B2B Food Manufacturer & Outsourcing Solution Provider** (gia công theo hợp đồng cho nhà hàng, khách sạn, café, chuỗi F&B và retail cần bánh nhưng không tự xây nhà máy).
- **Quy mô & Cơ sở:** Thành lập 2019, 11–50 người. Nhà máy chính **~1.000 m² tại Cát Lái (TP.HCM)**; 2 Hub phân phối tại **Hà Nội** và **Phú Quốc**.
- **Dải sản phẩm:** Bakery (Baguette, Sourdough, Buns), Viennoiserie/Pastry (Croissant, Tart), Cakes, Sweets, Fine Foods & Raw Ingredients. Phục vụ cả mẻ nhỏ artisan lẫn sản lượng công nghiệp lớn.
- **Tâm lý Founder người Úc:** Đa mảng, cực kỳ thẳng thắn (*no-bullshit*), ghét thử việc hời hợt, chú trọng trách nhiệm (*ownership*), tiến độ (*deadlines*) và ROI thực tế.
- **💬 Câu định vị an toàn:** *"Em hiểu Sharon đang đóng vai trò như một contract manufacturer trong hệ sinh thái F&B—đứng phía sau hỗ trợ các nhà hàng, khách sạn và retail sản xuất bánh hoặc thực phẩm theo nhu cầu. Vì vậy bài toán AI không chỉ là chatbot, mà liên quan mật thiết đến forecasting, tồn kho, production planning, QA và quản lý đơn hàng."*

---

### 02. GIẢI THÍCH TỪ KHÓA CỐT LÕI TRONG JOB DESCRIPTION (JD)
1. **AI Implementation Specialist:** Kỹ sư ứng dụng đưa API, Agent và workflow có sẵn (Python, n8n, OpenAI/Claude) giải quyết trực tiếp bài toán kinh doanh, không phải R&D mô hình từ đầu.
2. **Flagship Programme:** Dự án mũi nhọn ưu tiên số 1 do Founder chỉ đạo, trực tiếp quyết định hiệu quả chi phí và tiến độ chuyển đổi số toàn công ty.
3. **Map Processes $\rightarrow$ Clear Requirements:** Đi thực tế xưởng/sales, vẽ lại luồng công việc hiện tại, xác định điểm nghẽn (nhập tay, sót đơn) và chuẩn hóa thành yêu cầu kỹ thuật.
4. **CRM / ERP / POS:** **CRM** (quản lý data khách sỉ/lẻ, lịch sử chăm sóc); **ERP** (quản trị tổng thể kho nguyên liệu bột/bơ, lệnh nướng từng mẻ bánh); **POS** (máy tính tiền tại quầy).
5. **From Prototype to Release:** Tự chủ vòng đời: dựng bản mẫu (prototype 1-2 tuần) $\rightarrow$ cho nhân viên test $\rightarrow$ triển khai chạy thật (production).
6. **n8n / Make:** Công cụ tự động hóa kéo thả (Workflow Automation) kết nối Webhook và API cực nhanh giữa Zalo, Google Sheets, CRM mà không cần viết frontend phức tạp.
7. **From Office to Production Floor:** Tinh thần lăn xả: sẵn sàng xuống tận xưởng bánh Cát Lái quan sát lò nướng, thợ đóng gói để tìm cơ hội tự động hóa.

---

### 03. TỰ GIỚI THIỆU BẢN THÂN (SELF-INTRODUCTION)
> *"Hi, nice to meet you. My name is Vu.*  
> *I’m a Software and AI Engineer with a solid Computer Science degree from Ton Duc Thang University, currently completing my Master's.*  
> *At Innotech, I built backend microservices and agent workflows using Python, FastAPI, and PostgreSQL, focusing on integrating AI capabilities directly into enterprise APIs and databases.*  
> *What really excites me about Sharon JSC is your direct initiative to transform operations with AI. I see myself not just as a coder, but as an **internal builder**.*  
> *I’m comfortable going directly to the operations or sales floor, identifying bottlenecks, and shipping working automations fast with high ownership and strict respect for deadlines."*  

- **Ý chính tiếng Việt:** Tốt nghiệp CS, học Thạc sĩ, kinh nghiệm Innotech làm API/DB, định vị là **"Internal Builder"** bám sát thực tế xưởng bánh và tôn trọng deadline.

---

### 04. XỬ LÝ KHI TECH STACK HƠI LỆCH (OUT OF USUAL STACK)
> *"I'm 100% comfortable with that. To me, technology stacks are simply tools to solve business problems. My foundation in software architecture, databases, and APIs is very solid.*  
> *When you understand system logic and HTTP data flows under the hood, picking up a new framework, language, or low-code tool like n8n or Make takes me just a few days.*  
> *In fact, because I write clean custom code in Python and TypeScript, adapting to visual workflows or existing ERPs is actually much faster. My priority is always delivering reliable output on time."*  

- **Ý chính tiếng Việt:** Nền tảng CS/API vững thì công cụ nào cũng làm chủ trong vài ngày; biết code bài bản giúp làm visual tool (n8n/Make) nhanh hơn nhiều.

---

### 05. KINH NGHIỆM CRM/ZALO (CASE STUDY THỰC TẾ TIỆM THÚ Y)
> *"Yes, I actually built a tailored CRM solution from scratch for a real business—a veterinary shop run by my cousin.*  
> *Originally, they bought a 3rd-party software, but it was too clunky and didn't fit their daily workflow. On top of that, customer interactions on **Zalo** were overwhelming, leading to buried chats and missed appointments.*  
> *So I stepped in and built a custom web management portal for them. I streamlined customer profiles and linked their Zalo chats directly into the system.*  
> *Whenever a customer reached out, staff could track pet medical history and order status in one centralized dashboard.*  
> *That experience taught me that off-the-shelf CRMs often fail because of complexity, and my role is building lightweight, automated integrations that staff genuinely love using."*  

- **Ý chính tiếng Việt:** Case study thật: Phần mềm ngoài quá rối $\rightarrow$ Tự build Web Portal $\rightarrow$ Link chat Zalo vào quản lý hồ sơ tập trung $\rightarrow$ Hiểu rõ nỗi đau nhân viên lười dùng app phức tạp.

---

### 06. TẠI SAO NGHỈ VIỆC CŨ / KẾT THÚC THỰC TẬP?
> *"At Innotech, my role was a project-based AI engineering internship. I delivered the microservices, LangGraph workflows, and API pipelines as planned, and the contract concluded with great feedback.*  
> *At the university AI Lab, my work focused heavily on academic research. While I learned deep LLM concepts, my true passion is **shipping practical solutions into live business operations** where technology drives direct efficiency.*  
> *That’s why I want to join Sharon JSC full-time—to work directly with leadership on real-world impact."*  

- **Ý chính tiếng Việt:** Hoàn thành tốt hợp đồng Innotech; Lab thiên về học thuật; đam mê thực sự là đưa AI vào thực chiến kinh doanh hàng ngày.

---

### 07. CHIẾN LƯỢC DEAL LƯƠNG: 8TR THỬ VIỆC $\rightarrow$ 12-15TR+ CHÍNH THỨC
> *"As we discussed, I am completely fine starting with **8 million VND net during the 2-month probation period**.*  
> *My immediate priority is to hit the ground running, master your workflows, and prove my delivery speed with initial automations.*  
> *However, once I pass probation and demonstrate tangible ROI—such as saving operational hours or eliminating lost customer leads—my expected salary for the official contract is **between 12 to 15 million VND**, with full standard benefits.*  
> *I believe in delivering measurable value first before asking for rewards."*  

- **Ý chính tiếng Việt:** Giữ chữ tín với số 8tr đã lỡ nói (coi là mức thử việc chứng minh năng lực); mở đường lên 12-15tr+ chính thức; chốt câu *"Delivering value first before asking for rewards"*.

---

### 08. DEAL REMOTE (WFH 2-3 NGÀY/THÁNG) & THIẾT BỊ LÀM VIỆC
> *"Given the 6-day work week, my commitment is to be **100% on-site initially** to understand the bakery floor and sales operations thoroughly.*  
> *Once the core pipelines are stable, is there flexibility for **2 to 3 days per month to work remotely for deep-focus coding and system architecture**?*  
> *I ensure 100% output transparency tracked via daily Git commits and progress logs.*  
> *Also, regarding hardware, does Sharon provide workstations/laptops, or is it a BYOD (Bring Your Own Device) setup?"*  

- **Ý chính tiếng Việt:** Cam kết on-site trước để nắm nghiệp vụ xưởng; xin 2-3 ngày/tháng ở nhà chỉ cho mục đích **"Deep-focus coding"**, đo bằng Git commit; hỏi rõ cấp máy hay tự mang laptop.

---

### 09. CƠ CẤU TEAM & LỘ TRÌNH MỞ RỘNG
> *"In this role, will I be working directly as an individual builder reporting to you, or is there an existing technical team?*  
> *And as the AI transformation expands, do you plan to scale the team with more developers?"*  

- **Ý chính tiếng Việt:** Xác định rõ làm một mình (solo builder) hay có đồng đội; thể hiện tầm nhìn quan tâm đến quy mô phát triển dài hạn.

---

### 10. ĐỀ XUẤT 4 TRỤ CỘT HỆ THỐNG AI NỘI BỘ CHO SHARON JSC
*(Dựa trên JD, Sharon chưa có sản phẩm AI đóng gói sẵn mà cần xây hệ thống tự động hóa nội bộ toàn diện)*:

1. **AI Operations Hub (Dashboard điều hành tập trung):**
   - Kết nối dữ liệu đa nguồn: CRM, ERP, POS, Kho hàng, Đơn đặt hàng, Sản xuất và Tài chính.
   - Hỗ trợ trả lời tức thì: *"Ngày mai xưởng Cát Lái cần nướng bao nhiêu baguette?", "Nguyên liệu bột/bơ nào sắp thiếu?", "Đơn hàng của khách sạn nào có nguy cơ trễ?"*
2. **Forecasting & Production Planning (Dự báo nhu cầu & Lập kế hoạch nướng):**
   - Dựa trên lịch sử đơn hàng, mùa vụ, lễ tết, hợp đồng khách sạn định kỳ và tồn kho thực tế.
   - Đề xuất số lượng sản xuất, lịch ca nướng, nguyên liệu cần mua. **Nguyên tắc:** Giai đoạn đầu *AI đề xuất, Quản lý xưởng bấm nút phê duyệt (Human-in-the-loop)*.
3. **Sales / CRM Copilot (Trợ lý bán hàng B2B):**
   - Tóm tắt lịch sử đơn hàng của từng khách sạn/quán café, tự soạn thảo báo giá theo hợp đồng, nhắc việc khi hợp đồng cung ứng sắp hết hạn, hỗ trợ phân loại lead Zalo.
4. **Production & QA Assistant (Hỏi đáp quy trình chuẩn RAG):**
   - Nhân viên xưởng hỏi: *"Quy trình xử lý batch bánh này?", "Shelf life của sản phẩm X?", "Nhiệt độ kho vượt mức xử lý sao?"*.
   - **RAG trên tài liệu chuẩn:** Chỉ trả lời dựa trên SOP, Recipe, QA checklist, HACCP đã duyệt; tuyệt đối không để AI tự bịa câu trả lời.

---

### 11. LỘ TRÌNH THỰC HIỆN 3 GIAI ĐOẠN & KIẾN TRÚC KỸ THUẬT

```text
CRM / ERP / POS / Kho
       ↓
  Data Warehouse (PostgreSQL)
       ↓
APIs + Workflow Automation (FastAPI / n8n)
       ↓
AI Agents + RAG Knowledge Base (Vector DB / LLM APIs)
       ↓
Dashboard điều hành / Zalo / Telegram Alert / Web App
```

- **Giai đoạn 1 (4–6 tuần) – MVP & Quick Wins:** Chuẩn hóa dữ liệu khách hàng & đơn hàng, Dashboard tồn kho cơ bản, Báo cáo tự động cuối ngày cho Founder qua Telegram/Zalo, Cảnh báo đơn trễ/hết nguyên liệu.
- **Giai đoạn 2 (2–3 tháng) – Mở rộng tích hợp:** Kết nối sâu CRM/ERP/POS, Chạy mô hình dự báo nhu cầu (Forecasting), Sinh lịch sản xuất tự động, Triển khai Sales Copilot & Phê duyệt workflow qua email/chat.
- **Giai đoạn 3 (6–12 tháng) – Tối ưu hóa sâu:** Tối ưu lịch nướng và lộ trình giao hàng liên tỉnh (HCM - HN - Phú Quốc), Phát hiện bất thường chất lượng bánh, Dự báo dòng tiền (cash flow).

---

### 12. CÂU NÓI CHIẾN LƯỢC & BỘ CÂU HỎI THÔNG MINH HỎI NGƯỢC FOUNDER

#### 💬 Câu chốt hạ đẳng cấp thể hiện tư duy kinh doanh (Business Mindset):
> *"If I were to start, I wouldn't build a fancy chatbot first. I would map the workflows from sales to production, clean the data schemas, and target a high-ROI bottleneck—such as demand forecasting or inventory alert.*  
> *Initially, AI acts as a **copilot** proposing recommendations for human approval. Once data accuracy and staff trust are proven, we scale to full agentic automation."*  
> *(Nếu được bắt đầu, em sẽ không xây chatbot hoa mỹ trước. Em sẽ mapping các quy trình từ sales đến xưởng bánh, chuẩn hóa dữ liệu và chọn một bài toán có ROI rõ như dự báo nhu cầu hoặc cảnh báo tồn kho. Giai đoạn đầu AI đóng vai trò copilot đề xuất để nhân sự phê duyệt. Khi dữ liệu và niềm tin đủ tốt mới mở rộng sang tự động hóa toàn phần).*

#### ❓ 3 Câu hỏi ngược đào sâu thực tế công ty:
1. *"What is the single biggest operational or sales bottleneck you want AI to solve in the first 30 days?"*  
   *(Điểm nghẽn lớn nhất trong 30 ngày đầu mà anh muốn AI xử lý là gì?)*
2. *"Between contract manufacturing for hotels/cafes and retail, which segment is creating the most communication overload right now?"*  
   *(Giữa mảng gia công B2B cho khách sạn/café và khách lẻ, nhóm nào đang làm nhân viên quá tải giao tiếp nhất?)*
3. *"How does the Cát Lái bakery currently sync daily orders and inventory with the Hanoi and Phu Quoc hubs?"*  
   *(Xưởng Cát Lái hiện đồng bộ đơn hàng và tồn kho với các hub Hà Nội và Phú Quốc qua công cụ gì?)*

---
*Tài liệu đúc kết đầy đủ, ngắn gọn, chuẩn ~220 dòng dành riêng cho buổi phỏng vấn Sharon JSC.*
