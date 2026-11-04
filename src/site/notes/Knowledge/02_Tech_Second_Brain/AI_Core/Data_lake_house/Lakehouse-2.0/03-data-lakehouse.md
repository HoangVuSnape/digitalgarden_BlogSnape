---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/data-lake-house/lakehouse-2-0/03-data-lakehouse/","title":"Data Lakehouse là gì và hoạt động như thế nào","pinned":"false","tags":["type/course","topic/data-lakehouse","status/stable"]}
---

# Data Lakehouse là gì và hoạt động như thế nào?

> **Tác giả:** Dipankar Mazumdar  
> **Ngày xuất bản:** 11/07/2024  
> **Thời gian đọc:** 19 phút  
> **Chủ đề:** Data Lakehouse, Apache Iceberg, Delta Lake, open architecture

## Tóm tắt

Data Lakehouse là một kiến trúc dữ liệu lai (hybrid data architecture) kết hợp những ưu điểm tốt nhất của Data Warehouse và Data Lake nhằm giải quyết các hạn chế vốn có của từng hệ thống. Nó hoạt động bằng cách xếp một tầng Open Table Format—chẳng hạn như Apache Hudi, Apache Iceberg hoặc Delta Lake—lên trên dịch vụ Cloud Object Storage chi phí thấp, mang lại các khả năng giao dịch (transactional capabilities) của Data Warehouse cho Data Lake trên đám mây, đồng thời vẫn duy trì khả năng mở rộng (scalability) với chi phí tối ưu.

![Kiến trúc Data Lakehouse](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Data_lake_house/data-lakehouse-architecture.png)

![Hệ sinh thái Lakehouse](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Data_lake_house/lakehouse-ecosystem.png)

Kiến trúc Lakehouse hỗ trợ quản lý đa dạng các loại dữ liệu, bao gồm dữ liệu có cấu trúc (structured), bán cấu trúc (semi-structured) và phi cấu trúc (unstructured), đáp ứng dải rộng các use case như Business Intelligence (BI), Machine Learning (ML) và Real-time streaming. Tính linh hoạt này giúp doanh nghiệp thoát khỏi kiến trúc hai tầng truyền thống (two-tier architecture)—vốn phải dùng riêng Warehouse cho relational workloads và Data Lake cho Machine Learning hay phân tích nâng cao. Nhờ đó, các tổ chức có thể cắt giảm chi phí vận hành và tinh gọn chiến lược dữ liệu bằng cách làm việc trên một kho lưu trữ dữ liệu duy nhất (single data store).

## Đọc thêm

Các bài viết chuyên sâu đi kèm với hướng dẫn này: *What is an Open Table Format?*, *Data Lakehouse vs Data Warehouse vs Data Lake*, và *Open Table Format vs Data Lakehouse*.

## Sự tiến hóa của các kiến trúc lưu trữ dữ liệu

### Từ Data Warehouse đến Data Lakehouse

Trong lịch sử, các tổ chức đã đầu tư mạnh mẽ vào việc xây dựng các kiến trúc dữ liệu tập trung và có khả năng mở rộng để cho phép truy cập nhiều dữ liệu hơn và hỗ trợ đa dạng các dạng analytical workloads. Khi nhu cầu đối với các khối lượng công việc này ngày càng tăng, các kiến trúc dữ liệu đã tiến hóa để giải quyết những nhu cầu phức tạp của việc xử lý và lưu trữ dữ liệu hiện đại.

Data Warehouse là một trong những hệ thống đầu tiên đóng vai trò làm kho lưu trữ tập trung cho các structured workloads, cho phép doanh nghiệp trích xuất insight lịch sử từ nhiều nguồn dữ liệu rời rạc. Tuy nhiên, chúng cũng mang lại nhiều thách thức, bao gồm các định dạng lưu trữ độc quyền (proprietary storage formats) dẫn đến rủi ro bị khóa chặt vào nhà cung cấp (vendor lock-in), cùng với sự hạn chế trong việc hỗ trợ các analytical workloads thế hệ mới, đặc biệt là dữ liệu phi cấu trúc phục vụ Machine Learning.

Data Lake xuất hiện như một thế hệ kiến trúc phân tích tiếp theo, cho phép các tổ chức mở rộng độc lập giữa Storage và Compute, từ đó tối ưu hóa tài nguyên và tăng cường hiệu quả chi phí. Data Lake hỗ trợ lưu trữ mọi loại dữ liệu—structured, semi-structured và unstructured—trong các hệ thống lưu trữ giá rẻ bằng các open file formats như Apache Parquet và Apache ORC. Mặc dù Data Lake mang lại tính linh hoạt cao với phương pháp schema-on-read, chúng lại thiếu các năng lực transactional (đảm bảo tính chất ACID) và thường xuyên đối mặt với thách thức liên quan đến chất lượng dữ liệu (data quality) và quản trị dữ liệu (governance).

Những thách thức từ hai phương pháp quản lý dữ liệu này đã thúc đẩy sự ra đời của một kiến trúc mới mang tên Data Lakehouse. Một Lakehouse mang các khả năng giao dịch của hệ quản trị cơ sở dữ liệu (DBMS) vào các Data Lake có khả năng mở rộng, cho phép chạy đa dạng các loại workload trên các định dạng lưu trữ mở.

## Data Lakehouse

Data Lakehouse kết hợp độ tin cậy và hiệu năng của Data Warehouse với khả năng mở rộng quy mô cùng hiệu quả chi phí của Data Lake. Hướng tiếp cận kết hợp này mang lại các tính năng như time-travel, indexing, schema evolution và các khả năng tối ưu hóa hiệu năng trên các định dạng mở có thể truy cập tự do.

### Đặc điểm chính

- **Open Data Architecture:** Lakehouse lưu trữ dữ liệu ở các định dạng mở. Điều này cho phép nhiều engine phân tích khác nhau (từ nhiều nhà cung cấp) cùng chạy trên một tập dữ liệu duy nhất, ngăn chặn tình trạng bị phụ thuộc vào định dạng độc quyền (vendor lock-in).
- **Hỗ trợ đa dạng loại dữ liệu & Workloads (Support for Varied Data Types & Workloads):** Data Lakehouse thích ứng với nhiều loại dữ liệu—structured, semi-structured và unstructured—và do đó được trang bị để xử lý đa dạng analytical workloads như BI, Machine Learning và Real-time analytics.
- **Hỗ trợ Transactional (ACID):** Data Lakehouse nâng cao độ tin cậy và tính nhất quán bằng cách cung cấp các đảm bảo ACID cho các giao dịch (như `INSERT` hay `UPDATE`), tương tự như trong các hệ thống RDBMS-OLAP. Điều này đảm bảo an toàn cho các tác vụ đọc và ghi đồng thời (concurrent reads and writes).
- **Giảm thiểu bản sao dữ liệu (Less data copies):** Data Lakehouse giảm thiểu sự trùng lặp dữ liệu vì các compute engines có thể truy cập trực tiếp dữ liệu từ các định dạng lưu trữ mở mà không cần copy qua lại.
- **Quản lý Schema (Schema management):** Data Lakehouse đảm bảo tuân thủ một schema cụ thể khi ghi dữ liệu mới vào kho lưu trữ. Hệ thống cũng tạo điều kiện cho schema evolution (tiến hóa schema) theo thời gian mà không cần phải ghi lại toàn bộ bảng, từ đó tiết kiệm chi phí lưu trữ và vận hành.
- **Data Quality và Governance:** Lakehouse đảm bảo tính toàn vẹn của dữ liệu và tích hợp các cơ chế quản trị (governance) cùng kiểm toán (auditing) mạnh mẽ. Các tính năng này duy trì các tiêu chuẩn chất lượng dữ liệu cao, tạo điều kiện thuận lợi cho việc tuân thủ pháp lý (như GDPR) và cho phép áp dụng các biện pháp quản lý dữ liệu an toàn.

## Sáu thành phần kỹ thuật

- **Lake Storage:** Tầng lưu trữ là nơi các tệp từ nhiều hệ thống vận hành khác nhau đổ về sau khi được nạp thông qua các quy trình ETL/ELT. Các dịch vụ Cloud Object Storage như Amazon S3, Azure Blob và Google Cloud Storage hỗ trợ bất kỳ loại dữ liệu nào và có thể mở rộng đến dung lượng gần như vô hạn. Hiệu quả về chi phí của chúng là một lợi thế vượt trội so với chi phí lưu trữ đắt đỏ của Data Warehouse truyền thống.
- **File Format:** Trong kiến trúc Lakehouse, các định dạng file như Apache Parquet hoặc ORC lưu trữ dữ liệu thô thực tế trên Object Storage. Các open formats này cho phép nhiều engine khác nhau cùng tiêu thụ dữ liệu cho các workloads đa dạng. Do thường được tổ chức theo dạng cột (column-oriented), chúng mang lại lợi thế vượt trội về tốc độ đọc dữ liệu.
- **Table Format:** Thành phần cốt lõi của kiến trúc Lakehouse là Table Format. Nó có bản chất mở và đóng vai trò như một tầng metadata nằm phía trên các file formats như Apache Parquet. Tầng này trừu tượng hóa sự phức tạp của cấu trúc dữ liệu vật lý bằng cách định nghĩa một schema trên các immutable data files. Nó cho phép các engine khác nhau đọc và ghi đồng thời trên cùng một tập dữ liệu, hỗ trợ các giao dịch dựa trên ACID. Các table formats như Apache Hudi, Apache Iceberg và Delta Lake mang lại các tính năng thiết yếu như schema evolution, partitioning và time-travel.
- **Storage Engine:** Storage Engine trong Lakehouse điều phối các tác vụ quản lý dữ liệu thiết yếu bao gồm clustering, compaction, cleaning và indexing nhằm tinh gọn việc tổ chức dữ liệu trên Cloud Object Storage để tăng tốc hiệu năng truy vấn. Cả các nền tảng Lakehouse mã nguồn mở và thương mại đều được trang bị các native storage engines nhằm nâng cao các năng lực này, tối ưu hóa layout lưu trữ một cách hiệu quả.
- **Catalog:** Thường được gọi là metastore, Catalog là một thành phần trọng yếu của kiến trúc Lakehouse giúp tìm kiếm và khám phá dữ liệu hiệu quả bằng cách theo dõi toàn bộ các bảng và metadata của chúng. Nó ghi nhận tên bảng, schema (tên và kiểu của các cột) và các con trỏ tham chiếu đến metadata cụ thể của từng bảng (table format).
- **Compute Engine:** Compute Engine trong Lakehouse xử lý dữ liệu và đảm bảo hiệu năng đọc/ghi tối ưu. Nó tương tác với dữ liệu thông qua các read/write APIs được cung cấp bởi table formats. Các compute engines được tinh chỉnh cho từng loại workload cụ thể, với các lựa chọn như Trino và Presto cho các truy vấn ad-hoc SQL có độ trễ thấp, Apache Flink cho streaming, và Apache Spark cho các tác vụ Machine Learning và xử lý phân tán.

## Ưu điểm của Data Lakehouse

Kiến trúc Lakehouse, với đặc trưng là các định dạng lưu trữ dữ liệu mở và các lựa chọn tối ưu chi phí, mang lại vô số lợi thế. Dưới đây là các lợi ích cốt lõi:

| Thuộc tính                                | Mô tả                                                                                                                 |
| :---------------------------------------- | :-------------------------------------------------------------------------------------------------------------------- |
| **Open Data Foundation**                  | Dữ liệu được lưu bằng các file format mở như Apache Parquet và table format như Apache Hudi, Iceberg hoặc Delta Lake. |
| **Unified Data Platform**                 | Kết hợp workload của Data Warehouse và Data Lake trên một nền tảng duy nhất.                                          |
| **Centralized Data Repository**           | Lưu trữ dữ liệu structured, semi-structured và unstructured trong cùng một hệ thống.                                  |
| **Cost Efficiency**                       | Giảm chi phí storage, vận hành và ETL nhờ object storage giá thấp và ít bản sao dữ liệu.                              |
| **Performance and Scalability**           | Cho phép scale storage và compute độc lập theo nhu cầu.                                                               |
| **Enhanced Query Performance**            | Tối ưu layout dữ liệu Parquet/ORC để cải thiện hiệu năng truy vấn.                                                    |
| **Data Governance and Management**        | Tập trung chính sách governance, bảo mật và giám sát dữ liệu.                                                         |
| **Improved Data Quality and Consistency** | Enforce schema và transaction consistency để tăng độ tin cậy.                                                         |
| **Support for Various Compute Engines**   | Hỗ trợ SQL engine, ML tool và streaming engine cùng truy cập dữ liệu.                                                 |

## Triển khai Data Lakehouse

Thiết kế mở và mang tính mô-đun của kiến trúc Data Lakehouse cho phép doanh nghiệp lựa chọn các công cụ và engine tốt nhất (best-of-breed) theo đúng nhu cầu cụ thể. Do đó, việc triển khai Lakehouse có thể biến hóa linh hoạt tùy theo từng use case. Phần này phác thảo các yếu tố cân nhắc phổ biến khi xây dựng một kiến trúc Lakehouse. Với độ phức tạp khác nhau (workloads, bảo mật, v.v.) và tool stack đa dạng, các hệ thống quy mô lớn có thể cần các hướng tiếp cận được may đo riêng.

### Data Ingestion
Giai đoạn đầu tiên trong kiến trúc Lakehouse bao gồm việc trích xuất và nạp dữ liệu vào một Data Lake đám mây chi phí thấp như Amazon S3, nơi dữ liệu hạ cánh ở định dạng thô (Parquet files). Hướng tiếp cận này áp dụng phương pháp "schema-on-read", nghĩa là không cần phải xử lý dữ liệu ngay lập tức khi vừa nạp vào. Khi dữ liệu đã nằm đúng vị trí, logic chuyển đổi (transformation) có thể được áp dụng để chuyển dần sang mô hình "schema-on-write", sắp xếp dữ liệu cho các analytical workloads cụ thể như ad-hoc SQL queries hoặc Machine Learning.

### Metadata và Transactional Layer
Để kích hoạt các năng lực transactional, Apache Hudi, Apache Iceberg hoặc Delta Lake có thể được lựa chọn làm Table Format. Chúng cung cấp một tầng metadata mạnh mẽ với schema giống như bảng cơ sở dữ liệu nằm phía trên các file dữ liệu vật lý trong Object Store. Cùng với Storage Engine, chúng áp dụng các chiến lược tối ưu hóa dữ liệu để duy trì hiệu năng truy vấn nhanh và hiệu quả. Tầng metadata cũng tạo điều kiện cho các tính năng như truy vấn time-travel, rollback phiên bản và schema evolution tương tự như một Data Warehouse truyền thống.

### Processing Layer
Compute Engine là một thành phần cốt yếu trong kiến trúc Lakehouse, chịu trách nhiệm xử lý các data files do Table Format quản lý. Tùy thuộc vào workload cụ thể, các distributed query engines dựa trên SQL như Presto hoặc Trino có thể được sử dụng cho phân tích tương tác ad-hoc, hoặc Apache Spark cho các tác vụ distributed ETL. Lakehouse table formats cung cấp nhiều kỹ thuật tối ưu như indexes, statistics, cùng với việc tối ưu layout dữ liệu bao gồm clustering, compaction và Z-ordering. Những điều này giúp các compute engines đạt được hiệu năng tương đương với các Data Warehouse truyền thống.

### Catalog Layer
Tầng Catalog trong kiến trúc Lakehouse chịu trách nhiệm theo dõi toàn bộ các bảng và duy trì metadata thiết yếu. Nó đảm bảo dữ liệu có thể được truy cập dễ dàng bởi các query engines, hỗ trợ quản lý dữ liệu hiệu quả, nâng cao tính khả dụng và khả năng quản trị. Các lựa chọn triển khai Catalog bao gồm Unity Catalog, AWS Glue, Hive Metastore, Apache Polaris hoặc các catalog dựa trên file system. Tầng này đóng vai trò then chốt trong việc duy trì các tiêu chuẩn chất lượng và quản trị dữ liệu bằng cách thiết lập các chính sách kiểm định dữ liệu, các biện pháp bảo mật và các giao thức tuân thủ.

## Use Cases

Kiến trúc Lakehouse được ứng dụng cho rất nhiều trường hợp sử dụng khác nhau. Dưới đây là một số ví dụ nổi bật:

### Unified Batch và Streaming
Các kiến trúc phân tích truyền thống thường tách biệt nơi lưu trữ real-time và batch, sử dụng các kho dữ liệu chuyên dụng cho real-time insights và Data Lake cho batch processing có độ trễ. Nền tảng Lakehouse bắc cầu nối khoảng cách này bằng cách đưa các khả năng streaming vào Data Lake, cho phép nạp dữ liệu chỉ trong vài phút và xây dựng các pipeline gia tăng (incremental pipelines) nhanh hơn. Sự tích hợp này giải quyết bài toán độ tươi của dữ liệu (data freshness) và loại bỏ nhu cầu đầu tư lớn vào hạ tầng ban đầu, biến nó thành một giải pháp có khả năng mở rộng quy mô và tối ưu chi phí cho các bài toán phân tích phức tạp.

### Đa dạng Analytical Workloads (Diverse Analytical Workloads)
Kiến trúc Lakehouse hỗ trợ nhiều kiểu dữ liệu—structured, semi-structured và unstructured—cho phép người dùng chạy cả BI và ML workloads trên cùng một tập dữ liệu mà không cần phải sao chép hay di chuyển dữ liệu tốn kém. Cách tiếp cận thống nhất này cho phép các data scientists và analysts dễ dàng truy cập và thao tác dữ liệu để huấn luyện các mô hình ML, triển khai thuật toán AI và thực hiện phân tích BI chuyên sâu. Bằng cách loại bỏ việc phải tạo và duy trì các bản trích xuất BI (extracts) và cubes riêng biệt, Lakehouse cắt giảm cả chi phí storage lẫn compute, đồng thời duy trì mô hình self-service đơn giản cho người dùng cuối. Nhờ đó, các tổ chức có thể tinh gọn vận hành dữ liệu và nâng cao tính linh hoạt trong phân tích, giúp việc khai thác insight liên miền (cross-domain) trở nên dễ dàng hơn.

### Quản lý Dữ liệu Tối ưu Chi phí (Cost-Effective Data Management)
Lakehouse tận dụng khả năng lưu trữ giá rẻ của Cloud Data Lake đồng thời cung cấp các năng lực quản lý và truy vấn dữ liệu tinh vi tương tự như Data Warehouse. Lợi thế kép này biến nó thành lựa chọn kinh tế cho cả startups lẫn các doanh nghiệp lớn cần tối ưu chi phí mà không phải hy sinh năng lực phân tích. Ngoài ra, kiến trúc mở và thống nhất của Lakehouse còn loại bỏ các chi phí phi tiền tệ, chẳng hạn như công sức vận hành và duy trì các pipeline ETL cồng kềnh cũng như việc phải tạo nhiều bản sao dữ liệu, giúp quy trình vận hành được tinh gọn hơn nữa.

## Ví dụ thực tế

ByteDance, Notion và Halodoc là những ví dụ điển hình về cách kiến trúc Lakehouse được áp dụng trong thực tế ngành công nghiệp:

- **ByteDance** đã xây dựng một Data Lakehouse quy mô exabyte sử dụng Apache Hudi để nâng cao hệ thống gợi ý (recommendation systems). Việc áp dụng các bảng Merge-on-Read (MOR), cơ chế indexing và tính năng Multi-Version Concurrency Control (MVCC) của Hudi cho phép ByteDance cung cấp các năng lực Machine Learning thời gian thực, đưa ra những đề xuất tức thì và phù hợp cho người dùng.
- **Notion** mở rộng hạ tầng dữ liệu bằng cách tự xây dựng một Lakehouse nội bộ để xử lý sự tăng trưởng dữ liệu chóng mặt và đáp ứng các yêu cầu sản phẩm, đặc biệt là Notion AI. Kiến trúc này sử dụng S3 để lưu trữ, Kafka và Debezium để nạp dữ liệu (data ingestion), và Apache Hudi để quản lý dữ liệu hiệu quả. Cấu hình này giúp Notion tiết kiệm đáng kể chi phí, tăng tốc độ nạp dữ liệu và mở rộng năng lực phục vụ phân tích cũng như phát triển sản phẩm.
- **Halodoc** áp dụng kiến trúc Lakehouse để nâng cao chất lượng dịch vụ chăm sóc sức khỏe thông qua việc xử lý và phân tích dữ liệu thời gian thực. Kiến trúc này giúp Halodoc giải quyết các thách thức liên quan đến quản lý khối lượng dữ liệu y tế khổng lồ, từ đó cải thiện việc chăm sóc bệnh nhân nhờ ra quyết định nhanh hơn, chính xác hơn và hỗ trợ cả batch lẫn stream processing phục vụ các can thiệp y tế kịp thời.

## Các công nghệ Data Lakehouse cốt lõi (Key Data Lakehouse Technologies)

### Giải pháp Mã nguồn Mở (Open Source Solutions)

#### Apache Hudi
Apache Hudi là một nền tảng Transactional Data Lakehouse mã nguồn mở được xây dựng xoay quanh một nhân cơ sở dữ liệu (database kernel). Nó cung cấp các lớp trừu tượng cấp bảng (table-level abstractions) trên các open file formats như Apache Parquet và ORC, từ đó mang lại các chức năng cốt lõi của Warehouse và Database trực tiếp vào Data Lake, đồng thời hỗ trợ các khả năng transactional như update và delete.

Hudi cũng tích hợp các dịch vụ bảng (table services) quan trọng gắn chặt với database kernel của nó. Các dịch vụ này có thể chạy tự động, quản lý các khía cạnh như ghi nhận metadata của bảng và sắp xếp storage layouts trên cả dữ liệu nạp vào lẫn dữ liệu phái sinh. Những khả năng này, kết hợp với các dịch vụ nền tảng cụ thể (công cụ ingestion, catalog sync, admin CLI, v.v.) trong Hudi, nâng tầm vai trò của nó từ một table format đơn thuần thành một nền tảng Data Lakehouse toàn diện và mạnh mẽ. Apache Hudi hỗ trợ rộng rãi nhiều nguồn dữ liệu và query engines như Apache Spark, Apache Flink, AWS Athena, Presto, Trino và StarRocks.

**Các tính năng chính của nền tảng Lakehouse Apache Hudi:**
- **Hỗ trợ tính biến đổi (Mutability Support):** Hudi cho phép cập nhật và xóa nhanh chóng thông qua cơ chế indexing linh hoạt, hỗ trợ các workloads như streaming, dữ liệu đến không theo thứ tự (out-of-order data) và khử trùng lặp dữ liệu (data deduplication).
- **Xử lý gia tăng (Incremental Processing):** Hudi tối ưu hóa hiệu quả bằng cách cho phép xử lý gia tăng dữ liệu mới. Tính năng này cho phép bạn thay thế các pipeline xử lý theo lô truyền thống bằng mô hình streaming gia tăng năng động hơn, tăng tốc độ nạp dữ liệu và giảm thời gian xử lý cho các analytical workloads.
- **Giao dịch ACID (ACID Transactions):** Hudi mang lại các đảm bảo giao dịch ACID cho Data Lake, cung cấp khả năng ghi nguyên tử và nhất quán (consistent and atomic writes) cùng với các kỹ thuật kiểm soát đồng thời (concurrency control) khác nhau, rất cần thiết cho việc quản lý các giao dịch chạy dài.
- **Time Travel:** Hudi cung cấp khả năng truy vấn dữ liệu lịch sử, cho phép người dùng rollback về các phiên bản trước của bảng để debug hoặc audit các thay đổi.
- **Quản lý bảng toàn diện (Comprehensive Table Management):** Hudi tích hợp các dịch vụ bảng tự động liên tục điều phối các tác vụ clustering, compaction, cleaning và indexing, đảm bảo hiệu năng cao cho các truy vấn phân tích.
- **Tối ưu hóa hiệu năng truy vấn (Query Performance Optimization):** Hudi giới thiệu một hệ thống con multi-modal indexing mới giúp tăng tốc các giao dịch ghi và nâng cao hiệu năng truy vấn, đặc biệt là trên các bảng có kích thước lớn hoặc nhiều cột (wide tables).
- **Tiến hóa và thực thi Schema (Schema Evolution and Enforcement):** Với Hudi, bạn có thể điều chỉnh schema của bảng khi dữ liệu tiến hóa, nâng cao khả năng phục hồi của pipeline bằng cách nhanh chóng phát hiện và ngăn chặn các vấn đề tiềm ẩn về tính toàn vẹn dữ liệu.

#### Apache Iceberg
Apache Iceberg là một Table Format được thiết kế để quản lý các tập dữ liệu phân tích quy mô lớn trong Cloud Data Lake, thúc đẩy kiến trúc Lakehouse. Về mặt kỹ thuật, Iceberg đóng vai trò như một đặc tả định dạng bảng (table format specification), cung cấp các API và thư viện cho phép các compute engines tương tác với các bảng theo đặc tả này. Nó giới thiệu các tính năng thiết yếu cho các Data Lake workloads, bao gồm schema evolution, hidden partitioning, các giao dịch tuân thủ ACID và khả năng time-travel. Những tính năng này đảm bảo việc quản lý dữ liệu mạnh mẽ tương tự như trong các Data Warehouse truyền thống.

#### Delta Lake
Delta Lake là một Open Source Table Format khác cho phép xây dựng kiến trúc Lakehouse trên nền tảng Cloud Data Lake. Bằng cách cung cấp một tầng tuân thủ ACID hoạt động trên các Cloud Object Stores, Delta Lake giải quyết các vấn đề điển hình về hiệu năng và tính nhất quán thường gặp ở Data Lake. Nó mang lại các tính năng như schema enforcement và evolution, time-travel, xử lý metadata hiệu quả và các thao tác DML, đóng vai trò sống còn để xử lý hiệu quả các workloads quy mô lớn trên Data Lake.

### Các nền tảng Lakehouse thương mại (Vendor Lakehouse Platforms)

#### Onehouse
Onehouse cung cấp một nền tảng dữ liệu toàn cầu giúp tinh giản việc nạp và chuyển đổi dữ liệu vào kiến trúc Lakehouse. Nó xóa bỏ rào cản giữa các Lakehouse table formats bằng cách hoạt động mượt mà với cả Apache Hudi, Apache Iceberg và Delta Lake (nhờ vào công nghệ Apache XTable). Nền tảng hỗ trợ nạp dữ liệu liên tục từ nhiều nguồn đa dạng, bao gồm event streams như Kafka, các cơ sở dữ liệu và Cloud Storage, cho phép cập nhật dữ liệu thời gian thực trong khi vẫn đảm bảo tính toàn vẹn dữ liệu thông qua các tối ưu hóa bảng tự động và các biện pháp kiểm soát chất lượng dữ liệu nghiêm ngặt. Onehouse cung cấp một ingestion pipeline được quản lý hoàn toàn (fully managed) với khả năng tự động co giãn serverless và hạ tầng tối ưu chi phí.

#### Databricks
Databricks Lakehouse Platform hợp nhất data engineering, machine learning và analytics trên một nền tảng duy nhất. Nó kết hợp độ tin cậy, khả năng quản trị và hiệu năng của Data Warehouse với khả năng mở rộng, tính linh hoạt và chi phí thấp của Data Lake. Bằng cách sử dụng Delta Lake làm tầng lưu trữ nền tảng và UniFormat để tương thích đa chiều giữa Apache Iceberg và Apache Hudi, nền tảng này hỗ trợ các giao dịch ACID, xử lý metadata ở quy mô lớn, đồng thời hợp nhất việc xử lý dữ liệu batch và streaming.

#### Snowflake
Snowflake Data Cloud cung cấp một nền tảng thống nhất, được quản lý toàn diện cho việc quản lý dữ liệu liền mạch và các năng lực phân tích nâng cao. Nó mang lại khả năng mở rộng gần như vô hạn, bảo mật mạnh mẽ và hỗ trợ gốc cho nhiều loại dữ liệu cũng như SQL workloads. Snowflake hiện hỗ trợ Apache Iceberg làm open table format để hiện thực hóa kiến trúc Lakehouse. Sự tích hợp này cho phép người dùng tận dụng metadata phong phú của Iceberg và việc lưu trữ tệp Parquet ngay trong hệ sinh thái Snowflake, hỗ trợ xử lý dữ liệu mượt mà, multi-table transactions, dynamic data masking và Row-Level Security, tất cả đều được vận hành trên hạ tầng Cloud Storage do chính khách hàng cung cấp.

## Tương lai của Data Lakehouse

Tương lai của Data Lakehouse được định hình bởi kiến trúc dữ liệu thực sự mở (truly open data architecture), đáp ứng nhu cầu liên tục về các giải pháp quản lý dữ liệu linh hoạt, có thể mở rộng quy mô và tối ưu chi phí. Lakehouse mang lại một nền tảng thống nhất có khả năng xử lý hiệu quả cả streaming và batch workloads, hỗ trợ dải rộng các khối lượng công việc phân tích bao gồm BI và ML. Với sự phát triển thần tốc của trí tuệ nhân tạo, đặc biệt là Generative AI, nhu cầu về các nền tảng vững chắc để xây dựng và triển khai các mô hình AI mạnh mẽ ngày càng tăng cao. Kiến trúc Lakehouse vươn lên đáp ứng thách thức này, cung cấp nền móng vững chắc cho các nhu cầu tiến hóa không ngừng của phân tích dữ liệu hiện đại.

## Kết luận

Kiến trúc Data Lakehouse tận dụng nền móng dữ liệu mở để hòa trộn các tính năng tốt nhất của Data Lake và Data Warehouse, tạo lập một nền tảng linh hoạt có thể xử lý hiệu quả nhiều loại analytical workloads. Kiến trúc này kết hợp việc quản lý dữ liệu tối ưu chi phí với hiệu năng mạnh mẽ, mang lại một hệ thống gắn kết cho cả hai quy trình xử lý dữ liệu batch và streaming. Bằng cách cho phép các tổ chức làm việc trên một kho lưu trữ dữ liệu duy nhất, hướng tiếp cận này không chỉ đơn giản hóa công tác quản lý mà còn trang bị cho doanh nghiệp khả năng nhanh chóng tích hợp các công nghệ mới và thích ứng với nhu cầu thị trường luôn biến đổi. Ngoài ra, việc hỗ trợ đa dạng kiểu dữ liệu và analytical workloads giúp khung kiến trúc Lakehouse xóa bỏ sự cần thiết của mô hình hai tầng (two-tier architecture), giúp tiết kiệm chi phí và nâng cao năng suất của các data teams.

## FAQ (Các câu hỏi thường gặp)

### Data Lakehouse là gì?
Data Lakehouse là một kiến trúc dữ liệu lai kết hợp các năng lực giao dịch (transactional) của Data Warehouse với khả năng mở rộng quy mô và chi phí thấp của Cloud Data Lake. Nó lưu trữ dữ liệu ở các định dạng mở trên Cloud Object Storage và bổ sung thêm tầng Table Format cung cấp các giao dịch ACID, quản lý schema và tính năng time-travel. Điều này cho phép doanh nghiệp chạy các khối lượng công việc BI, Machine Learning và streaming trên một kho lưu trữ dữ liệu duy nhất.

### Data Lakehouse khác gì so với Data Warehouse hoặc Data Lake?
Data Warehouse cung cấp các đảm bảo giao dịch mạnh mẽ nhưng dựa trên các định dạng lưu trữ độc quyền có nguy cơ bị khóa chặt vào nhà cung cấp và xử lý kém với dữ liệu phi cấu trúc. Data Lake lưu trữ mọi loại dữ liệu với chi phí rẻ ở các định dạng file mở nhưng thiếu các giao dịch ACID và thường gặp khó khăn về chất lượng dữ liệu cũng như quản trị. Lakehouse hợp nhất cả hai, mang lại độ tin cậy và hiệu năng chuẩn Warehouse cho tầng lưu trữ Data Lake mở có thể truy cập tự do.

### Các thành phần chính của kiến trúc Data Lakehouse là gì?
Một Lakehouse bao gồm 6 thành phần mô-đun: Lake Storage (các Cloud Object Stores như Amazon S3), Open File Formats (Apache Parquet hoặc ORC), Table Format (Apache Hudi, Apache Iceberg hoặc Delta Lake), Storage Engine xử lý clustering, compaction, cleaning và indexing, Catalog theo dõi các bảng và metadata của chúng, và Compute Engines như Spark, Flink, Presto hoặc Trino để đọc và ghi dữ liệu.

### Table Format đóng vai trò gì trong một Lakehouse?
Table Format là một tầng metadata mở nằm phía trên các file formats như Apache Parquet nhằm định nghĩa một schema trên các immutable data files. Nó cho phép các engine khác nhau đọc và ghi đồng thời trên cùng một tập dữ liệu với các giao dịch ACID, đồng thời mang lại các tính năng như schema evolution, partitioning và time-travel.

### Tại sao Apache Hudi được xem là nhiều hơn một Table Format đơn thuần?
Apache Hudi là một nền tảng Transactional Data Lakehouse được xây dựng xoay quanh một nhân cơ sở dữ liệu (database kernel), chứ không chỉ là một đặc tả metadata đơn thuần. Vượt ra ngoài các lớp trừu tượng cấp bảng, nó bao gồm các table services được tích hợp chặt chẽ để tự động hóa clustering, compaction, cleaning và indexing, cùng với các dịch vụ nền tảng như công cụ ingestion, catalog sync và admin CLI. Nó cũng hỗ trợ cập nhật và xóa nhanh chóng thông qua cơ chế indexing linh hoạt và cho phép xử lý gia tăng (incremental processing) dữ liệu mới.
