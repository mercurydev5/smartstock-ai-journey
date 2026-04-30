# ⚡ Month 10: Transformers (ফেব্রুয়ারি ২০২৭)

> **"Attention Is All You Need — এই paper-টা ML-এর ইতিহাস বদলে দিয়েছে!"** 💪🇮🇳🚀

[← Month 9](month-09-nlp-foundations.md) | [← Phase 4](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | ফেব্রুয়ারি ১-২৮, ২০২৭ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | Attention + Transformer architecture |
| 💻 মূল Project | Mini Bengali-English Translator |
| 📚 মূল Resource | Karpathy + CS224n + "Attention is All You Need" |

---

## 🗓️ Week-by-Week Plan

### Week 37 — Attention Mechanism

```python
# attention.py
import torch
import torch.nn as nn
import torch.nn.functional as F
import math

def scaled_dot_product_attention(Q, K, V, mask=None):
    """
    Multi-Head Attention-এর মূল অংশ
    Q: Query, K: Key, V: Value
    """
    d_k = Q.size(-1)
    # Attention scores
    scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(d_k)
    if mask is not None:
        scores = scores.masked_fill(mask == 0, -1e9)
    attention_weights = F.softmax(scores, dim=-1)
    output = torch.matmul(attention_weights, V)
    return output, attention_weights


class MultiHeadAttention(nn.Module):
    def __init__(self, d_model=512, num_heads=8):
        super().__init__()
        assert d_model % num_heads == 0

        self.d_model = d_model
        self.num_heads = num_heads
        self.d_k = d_model // num_heads

        self.W_q = nn.Linear(d_model, d_model)
        self.W_k = nn.Linear(d_model, d_model)
        self.W_v = nn.Linear(d_model, d_model)
        self.W_o = nn.Linear(d_model, d_model)

    def split_heads(self, x):
        batch_size = x.size(0)
        x = x.view(batch_size, -1, self.num_heads, self.d_k)
        return x.transpose(1, 2)

    def forward(self, Q, K, V, mask=None):
        Q, K, V = self.W_q(Q), self.W_k(K), self.W_v(V)
        Q, K, V = self.split_heads(Q), self.split_heads(K), self.split_heads(V)
        attn_output, weights = scaled_dot_product_attention(Q, K, V, mask)
        attn_output = attn_output.transpose(1, 2).contiguous()
        attn_output = attn_output.view(attn_output.size(0), -1, self.d_model)
        return self.W_o(attn_output), weights

print("✅ Attention mechanism implemented!")
```

### Week 38 — Transformer Architecture

```python
# transformer.py
import torch
import torch.nn as nn
import math

class PositionalEncoding(nn.Module):
    def __init__(self, d_model, max_len=5000, dropout=0.1):
        super().__init__()
        self.dropout = nn.Dropout(dropout)
        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len).unsqueeze(1).float()
        div_term = torch.exp(torch.arange(0, d_model, 2).float() * (-math.log(10000.0) / d_model))
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        self.register_buffer("pe", pe.unsqueeze(0))

    def forward(self, x):
        return self.dropout(x + self.pe[:, :x.size(1)])

class TransformerBlock(nn.Module):
    def __init__(self, d_model=256, num_heads=8, ff_dim=1024, dropout=0.1):
        super().__init__()
        self.attention = nn.MultiheadAttention(d_model, num_heads, dropout=dropout, batch_first=True)
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.feed_forward = nn.Sequential(
            nn.Linear(d_model, ff_dim),
            nn.GELU(),
            nn.Dropout(dropout),
            nn.Linear(ff_dim, d_model),
            nn.Dropout(dropout),
        )

    def forward(self, x, mask=None):
        attn_out, _ = self.attention(x, x, x, attn_mask=mask)
        x = self.norm1(x + attn_out)
        ff_out = self.feed_forward(x)
        return self.norm2(x + ff_out)

class MiniTransformer(nn.Module):
    def __init__(self, vocab_size, d_model=256, num_heads=8, num_layers=4, max_len=512):
        super().__init__()
        self.embedding = nn.Embedding(vocab_size, d_model)
        self.pos_encoding = PositionalEncoding(d_model, max_len)
        self.layers = nn.ModuleList([TransformerBlock(d_model, num_heads) for _ in range(num_layers)])
        self.output = nn.Linear(d_model, vocab_size)
        self.scale = math.sqrt(d_model)

    def forward(self, x, mask=None):
        x = self.embedding(x) * self.scale
        x = self.pos_encoding(x)
        for layer in self.layers:
            x = layer(x, mask)
        return self.output(x)

model = MiniTransformer(vocab_size=10000)
total_params = sum(p.numel() for p in model.parameters())
print(f"Mini Transformer: {total_params:,} parameters")
print("✅ Transformer architecture implemented!")
```

### Week 39-40 — Bengali-English Mini Translator

Project: Train a small seq2seq Transformer on Bengali-English pairs from AI4Bharat's Samanantar dataset.

---

## ✅ Month 10 Checklist

- [ ] "Attention Is All You Need" paper পড়া
- [ ] Karpathy "Let's Build GPT" video দেখা
- [ ] Attention mechanism implement করা
- [ ] Transformer architecture implement করা
- [ ] Mini translator project করা
- [ ] GitHub-এ push করা

---

[← Month 9](month-09-nlp-foundations.md) | [Month 11 →](month-11-huggingface.md)
