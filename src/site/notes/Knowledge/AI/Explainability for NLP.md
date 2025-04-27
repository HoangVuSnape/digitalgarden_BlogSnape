---
{"dg-publish":true,"permalink":"/knowledge/ai/explainability-for-nlp/","title":"Explainability for Natural Language Processing","pinned":"false"}
---

# Link
- [Explainability for Natural Language Processing](https://www.youtube.com/watch?v=3tnrGe_JA0s&t=54s)
- [XAINLP2020](https://xainlp2020.github.io/xainlp/home)
- [A Survey of the State of Explainable AI for Natural Language Processing](https://arxiv.org/pdf/2010.00711)
- [Sumary paper](Sumary%20paper.md)
# Note 

- ![](/img/user/assets/images/XAINLP_1.png)
	- Độ chính xác tỉ lệ nghịch với khả năng giải thích của mô hình
- ![](/img/user/assets/images/XAINLP_2.png)
- ![](/img/user/assets/images/XAINLP_3.png)
- ![](/img/user/assets/images/XAINLP_4.png)
- 

## Local vs Global
- ![](/img/user/assets/images/XAINLP_5.png)
- ![](/img/user/assets/images/XAINLP_6.png)
- ![](/img/user/assets/images/XAINLP_7.png)
- ![Surrogate](/img/user/assets/images/XAINLP_8.png)
- ![](/img/user/assets/images/XAINLP_9.png)
## NLP Explainable
- ![](/img/user/assets/images/XAINLP_10.png)
- ![](/img/user/assets/images/XAINLP_12.png)
- ![](/img/user/assets/images/XAINLP_13.png)

## Challange
- ![](/img/user/assets/images/XAINLP_14.png)
- ![](/img/user/assets/images/XAINLP_15.png)
- ![](/img/user/assets/images/XAINLP_16.png)
- ![](/img/user/assets/images/XAINLP_17.png)
- ![](/img/user/assets/images/XAINLP_18.png)


----

| **Challenge**                                                               | **Giải thích**                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **1. Explainability không phải chỉ là thử thách, mà là yêu cầu bắt buộc**   | Cả cộng đồng NLP đều nhận ra rằng khả năng giải thích (expandability) không còn là "nice-to-have" mà là điều **bắt buộc** khi phát triển hệ thống AI.                                                                                                                                                                                                                                                                                                                          |
| **2. Hiểu sai rằng phải đánh đổi giữa độ chính xác và khả năng giải thích** | Nhiều người nghĩ rằng **muốn giải thích được thì phải làm giảm độ chính xác**. Vì thế, họ hay huấn luyện mô hình phức tạp (deep learning) rồi mới dùng mô hình đơn giản (surrogate) để giải thích. Tuy nhiên, mô hình surrogate thường bị lỗi về **fidelity** (không thật sự phản ánh đúng cách mô hình ban đầu suy nghĩ). Thực tế, có thể thiết kế các **mô hình tự giải thích** vừa chính xác vừa dễ hiểu, ví dụ bằng cách kết hợp **learning + reasoning** (học + lý luận). |
| **3. Giải thích phải dựa trên từng đối tượng người dùng (stakeholders)**    | Mỗi nhóm người (AI engineer, người chuyên về luật pháp...) có **kỳ vọng** và **khả năng hiểu** khác nhau. Ví dụ: kỹ sư AI có thể hiểu quy tắc kỹ thuật, nhưng chuyên gia pháp lý cần giải thích dễ hiểu hơn. Vì vậy, **biết trước đối tượng** rồi thiết kế cách giải thích phù hợp là rất quan trọng.                                                                                                                                                                          |
| **4. Thiếu phương pháp và tiêu chí đánh giá chuẩn cho Explainability**      | Hiện tại rất nhiều nghiên cứu thiếu **bộ đánh giá chuẩn** cho khả năng giải thích. Một số gợi ý:  <br>- Cần đánh giá cả **trực giác** và **chất lượng** giải thích.  <br>- Đánh giá thêm về **coverage** (giải thích bao quát được bao nhiêu dự đoán) và **fidelity** (mức độ đúng với suy nghĩ mô hình gốc).  <br>- Phương pháp đánh giá cũng cần để ý **người dùng cuối là ai**.                                                                                             |
| **5. XAI trong NLP cần sự hợp tác liên ngành**                              | Nghiên cứu giải thích cho NLP phải kết hợp các lĩnh vực:  <br>- **Khoa học máy tính**  <br>- **Ngôn ngữ học tính toán (Computational Linguistics)**  <br>- **Tương tác người-máy (HCI)**.  <br>  <br>Sự phối hợp giữa NLP và HCI được cộng đồng rất hoan nghênh, và đây là hướng tiềm năng trong tương lai.                                                                                                                                                                    |
![](/img/user/assets/images/XAINLP_19.png)
# Evaluation
- ![](/img/user/assets/images/XAINLP_20.png)
	- ![](/img/user/assets/images/XAINLP_21.png)
- ![](/img/user/assets/images/XAINLP_22.png)
# More
- ![](/img/user/assets/images/XAINLP_23.png)

# Note Report
- 

---
# References
- [A Survey of the State of Explainable AI for Natural Language Processing](https://aclanthology.org/2020.aacl-main.46.pdf)
	- [Link yb related to Survey](https://www.youtube.com/watch?v=3tnrGe_JA0s)
- [Explainable Artificial Intelligence: A Survey of Needs, Techniques, Applications, and Future Direction](https://arxiv.org/pdf/2409.00265)

