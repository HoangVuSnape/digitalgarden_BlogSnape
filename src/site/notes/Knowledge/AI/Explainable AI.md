---
{"dg-publish":true,"permalink":"/knowledge/ai/explainable-ai/","title":"Explainable AI","pinned":"false"}
---

# Link
- [XAI - A Data Odyssey](https://www.youtube.com/playlist?list=PLqDyyww9y-1SwNZ-6CmvfXDAOdLS7yUQ4)
- [Blog realated to XA - christophm](https://christophm.github.io/interpretable-ml-book/ale.html)
- [BLog Alibi XAI](https://docs.seldon.io/projects/alibi/en/stable/overview/high_level.html)
- [Blog FB XAI](http://facebook.com/aivietnam.edu.vn/posts/pfbid02azTLQY4nZR5cmoETtTSf2awkJA4REYiGm2gPFqpunoaf48dHww2ahgscNnhcDV9ql?rdid=Hod4gfPSKwSydTGg#)
# Collections image
![](/img/user/assets/images/XAI_1.png)
![](/img/user/assets/images/XAI_2.png)
# Key words
Hiểu 2 khái niệm 
- Interpretable Model
- XAI - Explainable AI

- Building interpretable models
- LIME
- SHAP
- PDPs and ICE Plots
- ALEs
	- Friedmans H-Stat
- Giving human-friendly explanations

# Interpretable Models vs Explainable AI (XAI)
## **1. Interpretable Models (Mô hình có thể diễn giải được)**

🛠 **Định nghĩa**:  
Một mô hình được coi là **interpretable** nếu bản thân nó có thể được hiểu trực tiếp bởi con người mà không cần công cụ bổ sung.

🔍 **Đặc điểm**:

- Đơn giản và dễ hiểu
    
- Có thể giải thích trực tiếp dựa trên cấu trúc của mô hình
    
- Không cần thuật toán hoặc phương pháp đặc biệt để làm rõ quyết định của mô hình
    

📌 **Ví dụ về mô hình interpretable**:

- **Hồi quy tuyến tính (Linear Regression)** → Hệ số của từng đặc trưng cho thấy mức độ ảnh hưởng của nó đến kết quả.
    
- **Cây quyết định (Decision Tree)** → Các nhánh thể hiện rõ ràng cách mô hình đưa ra quyết định.
    

**🎯 Mục tiêu:** Chọn một mô hình dễ hiểu ngay từ đầu, không cần công cụ giải thích phức tạp.

---

## **2. Explainable AI (XAI - Trí tuệ nhân tạo có thể giải thích được)**

🛠 **Định nghĩa**:  
Explainable AI là một tập hợp các phương pháp và công cụ được sử dụng để **giải thích quyết định của mô hình phức tạp**, chẳng hạn như mạng nơ-ron sâu hoặc mô hình ensemble.

🔍 **Đặc điểm**:

- Dùng để giải thích các mô hình **không thể diễn giải trực tiếp** (ví dụ: deep learning, random forest).
    
- Cung cấp cái nhìn sâu hơn về cách mô hình hoạt động.
    
- Dùng các kỹ thuật như visualization, attention maps, hay mô hình thay thế để tạo ra giải thích.
    

📌 **Ví dụ về phương pháp Explainable AI**:

- **SHAP (SHapley Additive Explanations)** → Đánh giá mức độ đóng góp của từng đặc trưng vào dự đoán.
    
- **LIME (Local Interpretable Model-Agnostic Explanations)** → Xây dựng mô hình đơn giản xung quanh từng dự đoán để hiểu lý do của nó.
    
- **Gradient-weighted Class Activation Mapping (Grad-CAM)** → Giải thích mô hình deep learning bằng cách tạo bản đồ nhiệt trên ảnh.
    

**🎯 Mục tiêu:** Giải thích các mô hình AI phức tạp mà con người không thể hiểu trực tiếp.

---

### **So sánh nhanh**

| **Tiêu chí**          | **Interpretable Models**                 | **Explainable AI (XAI)**             |
| --------------------- | ---------------------------------------- | ------------------------------------ |
| **Độ phức tạp**       | Đơn giản, dễ hiểu                        | Phức tạp, cần phương pháp giải thích |
| **Cách tiếp cận**     | Trực tiếp                                | Sử dụng thuật toán bổ trợ            |
| **Mô hình điển hình** | Hồi quy tuyến tính, cây quyết định       | Deep learning, random forest         |
| **Tính minh bạch**    | Cao                                      | Phụ thuộc vào phương pháp XAI        |
| **Ứng dụng**          | Các bài toán cần tính giải thích rõ ràng | Khi cần hiểu mô hình phức tạp        |

---

### **Khi nào dùng Interpretable Models và Explainable AI?**

- Nếu cần một mô hình dễ hiểu ngay từ đầu → **Chọn Interpretable Models**
    
- Nếu cần độ chính xác cao từ mô hình phức tạp và vẫn muốn hiểu cách nó hoạt động → **Dùng Explainable AI**
    

👉 **Ví dụ thực tế**:

- Một ngân hàng cần mô hình để duyệt khoản vay → Họ có thể dùng **Decision Tree** (interpretable) hoặc **Random Forest + SHAP** (explainable AI).
    
- Một bác sĩ cần hiểu lý do mô hình AI chẩn đoán bệnh → Họ có thể dùng **Grad-CAM** để xem vùng ảnh mà mô hình quan tâm.
---

# Global and local
- **What method is interpreting? (Phương pháp đang giải thích cái gì?)**
    - **Global (Toàn cục - Toàn bộ mô hình)**: Giải thích cách mô hình hoạt động trên toàn bộ tập dữ liệu, giúp hiểu được các xu hướng và quy tắc chung mà mô hình học được.
    - **Local (Cục bộ - Dự đoán riêng lẻ)**: Giải thích tại sao mô hình đưa ra một dự đoán cụ thể cho một điểm dữ liệu cụ thể.
- **How method is calculated? (Cách phương pháp được tính toán?)**
    - **Permutations (Hoán vị)**: Kiểm tra sự thay đổi trong đầu ra của mô hình khi hoán đổi các giá trị đầu vào để đo mức độ ảnh hưởng của từng đặc trưng.
    - **Surrogate Models (Mô hình thay thế)**: Xây dựng một mô hình đơn giản (ví dụ: cây quyết định) để mô phỏng cách mô hình gốc hoạt động, giúp giải thích quyết định của nó dễ dàng hơn.
### **Phương pháp Global vs Local XAI**
- **Global XAI**: Hiểu toàn bộ mô hình
    - Feature Importance (trong Random Forest, XGBoost)
    - Partial Dependence Plots (PDP) 
    - Accumulated Local Effects (ALE)
- **Local XAI**: Giải thích từng dự đoán
    - LIME (Local Interpretable Model-Agnostic Explanations)
    - *SHAP (Shapley Additive Explanations)*
    - Counterfactual Explanations (các phương pháp giải thích bằng cách thay đổi input)

# Global
## **Permutation Feature Importance (Tầm quan trọng của đặc trưng bằng hoán vị)**

🔹 **Permutation Feature Importance (PFI)** là một phương pháp đánh giá mức độ quan trọng của từng đặc trưng (feature) trong một mô hình machine learning bằng cách **hoán vị ngẫu nhiên** (permutation) giá trị của từng đặc trưng và quan sát sự thay đổi trong hiệu suất mô hình.
link: 
- [Yb PDP](https://www.youtube.com/watch?v=VUvShOEFdQo)
- Slide 
Note
- Shuffe cột và record kết quả
![](/img/user/assets/images/XAI_3.png)

## Partial Dependence Plots (PDPs)

- PDP thể hiện mối quan hệ trung bình giữa một feature và đầu ra của mô hình.
- Nếu chỉ thay đổi một feature trong khi giữ nguyên các feature khác, dự đoán của mô hình sẽ thay đổi như thế nào?
	- Với mô hình dự đoán giá nhà 🏡, nếu bạn tăng diện tích từ **50m² → 100m² → 150m²**, giá nhà trung bình thay đổi như thế nào?
- PDP tính trung bình dự đoán mô hình trên toàn bộ dữ liệu để hiểu tác động của một feature đơn lẻ 
![](/img/user/assets/images/XAI_4.png)

### Math
- ![](/img/user/assets/images/XAI_5.png)
- ![](/img/user/assets/images/XAI_6.png)

## Individual Conditional Expectation (ICE) Plots
- **ICE thể hiện tác động của một feature lên từng quan sát riêng lẻ** (thay vì trung bình như PDP).
- Từng quan sát cụ thể phản ứng như thế nào với sự thay đổi của feature đó?
- PDP có thể che giấu sự biến thiên giữa các mẫu. ICE giúp khắc phục điều này bằng cách hiển thị đường cong riêng lẻ cho từng quan sát.
- ![](/img/user/assets/images/XAI_7.png)
- ![](/img/user/assets/images/XAI_8.png)

## Accumulated Local Effect Plots (ALEs)
- **Accumulated Local Effects (ALE)** là một phương pháp **giải thích** mô hình machine learning bằng cách **hiển thị ảnh hưởng** của **một đặc trưng (feature)** đến **dự đoán đầu ra**, **khắc phục được các nhược điểm** của phương pháp nổi tiếng **Partial Dependence Plots (PDPs)**.
- ![](/img/user/assets/images/XAI_9.png)
![](/img/user/assets/images/XAI_10.png)
![](/img/user/assets/images/XAI_11.png)

![](/img/user/assets/images/XAI_12.png)

---
# Local
## LIME (Local Interpretable Model-Agnostic Explanations)
Link: [LIME](https://www.youtube.com/watch?v=dQ_jvRkzN1Q)
![](/img/user/assets/images/XAI_13.png)
### Tips
#### Generate sample -step 2
 ![](/img/user/assets/images/XAI_14.png)
 - ![](/img/user/assets/images/XAI_15.png)
 - ![](/img/user/assets/images/XAI_16.png)
![](/img/user/assets/images/XAI_17.png)
![](/img/user/assets/images/XAI_18.png)

#### Weighting samples
- ![](/img/user/assets/images/XAI_19.png)
- ![](/img/user/assets/images/XAI_20.png)
- ![](/img/user/assets/images/XAI_21.png)


## Shap
#### References
- [Github Shap](https://github.com/conorosully/SHAP-tutorial)
- [Shap YB](https://www.youtube.com/playlist?list=PLqDyyww9y-1SJgMw92x90qPYpHgahDLIK)

- SHapley Additive exPlanations
- ![](/img/user/assets/images/XAI_22.png)
- ![](/img/user/assets/images/XAI_23.png)
- 

### Example

Ở ví dụ 2 người cha làm rõ lắm về công thức
- ![](/img/user/assets/images/XAI_24.png)
- ![](/img/user/assets/images/XAI_25.png)
- ![](/img/user/assets/images/XAI_26.png)
#### Note


----
![](/img/user/assets/images/XAI_27.png)
![](/img/user/assets/images/XAI_28.png)

### Mathematics
![](/img/user/assets/images/XAI_29.png)
