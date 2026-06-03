---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/nlp/moe-v2/","pinned":"false"}
---

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603151327868.png)

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603151357072.png)


--------
# Note 
- Hiện tại đang có 3/3
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603151545858.png)
![652](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603151918112.png)
![652](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603152012695.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603152234814.png)

- Moe is chuyên mô 2 tầng , neural và gate. 
	- H1 expert có tín hiệu mạnh - routing , tôi cần router đến expert này. 
	- nó không hiểu ngữ nghĩa và mà nó truyền đến tín hiệu mạnh nhất.  
	- H2 : 4/2026 : Làm sao phân vùng cái cụm các học này tốt hơn. 
	- H3: Routing load balance cần data distribution luôn không đều 
		- Được cân bằng hoàn hảo -> nếu expert sai thì  khi những lĩnh vực noise hay outline(có thể phải chạm nhận data balance)
	- H4: Token dropping - skip , move token qua expert khác nếu quá tải. 
----
# Scalling law
**![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603155240674.png)**
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603152405505.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603152640753.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603152737665.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603152911428.png)
- Dense: Chinchilla 
- Chạy labs - rule force tìm số expert cần token cho mỗi expert ? Cần tìm cho cái này. 
- Đang tối ưu đang thử nghiệm để cho ra các para thử nghiệm
- H3 : ROuting dynamic - load toàn bộ - Lên quá trình inference thì ???
	- Mình chưa dư đoán được cái routing động sẽ xài 
		- COst rất lớn gpu phải giao tiếp với nhau = làm cho moe - chậm hơn 15. 
		- Hardware bị đuối thì ?? thì cần kiến trúc nào - dđ
# Research Gap
- HÌnh 1/1
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603153138470.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603153454511.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603153612150.png)
- H3: active token nhiều inference - thì cần nhiều hơn. Thêm chiều sâu sử lý hơn. nếu easy: 4 , complex: 6  - > adaptive refences. 
- H4: Làm sao train rẻ hơn,  -> Làm sao resoning tốt hơn -> làm sao routing tốt hơn? (HỌc hơn - hard code để thay vì fix cứng)

----
# Kimi K2
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603155319004.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603154029000.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603154258412.png)
![652](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603154348634.png)
![652](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603154700118.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/NLP/IMG-20260603154918003.png)
- H2: CHọn production giảm 128 -> 64 thì giảm: - rule ngay từ đầu 
- H3: Muon problem - adam w, adam. đúng tối ưu con ma trận
	- Muon - Q, K, V, ma trận của gating - muon 
		- Attention logit - nó bị bắn lớn, loss???-
	- Moun CLip - QK Clip - rescales , Finetune không ổn định trên data lớn. 
- H4: Agent RL - > thu thập data để làm train chất lượng cao dự vào lỗi 
	  - tạo train data 10k data - Loop. 


---------
- **H5: Gian lận phần thưởng - Token budget-Control - Gắn max - Suy nghĩ hiệu quả trong thời gian giới hạn. -** Scalling law moe ?. Rule force = load balance, specialization expert. 
	- Vẫn đang trong quá trình tạo ra expert. 

