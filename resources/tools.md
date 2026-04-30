# 🛠️ Tools - SmartStock AI Learning Journey

> **"সঠিক tool সঠিক কাজের জন্য! এই list bookmark করে রাখুন!"**

[← Resources README](./README.md) | [← Main README](../README.md)

---

## 💻 Programming Languages

| Tool | Use | When to Learn | Link |
|------|-----|---------------|------|
| **Python** ⭐ | Everything | Month 1 | https://python.org |
| **JavaScript/TypeScript** ⭐ | Frontend (Next.js) | Month 17-18 | https://typescriptlang.org |
| SQL | Database queries | Month 2-3 | - |
| Bash/Shell | Automation | Month 3+ | - |

---

## 📝 IDEs & Development Environment

| Tool | Use | Free? | Link |
|------|-----|-------|------|
| **VS Code** ⭐⭐⭐ | Primary IDE | ✅ Free | https://code.visualstudio.com |
| **Jupyter Notebook** ⭐⭐⭐ | Data exploration | ✅ Free | https://jupyter.org |
| **Google Colab** ⭐⭐⭐ | Free GPU (T4) | ✅ Free | https://colab.research.google.com |
| Colab Pro | Better GPU (A100) | ₹800/month | https://colab.research.google.com |
| PyCharm | Python IDE | ✅ Community free | https://jetbrains.com/pycharm |

### VS Code Extensions for ML
```
Python (Microsoft) ⭐
Pylance ⭐
Jupyter ⭐
GitLens
Docker
Thunder Client (API testing)
GitHub Copilot (₹1000/month but worth it for productivity)
```

---

## 🔀 Version Control

| Tool | Use | Link |
|------|-----|------|
| **Git** ⭐⭐⭐ | Version control | https://git-scm.com |
| **GitHub** ⭐⭐⭐ | Code hosting, portfolio | https://github.com |
| DVC | Data version control | https://dvc.org |

```bash
# Essential Git commands
git init
git add .
git commit -m "feat: add Bengali sentiment model"
git push origin main
git branch feature/rag-system
```

---

## 🔥 Deep Learning Frameworks

| Framework | Use | When to Learn | Link |
|-----------|-----|---------------|------|
| **PyTorch** ⭐⭐⭐ | Primary DL framework | Month 4-5 | https://pytorch.org |
| TensorFlow/Keras | Alternative to PyTorch | Optional | https://tensorflow.org |
| JAX | High-performance ML | Month 15+ (optional) | https://github.com/google/jax |

### Why PyTorch First?
```
✅ More Pythonic (feels natural)
✅ Dominates research community
✅ Better debugging
✅ Used by: Meta, OpenAI, Hugging Face, etc.
✅ Most tutorials use PyTorch
```

---

## 🤗 LLM Libraries & Tools

| Tool | Use | Link |
|------|-----|------|
| **Hugging Face Transformers** ⭐⭐⭐ | Models, fine-tuning | https://huggingface.co/transformers |
| **PEFT** ⭐⭐⭐ | LoRA, QLoRA fine-tuning | https://github.com/huggingface/peft |
| **TRL** ⭐⭐⭐ | SFT, RLHF, DPO training | https://github.com/huggingface/trl |
| **bitsandbytes** ⭐⭐ | 4-bit/8-bit quantization | https://github.com/TimDettmers/bitsandbytes |
| **Ollama** ⭐⭐⭐ | Run LLMs locally | https://ollama.ai |
| **vLLM** ⭐⭐ | Fast LLM serving | https://github.com/vllm-project/vllm |
| **llama.cpp** ⭐⭐ | CPU inference | https://github.com/ggerganov/llama.cpp |
| **TGI** ⭐⭐ | Text Generation Inference | https://github.com/huggingface/text-generation-inference |

---

## 🔗 LLM Frameworks

| Framework | Use | Link |
|-----------|-----|------|
| **LangChain** ⭐⭐⭐ | LLM chains, agents, RAG | https://langchain.com |
| **LlamaIndex** ⭐⭐ | Data + LLM (RAG) | https://llamaindex.ai |
| **Haystack** ⭐⭐ | Search + LLM pipelines | https://haystack.deepset.ai |
| **DSPy** ⭐ | Programming LM prompts | https://github.com/stanfordnlp/dspy |

---

## 🗄️ Vector Databases

| DB | Use | Free Tier | Link |
|----|-----|-----------|------|
| **Qdrant** ⭐⭐⭐ | Open-source, fast | ✅ Self-host | https://qdrant.tech |
| Chroma | Simple, great for dev | ✅ Local | https://trychroma.com |
| Pinecone | Managed cloud | ✅ Free tier | https://pinecone.io |
| Weaviate | GraphQL support | ✅ Free tier | https://weaviate.io |
| Milvus | High-scale | ✅ Open source | https://milvus.io |

### Recommendation:
```
Development: Chroma (simple, local)
Production: Qdrant (self-hosted on E2E Networks)
```

---

## 📊 Experiment Tracking & MLOps

| Tool | Use | Free? | Link |
|------|-----|-------|------|
| **Weights & Biases** ⭐⭐⭐ | Experiment tracking | ✅ Free for individuals | https://wandb.ai |
| MLflow | Open-source tracking | ✅ Free | https://mlflow.org |
| Neptune | Team tracking | Free tier | https://neptune.ai |
| DagsHub | Git + DVC + MLflow | ✅ Free | https://dagshub.com |

```python
# W&B quick setup
import wandb
wandb.init(project="smartstock-bengali-model", config={"lr": 1e-4})
wandb.log({"loss": 0.5, "accuracy": 0.85})
```

---

## 🚀 Deployment Tools

### Backend
| Tool | Use | When | Link |
|------|-----|------|------|
| **FastAPI** ⭐⭐⭐ | Python REST API | Month 17 | https://fastapi.tiangolo.com |
| **Streamlit** ⭐⭐⭐ | Quick ML demos | Month 3+ | https://streamlit.io |
| Gradio | ML demos | Month 8+ | https://gradio.app |
| Flask | Simple Python API | Optional | https://flask.palletsprojects.com |

### Frontend
| Tool | Use | When | Link |
|------|-----|------|------|
| **Next.js 14** ⭐⭐⭐ | React framework | Month 17-18 | https://nextjs.org |
| React Native (Expo) | Mobile app | Month 18 | https://expo.dev |
| Tailwind CSS | Styling | Month 17+ | https://tailwindcss.com |

### DevOps
| Tool | Use | Link |
|------|-----|------|
| **Docker** ⭐⭐⭐ | Containerization | https://docker.com |
| Docker Compose | Multi-container | https://docs.docker.com/compose |
| GitHub Actions | CI/CD | https://github.com/features/actions |
| Vercel | Frontend hosting (free) | https://vercel.com |
| Render | Backend hosting (free tier) | https://render.com |

---

## ☁️ Cloud & GPU Providers

### Indian Providers (Cheaper!)
| Provider | GPU | Cost | Link |
|----------|-----|------|------|
| **E2E Networks** ⭐⭐⭐ | A100 80GB | ~₹40K/month | https://www.e2enetworks.com |
| Yotta | H100 | ₹50K+/month | https://yotta.com |
| Jarvislabs | A100, RTX | ₹2K-15K/month | https://jarvislabs.ai |
| Tata Communications | Enterprise | Contact | https://www.tatacommunications.com |

### International (Good for smaller GPUs)
| Provider | GPU | Cost | Link |
|----------|-----|------|------|
| **Hetzner** ⭐⭐ | RTX 4090 | ~₹12K/month | https://hetzner.com/cloud |
| RunPod | A100, H100 | $0.74/hr (A100) | https://runpod.io |
| Lambda Labs | A100 | $1.10/hr | https://lambdalabs.com |
| Vast.ai | Various | Cheapest spot | https://vast.ai |

### Big Cloud
| Provider | Cheapest GPU | Link |
|----------|-------------|------|
| Google Cloud | T4 (Colab) free | https://cloud.google.com |
| AWS | g4dn.xlarge ~₹30/hr | https://aws.amazon.com |
| Azure | NC6 ~₹25/hr | https://azure.microsoft.com |

---

## 🇮🇳 Indic NLP Tools

| Tool | Use | Link |
|------|-----|------|
| **AI4Bharat tools** ⭐⭐⭐ | Indic NLP | https://ai4bharat.org |
| IndicNLP Library | Bengali NLP | https://github.com/AI4Bharat/indicnlp_corpus |
| IndicTrans2 | Indic translation | https://github.com/AI4Bharat/IndicTrans2 |
| Anudesh | Indic instruction data | https://huggingface.co/datasets/ai4bharat/anudesh |
| Dhruva | Indic AI API | https://bhashini.gov.in/dhruva |
| Bhashini | Government Indic AI | https://bhashini.gov.in |

---

## 🔊 Speech Tools (for Voice Features)

| Tool | Use | Link |
|------|-----|------|
| **OpenAI Whisper** ⭐⭐⭐ | Speech-to-text (Bengali!) | https://github.com/openai/whisper |
| ElevenLabs | Text-to-speech | https://elevenlabs.io |
| Coqui TTS | Open-source TTS | https://github.com/coqui-ai/TTS |
| IndicTTS | Indic TTS (AI4Bharat) | https://models.ai4bharat.org/#/tts |

---

> 💪🇮🇳🚀 **সব tools একসাথে শিখতে যাবেন না! Phase অনুযায়ী যা দরকার তা শিখুন!**
