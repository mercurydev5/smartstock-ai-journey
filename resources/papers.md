# 📄 Must-Read Papers - SmartStock AI Learning Journey

> **"Research papers পড়া ভয়ের কিছু না! Practice করুন - easy হয়ে যাবে!"**

[← Resources README](./README.md) | [← Main README](../README.md)

---

## 🗺️ Reading Order

```
Phase 3-4 (Month 7-12):
Papers 1-4 (Transformers, BERT, RNNs basics)

Phase 5 (Month 13-14):
Papers 5-8 (GPT-3, RLHF, LoRA, QLoRA)

Phase 6 (Month 15-16):
Papers 9-11 (CoT, RAG, DPO)

Phase 7 (Month 17-20):
Llama, multimodal papers as needed
```

---

## 📚 The Essential 11 Papers

### 1. "Attention Is All You Need" (Transformer) ⭐⭐⭐
- **Authors:** Vaswani et al. (Google Brain)
- **Year:** 2017
- **URL:** https://arxiv.org/abs/1706.03762
- **Why it matters:** The paper that changed everything. All modern LLMs use this architecture.
- **Key contribution:** Self-attention mechanism, multi-head attention, positional encoding
- **How to read:** Start with the abstract + Figure 1. Then sections 3-4 slowly.
- **What to implement:** Project 18 (Mini-Transformer from scratch)
- **Reading time:** 3-4 hours

---

### 2. BERT: Pre-training of Deep Bidirectional Transformers ⭐⭐⭐
- **Authors:** Devlin et al. (Google)
- **Year:** 2018
- **URL:** https://arxiv.org/abs/1810.04805
- **Why it matters:** Established transfer learning as standard for NLP
- **Key contribution:** Bidirectional transformers, masked language modeling, next sentence prediction
- **When to read:** Month 8-9
- **Reading time:** 2-3 hours

---

### 3. GPT-3: Language Models are Few-Shot Learners ⭐⭐⭐
- **Authors:** Brown et al. (OpenAI)
- **Year:** 2020
- **URL:** https://arxiv.org/abs/2005.14165
- **Why it matters:** Showed emergent abilities of large models
- **Key contribution:** In-context learning (zero/few-shot), scaling laws
- **When to read:** Month 11
- **Reading time:** 4-5 hours (long paper, skim carefully)

---

### 4. InstructGPT: Training Language Models to Follow Instructions (RLHF) ⭐⭐⭐
- **Authors:** Ouyang et al. (OpenAI)
- **Year:** 2022
- **URL:** https://arxiv.org/abs/2203.02155
- **Why it matters:** How ChatGPT actually works. RLHF explained.
- **Key contribution:** Reinforcement Learning from Human Feedback (RLHF)
- **When to read:** Month 12-13
- **Reading time:** 3 hours

---

### 5. LoRA: Low-Rank Adaptation of Large Language Models ⭐⭐⭐
- **Authors:** Hu et al. (Microsoft)
- **Year:** 2021
- **URL:** https://arxiv.org/abs/2106.09685
- **Why it matters:** Makes fine-tuning affordable on consumer GPUs
- **Key contribution:** Low-rank matrices for parameter-efficient fine-tuning
- **When to read:** Month 13-14
- **Reading time:** 2 hours
- **Practical use:** Project 21 (Llama QLoRA fine-tuning)

---

### 6. QLoRA: Efficient Finetuning of Quantized LLMs ⭐⭐⭐
- **Authors:** Dettmers et al. (UW)
- **Year:** 2023
- **URL:** https://arxiv.org/abs/2305.14314
- **Why it matters:** Fine-tune 65B models on a single 48GB GPU
- **Key contribution:** 4-bit NF4 quantization + LoRA + paged optimizers
- **When to read:** Month 14
- **Reading time:** 2 hours
- **Direct application:** SmartStock model fine-tuning

---

### 7. LLaMA 2: Open Foundation and Fine-Tuned Chat Models ⭐⭐⭐
- **Authors:** Touvron et al. (Meta)
- **Year:** 2023
- **URL:** https://arxiv.org/abs/2307.09288
- **Why it matters:** Open-source LLM that democratized AI
- **Key contribution:** Pre-training + RLHF + safety fine-tuning details
- **When to read:** Month 14
- **Reading time:** 3 hours

---

### 8. LLaMA 3: The Llama 3 Herd of Models ⭐⭐⭐
- **Authors:** Meta AI Team
- **Year:** 2024
- **URL:** https://arxiv.org/abs/2407.21783
- **Why it matters:** State-of-the-art open model, what you'll fine-tune
- **Key contribution:** GQA, improved pretraining, multilingual capability
- **When to read:** Month 14-15
- **Reading time:** 4 hours

---

### 9. Chain-of-Thought Prompting Elicits Reasoning ⭐⭐
- **Authors:** Wei et al. (Google)
- **Year:** 2022
- **URL:** https://arxiv.org/abs/2201.11903
- **Why it matters:** How to make LLMs reason step by step
- **Key contribution:** CoT prompting for complex reasoning tasks
- **When to read:** Month 15
- **Practical use:** SmartStock AI reasoning for business advice

---

### 10. RAG: Retrieval-Augmented Generation for Knowledge-Intensive NLP ⭐⭐⭐
- **Authors:** Lewis et al. (Facebook AI)
- **Year:** 2020
- **URL:** https://arxiv.org/abs/2005.11401
- **Why it matters:** How to give LLMs access to up-to-date information
- **Key contribution:** Dense retrieval + seq2seq generation
- **When to read:** Month 14-15
- **Direct application:** Project 22 (RAG Business Assistant)

---

### 11. DPO: Direct Preference Optimization ⭐⭐
- **Authors:** Rafailov et al. (Stanford)
- **Year:** 2023
- **URL:** https://arxiv.org/abs/2305.18290
- **Why it matters:** RLHF alternative - simpler and often better
- **Key contribution:** Directly optimize policy without reward model
- **When to read:** Month 15-16
- **Reading time:** 2 hours

---

## 🎯 Bonus Papers (Optional but Great)

### Architecture Papers
| Paper | URL | Why Read |
|-------|-----|---------|
| GPT-2 | https://d4mucfpksywv.cloudfront.net/better-language-models/language_models_are_unsupervised_multitask_learners.pdf | GPT evolution |
| T5 | https://arxiv.org/abs/1910.10683 | Text-to-text framework |
| RoBERTa | https://arxiv.org/abs/1907.11692 | BERT improvement |
| FLAN | https://arxiv.org/abs/2109.01652 | Instruction tuning |

### Efficiency Papers
| Paper | URL | Why Read |
|-------|-----|---------|
| FlashAttention | https://arxiv.org/abs/2205.14135 | Fast attention |
| FlashAttention-2 | https://arxiv.org/abs/2307.08691 | Even faster |
| Mixtral | https://arxiv.org/abs/2401.04088 | MoE architecture |
| GPTQ | https://arxiv.org/abs/2210.17323 | Post-training quantization |

### Indian AI Papers
| Paper | URL | Why Read |
|-------|-----|---------|
| IndicBERT | https://arxiv.org/abs/2212.05409 | Bengali/Hindi BERT |
| IndicTrans2 | https://arxiv.org/abs/2305.16307 | Best Indian translation |
| Samanantar | https://arxiv.org/abs/2104.05596 | Largest Indian parallel corpus |
| AI4Bharat | https://arxiv.org/abs/2207.06222 | Indic NLP survey |

---

## 📖 How to Read a Research Paper

### Three-Pass Method:
```
Pass 1 (15 minutes):
- Title, abstract, introduction
- Section headings and conclusion
- Decide: worth reading more?

Pass 2 (1-2 hours):
- Read fully but skip proofs
- Note key figures and tables
- Understand the main contribution

Pass 3 (3-5 hours):
- Re-read carefully
- Understand all math
- Try to implement the key idea
- Note open questions
```

### Questions to Answer After Reading:
1. What problem does this paper solve?
2. What's the main contribution (novel idea)?
3. How does it compare to previous work?
4. What are the limitations?
5. How can I apply this to SmartStock AI?

---

> 💪🇮🇳🚀 **প্রতিটি paper একটি breakthrough! আস্তে আস্তে পড়ুন, বুঝুন - rush করবেন না!**
