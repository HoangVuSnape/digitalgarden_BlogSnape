---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/nlp/sllm-and-llm/","pinned":"false","tags":["type/paper","topic/slm","topic/llm","status/stable"]}
---

# Link
- [[2409.15790] Small Language Models: Survey, Measurements, and Insights](https://arxiv.org/abs/2409.15790)
	- [UbiquitousLearning/SLM_Survey](https://github.com/UbiquitousLearning/SLM_Survey)

- [2410.20011 - ASurvey of Small Language Models](https://arxiv.org/pdf/2410.20011)


- [Vibe-Tuning: The Art of Fine-Tuning Small Language Models with a Prompt](https://www.distillabs.ai/blog/vibe-tuning-the-art-of-fine-tuning-small-language-models-with-a-prompt)
	- Trong này có các resource review thử về lĩnh vực này. 
- [Perlexity SLLM](https://www.perplexity.ai/search/minh-dang-muon-nghien-cuu-linh-Qk6v71LUQ5ySkY3uCleiQg#0)
- [NexaAI/Awesome-LLMs-on-device: Awesome LLMs on Device: A Comprehensive Survey](https://github.com/NexaAI/Awesome-LLMs-on-device)


- [Build a Small Language Model (SLM) From Scratch | by Shravan Kumar | Medium](https://medium.com/@shravankoninti/build-a-small-language-model-slm-from-scratch-3ddd13fa6470)
	- [Vizuara AI Labs Small Language Model Scratch Final.ipynb - Colab](https://colab.research.google.com/drive/1k4G3G5MxYLxawmPfAknUN7dbbmyqldQv?usp=sharing&source=post_page-----3ddd13fa6470---------------------------------------#scrollTo=s_2WjvUszhIe)
	- 
# Question
Định Hướng nghiên cứu SLLM  thì cần làm gì.
- Để viết blog hay nghiên cứu lĩnh vực thì học đang nghiên cứu cái gì.

# Imgs
## Paper  1 - survey
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20251126202858594.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20251126202953815.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20251126203011049.png)
# Tổng hợp Xu hướng LLM 2024: Hiệu năng, Kiến trúc & Dữ liệu

Note tổng hợp phân tích từ các báo cáo nghiên cứu về sự tiến hóa của LLM (Small Language Models & Open Source LLMs) giai đoạn 2022 - 2024.

---

## 1. Đánh giá Hiệu năng (Performance Benchmark)
*Bối cảnh: So sánh các mô hình nhỏ (SLM) chạy trên thiết bị (On-device AI) như OpenELM, Phi-2, Qwen, Gemma.*

### a) First Token Time (Time To First Token - TTFT)
* **Ý nghĩa:** Độ trễ ban đầu. Thời gian từ lúc bấm "Gửi" đến khi chữ đầu tiên hiện ra.
* **Đánh giá:** Càng **thấp** càng tốt (Phản hồi tức thì).
* **Insight:** Các mô hình mới (như OpenELM) dù có kích thước lớn hơn nhưng đã tối ưu được độ trễ thấp hơn so với các đối thủ cũ (Pythia).

### b) Decode Latency per Token
* **Ý nghĩa:** Tốc độ "tuôn chữ". Khả năng duy trì hội thoại sau token đầu tiên.
* **Đánh giá:** Càng **thấp** càng tốt. Nếu cao $\rightarrow$ trải nghiệm chậm chạp.

### c) Memory Usage (RAM/VRAM)
* **Ý nghĩa:** Tài nguyên phần cứng tiêu thụ. Chỉ số sống còn cho Edge AI (điện thoại, laptop).
* **Đánh giá:** Càng **thấp** càng tốt.
* **Điểm nhấn:** Xu hướng phá vỡ quy luật "To là Nặng".
    * *Ví dụ:* OpenELM-3B (+21.1% tham số so với Gemma-2B) nhưng lại tiêu tốn ít hơn 13.8% bộ nhớ.
    * $\rightarrow$ Chứng tỏ kiến trúc được tối ưu hóa cực tốt cho phần cứng hạn chế.

---

## 2. Xu hướng Kiến trúc (Architectural Trends)
*Công thức "Build" một LLM hiện đại (State-of-the-Art) năm 2024/2025.*

### Các thành phần cốt lõi:
1.  **Attention Types (Cơ chế chú ý):**
    * *Dịch chuyển:* Từ **MHA** (Multi-Head Attention) $\rightarrow$ **GQA** (Grouped Query Attention).
    * *Lý do:* GQA (dùng trong LLaMA 2, Mistral) giúp suy luận nhanh hơn, tốn ít bộ nhớ hơn MHA nhưng vẫn giữ được độ thông minh.

2.  **FFN Types (Mạng nơ-ron):**
    * *Dịch chuyển:* Từ Standard FFN $\rightarrow$ **Gated FFN** (SwiGLU).
    * *Lý do:* Học các biểu diễn phức tạp tốt hơn. Đây đã trở thành tiêu chuẩn ngành.

3.  **Activation (Hàm kích hoạt):**
    * *Dịch chuyển:* Từ GELU/ReLU $\rightarrow$ **SiLU**.
    * *Lý do:* Hoạt động mượt mà, giúp quá trình training ổn định hơn.

4.  **Normalization (Chuẩn hóa):**
    * *Dịch chuyển:* Layer Norm $\rightarrow$ **RMS Norm**.
    * *Lý do:* Tính toán đơn giản hơn, nhanh hơn và giúp model hội tụ tốt hơn.

5.  **Vocab (Bộ từ điển):**
    * *Dịch chuyển:* Từ nhỏ ($\le$ 50k) $\rightarrow$ **Lớn (>100k - 200k)**.
    * *Lý do:* Hỗ trợ đa ngôn ngữ tốt hơn và nén dữ liệu hiệu quả hơn.

> **Công thức vàng cho Model 2025:**
> GQA (Attention) + Gated SiLU (FFN) + RMS Norm + Large Vocab.

---

## 3. Xu hướng Dữ liệu (Pre-training Datasets)
*Sự thay đổi tư duy từ "Quantity" (Số lượng) sang "Quality" (Chất lượng).*

### Giai đoạn 1: 2022 - Kỷ nguyên "Vơ vét"
* **Đặc điểm:** Phân mảnh, dùng bất cứ gì tìm được.
* **Dataset:** The Pile, WuDaoCorpora, Reddit dump.

### Giai đoạn 2: 2023 - Kỷ nguyên "The Pile & Code"
* **Đặc điểm:** The Pile thống trị (chiếm ~69%).
* **Bước ngoặt:** Bắt đầu tích hợp dữ liệu Code (StarCoder) vào training.
* *Insight:* Học Code giúp AI tư duy logic tốt hơn, kể cả trong bài toán ngôn ngữ thường.

### Giai đoạn 3: 2024 - Kỷ nguyên "Refined & Synthetic"
* **Đặc điểm:** The Pile suy tàn (do nhiễu/rác). Lên ngôi của dữ liệu sạch và dữ liệu nhân tạo.
* **Dataset nổi bật:**
    * **RefinedWeb / RedPajama v2:** Dữ liệu web được lọc cực kỹ (loại bỏ quảng cáo, rác).
    * **FineWeb-Edu:** Chỉ giữ lại nội dung mang tính giáo dục cao.
    * **Cosmopedia:** Dữ liệu nhân tạo (Synthetic data) - dùng AI viết sách giáo khoa để dạy lại AI.

> **Actionable Advice:** Khi train/fine-tune model mới, **tránh dùng The Pile**. Hãy ưu tiên **FineWeb-Edu** hoặc **RedPajama** để đạt hiệu quả cao nhất.