---
{"dg-publish":true,"permalink":"/knowledge/kltn/tien-do-note/","title":"Tiến độ","pinned":"false"}
---


# Công việc
 - [x] Docker compose  
 - [x] Tìm hiểu về XAI ()
 - [x] Em đang xây dựng data để finetune embedding model -> enhance lại khả năng embedding của model. 
 - [x] Shap,  Các phương pháp hỗ trợ bên
 - [x] Cleaning data từ excel - dùng gemni để liệt kê nhưng từ khóa để nó tạo graph và gắn trọng số. Bên cạnh đó là tạo ra quan hệ và chiều nó có đi 2 chiều hay 1 chiều. 
 - [x] Cleaning lại hệ thống. Tối ưu theo kiểu package. Tách ra 
 - [x] Định nghĩ ra các topic trước để viết prompt.


# Link:
- [Meeting with teacher](Meeting%20with%20teacher.md)

---
# Tiến độ 
# Tuần 1 - 7.4
- Tìm hiểu Large Reasoning Model: Khái niệm, yêu cầu và hướng phát triển.
- Tìm hiểu thêm về Agent: Nghiên cứu MoE, MLoops.
- Tìm hiểu cách viết đề cương: Xây dựng lộ trình phát triển, xác định phạm vi và kế hoạch thực hiện
Khó khăn: 
- Mơ hồ về hướng đi nên đang tìm hiểu các hướng có thể


# Tuần 2 - 13.4
- Tiếp theo có thể làm hoàn thiện thêm data finetune để-> enhance embedding 
- Model Confidence 
- Tìm hiểu về GraphRAG, Explain retrieval trên Github 
- Các phương pháp Explainable AI - NLP mà đã thực hiện rồi. (Có 1 buổi meeting youtube mình phải coi đã). 

---
- TÌm hiểu và đang xây dựng bộ data để embedding model - để tăng khả năng tìm kiếm thông tin
- Tìm hiểu Graph Rag(Knowledge graph) - có demo về kiến thức của nó
- Tìm hiểu cơ bản về XAI: các kiến thức nền của nó từ XgBoost, RForest,

Khó khăn: Có nhiều kiến thức và kĩ thuật trong lĩnh vực này và chưa tìm hiểu hết và nắm rõ được 
# Tuần 3 - 20.4 
- Tiếp tục tìm hiểu về XAI: LIME, SHAP,Feature Importance,.. trước đây
- Xây dựng tiếp bộ data embedding : Được tầm 60%
- Tìm đọc paper và các survey liên quan 

# Tuần 4 -  27.4
- Tìm hiểu thêm các bài về XAI gần đây: cách diễn đạt hay cách visualize để tăng độ tin cậy
- Tìm hiểu các bài paper liên quan lĩnh vực XAI và RAG
- Đang bắt tay xây dựng Graph Rag: đang bị lỗi
- Đang tìm hiểu các nền tảng Để sử dụng làm Knowledge graph. 

# Tuần 5
- Làm về Graph knowldege. Ra được thực nghiệm trong vòng 3 tuần tới. 
- Làm được khung liên qua đến paper. 


# Tuần 6 5.4 
- Xử lý data và test với các chunking - clean lại data các trường đại học.
- Phân tích số lượng từ nghĩ trong data để hiểu và xây dựng.  
- Xây đựng bộ data graph: gồm filename, tags, topics. (60%)


# Tuần 7 11.4
## Note
- Đã gần như hoàn thành bộ graph (Lỗi 1 trường đại học)
	- Trong quá trình làm nhận thấy 1 số lỗi trong quá trình xây.  
- Đang thực hiện code để thực hiện hóa về graphrag bao gồm: **KnowledgeGraph**, **QueryEngine**, **Visualizer**. 
	- Quá trình xây gặp vấn đề về số lượng tags, topic nhiều quá nên k hiện thị đúng nội dung mong muốn
	- Và sau khi tìm hiểu thêm là cũng dính vấn đề chunking dài quá đã làm không khái quát đủ tốt về graph 
- Mình đang thấy cái hạn chế là cái embedding quá dài làm mấy đi khả năng xây graph knowledge. 
	- Có cảm giác không nên xây embedding quá dài mà phải clean lại data để cắt ngắn bớt. 
	- Data của HCMUTE đang bị lỗi nha. cần check lại sau. 
---
Tuần tới
Đầu tiên là hướng đến là 
- Phân cụm lại các tùy ý các content và 
- Sau đó mới thử nghiệm là phải sort lại các file name để nó group lại khoảng cách. 

## Questions
- Thắc mắc về cách setup database.
	- ![](/img/user/assets/images/TD1.png)
	- nó là những câu nhỏ
	- Nhưng về data của mình thì có ổn không? thầy có kinh nghiệm không ạ
		- ![](/img/user/assets/images/TD2.png)
		- ![](/img/user/assets/images/TD3.png)

Trong quá trình em tìm hiểu và tham khảo
- **Các dạng chunking nó ngắn hơn và gọn hơn nhiều so với dạng data cũ của em.**
- ![](/img/user/assets/images/TD4.png)

---
Thầy cho em xin kinh nghiệm là em nên đánh topics và tags 
- THì số lượng lấy tầm bao nhiêu nhiêu 
- ![](/img/user/assets/images/TD5.png)

----
# Sai xót
- Trong 1 chunk thì nó sẽ phải có 1 -3  topics thôi là tầm 3 chữ cái. Và mối quan hệ không quá dài. Để thể hiện được hình ảnh:
	- Mình đã tận dụng các semantic chunk của dự án trước để làm nên khi làm ra mới ngộ nhận ra vấn đề gặp phải. 
	- ![](/img/user/assets/images/TD6.png)

	- Đây là cách giải quyết:
	  ![](/img/user/assets/images/TD7.png)
	- ![](/img/user/assets/images/TD8.png)
	- ![](/img/user/assets/images/TD9.png)
- 