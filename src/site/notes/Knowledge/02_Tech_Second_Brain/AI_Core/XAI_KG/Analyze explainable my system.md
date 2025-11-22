---
created: 2025-05-06T13:53:35
updated:
  - 2025-11-22T21:38:52+07:00
  - 2025-05-18T14:55:01+07:00
  - 2025-05-18T14:18:22+07:00
  - 2025-05-18T14:17:03+07:00
  - 2025-05-18T14:13:14+07:00
  - 2025-05-18T00:41:03+07:00
  - 2025-05-18T00:40:31+07:00
  - 2025-05-18T00:39:42+07:00
  - 2025-05-18T00:38:27+07:00
  - 2025-05-18T00:33:30+07:00
  - 2025-05-18T00:23:26+07:00
  - 2025-05-18T00:22:29+07:00
  - 2025-05-18T00:21:46+07:00
  - 2025-05-18T00:20:57+07:00
  - 2025-05-16T19:29:45+07:00
  - 2025-05-16T19:28:58+07:00
  - 2025-05-06T17:10:54+07:00
  - 2025-05-06T17:03:29+07:00
  - 2025-05-06T15:29:37+07:00
  - 2025-05-06T15:21:45+07:00
  - 2025-05-06T15:15:50+07:00
  - 2025-05-06T15:11:18+07:00
  - 2025-05-06T15:01:15+07:00
  - 2025-05-06T15:00:25+07:00
  - 2025-05-06T14:53:09+07:00
  - 2025-05-06T14:51:30+07:00
  - 2025-05-06T14:50:02+07:00
  - 2025-05-06T14:42:09+07:00
  - 2025-05-06T14:38:55+07:00
  - 2025-05-06T14:01:37+07:00
  - 2025-05-06T13:59:22+07:00
  - 2025-05-06T13:57:58+07:00
  - 2025-05-06T13:56:53+07:00
  - 2025-05-06T13:55:59+07:00
  - 2025-05-06T13:54:12+07:00
  - 2025-11-22 21:37:26
title: Explainable my system
tags:
append_modified_update:
dg-publish: true
dg-home:
dg-pinned: "true"
aliases:
---
Link các tệp liên quan đến việc phân tích cần đọc
- XAI
	- [Explainability for NLP](Explainability%20for%20NLP.md)
	- [Explainable AI](Explainable%20AI.md)
	- [Knowledge Graph](Knowledge%20Graph.md)
	- [Sumary paper](Sumary%20paper.md)

- Data
	- [Data](../../../01_Projects/KLTN/Data.md)
	- [Project Information Technology](../../../01_Projects/ProjectCNTT/Project%20Information%20Technology.md)

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
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213842053.png)
	- 
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213842110.png)
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213842153.png)

- **Phân tích data tần xuất** hiện các từ của data đếm số lượng từ và cho ra bảng phân tích câu. 
	- Khi trả ra 1 câu kết quả thì nó sẽ thông kê như thế nào.
- **THể hiện câu hỏi đã từng hỏi và cho ra câu trả lời đúng**. Và sẽ cho kết quả tốt nếu có câu hỏi tương tự. 
- **Về việc phân tích data sql điểm chuẩn**
	- Theo quan điểm của mình thì query -> sql retrieval nên không nên tạo ra graph vì nó không có quan hệ gì cả. 
	- Qua bảng so sanh này thì mình có thể tạo ra 1 nhánh xuất phát từ điểm chuẩn của các trường. 
		- Nếu vậy là kiểu Ngành -> CHất lượng cao ,  thường ,anh -> Điểm chuẩn thpt, DGNL, học bạ.... 

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/XAI_KG/IMG-20251122213842199.png)

- 




