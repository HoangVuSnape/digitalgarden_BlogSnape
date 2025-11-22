---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/dev-ops-tools/jmeter-testing/jmeter/","pinned":"false"}
---

Đây là nới mình học để test server. 
# Link 
- [Jmeter - download](https://www.youtube.com/watch?v=1tJGRWABpW0)
# Purpose
- **Mục tiêu của việc kiểm thử này là gì?**
- **Máy chủ Vietnamnet có thể xử lý bao nhiêu yêu cầu mỗi phút?**

# Overview

### ## 1. Understand GUI (Hiểu về Giao diện đồ họa)

Đây là bước đầu tiên và cơ bản nhất. Trước khi đi sâu vào các tính năng phức tạp, bạn cần làm quen với giao diện của JMeter. Giao diện chính bao gồm các khu vực như **Menu Bar** (thanh menu), **Toolbar** (thanh công cụ), **Tree View** (cấu trúc cây của kịch bản test), và **Editor/Configuration Pane** (khung soạn thảo và cấu hình chi tiết cho từng thành phần). Việc hiểu rõ giao diện sẽ giúp bạn thao tác nhanh chóng và hiệu quả hơn.

### ## 2. Test Plan (Kế hoạch Kiểm thử)

**Test Plan** là thành phần gốc và quan trọng nhất trong JMeter. Nó giống như một "thùng chứa" tất cả các bước và yếu tố cần thiết để thực hiện một kịch bản kiểm thử. Mọi thứ bạn tạo ra, từ việc giả lập người dùng đến việc gửi yêu cầu và xem kết quả, đều phải nằm trong một Test Plan. Một Test Plan hoàn chỉnh sẽ định nghĩa luồng thực thi và các hành động của kịch bản kiểm thử.

### ## 3. Thread Group (Nhóm Luồng)

**Thread Group** là trái tim của mọi Test Plan. Nó quyết định số lượng người dùng ảo (virtual users) sẽ được giả lập để gửi yêu cầu đến máy chủ. Mỗi "thread" (luồng) tương ứng với một người dùng ảo.

Các thông số chính trong Thread Group bao gồm:

- **Number of Threads (users):** Số lượng người dùng ảo bạn muốn giả lập.
    
- **Ramp-Up Period (in seconds):** Thời gian (tính bằng giây) để JMeter khởi chạy hết tất cả các luồng đã định. Ví dụ, nếu bạn có 100 luồng và Ramp-Up là 10 giây, JMeter sẽ khởi động 10 luồng mỗi giây.
	- Ví dụ: Nếu chúng ta có 100 người dùng và 100 Ram-up trước đó thì sự chậm trễ giữa những người dùng sẽ là **1 giây (100 người dùng/ 100 giây)**
    
- **Loop Count:** Số lần mỗi người dùng ảo sẽ thực hiện lại kịch bản kiểm thử.
    

### ## 4. Samplers (Bộ lấy mẫu)

**Samplers** là thành phần thực hiện công việc chính: gửi các loại yêu cầu khác nhau đến máy chủ. JMeter hỗ trợ rất nhiều loại Samplers, phổ biến nhất là:

- **HTTP Request:** Gửi yêu cầu HTTP/HTTPS đến một máy chủ web. Đây là Sampler được sử dụng nhiều nhất để kiểm thử ứng dụng web.
    
- **FTP Request:** Gửi yêu cầu đến máy chủ FTP (tải lên/tải xuống file).
    
- **JDBC Request:** Gửi các câu truy vấn SQL đến cơ sở dữ liệu.
    
- **Java Request:** Thực thi mã Java tùy chỉnh.
    
- **SOAP/XML-RPC Request:** Gửi yêu cầu đến các dịch vụ web SOAP.
    

Mỗi Sampler sẽ mô phỏng một hành động của người dùng, ví dụ như truy cập một trang web hoặc đăng nhập vào hệ thống.

### ## 5. Listeners (Bộ lắng nghe)

**Listeners** có nhiệm vụ thu thập và hiển thị kết quả kiểm thử. Chúng không gửi yêu cầu mà chỉ "lắng nghe" các phản hồi từ máy chủ mà các Samplers đã gửi đi và trình bày dữ liệu đó dưới nhiều định dạng khác nhau.

Các Listeners phổ biến:

- **View Results Tree:** Hiển thị chi tiết từng yêu cầu và phản hồi, rất hữu ích cho việc gỡ lỗi (debug).
    
- **Summary Report / Aggregate Report:** Cung cấp bảng tổng hợp các chỉ số quan trọng như thời gian phản hồi trung bình, tối thiểu, tối đa, và thông lượng (throughput).
    
- **Graph Results:** Vẽ đồ thị biểu diễn các kết quả theo thời gian.
    

**Lưu ý:** Nên tắt hoặc hạn chế sử dụng Listeners có giao diện phức tạp (như View Results Tree) khi chạy kiểm thử tải lớn, vì chúng tiêu tốn nhiều bộ nhớ. Thay vào đó, hãy lưu kết quả ra file `.jtl` và mở lại sau khi chạy xong.

### ## 6. Recording (Ghi lại kịch bản)

Thay vì tạo từng Sampler một cách thủ công, bạn có thể sử dụng tính năng **Recording** của JMeter. JMeter hoạt động như một proxy server, ghi lại tất cả các hành động (request) bạn thực hiện trên trình duyệt web và tự động chuyển chúng thành các HTTP Request Samplers trong Test Plan. Điều này giúp tiết kiệm rất nhiều thời gian, đặc biệt với các luồng nghiệp vụ phức tạp. Bạn sẽ sử dụng thành phần **HTTP(S) Test Script Recorder** để thực hiện việc này.

### ## 7. CMD (Chạy bằng Dòng lệnh)

Khi thực hiện các bài kiểm thử tải thực sự (với số lượng người dùng lớn), không nên chạy JMeter ở chế độ giao diện đồ họa (GUI mode) vì nó tốn rất nhiều tài nguyên CPU và bộ nhớ. Thay vào đó, bạn nên chạy JMeter ở chế độ **Non-GUI (chế độ dòng lệnh - CMD)**.

Câu lệnh cơ bản:

Bash

```
jmeter -n -t [đường_dẫn_đến_file.jmx] -l [đường_dẫn_đến_file_kết_quả.jtl]
```
```
jmeter -n -t "C:\Users\Snape\Desktop\chatbotHRM\chatbotHRM.jmx" -l "C:\Users\Snape\Desktop\chatbotHRM"
```


- `-n`: Chạy ở chế độ Non-GUI.
    
- `-t`: Chỉ định file kịch bản `.jmx`.
    
- `-l`: Chỉ định file để lưu kết quả.
    

Chạy bằng dòng lệnh giúp JMeter hoạt động hiệu quả hơn và cho ra kết quả chính xác hơn.

### ## 8. Best Practices (Các Phương pháp Tốt nhất)

Đây là tập hợp các quy tắc và kinh nghiệm được khuyến nghị khi sử dụng JMeter để đảm bảo hiệu quả và độ chính xác:

- Luôn sử dụng phiên bản JMeter và Java mới nhất.
    
- Sử dụng chế độ Non-GUI (dòng lệnh) để chạy các bài test lớn.
    
- Hạn chế sử dụng Listeners trong khi chạy test.
    
- Không sử dụng "View Results Tree" hoặc "View Results in Table" cho các bài test tải nặng.
    
- Sử dụng biến (Variables) thay vì hard-code giá trị.
    
- Sử dụng các Assertion một cách hợp lý để xác thực phản hồi, nhưng đừng quá lạm dụng vì chúng cũng tiêu tốn tài nguyên.
    

Hy vọng những giải thích trên sẽ giúp bạn hiểu rõ hơn về các khái niệm cốt lõi trong JMeter!



# Note
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Jmeter_Testing/IMG-20251122221511055.png)
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Jmeter_Testing/IMG-20251122221511099.png)

- Trong bức hình 1:  thì 0-1000 là connect time. 1000-1200 là latency. 2000 - response time.  


Giải thích rõ từng **chỉ số thời gian (timing metrics)** bạn đang có:

---

### ✅ 1. **Connect Time**:

- **Là thời gian từ khi user gửi request cho đến khi kết nối được thiết lập với server.**
    
- Bao gồm: DNS lookup, TCP handshake, SSL handshake (nếu có).
    
- **Ý nghĩa**: Đo mức độ nhanh/chậm của việc kết nối ban đầu đến server.
    
- → Nếu **connect time cao**: có thể do mạng yếu, DNS chậm, hoặc server phản hồi chậm trong việc bắt tay kết nối.
    

---

### ✅ 2. **Latency** (hay đôi khi gọi là "server latency"):

- **Là thời gian từ lúc kết nối được thiết lập cho đến khi server trả lời xong (hoặc bắt đầu gửi data).**
    
- Tính từ: **kết thúc connect → nhận response từ server**
    
- Bao gồm: thời gian xử lý logic, truy vấn DB, gọi API nội bộ...
    
- **Ý nghĩa**: thể hiện độ trễ xử lý phía server.
    
- → Nếu **latency cao**: server bận, logic xử lý nặng, DB chậm, gọi service khác bị delay...
    

---

### ✅ 3. **Elapsed** (hay Response Time tổng thể):

- **Là thời gian tổng cộng từ khi gửi request cho đến khi nhận được toàn bộ response.**
    
- Công thức gần đúng:
    
    ```
    Elapsed ≈ Connect + Latency + thời gian truyền tải (transfer time)
    ```
    
- Thường bao gồm luôn cả **thời gian truyền response về client** (đặc biệt nếu payload lớn).
    

---

### 📌 Ví dụ minh hoạ cụ thể:

|Metric|Mô tả ngắn|Ví dụ số liệu (ms)|
|---|---|---|
|Connect|Từ khi gửi request đến khi kết nối xong|1600|
|Latency|Thời gian xử lý từ server|5100|
|Elapsed|Tổng thời gian nhận toàn bộ phản hồi|6337|

→ Như vậy:  
**Elapsed ≈ Connect + Latency + một ít transfer time (~600ms trong ví dụ trên).**

---

### ✅ Tổng kết mối quan hệ:

```
Connect < Latency < Elapsed
```

- Nếu:
    
    - **Connect cao** → mạng hoặc hạ tầng kết nối có vấn đề
        
    - **Latency cao** → server xử lý chậm (DB chậm, code nặng, quá tải)
        
    - **Elapsed cao** → có thể do cả 2 yếu tố trên, hoặc file trả về lớn
        

---

Nếu bạn đang test API (vd: bằng JMeter hoặc Postman), thì:

- **Connect time** được đo từ "request start" đến "server connected"
    
- **Latency** là từ "connected" đến "first byte received"
    
- **Elapsed** là từ "request start" đến "response fully received"

---


| 🏷 Cột              | 📖 Ý nghĩa                                 | 🔍 Diễn giải                                                                                                                                      |
| ------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Label**           | Tên request/test                           | Là tên của sampler trong JMeter. Ở đây là `"Get list employee"`. Nếu có nhiều sampler thì có thể có nhiều dòng.                                   |
| **# Samples**       | Số lượng request                           | Tổng số lần gửi request. Ở đây là **100** lần gửi request đến server.                                                                             |
| **Average**         | Thời gian phản hồi trung bình (ms)         | Trung bình thời gian 1 request mất từ lúc gửi đến lúc nhận đủ phản hồi. Ở đây là **31,190 ms** (≈ 31 giây).                                       |
| **Min**             | Thời gian phản hồi nhanh nhất (ms)         | Request có thời gian xử lý nhanh nhất là **5,226 ms**.                                                                                            |
| **Max**             | Thời gian phản hồi chậm nhất (ms)          | Request có thời gian xử lý chậm nhất là **44,125 ms**.                                                                                            |
| **Std. Dev.**       | Độ lệch chuẩn (ms)                         | Đo độ dao động/phân tán của thời gian phản hồi so với giá trị trung bình. Ở đây là **11,775.7 ms**, nghĩa là thời gian phản hồi dao động khá lớn. |
| **Error %**         | Tỷ lệ lỗi                                  | Tỷ lệ request bị lỗi (status code không phải 2xx). Ở đây là **0.00%** → tất cả request thành công.                                                |
| **Throughput**      | Tốc độ xử lý request (requests/giây)       | Trung bình xử lý được **2.3 request mỗi giây** trong suốt quá trình test.                                                                         |
| **Received KB/sec** | Dữ liệu nhận mỗi giây (KB)                 | Trung bình client nhận **20,503.67 KB/s** từ server trong suốt quá trình test.                                                                    |
| **Sent KB/sec**     | Dữ liệu gửi mỗi giây (KB)                  | Trung bình client gửi **15.58 KB/s** đến server.                                                                                                  |
| **Avg. Bytes**      | Kích thước trung bình mỗi response (Bytes) | Trung bình mỗi phản hồi trả về có kích thước **9,265,637.3 Bytes** (~9.3 MB) — khá lớn.                                                           |

$ kubectl --kubeconfig ./config-server port-forward dev-postgres-0 -n dev-postgres 5432:5432
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/DevOps_Tools/Jmeter_Testing/IMG-20251122221511140.png)



 docker run --rm -it -v "${PWD}:/backup" -e PGPASSWORD=your_password postgres:latest pg_restore --no-owner -h host.docker.internal -U hrm_user -d hrm_db /backup/dump-hrm_uat-202507211409.sql

