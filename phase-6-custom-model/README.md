# 🔬 Phase 6: Custom Model (মাস ১৫-১৭)

> **"এটাই সেই moment — নিজের AI model তৈরি করবো!"** 💪🇮🇳🚀

[← Back to Main README](../README.md)

---

## 📋 Phase Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | জুলাই-সেপ্টেম্বর ২০২৭ |
| 📆 মাস | মাস ১৫-১৭ |
| ⏰ মোট ঘণ্টা | ~১৮০ ঘণ্টা |
| 🎯 মূল লক্ষ্য | Custom 7B Bengali/Hindi business LLM |
| 💻 মূল Project | **SmartStock AI Brain v1** |
| 💰 Budget | ~₹৩০,০০০-৪০,০০০ (GPU rental) |

---

## 🗓️ Month Breakdown

### Month 15 (জুলাই ২০২৭) — Architecture Design
→ [month-15-architecture.md](month-15-architecture.md)
- SmartStock AI architecture finalize করা
- Bengali/Hindi business corpus collect করা
- Data cleaning ও preprocessing
- Tokenizer design (BPE for Bengali/Hindi)

### Month 16 (আগস্ট ২০২৭) — Training Pipeline
→ [month-16-training-pipeline.md](month-16-training-pipeline.md)
- Distributed training setup
- Pre-training on corpus
- **SmartStock AI Brain v1 training** 🧠
- Weights & Biases tracking

### Month 17 (সেপ্টেম্বর ২০২৭) — Optimization
→ [month-17-optimization.md](month-17-optimization.md)
- Quantization (GPTQ, AWQ)
- vLLM deployment
- Evaluation benchmarks
- Model card লেখা

---

## 🎯 Phase 6 শেষে আপনি পারবেন:

✅ Bengali/Hindi-focused LLM design করতে
✅ Custom tokenizer তৈরি করতে
✅ Large-scale training pipeline চালাতে
✅ Model quantize ও optimize করতে
✅ Production deployment করতে

---

## 🖥️ Hardware Requirements

| Task | Hardware | Cost |
|------|---------|------|
| Tokenizer training | CPU (free) | ₹০ |
| Data preprocessing | Colab Pro | ₹৮০০/month |
| Model pre-training | A100 80GB (E2E) | ₹৩০,০০০-৪০,০০০ |
| Fine-tuning | T4/V100 (RunPod) | ₹৫,০০০-১০,০০০ |
| Inference | CPU quantized | ₹০ |

> 💡 **টিপস:** E2E Networks-এ spot instance ব্যবহার করুন — ৪০-৫০% সাশ্রয়!
> **URL:** https://e2enetworks.com/

---

[← Phase 5](../phase-5-llms-finetuning/README.md) | [Month 15 →](month-15-architecture.md) | [Phase 7 →](../phase-7-launch/README.md)
