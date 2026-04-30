# 🏗️ Month 15: Architecture Design (জুলাই ২০২৭)

> **"ভালো architecture = ভালো model!"** 💪🇮🇳🚀

[← Phase 6](README.md) | [← Main README](../README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | জুলাই ১-৩১, ২০২৭ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | SmartStock AI architecture + data |
| 💻 মূল Output | Architecture doc + Bengali corpus |
| 📚 মূল Resource | Llama 2/3 technical reports |

---

## 🗓️ Week-by-Week Plan

### Week 57 — Architecture Design

```python
# smartstock_ai_architecture.py
"""
SmartStock AI Architecture Design
====================================

Model: SmartStock-7B
Based on: Llama 3.1 architecture
Vocabulary: Bengali + Hindi + English BPE tokenizer
"""

ARCHITECTURE = {
    "model_type": "decoder-only transformer",
    "base": "Llama 3.1 8B architecture",
    "parameters": "7B",
    "vocab_size": 65536,  # Bengali + Hindi + English
    "hidden_size": 4096,
    "num_layers": 32,
    "num_attention_heads": 32,
    "num_kv_heads": 8,   # Grouped Query Attention (GQA)
    "intermediate_size": 11008,
    "context_length": 4096,
    "positional_encoding": "RoPE (Rotary Position Embedding)",
    "activation": "SwiGLU",
    "normalization": "RMSNorm",
}

DATA_PLAN = {
    "Bengali_web": "50GB (CC-100 Bengali)",
    "Hindi_web": "30GB (CC-100 Hindi)",
    "Bengali_books": "5GB (Digital Grantha)",
    "Business_conversations": "2GB (synthetic + real)",
    "Bengali_Wikipedia": "500MB",
    "Hindi_Wikipedia": "500MB",
    "SmartStock_specific": "200MB (business dialogues)",
    "total": "~90GB",
}

print("SmartStock AI Architecture:")
for k, v in ARCHITECTURE.items():
    print(f"  {k}: {v}")
```

### Week 58-59 — Data Collection

```python
# data_collection.py
from datasets import load_dataset
import os
import json

def collect_bengali_corpus():
    """Collect Bengali training data"""
    sources = {
        "wikipedia": "20231101.bn",
        "cc100": "bn",
        "oscar": "unshuffled_deduplicated_bn",
    }

    print("📥 Bengali data collection plan:")
    for name, config in sources.items():
        print(f"  {name}: load_dataset('{name}', '{config}')")

    print("\n💡 Pro tips for Bengali data:")
    print("  1. CC-100: https://data.statmt.org/cc-100/")
    print("  2. OSCAR: https://oscar-project.org/")
    print("  3. AI4Bharat's corpus: https://ai4bharat.iitm.ac.in/")
    print("  4. Samanantar: https://ai4bharat.iitm.ac.in/samanantar")

def create_business_synthetic_data():
    """Create synthetic business conversation data"""
    templates = [
        {
            "conversation": [
                {"role": "user", "content": "আজকের বিক্রয় কত হয়েছে?"},
                {"role": "assistant", "content": "আজ মোট বিক্রয় ১৫,৩৫০ টাকা হয়েছে। ৪২টি transaction সম্পন্ন হয়েছে।"}
            ]
        },
    ]

    with open("business_synthetic_data.jsonl", "w", encoding="utf-8") as f:
        for item in templates:
            f.write(json.dumps(item, ensure_ascii=False) + "\n")

    print(f"✅ Generated synthetic business conversations")

collect_bengali_corpus()
create_business_synthetic_data()
```

### Week 60 — Tokenizer Training

```python
# train_tokenizer.py
from tokenizers import Tokenizer
from tokenizers.models import BPE
from tokenizers.trainers import BpeTrainer
from tokenizers.pre_tokenizers import Whitespace
from transformers import PreTrainedTokenizerFast

def train_bengali_hindi_tokenizer(files, vocab_size=65536):
    """Train BPE tokenizer for Bengali + Hindi + English"""
    tokenizer = Tokenizer(BPE(unk_token="[UNK]"))
    tokenizer.pre_tokenizer = Whitespace()

    trainer = BpeTrainer(
        vocab_size=vocab_size,
        special_tokens=["[UNK]", "[CLS]", "[SEP]", "[PAD]", "[MASK]",
                        "<|begin_of_text|>", "<|end_of_text|>",
                        "<|user|>", "<|assistant|>", "<|system|>"],
        min_frequency=2,
        show_progress=True,
    )

    tokenizer.train(files=files, trainer=trainer)
    tokenizer.save("smartstock_tokenizer.json")

    # Convert to HuggingFace format
    hf_tokenizer = PreTrainedTokenizerFast(tokenizer_file="smartstock_tokenizer.json")
    hf_tokenizer.save_pretrained("smartstock-tokenizer")

    print(f"✅ Tokenizer trained! Vocab size: {tokenizer.get_vocab_size()}")

    # Test
    test_texts = ["আজকের বিক্রয় কত?", "Today's sales report", "आज की बिक्री कितनी है?"]
    for text in test_texts:
        tokens = tokenizer.encode(text)
        print(f"'{text}' → {len(tokens.tokens)} tokens: {tokens.tokens[:5]}...")

print("Bengali + Hindi tokenizer ready!")
```

---

## ✅ Month 15 Checklist

- [ ] SmartStock AI architecture design করা
- [ ] Bengali + Hindi corpus collect করা (≥10GB)
- [ ] Business conversation synthetic data তৈরি করা (1000+ examples)
- [ ] Custom BPE tokenizer train করা
- [ ] Architecture doc লেখা
- [ ] GitHub-এ push করা

---

[← Phase 6 README](README.md) | [Month 16 →](month-16-training-pipeline.md)
