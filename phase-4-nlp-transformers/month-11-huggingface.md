# 🤗 Month 11: HuggingFace & IndicBERT (মার্চ ২০২৭)

> **"HuggingFace হলো AI-র App Store — হাজারো models একটু click-এ!"** 💪🇮🇳🚀

[← Month 10](month-10-transformers.md) | [← Phase 4](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | মার্চ ১-৩১, ২০২৭ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | HuggingFace ecosystem + Bengali chatbot |
| 💻 মূল Project | Bengali Customer Service Chatbot 🤖 |
| 📚 মূল Resource | HuggingFace NLP Course + AI4Bharat |

---

## 🗓️ Week-by-Week Plan

### Week 41 — HuggingFace Basics

```python
# huggingface_basics.py
from transformers import pipeline, AutoTokenizer, AutoModel
import torch

# 1. Easy pipelines
sentiment = pipeline("sentiment-analysis")
print(sentiment("SmartStock AI is amazing for small businesses!"))

translation = pipeline("translation_en_to_fr")
print(translation("Hello, how are you?"))

# 2. Tokenizers
tokenizer = AutoTokenizer.from_pretrained("bert-base-multilingual-cased")
text = "SmartStock AI — ব্যবসা পরিচালনা সহজ করে!"
tokens = tokenizer(text, return_tensors="pt", padding=True, truncation=True)
print(f"Input IDs shape: {tokens['input_ids'].shape}")
print(f"Tokens: {tokenizer.convert_ids_to_tokens(tokens['input_ids'][0][:10])}")

# 3. IndicBERT for Bengali
indic_tokenizer = AutoTokenizer.from_pretrained("ai4bharat/indic-bert")
bengali_text = "আজকের বিক্রয়ের হিসাব দিন"
indic_tokens = indic_tokenizer(bengali_text)
print(f"\nBengali tokenized: {indic_tokens['input_ids']}")
```

### Week 42 — Fine-tuning IndicBERT

```python
# finetune_indicbert.py
from transformers import (AutoTokenizer, AutoModelForSequenceClassification,
                           TrainingArguments, Trainer)
from datasets import Dataset
import torch
import numpy as np

# Bengali business intent classification
INTENTS = ["add_stock", "sell_product", "check_balance", "view_report", "add_expense", "greeting"]

# Sample data
training_data = [
    ("চাল যোগ করো ১০০ কেজি", "add_stock"),
    ("আজকের বিক্রয় দেখাও", "view_report"),
    ("৫ কেজি ডাল বিক্রি করো", "sell_product"),
    ("ব্যালেন্স কত আছে?", "check_balance"),
    ("বাড়ি ভাড়া ১৫০০০ টাকা খরচ হয়েছে", "add_expense"),
    ("হ্যালো, কেমন আছেন?", "greeting"),
    ("নতুন পণ্য stock-এ রাখো", "add_stock"),
    ("আজকের লাভ কত?", "view_report"),
]

label2id = {label: i for i, label in enumerate(INTENTS)}
id2label = {i: label for label, i in label2id.items()}

texts, labels = zip(*training_data)
labels_encoded = [label2id[l] for l in labels]

model_name = "ai4bharat/indic-bert"
tokenizer = AutoTokenizer.from_pretrained(model_name)

def tokenize(examples):
    return tokenizer(examples["text"], padding="max_length",
                     truncation=True, max_length=128)

dataset = Dataset.from_dict({"text": list(texts), "label": labels_encoded})
tokenized_dataset = dataset.map(tokenize, batched=True)

model = AutoModelForSequenceClassification.from_pretrained(
    model_name, num_labels=len(INTENTS),
    id2label=id2label, label2id=label2id
)

print(f"✅ IndicBERT loaded for {len(INTENTS)}-class intent classification")
print("Fine-tune with your Bengali business data!")
```

### Week 43-44 — Bengali Chatbot Project

```python
# bengali_chatbot.py
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch
import json

class SmartStockChatbot:
    """SmartStock AI - Bengali Business Chatbot"""

    def __init__(self, model_path=None):
        self.model_name = model_path or "ai4bharat/indic-bert"
        self.tokenizer = AutoTokenizer.from_pretrained(self.model_name)
        self.intents = ["add_stock", "sell_product", "check_balance",
                        "view_report", "add_expense", "greeting", "unknown"]
        self.inventory = {}
        self.sales = []
        self.expenses = []
        self.total_cash = 50000

    def parse_intent(self, text):
        """Simple rule-based intent parsing for demo"""
        text = text.lower()
        if any(w in text for w in ["যোগ", "নতুন", "stock", "রাখো"]):
            return "add_stock"
        elif any(w in text for w in ["বিক্রি", "বেচো", "sell"]):
            return "sell_product"
        elif any(w in text for w in ["ব্যালেন্স", "টাকা কত", "balance"]):
            return "check_balance"
        elif any(w in text for w in ["রিপোর্ট", "হিসাব", "বিক্রয়"]):
            return "view_report"
        elif any(w in text for w in ["খরচ", "expense", "ভাড়া", "বিল"]):
            return "add_expense"
        elif any(w in text for w in ["হ্যালো", "hello", "hi", "নমস্কার"]):
            return "greeting"
        return "unknown"

    def respond(self, user_input):
        """Generate response based on intent"""
        intent = self.parse_intent(user_input)

        if intent == "greeting":
            return "🙏 স্বাগতম! আমি SmartStock AI। আপনার ব্যবসায় কীভাবে সাহায্য করতে পারি?"
        elif intent == "check_balance":
            return f"💰 আপনার বর্তমান ব্যালেন্স: ৳{self.total_cash:,}"
        elif intent == "view_report":
            total_sales = sum(s["amount"] for s in self.sales)
            total_expenses = sum(e["amount"] for e in self.expenses)
            profit = total_sales - total_expenses
            return f"📊 রিপোর্ট:\n  মোট বিক্রয়: ৳{total_sales:,}\n  মোট খরচ: ৳{total_expenses:,}\n  নিট লাভ: ৳{profit:,}"
        else:
            return f"আপনি বললেন: {user_input}\n(আরো context দিন — আমি আরো intelligent হচ্ছি! 🤖)"

def demo_chatbot():
    bot = SmartStockChatbot()
    print("=" * 50)
    print("🤖 SmartStock AI Chatbot Demo")
    print("=" * 50)

    test_queries = [
        "হ্যালো! কেমন আছেন?",
        "আমার ব্যালেন্স কত?",
        "আজকের রিপোর্ট দেখাও",
    ]

    for query in test_queries:
        print(f"\n👤 User: {query}")
        response = bot.respond(query)
        print(f"🤖 Bot: {response}")

demo_chatbot()
```

---

## 🎊 Phase 4 Complete!

**আপনি এখন জানেন:**
✅ NLP preprocessing (Bengali & English)
✅ Word embeddings
✅ Transformer architecture
✅ HuggingFace ecosystem
✅ IndicBERT fine-tuning
✅ Bengali chatbot তৈরি

**Phase 5: LLMs & Fine-tuning শুরু হচ্ছে!** ⚡

---

## ✅ Month 11 Checklist

- [ ] HuggingFace NLP Course সম্পূর্ণ করা
- [ ] IndicBERT fine-tune করা
- [ ] Bengali Chatbot তৈরি ও deploy করা
- [ ] HuggingFace Spaces-এ model upload করা
- [ ] Phase 4 সম্পূর্ণ! 🎉

---

[← Month 10](month-10-transformers.md) | [Phase 5 →](../phase-5-llms-finetuning/README.md)
