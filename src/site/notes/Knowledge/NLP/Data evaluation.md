---
{"dg-publish":true,"permalink":"/knowledge/nlp/data-evaluation/","title":"Data evaluation","pinned":"false","tags":["#nlp","AI"]}
---


# Data ground truth 
## Link
- [BeIR/scifact](https://huggingface.co/datasets/BeIR/scifact)
- [BeIR/scifact-qrels](https://huggingface.co/datasets/BeIR/scifact-qrels)
- [code Hybrid search](https://github.com/qdrant/workshop-ultimate-hybrid-search/blob/main/notebooks/02-hybrid-search.ipynb)

- https://huggingface.co/datasets/hiieu/legal_eval/viewer/default/corpus
- https://huggingface.co/datasets/hiieu/legal_eval_label
- [Code enhance retrieval](https://github.com/thangnch/MiAI_HieuNgo_EmbedingFineTune/blob/main/TextEmbeddingMiAI_DEMO.ipynb)

## Review 
- ![](/img/user/assets/images/codeCorpus_question.png)
- ![](/img/user/assets/images/corpus.png)
- ![](/img/user/assets/images/query.png)
- ![](/img/user/assets/images/tableEvaluation.png)

Ở đây là Anh hiếu dùng 2 repo data hugging face. 
- repo1 dùng để lưu lại kết quả chunking (id_corpus, corpus text). Và ở đây còn tạo ra 1 data từ id question và câu hỏi.
- Repo2 lưu lại question id tương ứng với corpus_id: đây là bộ data test khả năng retrieval. 
	- có thể 1 question với 2 hoặc nhiều corpus id. 

## Note 
- Trong bài blog của anh hiếu thì mục đích là nâng cao khả năng retrieval nên dùng bộ data này để fine tune model embedding để khả năng tìm kiếm vector của nó tốt hơn. 
	- Trên đó ảnh cũng đã có kết quả thì ảnh qua đoạn code thì ảnh cho thấy kết quả retrieval nó tốt hơn khi được finetune.  [Code enhance retrieval](https://github.com/thangnch/MiAI_HieuNgo_EmbedingFineTune/blob/main/TextEmbeddingMiAI_DEMO.ipynb)
- Nhưng trong bài tham khảo của **Qdrant** và của repo tên **BeIR** ảnh dùng bộ data kiểu anh hiếu để đánh giá khả năng retrieval. Họ dùng các metric để đánh giá : **precission, recall, mrr, dcg, ndcg**
	- ![](/img/user/assets/images/code_GroundTruth1.png)
	- ![](/img/user/assets/images/code_GroundTruth2.png)
	- ![](/img/user/assets/images/code_GroundTruth3.png)

## Evaluation Metrics for Information Retrieval

## 1. Precision@K

Precision@K đo lường tỷ lệ số lượng kết quả đúng trong K kết quả đầu tiên.
### Công thức:

$$
Precision@K = \frac{\text{Relevant Documents in Top K}}{K}
$$
### Ví dụ:

Giả sử hệ thống trả về 5 tài liệu đầu tiên với độ liên quan như sau:

| Rank | Document ID | Relevant (1=Yes, 0=No) |
| ---- | ----------- | ---------------------- |
| 1    | D1          | 1                      |
| 2    | D2          | 0                      |
| 3    | D3          | 1                      |
| 4    | D4          | 1                      |
| 5    | D5          | 0                      |

Tính Precision@5:
$$
Precision@5 = \frac{3}{5} = 0.6
$$
---
## 2. Recall@K

Recall@K đo lường tỷ lệ tài liệu liên quan được tìm thấy trong K kết quả đầu tiên so với tổng số tài liệu liên quan có trong tập dữ liệu.

### Công thức:

$$
Recall@K = \frac{\text{Relevant Documents in Top K}}{\text{Total Relevant Documents}}
$$

### Ví dụ:

Giả sử có tổng cộng 10 tài liệu liên quan trong toàn bộ tập dữ liệu, trong đó chỉ 3 tài liệu xuất hiện trong 5 kết quả đầu tiên:

$$
Recall@5 = \frac{3}{10} = 0.3
$$

---

## 3. Mean Reciprocal Rank (MRR)

MRR đo lường vị trí của kết quả liên quan đầu tiên trong danh sách kết quả.

### Công thức:

$$
MRR = \frac{1}{N} \sum_{i=1}^{N} \frac{1}{rank_i}
$$

Với \(rank_i\) là vị trí của kết quả liên quan đầu tiên cho truy vấn thứ \(i\).

### Ví dụ:

Giả sử ba truy vấn có kết quả liên quan đầu tiên xuất hiện ở vị trí 1, 3 và 2:

$$
MRR = \frac{1}{3} \left( \frac{1}{1} + \frac{1}{3} + \frac{1}{2} \right) = \frac{1}{3} (1 + 0.333 + 0.5) = 0.611
$$

---

## 4. Discounted Cumulative Gain (DCG@K)

DCG@K đánh giá chất lượng của danh sách kết quả bằng cách giảm trọng số của các kết quả có vị trí thấp hơn.
### Công thức:

$$
DCG@K = \sum_{i=1}^{K} \frac{rel_i}{\log_2(i+1)}
$$

Với \(rel_i\) là mức độ liên quan của kết quả tại vị trí \(i\).

### Ví dụ:

Giả sử danh sách kết quả có mức độ liên quan \([3, 2, 3, 0, 1]\):

$$
DCG@5 = \frac{3}{\log_2(2)} + \frac{2}{\log_2(3)} + \frac{3}{\log_2(4)} + \frac{0}{\log_2(5)} + \frac{1}{\log_2(6)}
$$

Bảng tính toán chi tiết:

| Rank | Relevance | $$(\log_2(rank+1))$$ | Contribution |
| ---- | --------- | -------------------- | ------------ |
| 1    | 3         | 1.0                  | 3.000        |
| 2    | 2         | 1.585                | 1.262        |
| 3    | 3         | 2.0                  | 1.500        |
| 4    | 0         | 2.322                | 0.000        |
| 5    | 1         | 2.585                | 0.387        |

Tổng cộng:
$$
DCG@5 = 3 + 1.262 + 1.500 + 0 + 0.387 = 6.149
$$
---
## 5. Normalized DCG (NDCG@K)

NDCG@K chuẩn hóa DCG bằng cách chia cho giá trị DCG tối đa có thể đạt được (IDCG).
### Công thức:
$$
NDCG@K = \frac{DCG@K}{IDCG@K}
$$
Với \(IDCG@K\) là giá trị DCG nếu kết quả được sắp xếp theo mức độ liên quan giảm dần.
### Ví dụ:
Giả sử danh sách kết quả tối ưu có mức độ liên quan \([3, 3, 2, 1, 0]\), ta tính IDCG@5:

$$
IDCG@5 = \frac{3}{\log_2(2)} + \frac{3}{\log_2(3)} + \frac{2}{\log_2(4)} + \frac{1}{\log_2(5)} + \frac{0}{\log_2(6)}
$$
Bảng tính toán:

| Rank | Relevance | $$(\log_2(rank+1))$$ | Contribution |
| ---- | --------- | -------------------- | ------------ |
| 1    | 3         | 1.0                  | 3.000        |
| 2    | 3         | 1.585                | 1.892        |
| 3    | 2         | 2.0                  | 1.000        |
| 4    | 1         | 2.322                | 0.431        |
| 5    | 0         | 2.585                | 0.000        |

Tổng cộng:
$$
IDCG@5 = 3 + 1.892 + 1.000 + 0.431 + 0 = 6.323
$$

Tính NDCG@5:
$$
NDCG@5 = \frac{6.149}{6.323} = 0.973
$$
# Đánh giá RAG với Evaluate Ragas

## 1. Giới thiệu về Evaluate Ragas
Evaluate Ragas là một thư viện hỗ trợ đánh giá các hệ thống Retrieval-Augmented Generation (RAG). Nó cung cấp các tiêu chí đánh giá chất lượng của truy xuất thông tin và sinh văn bản, giúp tối ưu hóa mô hình RAG.
## 2. Các tiêu chí đánh giá
Ragas đánh giá hệ thống RAG dựa trên các tiêu chí chính:
- **Context Precision**: Đánh giá mức độ liên quan của ngữ cảnh được truy xuất.
- **Faithfulness**: Đánh giá mức độ chính xác của câu trả lời dựa trên ngữ cảnh.
- **Answer Relevance**: Đánh giá mức độ liên quan của câu trả lời với câu hỏi.
- **Context Recall**: Đánh giá mức độ đầy đủ của ngữ cảnh được truy xuất.
## 3. Công thức tính điểm
### 3.1. Context Precision
$$
\text{Precision} = \frac{|\text{Context relevant} \cap \text{Retrieved context}|}{|\text{Retrieved context}|}
$$
Giải thích: Tính tỷ lệ tài liệu truy xuất thực sự liên quan so với tổng số tài liệu truy xuất.
### 3.2. Faithfulness
$$
\text{Faithfulness} = \frac{\text{Factually correct statements}}{\text{Total statements in answer}}
$$
Giải thích: Kiểm tra câu trả lời có dựa trên bằng chứng trong ngữ cảnh không.
### 3.3. Answer Relevance
$$
\text{Relevance} = \text{Similarity}(\text{Answer}, \text{Question})
$$
Giải thích: Tính toán mức độ liên quan giữa câu trả lời và câu hỏi bằng cosine similarity.
### 3.4. Context Recall
$$
\text{Recall} = \frac{|\text{Context relevant} \cap \text{Retrieved context}|}{|\text{Relevant context}|}
$$
Giải thích: Đo lường mức độ đầy đủ của ngữ cảnh truy xuất.

## 4. Dataset đầu vào
Dataset thường có 4 cột chính:

| question      | answer  | contexts  | ground_truth  |
|--------------|---------|-----------|---------------|
| Câu hỏi của người dùng | Câu trả lời từ mô hình RAG | Ngữ cảnh được truy xuất | Câu trả lời đúng theo dữ liệu gốc |

### Ý nghĩa:
- `question`: Câu hỏi đầu vào cần trả lời.
- `answer`: Câu trả lời do mô hình sinh ra.
- `contexts`: Các đoạn văn bản được truy xuất để hỗ trợ trả lời.
- `ground_truth`: Câu trả lời đúng dựa trên dữ liệu có sẵn.

## 5. Cách tạo dataset
Có thể tạo dataset bằng cách:
1. **Thu thập câu hỏi và câu trả lời gốc**: Sử dụng dữ liệu từ nguồn đáng tin cậy.
2. **Thêm ngữ cảnh truy xuất**: Tạo hoặc sử dụng mô hình RAG để lấy các đoạn văn bản phù hợp.
3. **Tạo ground truth**: Gán câu trả lời đúng để so sánh.
4. **Lưu dưới dạng bảng dữ liệu**: Sử dụng pandas để lưu dữ liệu dưới dạng CSV hoặc JSON.


