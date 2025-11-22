---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/dev-ops-tools/web-cloud/aws/","pinned":"false"}
---

# Link
- [Gen policies](https://awspolicygen.s3.amazonaws.com/policygen.html)
- [Course: Vỡ lòng về Amazon Web Services | Udemy](https://www.udemy.com/course/vo-long-ve-amazon-web-services/learn/lecture/47226313#questions/22858183)

# Overview
Chào bạn, hình ảnh bạn cung cấp là đề cương của một khóa học về Amazon Web Services (AWS). Dưới đây là giải thích chi tiết về các khái niệm và dịch vụ được đề cập trong từng phần:
### 1. Cơ bản về Cloud và AWS
- **Cloud (Điện toán đám mây):** Là việc cung cấp các tài nguyên công nghệ thông tin (như máy chủ, lưu trữ, cơ sở dữ liệu, mạng) qua Internet theo mô hình "trả tiền theo mức sử dụng". Thay vì phải mua và quản lý cơ sở hạ tầng vật lý, bạn có thể thuê tài nguyên từ một nhà cung cấp như AWS.
- **AWS (Amazon Web Services):** Là nền tảng điện toán đám mây toàn diện và được sử dụng rộng rãi nhất thế giới của Amazon. AWS cung cấp hơn 200 dịch vụ đầy đủ tính năng từ các trung tâm dữ liệu trên toàn cầu.
### 2. AWS Global Infrastructure và chi tiết về Group, Roles và Policies
- **AWS Global Infrastructure (Cơ sở hạ tầng toàn cầu của AWS):** Là mạng lưới các trung tâm dữ liệu của AWS trên khắp thế giới. Các thành phần chính bao gồm:
    - **Regions (Vùng):** Là các khu vực địa lý riêng biệt trên thế giới (ví dụ: us-east-1 ở Bắc Virginia, ap-southeast-1 ở Singapore).
    - **Availability Zones (AZ - Vùng sẵn sàng):** Mỗi Region bao gồm nhiều AZ. Mỗi AZ là một hoặc nhiều trung tâm dữ liệu độc lập với nguồn điện, làm mát và mạng riêng. Việc triển khai ứng dụng trên nhiều AZ giúp tăng tính sẵn sàng cao (High Availability).
- **Group, Roles, Policies:** Đây là các thành phần của dịch vụ **AWS IAM (Identity and Access Management)**, dùng để quản lý quyền truy cập vào tài nguyên AWS một cách an toàn
    - **Group (Nhóm):** Là một tập hợp các người dùng (IAM Users). Thay vì gán quyền cho từng người dùng, bạn có thể gán quyền cho một Group, và mọi người dùng trong Group đó sẽ có các quyền tương ứng.
    - **Role (Vai trò):** Là một danh tính với các quyền cụ thể mà một thực thể đáng tin cậy (như một dịch vụ AWS, một ứng dụng, hoặc người dùng từ tài khoản khác) có thể "đảm nhận" (assume) để có được các quyền tạm thời. Đây là cách bảo mật và linh hoạt để cấp quyền mà không cần chia sẻ thông tin đăng nhập lâu dài.
    - **Policy (Chính sách):** Là một tài liệu (dạng JSON) định nghĩa các quyền hạn. Policy mô tả hành động nào được phép hoặc bị từ chối trên tài nguyên AWS nào.
### 3. Setup kiến trúc AWS đơn giản với Subnet, Security Group, EC2 và RDS
- **EC2 (Elastic Compute Cloud):** Dịch vụ cung cấp các máy chủ ảo (gọi là instances) trên đám mây. Đây là một trong những dịch vụ cốt lõi của AWS, cho phép bạn thuê và quản lý máy chủ để chạy ứng dụng.
- **Subnet (Mạng con):** Là một phân đoạn của mạng riêng ảo (VPC - Virtual Private Cloud). Subnet được dùng để cô lập các tài nguyên mạng với nhau, có thể là `public` (có thể truy cập từ Internet) hoặc `private` (không thể truy cập trực tiếp từ Internet).
- **Security Group (Nhóm bảo mật):** Hoạt động như một tường lửa ảo cho các máy chủ EC2, kiểm soát lưu lượng truy cập vào và ra. Bạn có thể định nghĩa các quy tắc (rules) để cho phép hoặc chặn truy cập từ các địa chỉ IP hoặc các Security Group khác.
- **RDS (Relational Database Service):** Dịch vụ quản lý cơ sở dữ liệu quan hệ (như MySQL, PostgreSQL, SQL Server). AWS sẽ tự động hóa các công việc quản trị tốn thời gian như cài đặt, vá lỗi, sao lưu, phục hồi, giúp bạn tập trung vào việc phát triển ứng dụng.
### 4. Tìm hiểu về các hình thức lưu trữ S3, EBS, EFS
- **S3 (Simple Storage Service):** Dịch vụ lưu trữ đối tượng (object storage). Dùng để lưu trữ và truy xuất bất kỳ lượng dữ liệu nào từ bất cứ đâu. Rất phù hợp cho việc lưu trữ file (hình ảnh, video), sao lưu dữ liệu, hosting website tĩnh. S3 có độ bền và khả năng mở rộng gần như vô hạn.
- **EBS (Elastic Block Store):** Dịch vụ cung cấp các ổ đĩa ảo (block storage) hiệu năng cao, giống như ổ cứng cho máy chủ EC2. Mỗi ổ EBS chỉ có thể được gắn vào một máy chủ EC2 tại một thời điểm. Thường được dùng làm ổ đĩa hệ điều hành hoặc lưu trữ dữ liệu cần truy xuất nhanh của ứng dụng.
- **EFS (Elastic File System):** Dịch vụ cung cấp hệ thống tệp tin chia sẻ (shared file system) trên nền tảng đám mây. Một hệ thống tệp EFS có thể được truy cập đồng thời bởi nhiều máy chủ EC2. Rất hữu ích khi nhiều máy chủ cần truy cập và chỉnh sửa cùng một bộ dữ liệu.
### 5. AWS Compute Services: EC2, ECS, EKS và Lambda
Đây là các dịch vụ "tính toán" (compute) của AWS, cung cấp các cách khác nhau để chạy mã nguồn của bạn.
- **EC2:** (Đã giải thích ở trên) Cung cấp máy chủ ảo, cho bạn toàn quyền kiểm soát hệ điều hành và môi trường chạy.
- **ECS (Elastic Container Service):** Dịch vụ điều phối container (container orchestration) do AWS quản lý. Dùng để triển khai, quản lý và mở rộng các ứng dụng được đóng gói trong Docker container một cách dễ dàng.
- **EKS (Elastic Kubernetes Service):** Dịch vụ Kubernetes được quản lý bởi AWS. Kubernetes là một nền tảng điều phối container mã nguồn mở rất mạnh mẽ. EKS giúp bạn chạy Kubernetes trên AWS mà không cần phải tự cài đặt và quản lý "control plane" (cụm điều khiển) của Kubernetes.
- **Lambda:** Dịch vụ tính toán phi máy chủ (serverless). Bạn chỉ cần tải mã nguồn lên, và Lambda sẽ tự động chạy và mở rộng mã nguồn đó khi có yêu cầu (trigger) mà không cần bạn phải quản lý bất kỳ máy chủ nào. Bạn chỉ trả tiền cho thời gian tính toán thực tế.
### 6. AWS RDS, AWS DynamoDB và thử kết nối với Python
- **RDS:** (Đã giải thích ở trên) Dịch vụ CSDL quan hệ.
- **DynamoDB:** Dịch vụ cơ sở dữ liệu NoSQL (key-value và document) do AWS quản lý. Nó nổi tiếng với hiệu suất cực nhanh (độ trễ mili giây) ở mọi quy mô. Rất phù hợp cho các ứng dụng web, di động, game, IoT cần truy xuất dữ liệu nhanh và linh hoạt.
- **Kết nối với Python:** Đề cập đến việc sử dụng **AWS SDK for Python (Boto3)**. Đây là thư viện cho phép các nhà phát triển Python viết mã để tương tác với các dịch vụ AWS như RDS, DynamoDB, S3, v.v.
### 7. Triển khai kiến trúc High Availability trên AWS với ELB và Auto Scaling Group
- **High Availability (HA - Tính sẵn sàng cao):** Là khả năng của một hệ thống có thể tiếp tục hoạt động ngay cả khi một hoặc nhiều thành phần của nó bị lỗi. Trên AWS, HA thường đạt được bằng cách triển khai tài nguyên trên nhiều Availability Zone.
- **ELB (Elastic Load Balancing):** Dịch vụ tự động phân phối lưu lượng truy cập đến các máy chủ (ví dụ: các instance EC2) ở nhiều Availability Zone khác nhau. Điều này giúp tăng khả năng chịu lỗi và đảm bảo không có máy chủ nào bị quá tải.
- **Auto Scaling Group (Nhóm tự động mở rộng):** Tự động điều chỉnh số lượng máy chủ EC2 trong một nhóm dựa trên nhu cầu thực tế (ví dụ: CPU tăng cao) hoặc theo lịch trình. Nó giúp đảm bảo bạn luôn có đủ tài nguyên để xử lý tải (scale out), đồng thời tiết kiệm chi phí bằng cách loại bỏ các máy chủ không cần thiết (scale in). Nó cũng giúp tăng HA bằng cách tự động thay thế các máy chủ bị lỗi.

# Video 2
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351075.png)

In this picture it have 3 AC and in this AZ(Availability Zone) it have lot of DC(Data center), especially in picutre 2,3,2 = DC.  
- AC > DC. 


- 6 Stategies to locate regions
	- Latency
	- Compliance - Tuân thủ dữ liệu phải đặt ở vùng lãnh thổ
	- Distance recovery
	- Go global
	- Cost
	- Reduce blast redis. 

Region and edge location
- CDN service
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351311.png)
- Yellow is region = edge loctions are blue points.
	- Edge locations have cache infomations so user can get and push fast info.

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351346.png)

### VPC- Virtual Private Cloud. 
Mỗi người có 1 virtual cloud khác nhau mà k lận ai được 
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351578.png)



## IAM in detail
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351731.png)
 - Users, policies, role, group, 
- Chính sách dựa trên danh tính (Identity-Based Policies)
- Chính sách dựa trên tài nguyên (Resource-Based Policies)

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351792.png)

- Policies: Identity policied , resource policies
	- [AWS Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html)
	- 


![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351897.png)
## VPC - subnet
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351952.png)

- Quan trọng là route table. 


# NACL
- Network access control list
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352010.png)
- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352069.png)

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352124.png)
- Nó sẽ for từ trên xuống dưới thì nếu là đúng port thì nó sẽ return 
----
### Security group
- Stateful: inbound sẽ vào là ra được (resquest is allowed)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352196.png)

- Khi 2 private subnet thì cần 2 AZ khác nhau chứ k được cùng. 
- Tạo route table để có thể public / Private thì chỉ có local. 
	- Trong public 
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352249.png)
#### Dựng demo thử 
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352310.png)

Kiên thức cần năm: 
- Cài vpc, cài public subnet , private 2 cái, 
	- Cài route
	- Cài **security group.** : rất quan trọng cần setting loại gì. 
- Subnet, private 
	- Sai AZ khác nhau. 
	- Nhớ là địa chỉ...
- Người ta dùng code cloud để tạo thì mình có thể tìm hiểu cái này. 
- Chưa thử NACL để bảo vệ ...
mypassword

# Video 
- S3 unlimited storage - max 5tb/file 
	- Cần create bucket to save objects - global unique
	- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352368.png)
-  ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352417.png)
- Cái này tính tiền rất nhanh xài là phải xóa đi. 

---
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352478.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352581.png)

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352622.png)

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222352667.png)
- Note:
	- EBS: khó tạo ra 1 cái khác và phải mount lại...
	- 
