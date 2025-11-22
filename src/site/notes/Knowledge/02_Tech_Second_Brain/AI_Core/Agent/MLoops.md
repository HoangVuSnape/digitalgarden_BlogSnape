---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/agent/m-loops/","pinned":"false"}
---

# Cấu trúc thư mục dự án

```
Mloops/
│
├── __init__.py
├── models/
│   ├── __init__.py
│   ├── product.py
│   └── order.py
├── services/ 
│   ├── __init__.py
│   ├── product_service.py
│   └── order_service.py
└── main.py
```
# 1. Module (Mô-đun)

Định nghĩa: Một Module về cơ bản là một file Python duy nhất (có đuôi .py). File này chứa các định nghĩa và câu lệnh Python (hàm, lớp, biến).
Mục đích: Giúp bạn tổ chức code thành các đơn vị logic nhỏ hơn, dễ quản lý và tái sử dụng. Thay vì viết tất cả code vào một file lớn, bạn chia nhỏ ra thành các module chức năng.
Sử dụng: Bạn dùng lệnh import để sử dụng code từ một module khác.
# 2. Package (Gói)

Định nghĩa: Một Package là một tập hợp các module (và có thể cả các sub-package khác) được tổ chức trong một thư mục.
Điều kiện bắt buộc: Để Python coi một thư mục là một Package, thư mục đó phải chứa một file đặc biệt tên là __init__.py (file này có thể trống).
Mục đích: Giúp bạn tổ chức các module liên quan lại với nhau, tạo ra một không gian tên (namespace) riêng biệt và cho phép cấu trúc project lớn hơn một cách có hệ thống (phân cấp).


Sử dụng: Bạn cũng dùng lệnh import, nhưng thường sử dụng dấu chấm (.) để truy cập các module bên trong package (ví dụ: import my_package.my_module).

| Đặc điểm    | Module                                   | Package                                                           |
| ----------- | ---------------------------------------- | ----------------------------------------------------------------- |
| Định nghĩa  | Một file Python (.py)                    | Một thư mục chứa file __init__.py và các module/sub-package       |
| Bản chất    | Đơn lẻ                                   | Phân cấp, chứa nhiều file/thư mục con                             |
| Mục đích    | Tổ chức code trong một file, tái sử dụng | Tổ chức các module liên quan, cấu trúc project lớn                |
| Yêu cầu     | Không có gì đặc biệt ngoài là file .py   | Phải có file __init__.py trong thư mục                            |
| Import      | import my_module                         | import my_package.my_module hoặc from my_package import my_module |
Run: python -m Mloops.main

---
# Ví dụ để hiểu thêm về package
- Tại vì dạng viết kiểu oop về module thì nó sẽ đơn giản hơn package 1 tí và kiểu package sẽ khó nhìn hơn. 

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828137.png)