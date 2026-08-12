---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/dev-ops-tools/web-cloud/gate-way-proxy-grpc/","title":"Gate way, proxy,..","pinned":"false","tags":["type/howto","topic/web-dev","status/stable"]}
---

# Link
- [Forward, reverse proxy](https://viblo.asia/p/forward-proxy-va-reverse-proxy-WAyK8rM9lxX#_3-reverse-proxy-4)
- 
# Forward proxy và reverse proxy

- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351106.png)

# API Gateway & Graph QL

Tất nhiên rồi, mình cùng phân tích sâu hơn về Kong Gateway và AWS API Gateway nhé.

Dưới đây là so sánh chi tiết giữa hai lựa chọn phổ biến này, đồng thời giải đáp thắc mắc của bạn về GraphQL.

### **So sánh chi tiết Kong Gateway và AWS API Gateway**

|Tiêu chí|Kong Gateway (Open-Source & Enterprise)|AWS API Gateway|
|:--|:--|:--|
|**Mô hình triển khai**|**Linh hoạt:** Có thể tự host (on-premise), trên bất kỳ nhà cung cấp đám mây nào (AWS, GCP, Azure), hoặc Kubernetes.|**Dịch vụ được quản lý (Managed Service):** Chỉ chạy trên hạ tầng của AWS.|
|**Chi phí**|**Tiết kiệm hơn ở quy mô lớn:** Bản open-source miễn phí. Bản Enterprise có phí license nhưng có thể rẻ hơn khi lưu lượng truy cập (traffic) rất lớn.|**Dễ dự đoán ở quy mô nhỏ:** Trả tiền theo mức độ sử dụng (pay-as-you-go). Có bậc miễn phí (free-tier) khá hào phóng cho các dự án nhỏ và vừa.|
|**Cộng đồng & Plugin**|**Rất lớn và đa dạng:** Hệ sinh thái plugin phong phú từ cộng đồng (miễn phí) và từ Kong (trả phí). Dễ dàng tùy chỉnh và mở rộng.|**Phụ thuộc vào AWS:** Plugin và các tính năng mở rộng chủ yếu do AWS phát triển. Tích hợp sâu với các dịch vụ khác của AWS.|
|**Vendor Lock-in**|**Không:** Vì là mã nguồn mở và có thể chạy ở bất cứ đâu, việc di chuyển sang nền tảng khác dễ dàng hơn.|**Cao:** Rất khó để di chuyển sang nhà cung cấp khác nếu không thiết kế lại hệ thống.|
|**Quản lý & Vận hành**|**Cần chuyên môn:** Đội ngũ của bạn phải tự cài đặt, cấu hình, giám sát và bảo trì.|**Đơn giản:** AWS lo phần vận hành hạ tầng, bạn chỉ cần tập trung vào việc cấu hình API.|

Export to Sheets

---

### **Khi nào nên chọn Kong Gateway? 🤔**

- **Ưu tiên mã nguồn mở và không muốn bị "vendor lock-in":** Đây là lợi thế lớn nhất của Kong. Bạn có toàn quyền kiểm soát và có thể triển khai ở bất cứ đâu.
- **Cần các tính năng tùy chỉnh cao:** Với hệ thống plugin đa dạng, bạn có thể "may đo" gateway theo đúng nhu cầu đặc thù của dự án.
- **Kiến trúc Multi-cloud hoặc Hybrid-cloud:** Khi hệ thống của bạn không chỉ nằm trên AWS mà còn ở các nhà cung cấp khác hoặc tại trung tâm dữ liệu riêng.
- **Có đội ngũ vận hành (DevOps/SRE) mạnh:** Việc tự quản lý đòi hỏi kiến thức và nguồn lực để đảm bảo hệ thống chạy ổn định và an toàn.

---

### **Khi nào nên chọn AWS API Gateway? ☁️**

- **Hệ thống chủ yếu trên AWS:** Tận dụng sự tích hợp liền mạch với các dịch vụ khác như Lambda, S3, DynamoDB để xây dựng kiến trúc serverless một cách nhanh chóng.
- **Muốn giảm gánh nặng vận hành:** Nếu bạn không muốn lo lắng về việc cài đặt, vá lỗi, hay mở rộng (scaling) hạ tầng cho gateway.
- **Tốc độ phát triển là ưu tiên:** AWS API Gateway giúp bạn nhanh chóng đưa API ra thị trường mà không cần quan tâm nhiều đến hạ tầng bên dưới.
- **Dự án vừa và nhỏ:** Mô hình trả tiền theo lượng request rất phù hợp khi traffic chưa quá lớn.

---

### **Còn GraphQL thì sao? Nó có thay thế được API Gateway không? 🚀**

Đây là một câu hỏi rất hay!

**Câu trả lời ngắn gọn là: Không, GraphQL không thay thế API Gateway.** Thay vào đó, chúng giải quyết các vấn đề khác nhau nhưng có thể hoạt động rất hiệu quả cùng nhau.

- **API Gateway (như Kong, AWS API Gateway):** Đóng vai trò là "người gác cổng" cho toàn bộ hệ thống của bạn. Nó xử lý các vấn đề ở tầng hạ tầng và bảo mật như:
    
    - **Xác thực và ủy quyền (Authentication & Authorization):** Ai được phép gọi API?
    - **Giới hạn lưu lượng (Rate Limiting):** Ngăn chặn các cuộc tấn công DoS hoặc việc lạm dụng API.
    - **Giám sát và ghi log (Monitoring & Logging):** Theo dõi sức khỏe và lưu lượng của API.
    - **Routing:** Điều hướng request đến đúng microservice.
    - **Caching:** Tăng tốc độ phản hồi cho các request lặp lại.
- **GraphQL:** Là một **ngôn ngữ truy vấn (query language)** cho API và là một runtime để thực thi các truy vấn đó. Nó giải quyết vấn đề ở tầng dữ liệu:
    
    - **Chống over-fetching và under-fetching:** Client có thể yêu cầu chính xác những dữ liệu mình cần, không thừa, không thiếu.
    - **Tổng hợp dữ liệu từ nhiều nguồn:** Một truy vấn GraphQL duy nhất có thể lấy dữ liệu từ nhiều microservice, database, hoặc API khác nhau.
    - **Cung cấp schema rõ ràng:** Giúp đội ngũ frontend và backend làm việc độc lập và hiệu quả hơn.

**Kết hợp hoàn hảo:** Bạn hoàn toàn có thể đặt một GraphQL server **phía sau** một API Gateway.

_Mô hình phổ biến:_

```
Client -> AWS API Gateway / Kong Gateway -> GraphQL Server (e.g., Apollo Server) -> Microservices / Databases
```

Trong mô hình này:

1. **API Gateway** sẽ lo các vấn đề về bảo mật, rate limiting, logging.
2. Sau khi request vượt qua được "cổng" này, nó sẽ được chuyển đến **GraphQL Server**.
3. **GraphQL Server** sẽ phân tích truy vấn, gọi đến các microservice cần thiết để lấy dữ liệu, tổng hợp lại và trả về cho client một response duy nhất.


----
# GRPC & Protobuf

The article discusses gRPC and Protocol Buffers (Protobuf) as options for communication between clients and servers or services within a **microservice architecture**.

**gRPC (Google Remote Procedure Call)** is an open-source framework developed by Google that allows applications running on different machines and using different programming languages to **communicate effectively and reliably**. It uses a client-server model for data transfer.

**Protocol Buffers (Protobuf)** is a data format also developed by Google. It is used to **define data structures independently of language and platform**. Protobuf uses `.proto` files to define these structures, and a compiler generates code for desired programming languages.

**How they work together**: gRPC uses Protobuf as the standard data format for all messages transferred between applications. This choice is highlighted as a key reason why gRPC data is **very small in size and high in speed** compared to traditional methods like JSON or XML. The source illustrates this by showing that a sample company data serialized with JSON is 102 bytes, while the same data serialized with Protobuf using a defined schema is only 50 bytes. The efficiency comes from Protobuf encoding data as binary values without including field names, relying on predefined field tags (like 1, 2, 3) for decoding.

**Advantages of gRPC**:

- **Reduces the amount of data transferred**, decreasing bandwidth usage and increasing speed.
- **Saves system resources**, minimizes network congestion, and increases system performance due to using Protobuf.
- **Supports many programming languages**.
- Provides **reliable and efficient data transfer** through its RPC (Remote Procedure Call) model.
- Supports **multiple connection types**, including Unary RPC, Server streaming RPC, Client streaming RPC, and Bidirectional streaming RPC.
- Its development, support, and maintenance are **backed by Google**.

**Considerations/Disadvantages of gRPC**:

- gRPC uses **HTTP/2** instead of HTTP/1.1, which may have limitations regarding browser support. Deploying services using gRPC in a system may require service registry for components like load balancers.
- Data encoded with Protobuf is **difficult to read normally** compared to text-based formats like JSON or XML.
- The mechanism requires defining data schemas (`.proto` files) on **both the client and server sides**, necessitating synchronization of these files during development.

**Comparison with REST**: The source provides a small comparison:

- gRPC uses **HTTP/2** while REST typically uses **HTTP/1.1**.
- gRPC transfers data using **Protobuf (optimized binary)**, whereas REST commonly uses **JSON (text-based)**.
- gRPC has **strict, required data types**, while REST has more **flexible, optional data types**.

**Purpose and Use Case**: gRPC is primarily used to help systems **optimize bandwidth and speed** when transferring information between clients/servers or services. While it offers significant benefits in these areas, particularly in **large systems and microservice architectures**, the decision to use gRPC requires considering trade-offs, such as the complexity of using HTTP/2, the difficulty in reading binary data, and the need for schema synchronization compared to simpler JSON-based methods.

### Cú pháp cmd:
```cmd
python -m grpc_tools.protoc --proto_path=. --python_out=. --grpc_python_out=. greet.proto
```


![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Web_Cloud/IMG-20251122222351481.png)


### Enum
- Được sử dụng để định nghĩa các giá trị cố định.
- Đảm bảo rằng chỉ các giá trị hợp lệ được sử dụng trong ứng dụng.
- Giúp mã nguồn dễ đọc và giảm lỗi do sử dụng giá trị không hợp lệ.

### **`yield`**

- Biến hàm thành một generator (bộ sinh).
- Mỗi lần gặp `yield`, hàm tạm dừng và trả về một giá trị, nhưng trạng thái hàm được giữ lại để tiếp tục lần sau.
- Cho phép trả về nhiều giá trị tuần tự (dùng trong vòng lặp).