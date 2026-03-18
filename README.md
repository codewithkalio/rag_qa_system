# RAG Q&A System — Observations

## 🧭 Problem Statement

Build a RAG-powered Q&A system using your own data, not a tutorial dataset. Choose a real knowledge base that matters to your work or interests. Real-world data is messy and unpredictable, and those retrieval failures are where the most useful learning happens.

## 📊 Data

Three documents loaded from different formats: a live web article (WebBaseLoader), a PDF (PyPDFLoader), and an ODT file (UnstructuredODTLoader). "CrossFit" topic chosen because the content is factual, verifiable, and domain-specific enough to make retrieval failures obvious.

## ✅ What Worked

### 📦 Chunk size 800

Preserved complete thoughts across multi-sentence answers. Questions like "What is the coach responsible for?" span several consecutive sentences — smaller chunks (tested at 500) risk splitting the answer mid-thought, leaving the LLM with incomplete context.

### ⚖️ EnsembleRetriever (BM25 + vector similarity)

Improved retrieval for both conceptual and keyword-specific questions. BM25 weight was set higher than the default (0.6 vs 0.4 vector) after finding that niche terms like "hopper" were missed by vector search alone — the embedding space has no semantic neighbors for CrossFit-specific vocabulary.

### 🎯 Tight system prompt

Reduced hallucination. An early version produced answers that were accurate but tangential. Adding explicit constraints ("answer only what is directly asked," "do not rely on your own knowledge") brought responses in line with the source material. Mid-way thru development AI removed the RetrievalQA, I'm not sure if this was before or after the system prompt was tightened.  RetrievalQA was re-introduced at the end of development.

## ⚠️ What Was Challenging?

### 🌐 Live web scraping is unreliable

WebBaseLoader fetches the page on every run. If the page content changes between runs, chunk boundaries shift and retrieval results change — even with identical code. Lesson: save web content locally and load it as a pre-formatted file.

### 🔎 Vector search fails on niche keywords

"What is a hopper used for?" returned irrelevant chunks under the default 70/30 vector/BM25 weighting because "hopper" has no semantic neighbors in the embedding space. Fix: raise BM25 weight and increase k to widen the candidate pool.

### 🏞️ Broad test questions are hard to evaluate

Early test questions covered concepts that appeared throughout the documents, making it difficult to verify whether the right chunk was retrieved. Switching to questions with fewer, more specific mentions made retrieval quality easier to assess.

## 🚀 What I'd Improve

- Save the web article as a local file to make runs reproducible
- Test a re-ranker on top of the ensemble to handle query-type mismatch between vector and keyword search
- Add a simple UI (Streamlit) for interactive testing
- Dive deeper into Eval methodology.  Manually comparing results becomes cumbersome when iterating.

