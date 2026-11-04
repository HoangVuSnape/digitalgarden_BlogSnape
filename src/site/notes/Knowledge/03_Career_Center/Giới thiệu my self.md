---
{"dg-publish":true,"permalink":"/knowledge/03-career-center/gioi-thieu-my-self/","pinned":"false"}
---



# 🎙️ BÀI GIỚI THIỆU HOÀN CHỈNH (THEO ĐÚNG DÒNG THỜI GIAN)

### 1. Mở đầu (Hook)
> *"Em chào anh/chị. Em là Vũ, tốt nghiệp ngành **Khoa học Máy tính** và hiện đang tiếp tục học chương trình **Thạc sĩ Khoa học Máy tính tại Đại học Tôn Đức Thắng (TDTU)**.*
>
> *Định hướng của em là một AI Engineer chuyên sâu về **Advanced RAG, AI Agents và tối ưu hóa hệ thống**."*

---

### 2. Dự án nền tảng: Tư vấn tuyển sinh (Từ RAG đến GraphRAG)
> *"Khởi điểm từ thời sinh viên, dự án mà em dành rất nhiều tâm huyết tự tay xây dựng từ đầu là **Hệ thống AI Tư vấn Tuyển sinh Đại học**.*
>
> * **Bài toán:** Thông tin tuyển sinh trên mạng rất phân mảnh, nhiều nguồn tin không chuẩn. Mục tiêu của em là xây dựng một **Knowledge Base** chuẩn xác tuyệt đối, chỉ lấy từ đề án chính thức của các trường.
>
> *Trong quá trình làm và liên tục thử nghiệm (test) thực tế nhiều trường hợp về mặt ngữ nghĩa, em đã giải quyết 4 bài toán lớn:*
>
> 1. **Về xử lý văn bản:** Thay vì cắt theo độ dài ký tự thô làm đứt gãy câu chữ, em dùng **Semantic Chunking** để cắt theo sự trọn vẹn ngữ nghĩa, sau đó lưu trữ và đánh chỉ mục vector trên **Qdrant**.
> 2. **Về tìm kiếm:** Khi test, em thấy có rất nhiều case về mã ngành, từ viết tắt mà Vector Search thuần túy rất dễ bỏ sót. Em kết hợp **Hybrid Search giữa BM25 và Vector**. Qua đo đạc bằng bộ metrics Ragas, cả độ phủ (Context Recall) và độ chính xác (Context Precision) đều tăng rõ rệt.
> 3. **Cơ chế tự phản biện và sửa sai:** Để chống hiện tượng AI 'chém gió' (hallucination) khi truy vấn không đủ, em áp dụng cơ chế của **Self-RAG và Corrective RAG (CRAG)**: Hệ thống tự đánh giá xem câu trả lời nháp có bám sát tài liệu không; nếu tài liệu bị thiếu hoặc câu hỏi mơ hồ, hệ thống sẽ kích hoạt **Query Rewriting** để viết lại câu truy vấn, tìm kiếm bổ sung rồi mới sinh lại câu trả lời.
> 4. **Mở rộng sang GraphRAG:** Khi thí sinh hỏi những câu so sánh đa chiều phức tạp như: *'Em 24 điểm khối A01 thì học được ngành IT nào ở TP.HCM có học phí dưới 30 triệu?'* – Vector RAG dạng văn bản phẳng bị đứt gãy ngữ cảnh. Em xây dựng **Knowledge Graph** liên kết giữa: **Trường ➡️ Ngành ➡️ Tổ hợp môn ➡️ Điểm chuẩn ➡️ Học phí** để AI duyệt đồ thị (Graph Traversal), giải quyết các câu hỏi bắc cầu nhiều bước (multi-hop reasoning) mà Vector thuần túy không làm được."*
>
> `[dừng 1s - bước chuyển quan trọng sang Research Lab]`

---

### 3. Nghiên cứu tại Research Lab TDTU (Cầu nối học thuật chuyên sâu)
> *"Sau khi hoàn thành hệ thống tuyển sinh và bước vào chương trình Thạc sĩ, em tham gia làm việc tại **Research Lab của trường TDTU**.*
>
> *Tại Lab, em tập trung chuyên sâu vào hướng **Model Distillation & Optimization** (Chưng cất và tối ưu hóa mô hình):*
> * Bài toán ở đây là làm sao nén các mô hình ngôn ngữ lớn cồng kềnh thành các mô hình nhỏ gọn (small language models) để chạy được trên phần cứng giới hạn.
> * Em trực tiếp thực hiện các thực nghiệm nén mô hình, tối ưu hóa quá trình inference để vừa giảm độ trễ (latency), tiết kiệm chi phí tính toán, mà vẫn giữ được độ chính xác tương đương mô hình gốc.  
>
> *Quá trình làm tại Research Lab giúp em hiểu rất sâu về bản chất bên trong của mô hình và tư duy tối ưu tài nguyên tính toán."*
>
> `[dừng 1s - bước chuyển ra Doanh nghiệp]`

---

### 4. Thực chiến Doanh nghiệp: Innotech & Dự án gần nhất (Agent trên Database / HR)
> *"Mang những kinh nghiệm từ cả RAG lẫn tối ưu mô hình ra môi trường doanh nghiệp, em gia nhập **Innotech** và đặc biệt trong **3 tháng gần đây**, em giữ vai trò **kỹ sư phụ trách chính (Core Engineer)** phát triển giải pháp **AI Agent hỏi đáp trực tiếp trên Database nội bộ**.*
>
> *Ví dụ cụ thể là mảng **Dữ liệu Nhân sự (HR)**:*
> * Em trực tiếp thiết kế pipeline Data Engineering kết hợp Agentic Workflow, cho phép HR Manager hoặc nhân viên hỏi bằng ngôn ngữ tự nhiên, ví dụ: *'Thống kê danh sách nhân sự thử việc quý này kèm đánh giá hiệu suất?'*
> * Hệ thống sẽ tự phân tích ý định, sinh câu truy vấn SQL an toàn (Text-to-SQL kèm guardrails chặt chẽ), truy xuất dữ liệu có cấu trúc và tổng hợp thành báo cáo trực quan cho người dùng. Toàn bộ logic routing và kiểm soát an toàn em đều tự tay lập trình."*
>
> `[dừng 1s - kết bài]`

---

### 5. Kiến trúc Mở & Tầm nhìn tương lai
> *"Về mặt kiến trúc công nghệ, em luôn thiết kế hệ thống theo tiêu chí **Model-Agnostic (không bó buộc vào một bên nào)**:*
> * Hệ thống có thể linh hoạt cấu hình động qua **Base URL, API Key và Model ID** – dễ dàng switch qua lại giữa các Cloud API (OpenAI, Claude) hoặc **Local LLMs tự host qua Ollama/vLLM** khi doanh nghiệp yêu cầu bảo mật dữ liệu tuyệt đối.
> * Đồng thời, em ứng dụng chuẩn **MCP (Model Context Protocol)** để chuẩn hóa cách AI Agent kết nối với các công cụ và cơ sở dữ liệu bên ngoài.
>
> *Tóm lại, từ tư duy thuật toán của ngành **Khoa học máy tính**, kinh nghiệm thực nghiệm chuyên sâu tại **Research Lab TDTU**, cho đến khả năng trực tiếp làm chủ sản phẩm **Agent & RAG tại doanh nghiệp**, em tin mình có thể tạo ra giá trị ngay cho các bài toán AI của công ty. Em cảm ơn anh/chị ạ."*