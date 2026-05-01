# 🚀 Advanced Projects (Phase 5-7)

> **"আপনি এখন LLM-এর জগতে! এই projects আপনাকে industry-level AI engineer বানাবে!"**

[← Projects README](./README.md) | [← Main README](../README.md)

---

## Overview

| | Details |
|---|---|
| **Phase** | 5-7 (Month 13-20) |
| **Projects** | 8 projects |
| **Skills** | LLMs, Fine-tuning, RAG, Full-stack AI |
| **GPU Required** | Yes - from Month 15+ (E2E Networks, Colab Pro) |
| **Estimated Time** | 8 months (2 hrs/day) |

---

## 🧠 Project 20: Bengali nanoGPT

### 🎯 Objective
Andrej Karpathy-এর nanoGPT architecture দিয়ে Bengali language model train করুন।

### 📚 Skills Practiced
- GPT architecture (decoder-only transformer)
- Character-level tokenization vs BPE
- Pretraining on Bengali Wikipedia
- Text generation with temperature
- Perplexity evaluation
- GPU optimization techniques

### 💻 Starter Code Outline
```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import math

# Hyperparameters for Bengali nanoGPT
# Small version to train on Colab/local GPU
config = {
    "vocab_size": 8000,      # BPE vocabulary
    "n_embd": 256,           # Embedding dimension
    "n_head": 8,             # Attention heads
    "n_layer": 6,            # Transformer layers
    "block_size": 256,       # Context length
    "dropout": 0.1,
    "batch_size": 32,
    "learning_rate": 3e-4,
    "max_iters": 5000,
}

class CausalSelfAttention(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.n_head = config["n_head"]
        self.n_embd = config["n_embd"]
        self.c_attn = nn.Linear(config["n_embd"], 3 * config["n_embd"])
        self.c_proj = nn.Linear(config["n_embd"], config["n_embd"])
        self.dropout = nn.Dropout(config["dropout"])
        # Causal mask - can't look into future!
        self.register_buffer("bias", torch.tril(
            torch.ones(config["block_size"], config["block_size"])
        ).view(1, 1, config["block_size"], config["block_size"]))

    def forward(self, x):
        B, T, C = x.size()
        q, k, v = self.c_attn(x).split(self.n_embd, dim=2)
        k = k.view(B, T, self.n_head, C // self.n_head).transpose(1, 2)
        q = q.view(B, T, self.n_head, C // self.n_head).transpose(1, 2)
        v = v.view(B, T, self.n_head, C // self.n_head).transpose(1, 2)

        # Flash attention (PyTorch 2.0+) - much faster!
        y = F.scaled_dot_product_attention(q, k, v, is_causal=True,
                                           dropout_p=self.dropout.p if self.training else 0)
        y = y.transpose(1, 2).contiguous().view(B, T, C)
        return self.c_proj(y)

class Block(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.ln_1 = nn.LayerNorm(config["n_embd"])
        self.attn = CausalSelfAttention(config)
        self.ln_2 = nn.LayerNorm(config["n_embd"])
        self.mlp = nn.Sequential(
            nn.Linear(config["n_embd"], 4 * config["n_embd"]),
            nn.GELU(),
            nn.Linear(4 * config["n_embd"], config["n_embd"]),
            nn.Dropout(config["dropout"])
        )

    def forward(self, x):
        x = x + self.attn(self.ln_1(x))
        x = x + self.mlp(self.ln_2(x))
        return x

class BengaliGPT(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.transformer = nn.ModuleDict({
            'wte': nn.Embedding(config["vocab_size"], config["n_embd"]),
            'wpe': nn.Embedding(config["block_size"], config["n_embd"]),
            'drop': nn.Dropout(config["dropout"]),
            'h': nn.ModuleList([Block(config) for _ in range(config["n_layer"])]),
            'ln_f': nn.LayerNorm(config["n_embd"])
        })
        self.lm_head = nn.Linear(config["n_embd"], config["vocab_size"], bias=False)

    def forward(self, idx, targets=None):
        B, T = idx.size()
        pos = torch.arange(T, device=idx.device)
        x = self.transformer.wte(idx) + self.transformer.wpe(pos)
        x = self.transformer.drop(x)
        for block in self.transformer.h:
            x = block(x)
        x = self.transformer.ln_f(x)
        logits = self.lm_head(x)

        loss = None
        if targets is not None:
            loss = F.cross_entropy(logits.view(-1, logits.size(-1)), targets.view(-1))
        return logits, loss

    @torch.no_grad()
    def generate(self, idx, max_new_tokens, temperature=0.8, top_k=40):
        for _ in range(max_new_tokens):
            idx_cond = idx[:, -config["block_size"]:]
            logits, _ = self(idx_cond)
            logits = logits[:, -1, :] / temperature
            if top_k is not None:
                v, _ = torch.topk(logits, min(top_k, logits.size(-1)))
                logits[logits < v[:, [-1]]] = -float('Inf')
            probs = F.softmax(logits, dim=-1)
            idx_next = torch.multinomial(probs, num_samples=1)
            idx = torch.cat((idx, idx_next), dim=1)
        return idx

# Dataset preparation
"""
Bengali Wikipedia download:
wget https://dumps.wikimedia.org/bnwiki/latest/bnwiki-latest-pages-articles.xml.bz2
python -m wikiextractor.WikiExtractor bnwiki-latest-articles.xml.bz2

Or use Hugging Face datasets:
from datasets import load_dataset
dataset = load_dataset("wikipedia", "20220301.bn")
"""
```

### ✅ Completion Criteria
- [ ] Model trains on Bengali Wikipedia text
- [ ] Generates coherent Bengali sentences
- [ ] Perplexity < 50 on validation set
- [ ] Multiple temperature settings tested
- [ ] Training loss curve looks healthy
- [ ] Demo: Generate Bengali text from prompts
- [ ] Share model on Hugging Face Hub!

### 🚀 Bonus Challenges
1. BPE tokenizer with SentencePiece
2. Multi-GPU training with DDP
3. Evaluate on Bengali NLP benchmarks
4. Scale up to 100M parameters

---

## 🦙 Project 21: Llama 3.1 8B QLoRA Fine-tuning

### 🎯 Objective
Meta's Llama 3.1 8B-কে QLoRA দিয়ে kirana business domain-এ fine-tune করুন।

### 📚 Skills Practiced
- QLoRA (Quantized Low-Rank Adaptation)
- 4-bit quantization (bitsandbytes)
- LoRA rank, alpha, target modules
- Instruction fine-tuning format
- Hugging Face PEFT library
- Merging LoRA weights

### 💻 Starter Code Outline
```python
import torch
from transformers import (
    AutoModelForCausalLM, AutoTokenizer,
    TrainingArguments, BitsAndBytesConfig
)
from peft import LoraConfig, get_peft_model, prepare_model_for_kbit_training
from trl import SFTTrainer
from datasets import Dataset

# QLoRA configuration
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_use_double_quant=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.bfloat16
)

model_name = "meta-llama/Meta-Llama-3.1-8B-Instruct"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    quantization_config=bnb_config,
    device_map="auto"
)

# LoRA configuration
lora_config = LoraConfig(
    r=16,               # Rank - lower = fewer params
    lora_alpha=32,      # Scaling factor
    target_modules=["q_proj", "v_proj", "k_proj", "o_proj"],
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM"
)

model = prepare_model_for_kbit_training(model)
model = get_peft_model(model, lora_config)
model.print_trainable_parameters()
# Expected: ~0.5% of total parameters!

# Training dataset format (Llama 3.1 chat template)
def format_instruction(sample):
    return f"""<|begin_of_text|><|start_header_id|>system<|end_header_id|>
আপনি SmartStock AI, একটি intelligent inventory management assistant।
আপনি Bengali, Hindi এবং English-এ কথা বলতে পারেন।<|eot_id|>
<|start_header_id|>user<|end_header_id|>
{sample['instruction']}<|eot_id|>
<|start_header_id|>assistant<|end_header_id|>
{sample['output']}<|eot_id|>"""

# Training data examples
training_data = [
    {
        "instruction": "আমার দোকানে কোন পণ্যগুলো সবচেয়ে বেশি বিক্রয় হয়?",
        "output": "গত ৩০ দিনে আপনার top ৫টি পণ্য:\n"
                  "১. বাসমতি চাল ৫কেজি - ৪৫টি বিক্রয় - ₹৬,৭৫০ revenue\n"
                  "২. সরষের তেল ১লি - ৩৮টি - ₹৩,৮০০\n"
                  "৩. চিনি ১কেজি - ৩২টি - ₹১,৬০০\n"
                  "Diwali-র আগে এই পণ্যগুলো extra stock রাখুন! 🛒"
    },
    # Add 500+ high-quality examples
]

# Training
training_args = TrainingArguments(
    output_dir="./smartstock-llama",
    num_train_epochs=3,
    per_device_train_batch_size=4,
    gradient_accumulation_steps=4,
    learning_rate=2e-4,
    fp16=True,
    logging_steps=10,
    save_strategy="epoch",
    warmup_ratio=0.03,
    lr_scheduler_type="cosine"
)

trainer = SFTTrainer(
    model=model,
    args=training_args,
    train_dataset=Dataset.from_list(training_data),
    tokenizer=tokenizer,
    formatting_func=format_instruction,
    max_seq_length=2048
)

trainer.train()
```

### ✅ Completion Criteria
- [ ] 500+ high-quality training examples created
- [ ] Llama 3.1 8B fine-tuned with QLoRA
- [ ] Model responds accurately to business queries
- [ ] Bengali language responses natural
- [ ] Model uploaded to Hugging Face Hub
- [ ] Before/after comparison documented

### 🚀 Bonus Challenges
1. DPO (Direct Preference Optimization) further alignment
2. Multi-turn conversation fine-tuning
3. Quantize to GGUF for Ollama deployment
4. Benchmark against GPT-4 on business tasks

---

## 🔍 Project 22: RAG-Powered Business Assistant

### 🎯 Objective
LangChain + Qdrant + Llama দিয়ে একটি RAG system বানান।

### 📚 Skills Practiced
- RAG (Retrieval Augmented Generation) architecture
- Vector embeddings
- Qdrant vector database
- LangChain chains and agents
- Document chunking strategies
- Hybrid search (dense + sparse)

### 💻 Starter Code Outline
```python
from langchain.vectorstores import Qdrant
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.document_loaders import PyPDFLoader, TextLoader
from langchain.chains import RetrievalQA
from langchain_community.llms import Ollama
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams
import os

# Initialize Qdrant (local mode)
client = QdrantClient(path="./qdrant_storage")

# Create collection for business knowledge
client.create_collection(
    collection_name="smartstock_knowledge",
    vectors_config=VectorParams(size=768, distance=Distance.COSINE)
)

# Embedding model - multilingual for Bengali support!
embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/paraphrase-multilingual-mpnet-base-v2"
)

class SmartStockRAG:
    def __init__(self):
        self.vectorstore = Qdrant(
            client=client,
            collection_name="smartstock_knowledge",
            embeddings=embeddings
        )
        # Local LLM via Ollama
        self.llm = Ollama(
            model="smartstock-llama",  # Our fine-tuned model
            temperature=0.1
        )
        self.qa_chain = RetrievalQA.from_chain_type(
            llm=self.llm,
            chain_type="stuff",
            retriever=self.vectorstore.as_retriever(
                search_type="similarity",
                search_kwargs={"k": 5}
            ),
            return_source_documents=True
        )

    def add_business_documents(self, docs_path: str):
        """Business documents index করুন (invoices, catalogs, etc.)"""
        text_splitter = RecursiveCharacterTextSplitter(
            chunk_size=500,
            chunk_overlap=50,
            separators=["\n\n", "\n", "।", ".", " "]  # Bengali sentence separator!
        )
        # TODO: Load and split documents
        # TODO: Add to vectorstore

    async def query(self, question: str, user_id: str,
                    language: str = "bn") -> dict:
        """Answer business questions using RAG"""
        # Add language instruction to query
        if language == "bn":
            augmented_query = f"বাংলায় উত্তর দিন: {question}"
        else:
            augmented_query = question

        result = self.qa_chain({"query": augmented_query})
        return {
            "answer": result["result"],
            "sources": [doc.metadata for doc in result["source_documents"]],
            "language": language
        }

# Knowledge base content to index
knowledge_base = [
    """
    SmartStock AI Pricing:
    Free plan: 50 products, basic features, ₹0/month
    Basic plan: 500 products, analytics, WhatsApp bot, ₹299/month
    Pro plan: Unlimited products, AI forecasting, priority support, ₹799/month
    """,
    """
    How to add a product:
    1. Go to Inventory > Add Product
    2. Enter product name in Bengali or English
    3. Add barcode (scan or manual)
    4. Set current stock, minimum stock, price
    5. Click Save
    """,
    # Add 100+ knowledge base articles
]
```

### ✅ Completion Criteria
- [ ] 100+ knowledge base articles indexed
- [ ] Questions answered accurately with sources
- [ ] Bengali queries handled correctly
- [ ] Response latency < 3 seconds
- [ ] Multi-turn conversation context maintained
- [ ] Deployed as API

### 🚀 Bonus Challenges
1. Image-based queries (product photo → identify item)
2. CSV/Excel ingestion (invoice data)
3. Web search tool integration
4. Reranking with cross-encoder

---

## 🏗️ Project 23: SmartStock-7B Training (Custom LLM)

### 🎯 Objective
7 Billion parameter language model design এবং train করুন - specifically for Indian retail.

### 📚 Skills Practiced
- Large-scale model training
- Data curation for pretraining
- Multi-GPU distributed training (DDP/FSDP)
- Mixed precision training (bf16)
- Checkpointing and resumption
- Model evaluation and benchmarking

### 💻 Starter Code Outline
```python
import torch
import torch.distributed as dist
from torch.distributed.fsdp import FullyShardedDataParallel as FSDP
from torch.utils.data.distributed import DistributedSampler
from transformers import LlamaConfig, LlamaForCausalLM
import wandb

# SmartStock-7B Configuration
# Based on Llama-2 architecture with modifications
config = LlamaConfig(
    vocab_size=50000,          # Extended for Bengali/Hindi
    hidden_size=4096,
    intermediate_size=11008,
    num_hidden_layers=32,
    num_attention_heads=32,
    num_key_value_heads=8,     # GQA for efficiency
    max_position_embeddings=4096,
    rope_theta=10000.0,
    rms_norm_eps=1e-5,
)

# Training Data Mixture
"""
SmartStock-7B pretraining data:
- 40% Bengali Wikipedia + Bengali web text
- 20% Hindi text
- 20% English business/retail text
- 10% Indian product catalogs, invoices
- 10% Retail domain knowledge (inventory, supply chain)

Total: ~10B tokens
"""

def prepare_training_data():
    """Pretraining corpus prepare করুন"""
    datasets = {
        "bengali_wiki": load_bengali_wikipedia(),
        "hindi_text": load_hindi_corpus(),
        "english_retail": load_english_retail(),
        "indic_mix": load_indic_mix()
    }
    # Tokenize and shard into chunks
    return tokenize_and_shard(datasets)

# Multi-GPU training setup
def setup_distributed():
    dist.init_process_group(backend='nccl')
    torch.cuda.set_device(int(os.environ["LOCAL_RANK"]))

# Training loop with wandb logging
def train(config):
    wandb.init(project="smartstock-7b", config=config)
    model = LlamaForCausalLM(config)
    model = FSDP(model)  # Shard across GPUs

    optimizer = torch.optim.AdamW(
        model.parameters(),
        lr=3e-4,
        betas=(0.9, 0.95),
        weight_decay=0.1
    )

    # Cosine LR schedule with warmup
    scheduler = get_cosine_schedule_with_warmup(
        optimizer,
        num_warmup_steps=2000,
        num_training_steps=100000
    )

    for step, batch in enumerate(train_loader):
        loss = model(**batch).loss
        loss.backward()
        torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
        optimizer.step()
        scheduler.step()
        optimizer.zero_grad()

        if step % 100 == 0:
            wandb.log({"loss": loss.item(), "step": step})
```

### 💰 GPU Requirement
```
For SmartStock-7B training:
- Minimum: 4× A100 80GB (E2E Networks: ~₹40K for 1 month)
- Alternative: Google TPU (TRC program - free for researchers!)
- Budget option: 8× RTX 4090 (Hetzner: ~₹30K/month)

Training time estimate: 3-5 days on 4× A100
```

### ✅ Completion Criteria
- [ ] Model architecture designed and verified
- [ ] Training data curated (10B+ tokens)
- [ ] Model trains without NaN losses
- [ ] Perplexity competitive with similar models
- [ ] Bengali generation quality assessed
- [ ] Model uploaded to Hugging Face Hub

---

## 📱 Project 24: SmartStock AI MVP (Full Stack)

### 🎯 Objective
Phase 7 - Month 18-এর full product। See [month-18-product-dev.md](../phase-7-launch/month-18-product-dev.md)

### 📚 Skills Practiced
- FastAPI backend
- Next.js 14 frontend
- React Native mobile app
- PostgreSQL database design
- JWT authentication
- Docker deployment
- CI/CD pipeline

### Architecture
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  Next.js    │    │  React      │    │  WhatsApp   │
│  Web App    │    │  Native App │    │  Bot        │
└──────┬──────┘    └──────┬──────┘    └──────┬──────┘
       │                  │                  │
       └──────────────────┴──────────────────┘
                          │
                   ┌──────▼──────┐
                   │  FastAPI    │
                   │  Backend    │
                   └──────┬──────┘
         ┌─────────────────┼─────────────────┐
         │                 │                 │
  ┌──────▼──────┐   ┌──────▼──────┐  ┌──────▼──────┐
  │ PostgreSQL  │   │   Qdrant    │  │  SmartStock │
  │  Database  │   │  VectorDB   │  │    7B LLM   │
  └─────────────┘   └─────────────┘  └─────────────┘
```

---

## 🤖 Project 25: WhatsApp Business Bot

### 🎯 Objective
WhatsApp Business API দিয়ে voice-enabled kirana assistant।

### 📚 Skills Practiced
- Twilio WhatsApp Business API
- Webhook handling
- Voice message transcription (Whisper)
- Multi-language detection
- Stateful conversation
- Rate limiting and security

### 💻 Key Implementation
```python
# See month-18-product-dev.md for full implementation
# Key features:
# 1. Text message handling (Bengali/Hindi/English)
# 2. Voice message → Whisper → AI response
# 3. Image → Product recognition
# 4. Quick reply buttons
# 5. Session management

WHATSAPP_COMMANDS = {
    "stock": "স্টক চেক করুন",
    "sale": "বিক্রয় যোগ করুন",
    "report": "রিপোর্ট দেখুন",
    "help": "সাহায্য",
    "alert": "stock alert list"
}
```

---

## 🎙️ Project 26: Voice-Enabled Inventory System

### 🎯 Objective
Whisper STT + ElevenLabs TTS দিয়ে fully voice-controlled inventory।

### 📚 Skills Practiced
- OpenAI Whisper (local deployment)
- ElevenLabs / Coqui TTS
- Wake word detection
- Intent extraction from speech
- Bengali speech processing
- Real-time audio streaming

### 💻 Starter Code Outline
```python
import whisper
from elevenlabs import generate, set_api_key
import sounddevice as sd
import numpy as np
import speech_recognition as sr

class VoiceInventorySystem:
    def __init__(self):
        self.whisper_model = whisper.load_model("medium")  # Better Bengali!
        self.recognizer = sr.Recognizer()
        set_api_key(os.getenv("ELEVENLABS_API_KEY"))

    def listen(self, duration: int = 5) -> str:
        """Microphone থেকে audio record করুন"""
        print("🎤 বলুন...")
        audio = sd.rec(int(duration * 16000), samplerate=16000, channels=1)
        sd.wait()
        # Transcribe with Whisper
        result = self.whisper_model.transcribe(
            audio.flatten(),
            language="bn",      # Bengali
            task="transcribe"
        )
        return result["text"]

    def speak(self, text: str, language: str = "bn"):
        """Text → Speech via ElevenLabs"""
        audio = generate(
            text=text,
            voice="Rahul",      # Indian English voice
            model="eleven_multilingual_v2"
        )
        # Play audio
        sd.play(np.frombuffer(audio, dtype=np.float32))
        sd.wait()

    def process_voice_command(self, command: str) -> str:
        """Voice command → Action → Response"""
        command_lower = command.lower()

        if "stock" in command_lower or "স্টক" in command_lower:
            # Extract product name and query inventory
            return self.check_stock(command)
        elif "বিক্রয়" in command_lower or "sale" in command_lower:
            return self.record_sale(command)
        elif "রিপোর্ট" in command_lower or "report" in command_lower:
            return self.generate_report()
        else:
            # Fall through to AI model
            return self.ai_response(command)
```

### ✅ Completion Criteria
- [ ] Bengali voice commands recognized correctly
- [ ] Common inventory queries work via voice
- [ ] Response in Bengali audio
- [ ] < 5 second end-to-end latency
- [ ] Works on Android phone (React Native)
- [ ] 10 demo videos recorded

---

## 🌍 Project 27: Multi-Language Support System

### 🎯 Objective
Bengali, Hindi, English, Tamil সব language-এ SmartStock AI।

### 📚 Skills Practiced
- i18n (internationalization)
- Language detection (langdetect, FastText)
- AI4Bharat translation models (IndicTrans2)
- Dynamic UI language switching
- Multilingual model inference

### 💻 Implementation
```python
from transformers import AutoModelForSeq2SeqLM, AutoTokenizer

# IndicTrans2 - Best Indian language translation model!
# Supports: Bengali, Hindi, Tamil, Telugu, Kannada, Malayalam, etc.
INDIC_TRANS_MODEL = "ai4bharat/indictrans2-indic-en-1B"

class MultilingualProcessor:
    SUPPORTED_LANGUAGES = {
        "bn": "Bengali (বাংলা)",
        "hi": "Hindi (हिन्दी)",
        "en": "English",
        "ta": "Tamil (தமிழ்)",
        "te": "Telugu (తెలుగు)",
        "mr": "Marathi (मराठी)",
        "gu": "Gujarati (ગુજરાતી)"
    }

    def detect_language(self, text: str) -> str:
        """Detect language of input text"""
        # Bengali Unicode range: U+0980 to U+09FF
        # Hindi: U+0900 to U+097F
        # Tamil: U+0B80 to U+0BFF
        import re
        if re.search(r'[\u0980-\u09FF]', text): return "bn"
        if re.search(r'[\u0900-\u097F]', text): return "hi"
        if re.search(r'[\u0B80-\u0BFF]', text): return "ta"
        return "en"

    def translate_to_english(self, text: str, src_lang: str) -> str:
        """Any Indian language → English for AI processing"""
        # Use IndicTrans2 for translation
        pass

    def translate_from_english(self, text: str, tgt_lang: str) -> str:
        """English AI response → Target language"""
        pass

    def process_multilingual_query(self, query: str) -> dict:
        """Full pipeline: detect → translate → AI → translate back"""
        detected_lang = self.detect_language(query)
        if detected_lang != "en":
            english_query = self.translate_to_english(query, detected_lang)
        else:
            english_query = query

        # Get AI response in English
        ai_response = get_ai_response(english_query)

        # Translate back to user's language
        if detected_lang != "en":
            final_response = self.translate_from_english(ai_response, detected_lang)
        else:
            final_response = ai_response

        return {"response": final_response, "detected_language": detected_lang}
```

### ✅ Completion Criteria
- [ ] 4+ languages supported
- [ ] Language auto-detected
- [ ] UI changes language dynamically
- [ ] AI responses in correct language
- [ ] Translation quality acceptable (BLEU > 20)

---

## 📊 Advanced Projects Progress Tracker

| # | Project | Status | GitHub | Deployed | Notes |
|---|---------|--------|--------|----------|-------|
| 20 | Bengali nanoGPT | ⬜ | ⬜ | ⬜ HF Hub | Needs GPU |
| 21 | Llama QLoRA Fine-tune | ⬜ | ⬜ | ⬜ HF Hub | A100 needed |
| 22 | RAG Business Assistant | ⬜ | ⬜ | ⬜ API | Local first |
| 23 | SmartStock-7B Training | ⬜ | ⬜ | ⬜ HF Hub | 4×A100 needed |
| 24 | SmartStock AI MVP | ⬜ | ⬜ | ⬜ Live! | THE main product |
| 25 | WhatsApp Bot | ⬜ | ⬜ | ⬜ Live! | Twilio setup |
| 26 | Voice Inventory | ⬜ | ⬜ | ⬜ Live! | Whisper + TTS |
| 27 | Multi-language | ⬜ | ⬜ | ⬜ Live! | IndicTrans2 |

---

## 💰 GPU Budget for Advanced Projects

| Project | GPU Needed | Duration | Cost (₹) |
|---------|-----------|----------|----------|
| Bengali nanoGPT | 1× T4 (Colab) | 2 hours | Free |
| Llama QLoRA | 1× A100 (E2E) | 8 hours | ₹1,500 |
| RAG System | CPU/T4 | - | Free |
| SmartStock-7B | 4× A100 | 5 days | ₹35,000 |
| **Total** | | | **~₹37,000** |

---

> 💪🇮🇳🚀 **এই advanced projects শেষ করলে আপনি top 5% AI engineers-এর মধ্যে থাকবেন! Launch করুন SmartStock AI!**
