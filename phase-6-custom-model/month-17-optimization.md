# ⚡ Month 17: Optimization & Deployment (সেপ্টেম্বর ২০২৭)

> **"Model তৈরি হয়েছে! এখন এটাকে production-ready করার সময়!"** 💪🇮🇳🚀

[← Month 16](month-16-training-pipeline.md) | [← Phase 6](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | সেপ্টেম্বর ১-৩০, ২০২৭ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | Quantization + vLLM + Benchmarks |
| 💻 মূল Output | Production-ready SmartStock AI API |

---

## 🗓️ Week-by-Week Plan

### Week 65 — Quantization

```python
# quantization.py
"""
Model Quantization: FP32 → INT4/INT8
Reduces model size by 4-8x, speeds up inference 2-3x
"""

# Option 1: GPTQ (Post-training quantization)
# pip install auto-gptq
from auto_gptq import AutoGPTQForCausalLM, BaseQuantizeConfig
from transformers import AutoTokenizer

def quantize_with_gptq(model_path, output_path):
    """Quantize model to 4-bit using GPTQ"""
    quantize_config = BaseQuantizeConfig(
        bits=4,
        group_size=128,
        desc_act=False,
    )

    model = AutoGPTQForCausalLM.from_pretrained(model_path, quantize_config)
    tokenizer = AutoTokenizer.from_pretrained(model_path)

    # Calibration data (Bengali business sentences)
    calibration_data = [
        "আজকের বিক্রয় রিপোর্ট দাও",
        "চালের stock কত আছে?",
        "এই মাসের লাভ কত?",
    ]
    examples = [tokenizer(text, return_tensors="pt") for text in calibration_data]

    model.quantize(examples)
    model.save_quantized(output_path)
    print(f"✅ Quantized model saved to {output_path}")

# Size comparison
print("Model size comparison:")
print("  FP16 (7B): ~14GB")
print("  INT8 (7B): ~7GB")
print("  INT4/GPTQ (7B): ~4GB ← Use this!")
```

### Week 66 — vLLM Deployment

```python
# vllm_deployment.py
"""
vLLM: Very Large Language Model inference
Features:
- PagedAttention (efficient KV cache)
- Continuous batching
- Tensor parallelism
- OpenAI-compatible API
"""

# Install: pip install vllm

# Start vLLM server
import subprocess
def start_vllm_server(model_path, port=8000, tensor_parallel=1):
    cmd = [
        "python", "-m", "vllm.entrypoints.openai.api_server",
        "--model", model_path,
        "--port", str(port),
        "--tensor-parallel-size", str(tensor_parallel),
        "--quantization", "gptq",
        "--max-model-len", "4096",
        "--dtype", "float16",
    ]
    print(f"Starting vLLM server: {' '.join(cmd)}")

# FastAPI wrapper for SmartStock
from fastapi import FastAPI
from pydantic import BaseModel
import httpx

app = FastAPI(title="SmartStock AI API", version="1.0.0")

class ChatRequest(BaseModel):
    message: str
    conversation_id: str = None
    language: str = "bengali"

@app.post("/chat")
async def chat(request: ChatRequest):
    """Main chat endpoint"""
    system_prompt = """আপনি SmartStock AI — একটি বাংলা ব্যবসায়িক সহকারী।
    আপনি ছোট ব্যবসার মালিকদের stock management, sales tracking, এবং business analytics-এ সাহায্য করেন।
    সবসময় বাংলায় উত্তর দিন।"""

    # Call vLLM API
    async with httpx.AsyncClient() as client:
        response = await client.post(
            "http://localhost:8000/v1/chat/completions",
            json={
                "model": "smartstock-7b",
                "messages": [
                    {"role": "system", "content": system_prompt},
                    {"role": "user", "content": request.message}
                ],
                "max_tokens": 512,
                "temperature": 0.7,
            }
        )
    return {"response": response.json()["choices"][0]["message"]["content"]}

print("✅ SmartStock AI API ready!")
print("Run: uvicorn vllm_deployment:app --host 0.0.0.0 --port 8080")
```

### Week 67-68 — Evaluation & Model Card

```python
# benchmarks.py
"""
SmartStock AI Evaluation Benchmarks
"""

BENCHMARK_TASKS = {
    "Bengali intent classification": {
        "dataset": "smartstock_intents",
        "metric": "accuracy",
        "target": "> 95%",
    },
    "Business entity extraction": {
        "dataset": "smartstock_ner",
        "metric": "F1",
        "target": "> 90%",
    },
    "Bengali response quality": {
        "dataset": "smartstock_qa",
        "metric": "BLEU + human eval",
        "target": "BLEU > 0.4",
    },
    "Hallucination rate": {
        "dataset": "smartstock_factual",
        "metric": "factuality rate",
        "target": "< 5% hallucination",
    },
    "Inference speed": {
        "hardware": "A10G GPU",
        "metric": "tokens/second",
        "target": "> 50 tokens/s",
    },
}

print("SmartStock AI Benchmark Suite:")
for task, info in BENCHMARK_TASKS.items():
    print(f"\n📊 {task}")
    for k, v in info.items():
        print(f"   {k}: {v}")
```

---

## 🎊 Phase 6 Complete! আপনার নিজের AI তৈরি!

**আপনি তৈরি করেছেন:**
✅ Custom Bengali/Hindi tokenizer
✅ SmartStock-7B pre-trained model
✅ Quantized (INT4) production model
✅ vLLM deployment
✅ FastAPI backend

**Phase 7: LAUNCH এর সময়!** 🚀

---

## ✅ Month 17 Checklist

- [ ] GPTQ quantization করা
- [ ] vLLM server setup করা
- [ ] FastAPI wrapper তৈরি করা
- [ ] Benchmarks run করা
- [ ] Model card লেখা
- [ ] HuggingFace-এ model publish করা ✅

---

[← Month 16](month-16-training-pipeline.md) | [Phase 7 →](../phase-7-launch/README.md)
