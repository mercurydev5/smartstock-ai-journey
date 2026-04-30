# 🧠 Month 12: LLM Fundamentals (এপ্রিল ২০২৭)

> **"Karpathy-র সাথে নিজের GPT বানাই!"** 💪🇮🇳🚀

[← Phase 5](README.md) | [← Main README](../README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | এপ্রিল ১-৩০, ২০২৭ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | GPT architecture + Bengali nanoGPT |
| 💻 মূল Project | Bengali nanoGPT (trained on Wikipedia) |
| 📚 মূল Resource | Karpathy "Let's Build GPT" video (2h 13m) |

---

## 🗓️ Week-by-Week Plan

### Week 45 — Karpathy's "Let's Build GPT" 🎯

⭐ **এই সপ্তাহের সবচেয়ে গুরুত্বপূর্ণ কাজ:**

```bash
# Watch: https://www.youtube.com/watch?v=kCc8FmEb1nY
# Duration: 2 hours 13 minutes
# Follow along with code!
```

```python
# Following Karpathy's nanoGPT structure
import torch
import torch.nn as nn
import torch.nn.functional as F

# Hyperparameters (start small!)
batch_size = 16
block_size = 32  # context length
max_iters = 5000
eval_interval = 500
learning_rate = 1e-3
device = "cuda" if torch.cuda.is_available() else "cpu"
n_embd = 64
n_head = 4
n_layer = 4
dropout = 0.0

class Head(nn.Module):
    """One head of self-attention"""
    def __init__(self, head_size):
        super().__init__()
        self.key = nn.Linear(n_embd, head_size, bias=False)
        self.query = nn.Linear(n_embd, head_size, bias=False)
        self.value = nn.Linear(n_embd, head_size, bias=False)
        self.register_buffer("tril", torch.tril(torch.ones(block_size, block_size)))
        self.dropout = nn.Dropout(dropout)

    def forward(self, x):
        B, T, C = x.shape
        k = self.key(x)
        q = self.query(x)
        wei = q @ k.transpose(-2, -1) * k.shape[-1]**-0.5
        wei = wei.masked_fill(self.tril[:T, :T] == 0, float("-inf"))
        wei = F.softmax(wei, dim=-1)
        wei = self.dropout(wei)
        v = self.value(x)
        return wei @ v

class GPTLanguageModel(nn.Module):
    def __init__(self, vocab_size):
        super().__init__()
        self.token_embedding = nn.Embedding(vocab_size, n_embd)
        self.position_embedding = nn.Embedding(block_size, n_embd)
        # ... (complete following Karpathy's video)

    def generate(self, idx, max_new_tokens):
        for _ in range(max_new_tokens):
            idx_cond = idx[:, -block_size:]
            logits, loss = self(idx_cond)
            logits = logits[:, -1, :]
            probs = F.softmax(logits, dim=-1)
            idx_next = torch.multinomial(probs, num_samples=1)
            idx = torch.cat((idx, idx_next), dim=1)
        return idx

print("✅ GPT skeleton ready!")
print("Follow Karpathy's video for complete implementation")
```

### Week 46 — Bengali nanoGPT Data Preparation

```python
# bengali_data_prep.py
import os
import urllib.request
import json

def download_bengali_data():
    """Download Bengali Wikipedia data"""
    print("📥 Bengali Wikipedia data download করছি...")
    # Use Wikipedia API or HuggingFace datasets
    from datasets import load_dataset

    # Small Bengali text dataset
    dataset = load_dataset("wikipedia", "20231101.bn", trust_remote_code=True, split="train[:10%]")
    texts = [item["text"] for item in dataset if len(item["text"]) > 100]
    print(f"✅ Downloaded {len(texts)} Bengali articles")
    return texts

def prepare_character_tokenizer(texts):
    """Simple character-level tokenizer for Bengali"""
    all_text = "\n".join(texts[:100])  # Use small subset for training

    # Get all unique characters
    chars = sorted(list(set(all_text)))
    vocab_size = len(chars)
    print(f"Vocabulary size: {vocab_size} unique characters")
    print(f"Sample chars: {chars[:20]}")

    stoi = {ch: i for i, ch in enumerate(chars)}
    itos = {i: ch for i, ch in enumerate(chars)}

    encode = lambda s: [stoi.get(c, 0) for c in s]
    decode = lambda l: "".join([itos.get(i, "?") for i in l])

    return encode, decode, vocab_size, all_text

print("Bengali nanoGPT data preparation ready!")
```

### Week 47-48 — Train Bengali nanoGPT

```python
# train_bengali_nanogpt.py
import torch
import time

def train_nanogpt(model, train_data, val_data, max_iters=5000):
    """Training loop for nanoGPT"""
    optimizer = torch.optim.AdamW(model.parameters(), lr=3e-4)
    device = "cuda" if torch.cuda.is_available() else "cpu"
    model.to(device)

    print(f"Training on: {device}")
    print(f"Model parameters: {sum(p.numel() for p in model.parameters()):,}")
    print(f"Training for {max_iters} iterations...\n")

    start_time = time.time()
    for iter in range(max_iters):
        # Get batch
        ix = torch.randint(len(train_data) - 32, (16,))
        x = torch.stack([train_data[i:i+32] for i in ix]).to(device)
        y = torch.stack([train_data[i+1:i+33] for i in ix]).to(device)

        # Forward + backward
        logits, loss = model(x, y)
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        if iter % 500 == 0:
            elapsed = time.time() - start_time
            print(f"Step {iter}/{max_iters} | Loss: {loss.item():.4f} | Time: {elapsed:.1f}s")

    print("\n✅ Bengali nanoGPT training complete!")
    print("Now generate some Bengali text!")

print("Ready to train Bengali nanoGPT!")
print("This is YOUR first Bengali language model! 🎉")
```

---

## ✅ Month 12 Checklist

- [ ] Karpathy "Let's Build GPT" video (2h 13m) সম্পূর্ণ দেখা
- [ ] nanoGPT code follow করে implement করা
- [ ] Bengali Wikipedia data prepare করা
- [ ] Bengali nanoGPT train করা
- [ ] Generated Bengali text GitHub-এ share করা 🎉

---

[← Phase 5 README](README.md) | [Month 13 →](month-13-finetuning-peft.md)
