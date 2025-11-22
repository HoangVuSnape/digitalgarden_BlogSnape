---
{"dg-publish":true,"permalink":"/knowledge/1-xai-kg/knowledge-graph/","title":"Knowledge Graph","pinned":"false"}
---

2# References
- [# GraphRAG - Một sự nâng cấp mới của RAG truyền thống chăng?](https://viblo.asia/p/graphrag-mot-su-nang-cap-moi-cua-rag-truyen-thong-chang-EoW4oXRBJml)
- [Graph RAG 1 -Youtube](https://www.youtube.com/watch?v=NJJWAgEUtL8&t=3230s) : Có phần 2 nữa. 
- [Blog graph rag](https://gaodalie.substack.com/p/local-graphrag-langchain-gpt4o-easy)
- [Qdrant - Neoj4](https://qdrant.tech/documentation/examples/graphrag-qdrant-neo4j/)
# Questions
- Mình  chưa biết cách nó tổ chức và xây dựng như thế nào
	- Cần survey thêm nha


---
# GraphRAG: Graph-Enhanced Retrieval-Augmented Generation

## Overview

GraphRAG is an advanced question-answering system that combines the power of graph-based knowledge representation with retrieval-augmented generation. It processes input documents to create a rich knowledge graph, which is then used to enhance the retrieval and generation of answers to user queries. The system leverages natural language processing, machine learning, and graph theory to provide more accurate and contextually relevant responses.

## Motivation

Traditional retrieval-augmented generation systems often struggle with maintaining context over long documents and making connections between related pieces of information. GraphRAG addresses these limitations by:

1. Representing knowledge as an interconnected graph, allowing for better preservation of relationships between concepts.
2. Enabling more intelligent traversal of information during the query process.
3. Providing a visual representation of how information is connected and accessed during the answering process.

## Key Components

1. **DocumentProcessor**: Handles the initial processing of input documents, creating text chunks and embeddings.
    
2. **KnowledgeGraph**: Constructs a graph representation of the processed documents, where nodes represent text chunks and edges represent relationships between them.
    
3. **QueryEngine**: Manages the process of answering user queries by leveraging the knowledge graph and vector store.
    
4. **Visualizer**: Creates a visual representation of the graph and the traversal path taken to answer a query.
    

## Method Details

1. **Document Processing**:
    
    - Input documents are split into manageable chunks.
    - Each chunk is embedded using a language model.
    - A vector store is created from these embeddings for efficient similarity search.
2. **Knowledge Graph Construction**:
    
    - Graph nodes are created for each text chunk.
    - Concepts are extracted from each chunk using a combination of NLP techniques and language models.
    - Extracted concepts are lemmatized to improve matching.
    - Edges are added between nodes based on semantic similarity and shared concepts.
    - Edge weights are calculated to represent the strength of relationships.
    - ![](/img/user/assets/images/KG_1.png)
3. **Query Processing**:
    
    - The user query is embedded and used to retrieve relevant documents from the vector store.
    - A priority queue is initialized with the nodes corresponding to the most relevant documents.
    - The system employs a Dijkstra-like algorithm to traverse the knowledge graph:
        - Nodes are explored in order of their priority (strength of connection to the query).
        - For each explored node:
            - Its content is added to the context.
            - The system checks if the current context provides a complete answer.
            - If the answer is incomplete:
                - The node's concepts are processed and added to a set of visited concepts.
                - Neighboring nodes are explored, with their priorities updated based on edge weights.
                - Nodes are added to the priority queue if a stronger connection is found.
    - This process continues until a complete answer is found or the priority queue is exhausted.
    - If no complete answer is found after traversing the graph, the system generates a final answer using the accumulated context and a large language model.
4. **Visualization**:
    
    - The knowledge graph is visualized with nodes representing text chunks and edges representing relationships.
    - Edge colors indicate the strength of relationships (weights).
    - The traversal path taken to answer a query is highlighted with curved, dashed arrows.
    - Start and end nodes of the traversal are distinctly colored for easy identification.

## Benefits of This Approach

1. **Improved Context Awareness**: By representing knowledge as a graph, the system can maintain better context and make connections across different parts of the input documents.
    
2. **Enhanced Retrieval**: The graph structure allows for more intelligent retrieval of information, going beyond simple keyword matching.
    
3. **Explainable Results**: The visualization of the graph and traversal path provides insight into how the system arrived at its answer, improving transparency and trust.
    
4. **Flexible Knowledge Representation**: The graph structure can easily incorporate new information and relationships as they become available.
    
5. **Efficient Information Traversal**: The weighted edges in the graph allow the system to prioritize the most relevant information pathways when answering queries.
    

## Conclusion

GraphRAG represents a significant advancement in retrieval-augmented generation systems. By incorporating a graph-based knowledge representation and intelligent traversal mechanisms, it offers improved context awareness, more accurate retrieval, and enhanced explainability. The system's ability to visualize its decision-making process provides valuable insights into its operation, making it a powerful tool for both end-users and developers. As natural language processing and graph-based AI continue to evolve, systems like GraphRAG pave the way for more sophisticated and capable question-answering technologies.

---

![](../../assets/images/Knowledge/📂%2001_Projects/KLTN/Report/IMG-20251122205501486.svg)

---
### Luồng hoạt động chính

Hệ thống được chia thành các thành phần sau:

1. **DocumentProcessor**:
    
    - Xử lý tài liệu đầu vào:
        - Chia nhỏ tài liệu thành các đoạn (chunks).
        - Tạo embeddings cho các đoạn này bằng mô hình nhúng (embedding model).
        - Tạo một kho lưu trữ vector để tìm kiếm tương tự.
2. **KnowledgeGraph**:
    
    - Xây dựng đồ thị tri thức từ các đoạn văn bản:
        - Các đoạn văn bản là các nút của đồ thị.
        - Các mối quan hệ giữa các nút được biểu diễn bằng các cạnh, dựa trên độ tương đồng ngữ nghĩa và khái niệm chung.
        - Các cạnh được gán trọng số dựa trên mức độ quan hệ.
3. **QueryEngine**:
    
    - Xử lý câu hỏi người dùng:
        - Tìm kiếm các tài liệu liên quan nhất từ kho vector.
        - Sử dụng thuật toán tương tự Dijkstra để duyệt qua đồ thị tri thức.
        - Kết hợp thông tin từ các nút được duyệt để tạo câu trả lời.
        - Nếu không tìm được câu trả lời đầy đủ, LLM sẽ được sử dụng để tạo câu trả lời cuối cùng.
4. **Visualizer**:
    
    - Trực quan hóa đồ thị và đường đi:
        - Đồ thị được hiển thị với các nút và cạnh.
        - Đường đi qua các nút để trả lời câu hỏi được đánh dấu.
5. **GraphRAG Class**:
    
    - Là lớp chính quản lý toàn bộ luồng hoạt động:
        - Tích hợp các thành phần trên.
        - Xử lý tài liệu và câu hỏi một cách liền mạch.

### Các bước chạy cơ bản

1. **Tải dữ liệu**:
    
    - Tài liệu như file PDF được tải vào hệ thống.
    - Các đoạn văn bản được xử lý thành các chunk.
2. **Xây dựng đồ thị tri thức**:
    
    - Các mối quan hệ giữa nội dung được trích xuất và biểu diễn dưới dạng đồ thị.
3. **Truy vấn thông tin**:
    
    - Khi người dùng đặt câu hỏi, hệ thống sẽ tìm kiếm thông tin trong đồ thị tri thức.
    - Sử dụng các thuật toán để duyệt qua đồ thị và tạo câu trả lời.
4. **Hiển thị kết quả**:
    
    - Đồ thị tri thức và các đường đi có thể được trực quan hóa.
---

# Thực hiện

## Prompt
![](/img/user/assets/images/KG1.png)
![](/img/user/assets/images/KG2.png)
```python
def CreatePrompt(text: str) -> str:

  

    prompt = f"""

        # Nhiệm vụ: Trích xuất từ khóa nội dung (topics) và các thẻ (tags) từ đoạn văn về trường đại học.

  

        ## Mục tiêu:

        Cho một đoạn văn bất kỳ, hãy trích xuất:

        1. topics: danh sách từ khóa có trong nội dung văn bản(lấy từ văn bản). các từ khóa này có thể là tên trường, tên ngành, chuyên ngành,...

        2. tags: các nhãn cụ thể được chọn từ danh sách tag chuẩn bên dưới.

        ## Định dạng đầu ra:

  

        topics: [từ khóa 1, từ khóa 2, ...]  

        tags: [tag 1, tag 2, ...]

        Ví dụ:

        topics: [Đại học ABC, Trí tuệ nhân tạo, An ninh mạng, thời gian đạo tạo, điều kiện xét tuyển]

        tags: [Ngành đào tạo, Tên ngành, Chuyên ngành, Thời gian đào tạo, Điều kiện tuyển sinh, Xét điểm thi, Xét học bạ, Đại học ABC]

  

        Chú ý:

        - Không có hiện json ở đầu ra.

        - số lượng từ khóa trong topics không quá 12 từ khóa.

        - số lượng tag trong tags không quá 12 tags.

        - không có dấu câu ở cuối danh sách từ khóa và danh sách tag.

        - không có từ khóa nào lặp lại trong danh sách từ khóa và danh sách tag.

        -------

        ## Danh sách tags chuẩn:

  

        - Thông tin trường: Tên trường, Địa chỉ, Sứ mệnh, Mục tiêu, Website, Thông tin liên hệ, Số điện thoại, Email  

        - Ngành đào tạo, Tên ngành, Chuyên ngành, Mô tả ngành học, Thời gian đào tạo, Điều kiện tuyển sinh  

        - Chỉ tiêu tuyển sinh, Ngành học, Số lượng chỉ tiêu, Hình thức xét tuyển, Xét điểm thi, Xét học bạ, Thi đánh giá năng lực, Chứng chỉ quốc tế  

        - Điều kiện nhập học, Yêu cầu đầu vào, Học phí, Chính sách học bổng, Hỗ trợ tài chính  

        - Lịch tuyển sinh, Thời gian nhận hồ sơ, Thời gian thi, Thời gian công bố kết quả, Thời gian nhập học, Lịch thi riêng  

        - Chính sách hỗ trợ học tập, Chính sách việc làm, Hỗ trợ sinh viên, Vay vốn học tập, Hợp tác doanh nghiệp, học bổng, thực tập, việc làm

        - Cơ hội nghề nghiệp, Môi trường làm việc, Đối tác doanh nghiệp, Thực tập sinh, Việc làm sau tốt nghiệp  

        - Cơ sở vật chất, Thư viện, Phòng thí nghiệm, Ký túc xá, Sân thể thao, Cơ sở chính, Cơ sở vệ tinh, Câu lạc bộ, Hoạt động ngoại khóa  

        - Chương trình quốc tế, Du học, Liên kết đào tạo quốc tế, Thạc sĩ, Tiến sĩ  

        - Ngày hội tuyển sinh, Hội thảo hướng nghiệp, Webinar, Livestream tuyển sinh

  

        ---

  

        ## Ví dụ:

  

        ### Input 1:

        Đại học ABC có các ngành đào tạo đa dạng, trong đó nổi bật là ngành Công nghệ thông tin với nhiều chuyên ngành như Trí tuệ nhân tạo và An ninh mạng. Thời gian đào tạo 4 năm. Điều kiện xét tuyển gồm điểm thi tốt nghiệp THPT và học bạ.

  

        ### Kết quả:

        topics: [Đại học ABC, Trí tuệ nhân tạo, An ninh mạng, thời gian đạo tạo, điều kiện xét tuyển]  

        tags: [Ngành đào tạo, Tên ngành, Chuyên ngành, Thời gian đào tạo, Điều kiện tuyển sinh, Xét điểm thi, Xét học bạ, Đại học ABC]

  

        ---

  

        ### Input 2:

        Trường Đại học XYZ tọa lạc tại Quận 3, TP.HCM. Trường có sứ mệnh đào tạo nguồn nhân lực chất lượng cao cho khu vực miền Nam. Mọi thông tin tuyển sinh được công bố tại website chính thức.

  

        ### Kêt quả:

        topics: [Đại học XYZ, thông tin trường, địa chỉ, sứ mệnh, website tuyển sinh]  

        tags: [Tên trường, Địa chỉ, Sứ mệnh, Website, Thông tin tuyển sinh]

        ---

        Đây là nội dung văn bản cần phân tích:

        {text}

        """

    return prompt
```


![](/img/user/assets/images/KG3.png)

---
# Kiến thức thêm về Knowledge graph
