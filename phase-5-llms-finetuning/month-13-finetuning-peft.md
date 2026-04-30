# 🔧 Month 13: Fine-tuning & PEFT (মে ২০২৭)

> **"LoRA মানে কম GPU-তে বড় model fine-tune করার জাদু!"** 💪🇮🇳🚀

[← Month 12](month-12-llm-fundamentals.md) | [← Phase 5](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | মে ১-৩১, ২০২৭ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | LoRA/QLoRA + Llama 3.1 fine-tuning |
| 💻 মূল Project | Bengali Business Llama 3.1 8B |
| 💻 GPU | E2E Networks / RunPod A100 (₹৫,০০০-১০,০০০) |
| 📚 মূল Resource | HuggingFace PEFT + mlabonne guide |

---

## 🗓️ Week-by-Week Plan

### Week 49 — LoRA Theory

```python
# lora_explained.py
import torch
import torch.nn as nn

class LoRALayer(nn.Module):
    """
    LoRA: Low-Rank Adaptation
    W_new = W_original + B * A
    where B (d x r) and A (r x d) are low-rank matrices
    r << d (rank is much smaller than dimension)
    """
    def __init__(self, original_layer, rank=4, alpha=32):
        super().__init__()
        self.original = original_layer
        self.rank = rank
        self.alpha = alpha
        d_out, d_in = original_layer.weight.shape
        self.scale = alpha / rank

        # Low-rank matrices (much fewer parameters!)
        self.lora_A = nn.Linear(d_in, rank, bias=False)
        self.lora_B = nn.Linear(rank, d_out, bias=False)

        # Initialize: A random, B zeros (so initial output = original)
        nn.init.kaiming_uniform_(self.lora_A.weight)
        nn.init.zeros_(self.lora_B.weight)

        # Freeze original weights
        for param in self.original.parameters():
            param.requires_grad = False

    def forward(self, x):
        original_out = self.original(x)
        lora_out = self.lora_B(self.lora_A(x)) * self.scale
        return original_out + lora_out

# Comparison
d_model = 4096  # LLaMA hidden size
original_params = d_model * d_model
lora_params = d_model * 8 + 8 * d_model  # rank=8
print(f"Original linear layer: {original_params:,} parameters")
print(f"LoRA layer (r=8): {lora_params:,} parameters")
print(f"Parameter reduction: {original_params/lora_params:.0f}x fewer!")
```

### Week 50 — QLoRA Setup

```python
# qlora_setup.py
# Install: pip install transformers peft bitsandbytes accelerate trl

from transformers import (AutoModelForCausalLM, AutoTokenizer,
                           BitsAndBytesConfig, TrainingArguments)
from peft import LoraConfig, get_peft_model, prepare_model_for_kbit_training
from trl import SFTTrainer
import torch

# QLoRA Configuration (4-bit quantization)
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_use_double_quant=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.bfloat16,
)

# Load Llama 3.1 8B in 4-bit
model_name = "meta-llama/Meta-Llama-3.1-8B"

print("Loading Llama 3.1 8B with 4-bit quantization...")
# model = AutoModelForCausalLM.from_pretrained(
#     model_name,
#     quantization_config=bnb_config,
#     device_map="auto",
# )
# tokenizer = AutoTokenizer.from_pretrained(model_name)

# LoRA Config
lora_config = LoraConfig(
    r=16,                    # Rank
    lora_alpha=32,           # Scale
    target_modules=["q_proj", "v_proj", "k_proj", "o_proj",
                    "gate_proj", "up_proj", "down_proj"],
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM",
)

print("LoRA config ready!")
print(f"Rank: {lora_config.r}")
print(f"Target modules: {lora_config.target_modules}")
```

### Week 51-52 — Fine-tune for Bengali Business

```python
# bengali_business_dataset.py
# Create instruction dataset for SmartStock AI

training_examples = [
    {
        "instruction": "স্টক যোগ করুন",
        "input": "১০০ কেজি বাসমতি চাল, ক্রয়মূল্য ৬০ টাকা/কেজি, supplier করিম ভাই",
        "output": "✅ স্টক আপডেট হয়েছে:\n- পণ্য: বাসমতি চাল\n- পরিমাণ: ১০০ কেজি\n- ক্রয়মূল্য: ৬০ টাকা/কেজি\n- মোট বিনিয়োগ: ৬,০০০ টাকা\n- Supplier: করিম ভাই\nবিক্রয়মূল্য কত রাখবেন?"
    },
    {
        "instruction": "বিক্রয় রিপোর্ট",
        "input": "আজকের বিক্রয় দেখাও",
        "output": "📊 আজকের বিক্রয় রিপোর্ট:\n- মোট বিক্রয়: ১৫,৪৫০ টাকা\n- মোট transaction: ৩২টি\n- সেরা পণ্য: বাসমতি চাল (৮,০০০ টাকা)\n- নিট লাভ: ৩,৮০০ টাকা\nগতকালের তুলনায় ১২% বেশি! 📈"
    },
    {
        "instruction": "stock alert",
        "input": "কোন পণ্যের stock কম?",
        "output": "⚠️ কম stock সতর্কতা:\n🔴 চিনি: মাত্র ৩ কেজি বাকি (minimum: ১০ কেজি)\n🟡 তেল: মাত্র ৫ লিটার বাকি (minimum: ১০ লিটার)\n🟡 ময়দা: মাত্র ৮ কেজি বাকি\n\nআজই order দেওয়া উচিত!"
    },
]

def format_instruction(example):
    """Alpaca-style instruction formatting"""
    return f"""### Instruction:
{example['instruction']}

### Input:
{example['input']}

### Response:
{example['output']}"""

for ex in training_examples[:1]:
    print(format_instruction(ex))
    print()
```

---

## ✅ Month 13 Checklist

- [ ] LoRA paper পড়া (arXiv:2106.09685)
- [ ] QLoRA paper পড়া (arXiv:2305.14314)
- [ ] PEFT library ব্যবহার করা
- [ ] Bengali business dataset তৈরি করা (500+ examples)
- [ ] Llama 3.1 8B fine-tune করা
- [ ] Model HuggingFace-এ upload করা
- [ ] E2E Networks-এ GPU rent করা

---

[← Month 12](month-12-llm-fundamentals.md) | [Month 14 →](month-14-rag-advanced.md)
