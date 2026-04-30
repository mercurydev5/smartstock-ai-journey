# 📝 Month 9: NLP Foundations (জানুয়ারি ২০২৭)

> **"বাংলা ভাষাকে AI বোঝাবো — এটাই আমাদের mission!"** 💪🇮🇳🚀

[← Phase 4](README.md) | [← Main README](../README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | জানুয়ারি ১-৩১, ২০২৭ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | NLP fundamentals + Bengali processing |
| 💻 মূল Project | Bengali Sentiment Analyzer |
| 📚 মূল Resource | CS224n + AI4Bharat |

---

## 🗓️ Week-by-Week Plan

### Week 33 — Text Processing Basics

```python
# text_processing.py
import re
import string
from collections import Counter
import nltk
from nltk.tokenize import word_tokenize, sent_tokenize
from nltk.corpus import stopwords

nltk.download("punkt")
nltk.download("stopwords")

def preprocess_english_text(text):
    """NLP preprocessing pipeline"""
    # Lowercase
    text = text.lower()
    # Remove punctuation
    text = text.translate(str.maketrans("", "", string.punctuation))
    # Tokenize
    tokens = word_tokenize(text)
    # Remove stopwords
    stop_words = set(stopwords.words("english"))
    tokens = [t for t in tokens if t not in stop_words]
    return tokens

def preprocess_bengali_text(text):
    """Bengali text preprocessing"""
    # Remove punctuation (Bengali + ASCII)
    text = re.sub(r"[।!?,।\.\-\(\)\[\]]", " ", text)
    # Normalize whitespace
    text = re.sub(r"\s+", " ", text).strip()
    # Tokenize by space (basic for Bengali)
    tokens = text.split()
    return tokens

# TF-IDF
from sklearn.feature_extraction.text import TfidfVectorizer

documents = [
    "আজকের বিক্রি ভালো হয়েছে",
    "চালের স্টক শেষ হয়ে যাচ্ছে",
    "নতুন supplier contact করতে হবে",
    "গ্রাহক ফিরে এসেছেন",
]
vectorizer = TfidfVectorizer()
tfidf_matrix = vectorizer.fit_transform(documents)
print(f"TF-IDF shape: {tfidf_matrix.shape}")
print(f"Vocabulary size: {len(vectorizer.vocabulary_)}")
```

### Week 34 — Word Embeddings

```python
# word_embeddings.py
from gensim.models import Word2Vec, KeyedVectors
import numpy as np

# Train Word2Vec on Bengali business sentences
sentences = [
    ["চাল", "ডাল", "তেল", "মুদি", "দোকান"],
    ["বিক্রি", "কেনা", "লাভ", "ক্ষতি", "হিসাব"],
    ["স্টক", "পণ্য", "সরবরাহ", "চাহিদা"],
    ["গ্রাহক", "ক্রেতা", "বিক্রেতা", "সম্পর্ক"],
]

model = Word2Vec(sentences, vector_size=50, window=3, min_count=1, epochs=100)

# Similar words
try:
    similar = model.wv.most_similar("বিক্রি", topn=3)
    print("বিক্রি-র সাথে মিলে এমন শব্দ:")
    for word, score in similar:
        print(f"  {word}: {score:.3f}")
except:
    print("More data needed for meaningful word vectors")

# Use pre-trained Bengali embeddings from AI4Bharat
print("\n✅ For production: use AI4Bharat's IndicBERT embeddings!")
print("   https://huggingface.co/ai4bharat/indic-bert")
```

### Week 35 — Sentiment Analysis

```python
# bengali_sentiment.py
from transformers import pipeline, AutoTokenizer, AutoModelForSequenceClassification

# Use AI4Bharat's model for Bengali
def create_bengali_sentiment_analyzer():
    """Bengali sentiment analysis using IndicBERT"""
    model_name = "ai4bharat/indic-bert"

    tokenizer = AutoTokenizer.from_pretrained(model_name)
    # Fine-tuned version for sentiment (use community models)

    # For now, use multilingual model
    sentiment_pipeline = pipeline("sentiment-analysis",
                                   model="nlptown/bert-base-multilingual-uncased-sentiment")
    return sentiment_pipeline

# Business reviews in Bengali
reviews = [
    "এই দোকানের সেবা অসাধারণ! সবসময় তাজা পণ্য পাওয়া যায়।",
    "দাম অনেক বেশি। অন্য দোকানে কম পাই।",
    "ঠিকঠাক। মাঝেমধ্যে stock out থাকে।",
    "খুব ভালো! মালিক অনেক friendly।",
]

print("Bengali Business Review Sentiment Analysis:")
print("-" * 50)
for review in reviews:
    # Simplified rule-based for demo
    positive_words = ["অসাধারণ", "ভালো", "তাজা", "friendly"]
    negative_words = ["বেশি", "কম", "stock out", "সমস্যা"]
    pos_count = sum(1 for w in positive_words if w in review)
    neg_count = sum(1 for w in negative_words if w in review)
    sentiment = "😊 Positive" if pos_count > neg_count else ("😞 Negative" if neg_count > pos_count else "😐 Neutral")
    print(f"\n📝 {review[:50]}...")
    print(f"   Sentiment: {sentiment}")
```

### Week 36 — Bengali Sentiment Analyzer Project

Full project with AI4Bharat embeddings, custom dataset, evaluation metrics.

---

## ✅ Month 9 Checklist

- [ ] Text preprocessing (tokenization, stopwords)
- [ ] TF-IDF implement করা
- [ ] Word2Vec বোঝা ও train করা
- [ ] Bengali Sentiment Analyzer তৈরি করা
- [ ] AI4Bharat resources explore করা
- [ ] GitHub-এ push করা

---

[← Phase 4 README](README.md) | [Month 10 →](month-10-transformers.md)
