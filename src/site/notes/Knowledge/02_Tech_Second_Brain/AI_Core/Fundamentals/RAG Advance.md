---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/fundamentals/rag-advance/","title":"RAG Advance","pinned":"false"}
---

# Introduction
The sources provide information on RAG (Retrieval Augmented Generation) and its various strategies within the LangGraph framework.

### **RAG Overview:**

- RAG involves using an agent to determine how to retrieve the most relevant information before answering a user's question.
- LangGraph offers different RAG strategies.

### **RAG Strategies:**

- **Adaptive RAG:** This strategy unites query analysis with active/self-corrective RAG. There is an available implementation that uses local LLMs.
- **Corrective RAG:** This uses an LLM to grade the quality of retrieved information. If the quality is low, it retrieves information from another source. There is an available implementation that uses local LLMs.
- **Self-RAG:** This incorporates self-reflection and self-grading on retrieved documents and generations. There is an available implementation that uses local LLMs.
### RAG advance 
	- 

# RAG Advance
[RAG của bạn tiến và đoàn](https://www.youtube.com/watch?v=OHFGDn_tJwA): Ở đây là link youtube bạn đó làm 1 dự án thực tế bạn có giải thích kĩ ra về các bước rag. Vì dự án của công ty nên không có show code. Nhưng mình biết rõ được các flow chạy. 
Hoặc ở trong đây :
[Advance RAG- Hơi Non](https://atekco.io/1713861441749-toan-canh-cac-ky-thuat-advanced-rag/): Đây là bài blog cũng nói đến chủ đề này. 
[Survey Retrieval](https://arxiv.org/pdf/2312.10997): bài paper survey về rag. 
## Link tham khảo
- https://medium.com/ai-advances/advanced-rag-techniques-unlocking-the-next-level-040c205b95bc
- https://pub.towardsai.net/advanced-rag-techniques-an-illustrated-overview-04d193d8fec6
- https://towardsdatascience.com/advanced-retrieval-augmented-generation-from-theory-to-llamaindex-implementation-4de1464a9930
- https://arxiv.org/pdf/2404.01037
- https://github.com/ianhojy/auto-hyde/tree/main

