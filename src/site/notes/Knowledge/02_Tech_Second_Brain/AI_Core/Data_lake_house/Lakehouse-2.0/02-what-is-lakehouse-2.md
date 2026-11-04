---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/data-lake-house/lakehouse-2-0/02-what-is-lakehouse-2/","title":"Lakehouse 2.0 là gì","pinned":"false","tags":["type/course","topic/data-lakehouse","status/stable"]}
---

## Lakehouse 2.0 là gì?

**Lakehouse 2.0** không phải một chuẩn chính thức duy nhất, mà là cách tiến hóa từ:

- **Data warehouse**: dữ liệu có cấu trúc, query tốt nhưng đắt và kém linh hoạt.
- **Data lake**: rẻ, linh hoạt nhưng thiếu transaction, schema, governance.

Lakehouse 2.0 kết hợp ưu điểm của cả hai:

```text
Object Storage
  └── Parquet / Iceberg / Delta / Hudi
          ↑
   Table format + Catalog
          ↑
 Spark / Trino / Flink / dbt / DuckDB
          ↑
 BI / ML / Semantic layer / Governance
```

Các đặc điểm chính:

1. **Storage và compute tách rời**  
   Dữ liệu nằm trên S3/MinIO; Spark, Trino hoặc engine khác đọc độc lập.

2. **Open table format**  
   Apache Iceberg/Delta/Hudi cung cấp:
   - ACID transaction
   - schema evolution
   - time travel
   - partition evolution
   - snapshot/versioning

3. **Multi-engine**  
   Cùng một bảng có thể được đọc/ghi bởi Spark, Trino, dbt, Flink, DuckDB…

4. **Medallion architecture**

   ```text
   Bronze → Silver → Gold
   raw       cleaned    business-ready
   ```

5. **ELT và governance hiện đại**  
   dbt để transform, Dagster/Airflow để orchestration, DataHub để catalog/lineage, BI/semantic layer để phục vụ người dùng.

6. **Hỗ trợ batch và streaming**  
   Lakehouse 2.0 thường hướng tới một nền tảng thống nhất cho cả dữ liệu batch và streaming.

---

## Repo này đã áp dụng chưa?

### Kết luận ngắn

**Có. Repo đã áp dụng phần lớn nền tảng Lakehouse 2.0, đặc biệt là phần storage, table format, multi-engine và medallion.**

Tuy nhiên, đây vẫn là **lab/demo hoặc reference architecture**, chưa phải nền tảng production hoàn chỉnh.

---

## 1. `local-lakehouse` — đã có nền tảng Lakehouse 2.0

Trong `local-lakehouse README`, repo tích hợp:

- Apache Spark
- Apache Iceberg REST Catalog
- MinIO làm object storage tương tự S3
- dbt
- Trino

Cấu hình trong `docker-compose.yml` thể hiện rõ:

```text
MinIO → lưu file dữ liệu
Iceberg REST Catalog → quản lý metadata/table
Spark → xử lý dữ liệu
Trino → query SQL
dbt → transformation
```

Trino sử dụng Iceberg catalog và đọc dữ liệu từ MinIO trong `iceberg.properties`.

dbt cũng cấu hình model sử dụng Iceberg trong `dbt_project.yml`:

```yaml
+materialized: table
+file_format: iceberg
```

### Đánh giá

| Thành phần Lakehouse 2.0 | Trạng thái |
|---|---|
| Object storage | Có — MinIO |
| Open table format | Có — Iceberg |
| Catalog | Có — Iceberg REST Catalog |
| Tách storage/compute | Có |
| Multi-engine | Có — Spark và Trino |
| ELT | Có — dbt |
| Batch processing | Có |
| Streaming | Chưa có |
| Orchestration | Chưa có |
| Data governance/lineage | Chưa có đầy đủ |
| BI/semantic layer | Chưa có |

**Kết luận:** `local-lakehouse` là một **Lakehouse 2.0 learning stack**, tập trung vào Iceberg và kiến trúc storage/compute tách rời.

---

## 2. `ngods-stocks` — áp dụng rộng và đầy đủ hơn

`ngods-stocks` gần với một **modern open-source data stack trên nền Lakehouse**.

README mô tả các thành phần trong `ngods-stocks README`:

- Spark để xử lý dữ liệu
- Iceberg làm storage/table format
- Trino để query
- dbt để transform
- Dagster để orchestration
- DataHub để metadata/lineage
- Cube để semantic layer
- Metabase để visualization
- MinIO làm S3-compatible object storage

Pipeline có dạng:

```text
Yahoo Finance
    ↓
Bronze Iceberg
    ↓
Silver Iceberg
    ↓
Gold analytics
    ↓
Trino / Cube / Metabase
```

Các project dbt được tách thành:

- `bronze dbt project`
- `silver dbt project`
- `gold dbt project`

Bronze được cấu hình sử dụng Iceberg:

```yaml
+materialized: table
+schema: bronze
+file_format: iceberg
```

Trino cũng dùng Iceberg catalog, với:

- PostgreSQL làm catalog metadata
- MinIO làm warehouse
- Parquet làm file format

Cấu hình nằm trong `warehouse.properties`.

### Đánh giá

| Thành phần Lakehouse 2.0 | Trạng thái |
|---|---|
| Object storage | Có — MinIO |
| Open table format | Có — Iceberg |
| Catalog | Có — JDBC catalog |
| Tách storage/compute | Có |
| Multi-engine | Có — Spark, Trino |
| Medallion architecture | Có — Bronze/Silver/Gold |
| ELT | Có — dbt |
| Orchestration | Có — Dagster |
| Metadata/lineage | Có — DataHub |
| Semantic layer | Có — Cube |
| BI | Có — Metabase |
| Batch processing | Có |
| Streaming | Chưa thấy Kafka/Flink/streaming pipeline |
| Production security/governance | Còn hạn chế |

**Kết luận:** `ngods-stocks` đã áp dụng khá đầy đủ một kiến trúc **Lakehouse 2.0 dạng open-source**, nhưng chủ yếu là batch analytics demo.

---

## Những phần chưa hoàn chỉnh

Repo chưa thể xem là một Lakehouse 2.0 production-ready vì còn một số điểm:

- Chưa có streaming ingestion rõ ràng như Kafka/Flink.
- MinIO đang chạy local, chưa phải object storage production.
- Credentials đang hard-code trong Docker Compose/config.
- Chưa thấy data quality framework chuyên biệt.
- Chưa có cơ chế CI/CD và schema migration hoàn chỉnh.
- Governance, access control và audit còn đơn giản.
- Một số cấu hình còn mang tính demo, ví dụ Silver dùng cấu hình `ORC`/`ICEBERG` trong `silver dbt_project.yml`.

## Tóm tắt

- `local-lakehouse`: **Lakehouse 2.0 core**, phù hợp học Spark + Iceberg + MinIO + Trino.
- `ngods-stocks`: **Lakehouse 2.0 mở rộng**, có thêm dbt, Dagster, DataHub, Cube và Metabase.
- Cả hai **đã áp dụng tư duy Lakehouse 2.0**, nhưng `ngods-stocks` đầy đủ hơn.
- Chúng vẫn là **demo/reference stack**, chưa phải production lakehouse hoàn chỉnh.