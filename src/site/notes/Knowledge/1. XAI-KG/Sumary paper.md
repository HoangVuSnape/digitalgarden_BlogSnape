---
{"dg-publish":true,"permalink":"/knowledge/1-xai-kg/sumary-paper/","title":"Sumary paper","pinned":"false"}
---

#  Introduction 
- Đây là chỗ mình note lại để nói đến các bài paper của mình\ 
# A Survey of the State of Explainable AI for Natural Language Processing

**Báo cáo Tóm tắt về Khảo sát các Bài báo Hội nghị về AI Giải thích được cho Xử lý Ngôn ngữ Tự nhiên (2013-2019)**

Báo cáo này tóm tắt các phần quan trọng của bài báo "A Survey of the State of Explainable AI for Natural Language Processing" của Danilevsky và cộng sự. Bài khảo sát này nhằm mục đích cung cấp một cái nhìn tổng quan về tình hình hiện tại của **Trí tuệ Nhân tạo Giải thích được (XAI)** trong lĩnh vực **Xử lý Ngôn ngữ Tự nhiên (NLP)**, dựa trên các bài báo được công bố tại các hội nghị NLP lớn (ACL, NAACL, EMNLP, COLING) từ năm 2013 đến 2019. Mục tiêu của các tác giả là giúp các nhà phát triển hiểu rõ hơn về các kỹ thuật XAI hiện có cho mô hình NLP và chỉ ra những khoảng trống hiện tại trong lĩnh vực nghiên cứu này.

**Động lực:** Bài báo nhấn mạnh rằng mặc dù những tiến bộ gần đây đã cải thiện đáng kể chất lượng của các mô hình NLP tiên tiến, nhưng điều này thường đi kèm với việc **mô hình trở nên kém giải thích hơn**, đặc biệt là với sự trỗi dậy của các kỹ thuật hộp đen như học sâu. Sự thiếu minh bạch trong quá trình mô hình đưa ra kết quả có thể gây ra vấn đề, làm giảm niềm tin vào các hệ thống AI và cản trở phản hồi hữu ích từ người dùng để cải thiện mô hình. Do đó, XAI đã nổi lên như một lĩnh vực quan trọng, và khảo sát này tập trung cụ thể vào các công trình XAI trong lĩnh vực NLP. Theo hiểu biết tốt nhất của các tác giả, đây là khảo sát đầu tiên tập trung vào XAI trong NLP. Khảo sát tập trung vào "**giải thích kết quả**", nơi mục tiêu là hiểu cách một mô hình đưa ra một kết quả cụ thể.

**Phương pháp:** Các nhà nghiên cứu đã áp dụng một phương pháp có hệ thống để xác định và phân tích các bài báo liên quan.
*   **Xác định bài báo:** Họ xem xét các bài báo được công bố từ năm 2013 đến 2019 tại các hội nghị NLP lớn: ACL, NAACL, EMNLP và COLING.
*   **Lọc từ khóa:** Họ lọc các bài báo này dựa trên sự hiện diện của các thuật ngữ (đã được lemmat hóa) liên quan đến XAI (ví dụ: "explainability", "interpretability", "transparent") trong tiêu đề của chúng, thu được một tập hợp ban đầu gồm 107 bài báo. Lý do là nếu khả năng giải thích là một thành phần chính của công trình, các tác giả có nhiều khả năng sử dụng các từ khóa liên quan trong tiêu đề.
*   **Xác minh phạm vi:** Mỗi bài báo trong số 107 bài báo đã được xem xét để xác nhận rằng nó tập trung vào khả năng giải thích như một phương tiện để hiểu cách một mô hình đưa ra kết quả của nó. Các bài báo không phù hợp với tiêu chí này đã bị loại trừ. Quá trình lọc này dẫn đến tập hợp cuối cùng gồm **50 bài báo** được đưa vào khảo sát.
*   **Phân loại:** Các bài báo được chọn sau đó được phân loại dựa trên các khía cạnh được định nghĩa trong Mục 3 và 4 của bài khảo sát. Để đảm bảo **sự phân loại nhất quán**, mỗi bài báo được phân tích riêng lẻ bởi ít nhất **hai người đánh giá**, và tham khảo thêm ý kiến của những người đánh giá khác trong trường hợp có sự bất đồng. Để đơn giản hóa việc trình bày, mỗi bài báo được gán nhãn với danh mục chính áp dụng cho từng khía cạnh, mặc dù một số bài báo có thể bao gồm nhiều danh mục với mức độ nhấn mạnh khác nhau. Một trang web tương tác cũng đã được xây dựng để cung cấp thông tin chi tiết về từng bài báo được bao gồm trong khảo sát.

**Phân loại Giải thích:** Khảo sát phân loại các lời giải thích theo hai khía cạnh chính:
*   **Cục bộ (Local) so với Toàn cục (Global):** Phân biệt giữa các lời giải thích cho **một dự đoán cụ thể (cục bộ)** và các lời giải thích cho **toàn bộ quá trình dự đoán của mô hình (toàn cục)**. Phần lớn các bài báo được khảo sát (46 trên 50) tập trung vào các lời giải thích cục bộ.
*   **Tự giải thích (Self-Explaining) so với Hậu nghiệm (Post-Hoc):** Phân biệt giữa các lời giải thích **nảy sinh trực tiếp từ quá trình dự đoán (tự giải thích)** và những lời giải thích **đòi hỏi các bước xử lý bổ sung sau đó (hậu nghiệm)**. Các phương pháp tự giải thích tạo ra lời giải thích đồng thời với dự đoán, sử dụng thông tin từ quá trình dự đoán của mô hình. Các phương pháp hậu nghiệm yêu cầu các thao tác riêng biệt sau khi mô hình đã đưa ra dự đoán.

**Các khía cạnh của Giải thích:** Ngoài việc phân loại cấp cao, khảo sát còn khám phá hai khía cạnh quan trọng khác:
*   **Kỹ thuật Giải thích:** Đây là các cơ chế được sử dụng để tạo ra các "**giải thích thô**" hoặc các lý do toán học đằng sau kết quả của mô hình. Khảo sát xác định năm kỹ thuật chính:
    *   **Độ quan trọng của đặc trưng (Feature importance):** Giải thích bằng cách điều tra điểm số quan trọng của các đặc trưng khác nhau được sử dụng để đưa ra dự đoán cuối cùng. Cơ chế chú ý (attention mechanism) và độ nhạy đạo hàm bậc nhất (first-derivative saliency) là hai hoạt động thường được sử dụng ở đây.
    *   **Mô hình thay thế (Surrogate model):** Giải thích dự đoán của mô hình bằng cách học một mô hình thứ hai, thường dễ giải thích hơn, như một proxy. LIME là một ví dụ nổi tiếng sử dụng thao tác xáo trộn đầu vào (input perturbation).
    *   **Dựa trên ví dụ (Example-driven):** Giải thích dự đoán của một phiên bản đầu vào bằng cách xác định và trình bày các phiên bản khác, thường từ dữ liệu đã được gán nhãn, có ngữ nghĩa tương tự với phiên bản đầu vào.
    *   **Dựa trên nguồn gốc (Provenance-based):** Cung cấp lời giải thích bằng cách minh họa một phần hoặc toàn bộ quá trình suy diễn dự đoán.
    *   **Suy diễn khai báo (Declarative induction):** Tạo ra các biểu diễn dễ đọc cho con người, chẳng hạn như các quy tắc, cây quyết định và chương trình, làm lời giải thích.
    Các phương pháp **độ quan trọng của đặc trưng** và **mô hình thay thế** được sử dụng thường xuyên nhất.
*   **Kỹ thuật Trực quan hóa:** Tập trung vào cách các "**giải thích thô**" được trình bày cho người dùng cuối. Các kỹ thuật phổ biến bao gồm:
    *   **Độ nổi bật (Saliency):** Trực quan hóa điểm số quan trọng bằng cách sử dụng bản đồ nhiệt (heatmap) hoặc làm nổi bật. Đây là kỹ thuật trực quan hóa chiếm ưu thế và có mối tương quan chặt chẽ với độ quan trọng của đặc trưng.
    *   **Biểu diễn khai báo thô (Raw declarative representations):** Trực tiếp trình bày các quy tắc, cây quyết định hoặc chương trình đã học được.
    *   **Giải thích bằng ngôn ngữ tự nhiên (Natural language explanation):** Diễn đạt lời giải thích bằng ngôn ngữ mà con người có thể hiểu được, thường sử dụng các mẫu hoặc các mô hình ngôn ngữ phức tạp hơn.
    *   **Ví dụ thô (Raw examples):** Trình bày các phiên bản đầu vào tương tự như lời giải thích.

**Các Thao tác để Kích hoạt Khả năng Giải thích:** Khảo sát chi tiết các thao tác khác nhau được sử dụng kết hợp với các kỹ thuật giải thích để tạo ra lời giải thích:
*   **Độ nhạy đạo hàm bậc nhất (First-derivative saliency):** Ước tính đóng góp của đầu vào đối với đầu ra bằng cách tính đạo hàm riêng.
*   **Lan truyền độ liên quan theo lớp (Layer-wise relevance propagation):** Một cách khác để gán độ liên quan cho các đặc trưng được tính toán ở bất kỳ lớp trung gian nào của mạng nơ-ron.
*   **Xáo trộn đầu vào (Input perturbations):** Tạo ra các biến thể ngẫu nhiên của đầu vào để huấn luyện một mô hình dễ giải thích hơn (thường là mô hình tuyến tính).
*   **Cơ chế chú ý (Attention):** Một chiến lược để mạng nơ-ron giải thích các dự đoán bằng cách thêm các lớp chú ý, cho biết mô hình đang "**tập trung**" vào đâu.
*   **Tín hiệu cổng LSTM (LSTM gating signals):** Sử dụng đầu ra của các cổng bên trong các ô LSTM để giải thích.
*   **Thiết kế kiến trúc nhận biết khả năng giải thích (Explainability-aware architecture design):** Thiết kế kiến trúc mạng nơ-ron mô phỏng quá trình con người sử dụng để đưa ra giải pháp, làm cho mô hình học được (một phần) có thể giải thích được.

**Chất lượng Giải thích:** Khảo sát thảo luận về khía cạnh quan trọng là đánh giá chất lượng của các lời giải thích.
*   **Kỹ thuật Đánh giá:** Lĩnh vực này còn non trẻ, do đó có ít sự đồng thuận về cách đánh giá các lời giải thích. Phần lớn các công trình được xem xét (32 trên 50) thiếu đánh giá tiêu chuẩn hoặc chỉ bao gồm đánh giá không chính thức. Các phương pháp đánh giá chính thức hơn bao gồm so sánh với dữ liệu ground truth và đánh giá của con người.
*   **Độ bao phủ của quá trình dự đoán (Predictive Process Coverage):** Một khía cạnh quan trọng nhưng thường bị bỏ qua là phần nào của quá trình dự đoán (từ đầu vào đến đầu ra của mô hình) được bao phủ bởi một lời giải thích. Nhiều phương pháp giải thích chỉ giải thích một phần của quá trình này. Mức độ bao phủ có thể phụ thuộc vào các kỹ thuật giải thích được sử dụng.

**Những hiểu biết sâu sắc và hướng nghiên cứu tương lai:** Khảo sát kết luận bằng cách vạch ra một số hiểu biết sâu sắc và hướng nghiên cứu trong tương lai:
*   **Nhu cầu về thuật ngữ rõ ràng hơn và hiểu biết về khả năng giải thích** liên quan đến đối tượng mục tiêu.
*   **Mở rộng quy trình và chỉ số đánh giá**, đặc biệt là đánh giá của con người, là rất quan trọng. Cần có các chỉ số tiêu chuẩn để nghiên cứu cáctrade-off tiềm năng giữa khả năng giải thích và các đặc điểm khác của mô hình.
*   **Xem xét nghiêm túc vấn đề độ trung thực (fidelity) hoặc tính nhân quả (causality)** để đảm bảo rằng các lời giải thích được đưa ra thực sự giải thích đáng tin cậy cho dự đoán của mô hình.
*   Số lượng bài báo tập trung vào **giải thích toàn cục** còn thấp. Các mô hình hộp trắng có thể là một nền tảng tốt để nghiên cứu các kỹ thuật đánh giá lời giải thích.

Tóm lại, bài khảo sát này cung cấp một cái nhìn toàn diện về bối cảnh nghiên cứu về AI Giải thích được cho Xử lý Ngôn ngữ Tự nhiên từ năm 2013 đến 2019, chi tiết các phương pháp, kỹ thuật và thách thức khác nhau trong lĩnh vực này. Nó nhấn mạnh tầm quan trọng ngày càng tăng của khả năng giải thích trong NLP và kêu gọi nghiên cứu sâu hơn trong các lĩnh vực như chỉ số đánh giá tiêu chuẩn và hiểu biết sâu sắc hơn về những gì tạo nên một lời giải thích tốt.

---
# Responsibility: An Example-based Explainable AI approach via Training Process Inspection
- [Responsibility: An Example-based Explainable AI approach via Training Process Inspection](https://arxiv.org/pdf/2209.03433)
![](/img/user/assets/images/paper_1.png)

Nguồn cũng cho thấy các phương pháp XAI khác sử dụng các kỹ thuật trực quan hóa:
- **Grad-CAM**: Một phương pháp phổ biến cho mạng nơ-ron tích chập (CNN) để làm nổi bật các vùng phân biệt của hình ảnh mà mạng sử dụng để dự đoán lớp của hình ảnh. Hình 1 trong nguồn cho thấy một ví dụ về trực quan hóa Grad-CAM.
- **LIME (Local Interpretable Model-agnostic Explanations)**: Phương pháp này học một mô hình đơn giản, dễ diễn giải (thường là tuyến tính) xung quanh dự đoán cụ thể của một mô hình phức tạp hơn. Hình 1 trong nguồn cũng minh họa cách LIME có thể làm nổi bật các phần quan trọng của hình ảnh.
- **SHAP (Shapley Additive Explanations)**: Một phương pháp tiếp cận thống nhất để giải thích đầu ra của bất kỳ mô hình học máy nào, bằng cách tính toán đóng góp của mỗi đặc trưng vào dự đoán. Hình 1 trong nguồn hiển thị một ví dụ về trực quan hóa SHAP.
- **Responsibility**: Phương pháp này xác định ví dụ huấn luyện có trách nhiệm cao nhất cho một quyết định cụ thể và hiển thị ví dụ đó như một lời giải thích. Hình 1, 5 và 6 trong nguồn cho thấy các cách trực quan hóa ví dụ huấn luyện có trách nhiệm. Nguồn này cũng sử dụng **biểu đồ violin (violin plot)** (Hình 2) và **bản đồ nhiệt (heatmap)** (Hình 3) để phân tích và trực quan hóa các giá trị Responsibility.
- **Influence Functions**: Phương pháp này xác định các mẫu huấn luyện có ảnh hưởng nhất đến dự đoán của mô hình. Hình 1 trong nguồn minh họa một ví dụ trực quan hóa Influence Functions.
- **Nearest Neighbor (NN)**: Hiển thị ví dụ huấn luyện gần nhất với phiên bản đầu vào để cung cấp sự hiểu biết dựa trên sự tương đồng. Hình 1 trong nguồn cho thấy một ví dụ.

Tóm lại, có nhiều kỹ thuật trực quan hóa được sử dụng trong XAI, từ việc làm nổi bật các phần quan trọng của dữ liệu đầu vào (như saliency maps và highlighting), đến việc hiển thị các cấu trúc bên trong của mô hình (như quy tắc và chương trình), cung cấp lời giải thích bằng ngôn ngữ tự nhiên, hoặc trình bày các ví dụ liên quan từ dữ liệu huấn luyện. Sự lựa chọn kỹ thuật trực quan hóa phụ thuộc vào loại mô hình, loại dữ liệu, nhiệm vụ cụ thể và đối tượng người dùng mục tiêu.

---
# Explainable Artificial Intelligence: A Survey of Needs, Techniques, Applications, and Future Direction
Link: [Explainable Artificial Intelligence: A Survey of Needs, Techniques, Applications, and Future Direction](https://arxiv.org/pdf/2409.00265)
- [A Literature Review on Applications of Explainable Artificial Intelligence (XAI)](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10908240)
- ![](/img/user/assets/images/paper_3.png)
- ![](/img/user/assets/images/paper_4.png)

- ![](/img/user/assets/images/paper_5.png)
## Khái niệm cần nắm
- ![](/img/user/assets/images/paper_6.png)
## Stakeholders XAI
- ![](/img/user/assets/images/paper_7.png)
- ![](/img/user/assets/images/paper_8.png)

# TimeLine XAI 
![](/img/user/assets/images/paper_9.png)

---
## XAI techs
- ![](/img/user/assets/images/paper_10.png)
- ![](/img/user/assets/images/paper_11.png)

- ![](/img/user/assets/images/paper_12.png)
- ![](/img/user/assets/images/paper_13.png)

![](/img/user/assets/images/paper_14.png)
## Evaluation 
- ![](/img/user/assets/images/paper_15.png)

| **Human-Centered**          | **Mô tả**                                                                                                           |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Comprehensibility**       | - Giải thích rõ ràng, liên quan đến ngữ cảnh  <br>- Người dùng dễ hiểu và diễn giải  <br>- Hỗ trợ hình ảnh minh hoạ |
| **Trust**                   | - Minh bạch, nhất quán, đáng tin cậy  <br>- Cân bằng chi tiết và đơn giản  <br>- Đánh giá tâm lý người dùng         |
| **Subjective Satisfaction** | - Thu thập feedback người dùng  <br>- Đánh giá mức độ hữu ích, dễ sử dụng  <br>- Phản ứng cảm xúc của người dùng    |
| **Decision-Making Support** | - Cung cấp insight có thể hành động  <br>- Kiểm thử thực nghiệm  <br>- Giảm lỗi và giảm tải nhận thức               |

| **Computer-Centered** | **Mô tả**                                                                                                 |
| --------------------- | --------------------------------------------------------------------------------------------------------- |
| **Fidelity**          | - Giải thích phản ánh đúng model  <br>- Kiểm chứng được  <br>- Xử lý tốt các bài toán phức tạp            |
| **Consistency**       | - Ổn định, đồng đều  <br>- Dễ dự đoán với dữ liệu tương tự                                                |
| **Robustness**        | - Chống chịu thay đổi nhỏ ở đầu vào  <br>- Thích ứng với thay đổi của model  <br>- Khả năng tổng quát tốt |
| **Efficiency**        | - Tốc độ nhanh  <br>- Tối ưu tài nguyên  <br>- Dễ dàng mở rộng hệ thống                                   |
| **Sufficiency**       | - Đủ ý trong giải thích  <br>- Đo lường được độ tự tin của model                                          |
