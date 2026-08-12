---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/research-skills/mindset-read-paper/08-prompts-doc-paper-va-phan-bien/","pinned":"false","tags":["type/prompt","topic/research-method","status/stable"]}
---

# 🤖 BỘ PROMPTS AI HỖ TRỢ ĐỌC PAPER & PHẢN BIỆN KHOA HỌC

> **Tóm tắt:** Tập hợp các khung Prompt tiêu chuẩn được tối ưu hóa cho các công cụ AI (NotebookLM, ChatGPT, Gemini, Perplexity). Hỗ trợ nhà nghiên cứu đọc lướt nhanh bài báo, đóng vai Reviewer phản biện khắt khe (NeurIPS/ICLR format), và thiết kế trực quan hóa số liệu thực nghiệm.

---

## 🛠️ HƯỚNG DẪN SỬ DỤNG
1. Sao chép trực tiếp nội dung trong các khung `code block`.
2. Thay thế các thông tin trong ngoặc vuông `[ ]` (nếu có) phù hợp với lĩnh vực nghiên cứu của bạn.
3. Sử dụng tốt nhất khi đính kèm file PDF bài báo khoa học vào NotebookLM hoặc ChatGPT Plus / Gemini Advanced.

---

## 📌 PROMPT 1: ĐỌC & TÓM TẮT BÀI BÁO THEO TẦNG (STRUCTURED SUMMARY)

```text
You are an expert AI researcher in [SUBFIELD: e.g., Legal AI, Natural Language Processing, Continual Learning, RAG Systems]. 
Guide me through the attached paper step by step according to the following requirements:

1. High-Level Summary (5–7 sentences):
- Use simple, intuitive language.
- Clearly state: What problem does this paper solve? What is the main key idea? In what context of the field does it operate?

2. Section-by-Section Summary (Introduction -> Methodology -> Experiments -> Discussion):
- For each section, provide a concise summary (3–5 sentences, avoiding heavy math notation).

3. Methodology Deep-Dive:
- Describe the core framework/methodology (1–2 paragraphs), focusing on:
  a. Novel technical architecture / algorithm / technical idea.
  b. Data pipeline, experimental setup, evaluation metrics.
  c. State the 3 most important technical assumptions or modeling choices made by the authors.
```

---

## 📌 PROMPT 2: REVIEWER PHẢN BIỆN KHẮT KHE (BRUTALLY HONEST REVIEWER)

```text
Act as a brutally honest reviewer for a top AI conference (e.g., NeurIPS, ICML, ICLR).
Evaluate the attached paper and write a comprehensive review following this exact template:

1. Summary (3–6 sentences):
- Describe what the paper does, how it achieves its goals, and what the primary claims are.

2. Strengths:
- List 3–5 strongest points (e.g., novelty of idea, rigorous experimental design, clarity, potential impact, engineering effort).

3. Weaknesses (Focus heavily on this section):
- Identify 5 major weaknesses. For each point:
  (a) Describe the issue specifically.
  (b) Quote or cite the relevant location/section in the paper.
  (c) Explain why it is severe.
- Prioritize flaws such as: missing critical baselines, lack of ablation studies, unfair experimental setup, data leakage, selection bias, overclaimed conclusions.

4. Questions for Authors:
- Provide 3–5 specific, highly technical questions (similar to a rebuttal process) that the authors must answer to convince you.

5. Recommendation:
- Single score: [Strong Accept / Accept / Weak Accept / Borderline / Weak Reject / Reject]
- Provide a 3–5 sentence justification for your final rating decision.
```

---

## 📌 PROMPT 3: TRỰC QUAN HÓA SỐ LIỆU & BẢNG BIỂU ThỰC NGHIỆM (DATA & METRICS DESIGNER)

```text
Act as a data visualization and evaluation expert for Machine Learning experiments.
Based on the Experiments and Results section of the attached paper:

1. Executive Summary of Experimental Setup:
- List all datasets, tasks, and evaluation metrics used in the paper.
- List all primary baseline models compared against.

2. Chart & Visualization Proposals:
- Propose at least 3 specific chart types to effectively visualize the results (e.g., Bar chart comparing models per dataset, Line chart over epochs/memory size, Scatter plot of accuracy vs. latency).
- For each chart proposal specify:
  - X-axis, Y-axis, Series/Grouping.
  - Objective: What specific research question does this chart answer?

3. Summary Table Design:
- Design the structure of 1–2 consolidated tables (define columns and rows: models, datasets, metrics, configuration).
- Highlight recommendations (e.g., bold for best score, add a column for "Δ vs best baseline").

4. Raw Data Extraction:
- Extract raw numerical data from the text/tables into a clean, ready-to-copy Markdown table so I can easily paste it into Python/Pandas/R for plotting.
```
