---
{"dg-publish":true,"permalink":"/knowledge/01-projects/nckh/nckh/","title":"NCKH","pinned":"false"}
---

# QA
Link: [QA knee](https://www.worksafe.qld.gov.au/__data/assets/pdf_file/0022/24097/knee-injury-and-osteoarthritis-outcome-score-koos1.pdf)


Dưới đây là Survey câu hỏi KOOS
- Knee Injury and Osteoarthritis Outcome Score (KOOS): systematic review and meta-analysis of measurement properties

	
# CÔng việc trước đó đã làm 

# I. Generate and Push data
- Mình đã có vấn đề khi generate data với 2 phương pháp là **Oversampling** và **Augumentaion**. Nhưng khi mình set tỉ lệ có 1 chút vấn đề và dung lượng sinh ra rất lớn. Nên giờ mình sẽ làm là có 1 file jupyter khảo sát về data. 

- Mình có 1 jupyter, về push data lên hugging để sử dụng nhưng mà chư ổn lắm. Chắc mình sẽ sửa và up lại sau

![](/img/user/assets/images/Knowledge/01_Projects/NCKH/IMG-20251122215430263.png)
- Hướng data oversampling mình tạo ra không hợp lý và nó tăng quá nhiều bộ nhớ. 
# II. Preprocessing data
- Ở đây mình sẽ tìm hiểu data 
- Xử lý imbalance data, và preprocessing data: Histogram, brightly..

--- 
Tiếp theo mình sẽ cần phải review tất cả data và xử lý cho tối ưu hơn
Mình cần phải resize cho nó cùng kích thước để train

Chọn ra cái nào cần cân bằng hay sao.
Tiếp theo là dùng phương pháp nào...
**TÌm hiểu tiếp để còn đi chơi kkk**

---


# Đang làm tiếp 
- Hiện tại nói chung là xu hướng làm Các con llm nhưng mà độ chính xác thế nào nhưng mà k giải thích hay tại sao nó bị hư hay là những dấu hiệu hay khoanh vùng để chỉ ra là nghi ngừa bệnh đó thì rất khó thiếu phục

- Deep ... XAI - explainable AI. GradCam... Công nghệ mới là gì nữa vân vân.....


--- 
Làm tiếp preprocessing data

| Grade       | Number of images | % of total  |
| ----------- | ---------------- | ----------- |
| normal      | 2727             | 38.25%      |
| oa_doubtful | 1428             | 20.03%      |
| oa_mild     | 1702             | 23.87%      |
| oa_moderate | 934              | 13.10%      |
| oa_severe   | 338              | 4.74%       |
| **Total**   | **7129**         | **100.00%** |
THeo như gợi ý thì nên hương tới:
- 3 cái đầu là normal, doubtful, mild: 25, 25, 25% 
- Và moderate = 2.3 severe. -> 17% and  8%. tầm đó sẽ là hợp lý. 

| Grade       | Number of images | % of total  | Images to add |
| ----------- | ---------------- | ----------- | ------------- |
| Normal      | 2727             | 26.00%      | 0             |
| OA Doubtful | 2517             | 24.00%      | 1089          |
| OA Mild     | 2622             | 25.00%      | 920           |
| OA Moderate | 1783             | 17.00%      | 849           |
| OA Severe   | 839              | 8.00%       | 501           |
| **Total**   | **10488**        | **100.00%** |               |
|             |                  |             |               |

| Grade       | Images to add |
| ----------- | ------------- |
| Normal      | 0             |
| OA Doubtful | 1089          |
| OA Mild     | 920           |
| OA Moderate | 849           |
| OA Severe   | 501           |
Mình sẽ Preprocessing data -> rồi data agumentation. 


Tham khảo bảng từ paper [# A ResNet-based approach for accurate radiographic diagnosis of knee osteoarthritis](https://ietresearch.onlinelibrary.wiley.com/doi/full/10.1049/cit2.12079)

---
THis paper: https://onlinelibrary.wiley.com/doi/10.1155/2021/4931437

![](/img/user/assets/images/Knowledge/01_Projects/NCKH/IMG-20251122215430443.png)