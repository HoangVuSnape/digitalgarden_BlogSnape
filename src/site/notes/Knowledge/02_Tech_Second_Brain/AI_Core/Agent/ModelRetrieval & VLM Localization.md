---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/agent/model-retrieval-and-vlm-localization/","title":"ModelRetrieval VLM Localization","pinned":"false"}
---

Các câu hỏi: quantization của nó, nó được train như thế nào

# huyydangg/DEk21_hcmute_embedding

- [bkai-foundation-models/vietnamese-bi-encoder](https://huggingface.co/bkai-foundation-models/vietnamese-bi-encoder)
- **100,000 examples** - [BKAI -data ](https://www.kaggle.com/datasets/dintrn/legal-document-retrieval-bkai?select=corpus.csv)
- huyydangg/LEGAL-EVAL-Dataset: 3.87K 
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828106.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828271.png)

---
## Note
- Không biết có phải là [Data](https://huggingface.co/datasets/microsoft/ms_marco) translate to vietnamese. 
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828452.png)
- [SQUAD v2](https://www.kaggle.com/datasets/nkhachao/vietnamese-squad) Nó được train 
	- 36 hours to perform the translation on a fairly decent GPU
	-  142.192 question-answer pairs, with 11.873 from the dev set and 130.319 from the training set.
		- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828610.png)
-  Legal Text Retrieval [Data](https://www.kaggle.com/datasets/hariwh0/zaloai2021-legal-text-retrieval)
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828743.png)


# VLM Localization

## Link
- [Surfer-H Meets Holo1: Cost-Efficient Web Agent Powered by Open Weights](https://arxiv.org/pdf/2506.02865)

- As part of a broader agentic architecture, Holo1 acts as a policy, localizer, or validator, helping the agent understand and act in digital environments.

- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828887.png)

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215829079.png)

## Summary
Bài báo giới thiệu **Surfer-H**, một tác nhân web (web agent) hiệu quả về chi phí, tích hợp Mô hình Ngôn ngữ-Thị giác (VLM) để thực hiện các tác vụ do người dùng xác định trên web. Surfer-H được kết hợp với **Holo1**, một bộ sưu tập VLM mã nguồn mở mới, chuyên về điều hướng web và trích xuất thông tin.

**Tóm tắt chi tiết:**

1. **Vấn đề và Giải pháp:**
    
    - Các Mô hình Ngôn ngữ Lớn (LLM) truyền thống bị giới hạn trong một thế giới tĩnh và không thể thực hiện hành động hoặc truy cập thông tin cập nhật. Các tác nhân công cụ (tool-use agents) mở rộng LLM nhưng vẫn bị giới hạn bởi các công cụ được xác định trước.
    - Surfer-H giải quyết vấn đề này bằng cách tương tác trực tiếp với phần mềm thông qua Giao diện người dùng đồ họa (GUI) bằng cách sử dụng ảnh chụp màn hình, tương tự cách con người tương tác. Cách tiếp cận này giúp tránh sự phụ thuộc vào các tích hợp tùy chỉnh hoặc API, mở ra tiềm năng cho các tác nhân đa năng hơn.
2. **Kiến trúc Surfer-H:**
    
    - Surfer-H bao gồm ba module chính: **policy** (chính sách), **localizer** (bộ định vị), và **validator** (bộ xác thực), hoạt động theo trình tự.
    - **Policy** (Chính sách): Đề xuất các hành động cần thực hiện (ví dụ: nhấp, gõ văn bản, cuộn, làm mới trang, trả lời) và tạo ra suy nghĩ (thoughts) và ghi chú (notes) bằng ngôn ngữ tự nhiên.
    - **Localizer** (Bộ định vị): Nếu một hành động yêu cầu tương tác với một phần tử cụ thể trên trang web (ví dụ: nút, thanh tìm kiếm), localizer sẽ xác định và cung cấp tọa độ 2D của phần tử đó từ ảnh chụp màn hình.
    - **Validator** (Bộ xác thực): Khi tác nhân đưa ra câu trả lời, validator sẽ xem xét câu trả lời đó để phê duyệt. Nếu câu trả lời hợp lệ, tác vụ kết thúc; nếu không, phản hồi sẽ được đưa vào bộ nhớ của tác nhân để tiếp tục thực hiện.
    - Surfer-H chỉ sử dụng ảnh chụp màn hình và không yêu cầu Document Object Model (DOM) hoặc cây truy cập của trang web.
3. **Huấn luyện Holo1:**
    
    - Holo1 là một bộ VLM được thiết kế để cung cấp năng lượng cho các module của Surfer-H, tập trung vào khả năng hiểu thông tin phức tạp trên trang web và ánh xạ trạng thái sang hành động một cách chính xác.
    - Quá trình huấn luyện sử dụng một hỗn hợp lớn các tập dữ liệu đa dạng để bao quát tính rộng và phức tạp của web hiện đại. Các nhóm dữ liệu chính bao gồm:
        - **GUI Grounding (50.79%):** Dữ liệu thu thập từ web (WebCrawl) và dữ liệu tổng hợp (WebSynthetic) để phát hiện các phần tử UI dựa trên tín hiệu hình ảnh. WebCrawl chứa 89 triệu lần nhấp từ 4 triệu trang web, cùng với dữ liệu từ các tập dữ liệu mã nguồn mở như OS-Atlas. WebSynthetic được tạo ra để giải quyết các thách thức cụ thể như lịch, bảng dữ liệu quan hệ và giải thích biểu tượng.
        - **Complex Visual Understanding (32.28%):** Dữ liệu để đánh giá các đầu ra của localizer (Coordinate Validation), trích xuất các phần tử tương tác từ trang web (UI Extraction), và Hỏi đáp hình ảnh (VQA) tập trung vào biểu đồ, bảng và hiểu tài liệu.
        - **Behavior Learning (16.93%):** Dữ liệu dấu vết đa phương thức (multimodal traces) từ quá trình thực thi của tác nhân, cho phép mô hình học cách hành xử như một tác nhân, suy luận qua các ngữ cảnh dài và dự đoán hành động tiếp theo dựa trên lịch sử tác vụ.
            - Các dấu vết này được tạo ra từ hai bộ dữ liệu tác vụ: **WebVoyager** (643 tác vụ trên 15 trang web phổ biến) và **WebVoyagerExtended** (15.000 tác vụ trên 330 trang web).
        - **Feedback and Validation Learning:** Dữ liệu cho phép mô hình học cách đánh giá câu trả lời của tác nhân dựa trên nhiệm vụ và bằng chứng trực quan.
    - Mô hình Holo1 được tinh chỉnh từ trọng số Qwen 2.5-VL-Instruct và được huấn luyện trên toàn bộ tập dữ liệu để bao quát tất cả các khả năng của module (policy, localizer, validator).
4. **Các Benchmark và Đánh giá:**
    
    - **WebVoyager:** Là một bộ tiêu chuẩn chính thống để đánh giá tác nhân web. Surfer-H được đánh giá trên 643 tác vụ từ 10 trang web.
        - Điểm thành công được tính dựa trên đa số phiếu từ ba mẫu GPT-4o.
        - Surfer-H được phép thực hiện tối đa 30 bước và 10 lần thử lại cho mỗi tác vụ.
    - **Screenspot** (bao gồm Screenspot-V2, Screenspot-Pro) và **GroundUI**: Các benchmark hiện có tập trung vào định vị UI nói chung trên nhiều ứng dụng và nền tảng khác nhau.
    - **WebClick:** Một benchmark mới được giới thiệu, được thiết kế đặc biệt để đánh giá khả năng định vị UI trên môi trường web, nơi có các thành phần phức tạp và động như lịch và menu lồng nhau.
        - WebClick chứa 1.639 ảnh chụp màn hình từ hơn 100 trang web, được tuyển chọn từ dữ liệu tác vụ WebVoyager của tác nhân, tương tác web của con người trong các tác vụ hàng ngày và tương tác với giao diện lịch.
        - Nó được phát hành công khai theo giấy phép Apache-2 trên Hugging Face.

**Những điểm nổi bật:**

- **Hiệu suất tiên tiến và hiệu quả chi phí:** Surfer-H kết hợp với Holo1 đạt 92.2% độ chính xác trên WebVoyager sau 10 lần thử, ngang bằng với Surfer-H+GPT-4.1 (92.0%) nhưng với chi phí thấp hơn đáng kể ($0.13/tác vụ so với $0.54/tác vụ). Điều này cho thấy sự cân bằng Pareto-tối ưu giữa độ chính xác và hiệu quả chi phí.
- **Kỹ năng định vị vượt trội của Holo1:** Các mô hình Holo1 vượt trội hơn các mô hình tiên tiến khác cùng kích thước trên các benchmark định vị, bao gồm Screenspot và WebClick mới được giới thiệu. Holo1-3B và Holo1-7B đạt hiệu suất định vị trung bình cao nhất trong số các mô hình cùng kích thước.
- **Dữ liệu huấn luyện đa dạng và độc đáo:** Holo1 được huấn luyện trên sự kết hợp độc đáo của dữ liệu thu thập từ web, dữ liệu tổng hợp, và đặc biệt là dữ liệu dấu vết từ các lần thực thi tác nhân (bao gồm cả WebVoyager và WebVoyagerExtended). Điều này giúp mô hình học được các hành vi tác nhân phức tạp, khả năng quản lý bộ nhớ và lập kế hoạch.
- **Nguồn mở và đóng góp cho cộng đồng:** Nhóm đã công khai bộ dữ liệu đánh giá WebClick và trọng số mô hình Holo1 để thúc đẩy nghiên cứu trong lĩnh vực hệ thống tác nhân.
- 
## Key
- WebClick is a new benchmark specifically designed for evaluating web UI localization capabilities of Vision-Language Models (VLMs)
- 


Question:
- nó giống selenium không?

