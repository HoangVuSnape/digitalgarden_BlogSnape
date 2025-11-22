---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/dev-ops-tools/docker-gitlab/docker/","title":"Docker","pinned":"false"}
---

![../../../../assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/IMG-20251123002641465.png](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/IMG-20251123002641465.png)
## Phân biệt 
- Ở đây có 2 khái niệm là **Docker file** và Docker compose 

### 1. **Dockerfile**


- **Mục đích:** Dùng để định nghĩa cách xây dựng một Docker image.
- **Chức năng:**
    - Mô tả các bước cài đặt, thiết lập môi trường, cài đặt ứng dụng và phụ thuộc.
    - Tạo Docker image bằng lệnh `docker build`.
- **Cách sử dụng:**
    - File định dạng text, bắt đầu với `FROM` (chỉ định base image).
    - Các lệnh thường dùng: `COPY`, `RUN`, `CMD`, `EXPOSE`, `ENV`.
- **Ứng dụng:**
    - Tạo một image chứa môi trường ứng dụng hoặc dịch vụ cụ thể.
    - Cung cấp khả năng tái sử dụng và dễ dàng phân phối môi trường.

### 2. **Docker Compose**

- **Mục đích:** Dùng để quản lý và triển khai nhiều container liên kết với nhau trong một ứng dụng.
- **Chức năng:**
    - Định nghĩa cách nhiều container tương tác với nhau.
    - Chỉ định thông tin về volume, network, và môi trường.
    - Chạy nhiều container cùng lúc với lệnh `docker-compose up`.
- **Cách sử dụng:**
    - File YAML (ví dụ: `docker-compose.yml`), định nghĩa các dịch vụ.
    - Các thành phần chính: `services`, `networks`, `volumes`.
- **Ứng dụng:**
    - Quản lý ứng dụng phức tạp với nhiều dịch vụ (database, backend, frontend).
    - Dễ dàng cấu hình và triển khai toàn bộ hệ thống.

---

**So sánh ngắn gọn:**

- **Dockerfile:** Tập trung vào xây dựng **1 image** cho một dịch vụ cụ thể.
- **Docker Compose:** Tập trung vào quản lý và liên kết **nhiều container** trong hệ thống.

---

**Ứng dụng thực tiễn**

1. **Dockerfile:**
    - Tạo image tiêu chuẩn cho backend Python (Django, Flask).
    - Xây dựng môi trường phát triển đồng nhất giữa các nhóm lập trình viên.
2. **Docker Compose:**
    - Deploy hệ thống microservices (API Gateway, Service A, Service B).
    - Phát triển hệ thống với các thành phần như Nginx, Redis, PostgreSQL.
    - Mô phỏng môi trường staging trước khi lên production.

## Docker file
- Tóm lại về Docker
	- Có thể chạy theo các image trong file Dockerfile hoặc trực tiếp chạy container mà không cần file có sẵn. Tuy nhiên, việc chạy nhiều container gặp không ít khó khăn. Chẳng hạn, khi chạy mysql, bạn cần thêm tùy chọn -e, còn khi khởi động nginx thì phải sử dụng tùy chọn -p để thiết lập các cấu hình và tham số, điều này khá phức tạp. Hãy tưởng tượng nếu cần chạy nhiều container, bạn sẽ phải thực hiện rất nhiều lệnh docker run và thiết lập vô số thông số.
- ![../../../../assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/IMG-20251123002641526.png](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/IMG-20251123002641526.png)
## Docker compose
- Trong đó thì thương có 4 thành phần chính : Version, service, network, volumes
	-  Version và service bắt buộc
	- Chú ý là để có thể liên kết của FE và BE thì mình phải tạo 2 container và nó cùng network để giao tiếp.
---
- Nếu số lượng container chỉ rơi vào khoảng hai ba cái thì cũng có thể quản lý được, nhưng chúng ta đã dành không ít thời gian cho công việc này. Tuy nhiên, khi bạn cần thiết lập một môi trường với nhiều container hơn, điều đó sẽ trở nên rất nhàm chán khi phải lặp lại từng bước thủ công mỗi lần.

Tuy nhiên, may mắn thay, có một phương pháp hiệu quả hơn để xử lý điều này. Docker-compose là một công cụ hữu ích để định nghĩa và vận hành nhiều container trong ứng dụng Docker. Nó cho phép bạn tạo một tệp cấu hình YAML, trong đó bạn sẽ định nghĩa các dịch vụ cho ứng dụng của bạn cũng như tất cả các bước và cấu hình cần thiết để xây dựng các hình ảnh, khởi động các container và liên kết chúng lại với nhau. Cuối cùng, khi tất cả đã hoàn tất, bạn chỉ cần thực hiện một lệnh duy nhất để thiết lập mọi thứ.

---
Ví dụ :

- **version: "3.8"**  
    Chỉ định phiên bản cú pháp của Docker Compose.
- **services:**  
    Định nghĩa các dịch vụ sẽ chạy.
    - **web:**
        - `build: .` : Xây dựng image từ Dockerfile trong thư mục hiện tại (ứng dụng FastAPI).
        - `ports: "8000:8000"` : Mở cổng 8000 trên máy host và ánh xạ tới cổng 8000 của container.
        - `depends_on: - db` : Dịch vụ web sẽ khởi động sau khi dịch vụ db đã sẵn sàng.
        - `networks: - app-network` : Kết nối vào mạng riêng tên là app-network.
    - **db:**
        - `image: postgres:15` : Sử dụng image PostgreSQL phiên bản 15.
        - `restart: always` : Tự động khởi động lại container nếu bị dừng.
        - `environment:` : Thiết lập biến môi trường cho PostgreSQL (user, password, db name).
        - `volumes: - pgdata:/var/lib/postgresql/data` : Lưu trữ dữ liệu database vào volume tên pgdata để không bị mất khi container bị xóa.
        - `networks: - app-network` : Kết nối vào mạng app-network.
- **volumes:**
    - `pgdata:` : Định nghĩa volume dùng để lưu trữ dữ liệu của database.
- **networks:**
    - `app-network:` : Định nghĩa một mạng riêng để các container giao tiếp với nhau

----
# Docker More
 Các khái niệm note ở phần này là cache, mount đến gì đó, optimize docker, down -up, stage multi docker. 

## cache và optimize
- Ở ví dụ này **nếu thay đổi hàm server.py thì docker nào sẽ bị đỏ lên và nó sẽ chạy từ dòng đó**
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/IMG-20251123002641576.png)
- Như hình thì hàm copy là copy các file tại thư mục đó, nên nếu thay đổi server.py thì nó sẽ chạy lại từ dòng **COPY . .** và cơ chế cache đã sinh ra. 
	- Cache nó lưu trữ lại nhưng gì đã chạy trước đó nếu không có thay đổi 

Vì vậy optimize docker hay chương trình thì nên đặt những gì lên trước lên đầu
- Môi trường env, các requirements install. 
- ... Còn nữa. 

**Tiếp theo chú ý là docker build up thì nên build down đi để xóa chứ không dễ bị lỗi.** 

--- 
# Stage multi docker
- vì cơ chế cache đó nên nó là điểm nhớ. 
- Và nó sẽ cần làm nếu như các lib, depenci quá lớn. thì người ta sẽ tách build trước 
- Và dùng cái kia copy lại các luồng để chạy. chứ k phải build lại nữa. 
- Cái này chứ thực hành và có vẻ sẽ khó nên mình cần phải thực chiến sau.

---
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/IMG-20251123002641620.png)

# Network
- Ở ngoài không phải trong service thì có thể là local host
- Nhưng trong service cùng network thì file env phải được sửa cho cùng tên với service. THay localhost -> tên service thì mới có thể chọn tới mà giao tiếp 
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/IMG-20251123002641663.png)

# Healthcheck
- tại sao có cái có thể dùng crul -f rồi ping tới server nhưng có những server hay service tự build thì mình cần viết thêm file healthcheck để test.
- Bên cạnh đó dùng get để test server.
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Docker_Gitlab/IMG-20251123002641704.png)
# Experiment
- Networking : localhost với trong docker (giao tiếp qua )
- Nên thiết kế docker là những cái gì nên viết trước tại vì nó có cache để thay đổi- nó sẽ phải build lại
- Nên xóa volumne docker compose down -v
- Chạy 1 file docker thì sao. 

