---
{"dg-publish":true,"permalink":"/knowledge/1-xai-kg/analyze-explainable-my-system/","title":"Explainable my system","pinned":"true"}
---

Link các tệp liên quan đến việc phân tích cần đọc
- XAI
	- [Explainability for NLP](Explainability%20for%20NLP.md)
	- [Explainable AI](Explainable%20AI.md)
	- [Knowledge Graph](Knowledge%20Graph.md)
	- [Sumary paper](Sumary%20paper.md)

- Data
	- [Data](../📂%2001_Projects/KLTN/Data.md)
	- [Project Information Technology](../NLP/Project%20Information%20Technology.md)

- Link
	- [Slide Graph Rag pront X](https://drive.google.com/file/d/1yliCGONb2yJv0IoFQ-GWpgrv_bz70Wqf/view)
	- [Graph rag](https://github.com/HoangVuSnape/RAG_Techniques/blob/main/all_rag_techniques/graph_rag.ipynb)
	- [Explainable retrieval](https://github.com/HoangVuSnape/RAG_Techniques/blob/main/all_rag_techniques/explainable_retrieval.ipynb)

- API
	- [embedding model geminai](https://ai.google.dev/gemini-api/docs/embeddings)
	- 
----

Ở đây là nơi mình sẽ analyze hệ thống về lý thuyết trước khi thực hiện

Hiện tại sẽ phải là các bước để explain hệ thống, liệt kê ở dưới đây:
- Đâu tiên mình thấy là diễn tả retrieval: Tìm kiếm và giải thích sự liên quan. Link:[explainable_retrieval.ipynb](https://github.com/HoangVuSnape/RAG_Techniques/blob/main/all_rag_techniques/explainable_retrieval.ipynb)
	- Gồm 2 class Và nó sẽ thể hiện khả năng retrieval và explain. 

- **Knowledge graph**: Diễn tả đường đi của việc truy xuất thông tin qua mối qua hệ đường đi.
	- Nó không có save lại data đàng hoàng nên khó hên lại nó có save lại cái graph **networkx** 
	- ![](/img/user/assets/images/XAI1.png)
	- 
	- ![](/img/user/assets/images/XAI2.png)
	- ![](/img/user/assets/images/XAI3.png)

- **Phân tích data tần xuất** hiện các từ của data đếm số lượng từ và cho ra bảng phân tích câu. 
	- Khi trả ra 1 câu kết quả thì nó sẽ thông kê như thế nào.
- **THể hiện câu hỏi đã từng hỏi và cho ra câu trả lời đúng**. Và sẽ cho kết quả tốt nếu có câu hỏi tương tự. 
- **Về việc phân tích data sql điểm chuẩn**
	- Theo quan điểm của mình thì query -> sql retrieval nên không nên tạo ra graph vì nó không có quan hệ gì cả. 
	- Qua bảng so sanh này thì mình có thể tạo ra 1 nhánh xuất phát từ điểm chuẩn của các trường. 
		- Nếu vậy là kiểu Ngành -> CHất lượng cao ,  thường ,anh -> Điểm chuẩn thpt, DGNL, học bạ.... 

![](/img/user/assets/images/XAI4.png)

- 




