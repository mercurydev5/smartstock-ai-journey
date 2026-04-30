# ⚙️ Month 16: Training Pipeline (আগস্ট ২০২৭)

> **"Training শুরু করার সময়! এটাই journey-র সবচেয়ে exciting moment!"** 💪🇮🇳🚀

[← Month 15](month-15-architecture.md) | [← Phase 6](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | আগস্ট ১-৩১, ২০২৭ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | SmartStock AI Brain training |
| 💻 মূল Output | Trained SmartStock-7B base model |
| 🖥️ Hardware | E2E Networks A100 80GB |

---

## 🗓️ Week-by-Week Plan

### Week 61 — Training Setup

```python
# training_config.py
from dataclasses import dataclass

@dataclass
class SmartStockTrainingConfig:
    """SmartStock AI Training Configuration"""

    # Model Architecture
    model_name: str = "SmartStock-7B"
    vocab_size: int = 65536
    hidden_size: int = 4096
    num_layers: int = 32
    num_attention_heads: int = 32
    num_key_value_heads: int = 8
    intermediate_size: int = 11008
    max_position_embeddings: int = 4096
    rms_norm_eps: float = 1e-5

    # Training
    learning_rate: float = 3e-4
    min_lr: float = 3e-5
    warmup_steps: int = 2000
    max_steps: int = 100000
    batch_size: int = 32
    gradient_accumulation_steps: int = 4
    weight_decay: float = 0.1
    gradient_clip: float = 1.0

    # Hardware
    precision: str = "bf16"
    fsdp: bool = True  # Fully Sharded Data Parallel

    # Data
    dataset_path: str = "./data/bengali_hindi_corpus"
    context_length: int = 2048

config = SmartStockTrainingConfig()
effective_batch_size = config.batch_size * config.gradient_accumulation_steps
print(f"Effective batch size: {effective_batch_size}")
print(f"Training for {config.max_steps:,} steps")
print(f"Estimated training time on A100: ~72 hours")
```

### Week 62-63 — Training Loop

```python
# train_smartstock.py
import torch
import torch.nn as nn
from torch.utils.data import DataLoader
import wandb

class SmartStockTrainer:
    """Training loop for SmartStock AI"""

    def __init__(self, model, config, train_dataloader):
        self.model = model
        self.config = config
        self.train_dataloader = train_dataloader

        # Optimizer with cosine LR schedule
        self.optimizer = torch.optim.AdamW(
            model.parameters(),
            lr=config.learning_rate,
            weight_decay=config.weight_decay,
            betas=(0.9, 0.95),
        )

        # Initialize W&B tracking
        wandb.init(project="smartstock-ai", name="smartstock-7b-v1")

    def train_step(self, batch):
        input_ids = batch["input_ids"].to("cuda")
        labels = batch["labels"].to("cuda")

        with torch.cuda.amp.autocast(dtype=torch.bfloat16):
            outputs = self.model(input_ids)
            logits = outputs.logits
            shift_logits = logits[..., :-1, :].contiguous()
            shift_labels = labels[..., 1:].contiguous()
            loss = nn.CrossEntropyLoss()(
                shift_logits.view(-1, self.config.vocab_size),
                shift_labels.view(-1)
            )

        return loss

    def train(self, max_steps):
        self.model.train()
        total_loss = 0

        for step, batch in enumerate(self.train_dataloader):
            if step >= max_steps:
                break

            loss = self.train_step(batch)
            loss.backward()

            if (step + 1) % self.config.gradient_accumulation_steps == 0:
                torch.nn.utils.clip_grad_norm_(self.model.parameters(), 1.0)
                self.optimizer.step()
                self.optimizer.zero_grad()

            total_loss += loss.item()

            if step % 100 == 0:
                avg_loss = total_loss / (step + 1)
                wandb.log({"train/loss": avg_loss, "step": step})
                print(f"Step {step}: loss={avg_loss:.4f}")

print("✅ SmartStock AI Training pipeline ready!")
```

### Week 64 — Monitoring & Evaluation

```python
# evaluate.py
import torch
from transformers import AutoTokenizer

def evaluate_smartstock(model, tokenizer, prompts):
    """Evaluate SmartStock model on business prompts"""
    model.eval()

    print("📊 SmartStock AI Evaluation:")
    print("=" * 60)

    for prompt in prompts:
        inputs = tokenizer(prompt, return_tensors="pt").to("cuda")
        with torch.no_grad():
            outputs = model.generate(
                **inputs,
                max_new_tokens=200,
                temperature=0.7,
                do_sample=True,
                pad_token_id=tokenizer.eos_token_id,
            )
        response = tokenizer.decode(outputs[0][inputs["input_ids"].shape[1]:],
                                    skip_special_tokens=True)
        print(f"\n👤 User: {prompt}")
        print(f"🤖 SmartStock AI: {response}")
        print("-" * 40)

test_prompts = [
    "আজকের বিক্রয় কত হয়েছে?",
    "চালের stock কম আছে। কী করব?",
    "এই মাসে সবচেয়ে বেশি কী বিক্রি হয়েছে?",
]

print("Run evaluate_smartstock(model, tokenizer, test_prompts)")
```

---

## ✅ Month 16 Checklist

- [ ] Training config finalize করা
- [ ] E2E Networks A100 rent করা
- [ ] Training শুরু করা
- [ ] W&B দিয়ে loss track করা
- [ ] Checkpoint save করা (প্রতি ১০,০০০ steps)
- [ ] Base model evaluation করা
- [ ] Model HuggingFace-এ upload করা

---

[← Month 15](month-15-architecture.md) | [Month 17 →](month-17-optimization.md)
