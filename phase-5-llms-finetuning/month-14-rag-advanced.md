# 🔍 Month 14: RAG & Advanced LLM (জুন ২০২৭)

> **"RAG মানে LLM-কে আপনার নিজের business data দিয়ে smart করা!"** 💪🇮🇳🚀

[← Month 13](month-13-finetuning-peft.md) | [← Phase 5](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | জুন ১-৩০, ২০২৭ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | RAG + LangChain + Vector DBs |
| 💻 মূল Project | RAG-powered Business Assistant |
| 📚 মূল Resource | LangChain Docs + LlamaIndex |

---

## 🗓️ Week-by-Week Plan

### Week 53 — RAG Architecture

```python
# rag_basics.py
"""
RAG (Retrieval-Augmented Generation) Flow:

User Query
    ↓
[Embed query → vector]
    ↓
[Search vector DB → find relevant docs]
    ↓
[Combine query + relevant docs]
    ↓
[LLM generates response]
    ↓
Response with citations
"""

from langchain_community.embeddings import HuggingFaceEmbeddings
from langchain_community.vectorstores import Qdrant
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_core.documents import Document

# Step 1: Prepare business documents
business_docs = [
    """চালের দাম: বাসমতি চাল ৮০ টাকা/কেজি, মিনিকেট চাল ৬০ টাকা/কেজি।
    সর্বশেষ আপডেট: ২০২৭-০৫-১৫। Supplier: করিম এন্টারপ্রাইজ।""",

    """শুক্রবার বিশেষ অফার: সব পণ্যে ৫% ছাড়।
    শনিবার ও রবিবার: সবজি ১০% ছাড়।
    ঈদের সময়: মশলা ও সেমাই-তে বিশেষ ছাড়।""",

    """Payment নিতি: নগদ, bKash, Nagad গ্রহণযোগ্য।
    Credit: নিয়মিত গ্রাহক সর্বোচ্চ ৫০০০ টাকা বাকি নিতে পারবেন।
    Payment deadline: ৩০ দিনের মধ্যে পরিশোধ করতে হবে।""",
]

# Step 2: Chunk documents
text_splitter = RecursiveCharacterTextSplitter(chunk_size=200, chunk_overlap=20)
docs = [Document(page_content=doc) for doc in business_docs]
splits = text_splitter.split_documents(docs)
print(f"Created {len(splits)} text chunks")

# Step 3: Create embeddings (Bengali-friendly)
embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/paraphrase-multilingual-mpnet-base-v2"
)
print("✅ Multilingual embeddings ready!")
```

### Week 54 — Vector Database (Qdrant)

```python
# vector_db_setup.py
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams
from langchain_community.vectorstores import Qdrant
from langchain_community.embeddings import HuggingFaceEmbeddings

# Local Qdrant (free!)
client = QdrantClient(":memory:")  # In-memory for testing

# Create collection
COLLECTION_NAME = "smartstock_business_docs"
VECTOR_SIZE = 768  # For multilingual sentence transformers

client.create_collection(
    collection_name=COLLECTION_NAME,
    vectors_config=VectorParams(size=VECTOR_SIZE, distance=Distance.COSINE)
)

# Add documents
embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/paraphrase-multilingual-mpnet-base-v2"
)

vectorstore = Qdrant(
    client=client,
    collection_name=COLLECTION_NAME,
    embeddings=embeddings
)

# Add business documents
vectorstore.add_texts(business_docs)
print("✅ Vector database ready!")

# Search
results = vectorstore.similarity_search("চালের দাম কত?", k=2)
for doc in results:
    print(f"📄 {doc.page_content[:100]}...")
```

### Week 55-56 — Full RAG Pipeline

```python
# rag_business_assistant.py
from langchain.chains import RetrievalQA
from langchain_community.llms import HuggingFacePipeline
from transformers import pipeline as hf_pipeline
import torch

def create_business_rag(vectorstore, model_name="meta-llama/Meta-Llama-3.1-8B-Instruct"):
    """Create RAG chain for SmartStock"""

    # Load LLM (use your fine-tuned model!)
    llm_pipeline = hf_pipeline(
        "text-generation",
        model=model_name,
        torch_dtype=torch.float16,
        device_map="auto",
        max_new_tokens=512,
        temperature=0.1,
    )
    llm = HuggingFacePipeline(pipeline=llm_pipeline)

    # RAG Chain
    rag_chain = RetrievalQA.from_chain_type(
        llm=llm,
        chain_type="stuff",
        retriever=vectorstore.as_retriever(search_kwargs={"k": 3}),
        return_source_documents=True,
    )

    return rag_chain

# Bengali business queries to test
test_queries = [
    "চালের দাম কত?",
    "শুক্রবার কি ছাড় আছে?",
    "বাকিতে কেনার নিয়ম কী?",
]

print("✅ RAG Business Assistant ready!")
print("Test it with Bengali business queries!")
```

---

## 🎊 Phase 5 Complete!

**আপনি এখন জানেন:**
✅ GPT architecture (from scratch)
✅ LoRA + QLoRA fine-tuning
✅ RAG system তৈরি
✅ LangChain + Vector DBs
✅ Bengali nanoGPT trained

**Phase 6: Custom Model তৈরি করার সময়!** 🔬

---

## ✅ Month 14 Checklist

- [ ] RAG concept বোঝা
- [ ] Qdrant vector DB setup করা
- [ ] Multilingual embeddings ব্যবহার করা
- [ ] LangChain RAG chain তৈরি করা
- [ ] Business Assistant deploy করা
- [ ] Phase 5 সম্পূর্ণ! 🎉

---

[← Month 13](month-13-finetuning-peft.md) | [Phase 6 →](../phase-6-custom-model/README.md)
