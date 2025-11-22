---
created: 2025-04-23T18:38:42
updated:
  - 2025-11-22T21:39:28+07:00
  - 2025-04-27T10:02:04+07:00
  - 2025-04-27T10:01:23+07:00
  - 2025-04-27T10:00:24+07:00
  - 2025-04-27T09:54:58+07:00
  - 2025-04-27T09:53:07+07:00
  - 2025-04-27T09:52:08+07:00
  - 2025-04-27T09:02:04+07:00
  - 2025-04-27T08:54:32+07:00
  - 2025-04-27T08:53:43+07:00
  - 2025-04-27T08:52:23+07:00
  - 2025-04-26T22:52:42+07:00
  - 2025-04-26T22:46:55+07:00
  - 2025-04-26T19:59:45+07:00
  - 2025-04-26T19:49:42+07:00
  - 2025-04-26T19:34:14+07:00
  - 2025-04-26T19:32:58+07:00
  - 2025-04-26T19:31:53+07:00
  - 2025-04-26T19:30:51+07:00
  - 2025-04-26T19:29:53+07:00
  - 2025-04-26T19:28:16+07:00
  - 2025-04-26T19:26:09+07:00
  - 2025-04-26T19:15:10+07:00
  - 2025-04-23T21:53:09+07:00
  - 2025-04-23T21:03:35+07:00
  - 2025-04-23T20:24:29+07:00
  - 2025-04-23T20:06:03+07:00
  - 2025-04-23T20:05:29+07:00
  - 2025-04-23T20:02:13+07:00
  - 2025-04-23T19:45:33+07:00
  - 2025-04-23T19:43:14+07:00
  - 2025-04-23T19:07:57+07:00
  - 2025-04-23T18:56:05+07:00
  - 2025-04-23T18:54:40+07:00
  - 2025-04-23T18:49:33+07:00
  - 2025-04-23T18:43:32+07:00
  - 2025-04-23T18:39:31+07:00
  - 2025-11-22 21:37:27
title: Explainability for Natural Language Processing
tags:
append_modified_update:
dg-publish: true
dg-home:
dg-pinned: "false"
aliases:
---
# Link
- [Explainability for Natural Language Processing](https://www.youtube.com/watch?v=3tnrGe_JA0s&t=54s)
- [XAINLP2020](https://xainlp2020.github.io/xainlp/home)
- [A Survey of the State of Explainable AI for Natural Language Processing](https://arxiv.org/pdf/2010.00711)
- [Sumary paper](Sumary%20paper.md)
# Note 

- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917390.png)
	- Độ chính xác tỉ lệ nghịch với khả năng giải thích của mô hình
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917441.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917496.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917558.png)
- 

## Local vs Global
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917605.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917649.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917716.png)
- ![Surrogate](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917764.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917814.png)
## NLP Explainable
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917878.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917937.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213917981.png)

## Challange
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918041.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918141.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918197.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918246.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918279.png)


----

| **Challenge**                                                               | **Giải thích**                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **1. Explainability không phải chỉ là thử thách, mà là yêu cầu bắt buộc**   | Cả cộng đồng NLP đều nhận ra rằng khả năng giải thích (expandability) không còn là "nice-to-have" mà là điều **bắt buộc** khi phát triển hệ thống AI.                                                                                                                                                                                                                                                                                                                          |
| **2. Hiểu sai rằng phải đánh đổi giữa độ chính xác và khả năng giải thích** | Nhiều người nghĩ rằng **muốn giải thích được thì phải làm giảm độ chính xác**. Vì thế, họ hay huấn luyện mô hình phức tạp (deep learning) rồi mới dùng mô hình đơn giản (surrogate) để giải thích. Tuy nhiên, mô hình surrogate thường bị lỗi về **fidelity** (không thật sự phản ánh đúng cách mô hình ban đầu suy nghĩ). Thực tế, có thể thiết kế các **mô hình tự giải thích** vừa chính xác vừa dễ hiểu, ví dụ bằng cách kết hợp **learning + reasoning** (học + lý luận). |
| **3. Giải thích phải dựa trên từng đối tượng người dùng (stakeholders)**    | Mỗi nhóm người (AI engineer, người chuyên về luật pháp...) có **kỳ vọng** và **khả năng hiểu** khác nhau. Ví dụ: kỹ sư AI có thể hiểu quy tắc kỹ thuật, nhưng chuyên gia pháp lý cần giải thích dễ hiểu hơn. Vì vậy, **biết trước đối tượng** rồi thiết kế cách giải thích phù hợp là rất quan trọng.                                                                                                                                                                          |
| **4. Thiếu phương pháp và tiêu chí đánh giá chuẩn cho Explainability**      | Hiện tại rất nhiều nghiên cứu thiếu **bộ đánh giá chuẩn** cho khả năng giải thích. Một số gợi ý:  <br>- Cần đánh giá cả **trực giác** và **chất lượng** giải thích.  <br>- Đánh giá thêm về **coverage** (giải thích bao quát được bao nhiêu dự đoán) và **fidelity** (mức độ đúng với suy nghĩ mô hình gốc).  <br>- Phương pháp đánh giá cũng cần để ý **người dùng cuối là ai**.                                                                                             |
| **5. XAI trong NLP cần sự hợp tác liên ngành**                              | Nghiên cứu giải thích cho NLP phải kết hợp các lĩnh vực:  <br>- **Khoa học máy tính**  <br>- **Ngôn ngữ học tính toán (Computational Linguistics)**  <br>- **Tương tác người-máy (HCI)**.  <br>  <br>Sự phối hợp giữa NLP và HCI được cộng đồng rất hoan nghênh, và đây là hướng tiềm năng trong tương lai.                                                                                                                                                                    |

---
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918340.png)
# Evaluation
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918392.png)
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918433.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918494.png)
---

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


# More
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213918599.png)

---
# References
- [A Survey of the State of Explainable AI for Natural Language Processing](https://aclanthology.org/2020.aacl-main.46.pdf)
	- [Link yb related to Survey](https://www.youtube.com/watch?v=3tnrGe_JA0s)
- [Explainable Artificial Intelligence: A Survey of Needs, Techniques, Applications, and Future Direction](https://arxiv.org/pdf/2409.00265)

