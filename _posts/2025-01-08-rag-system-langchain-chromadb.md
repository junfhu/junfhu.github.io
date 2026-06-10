---
title: "Building a RAG System with LangChain and ChromaDB"
categories:
  - AI
  - Development
tags:
  - llm
  - rag
  - langchain
  - chromadb
  - python
date: 2025-01-08
---

Retrieval-Augmented Generation (RAG) is the most practical pattern for grounding LLM responses in your own documents. Here's how to build one from scratch.

## Architecture

```
Documents → Chunk → Embed → Vector Store
                                    ↓
User Query → Embed → Similarity Search → Retrieved Context
                                                ↓
                              LLM + Context → Answer
```

## Implementation

### 1. Load and Chunk Documents

```python
from langchain.document_loaders import DirectoryLoader, TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter

loader = DirectoryLoader("./docs/", glob="**/*.md", loader_cls=TextLoader)
documents = loader.load()

splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200
)
chunks = splitter.split_documents(documents)
```

### 2. Create Embeddings and Store in ChromaDB

```python
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma

embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(
    documents=chunks,
    embedding=embeddings,
    persist_directory="./chroma_db"
)
```

### 3. Build the RAG Chain

```python
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI

retriever = vectorstore.as_retriever(search_kwargs={"k": 4})
llm = ChatOpenAI(model="gpt-4o", temperature=0)

qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=retriever,
    return_source_documents=True
)

result = qa_chain("What's the deployment process?")
print(result["result"])
# Also check result["source_documents"] for citations
```

## Tips for Production

1. **Chunk size matters** — 500-1500 tokens works well for most use cases
2. **Add metadata** to chunks (source file, page number, date) for better filtering
3. **Re-rank results** — use a cross-encoder to improve retrieval quality
4. **Cache embeddings** — don't recalculate for unchanged documents
5. **Stream responses** — users hate waiting for the full answer to generate

RAG isn't just for chatbots — it's great for internal documentation search, customer support, and compliance auditing.
