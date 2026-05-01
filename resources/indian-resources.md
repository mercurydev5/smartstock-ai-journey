# 🇮🇳 Indian Resources - SmartStock AI Learning Journey

> **"ভারতীয় AI ইকোসিস্টেম দ্রুত বড় হচ্ছে! এই resources আপনার কাছের!"**

[← Resources README](./README.md) | [← Main README](../README.md)

---

## 🤖 Indian AI Research Organizations

### 1. AI4Bharat (IIT Madras) ⭐⭐⭐
- **URL:** https://ai4bharat.org
- **GitHub:** https://github.com/AI4Bharat
- **Focus:** NLP tools for 22+ Indian languages
- **Key Models:**
  - **IndicBERT** - BERT for 12 Indian languages
  - **IndicTrans2** - Best Indian language translation (English ↔ 22 languages)
  - **Samanantar** - Largest Indian parallel corpus (49M sentence pairs)
  - **Bhasha-Abhijnaanam** - Language identification
  - **IndicWhisper** - Speech recognition for Indian languages
  - **IndicTTS** - Text-to-speech for Indian languages
- **Why critical for SmartStock AI:**
  - Bengali + Hindi + English support
  - Free and open-source
  - State-of-the-art for Indian languages

```python
# Using AI4Bharat's IndicTrans2 for Bengali translation
from transformers import AutoModelForSeq2SeqLM, AutoTokenizer

model_name = "ai4bharat/indictrans2-en-indic-1B"
tokenizer = AutoTokenizer.from_pretrained(model_name, trust_remote_code=True)
model = AutoModelForSeq2SeqLM.from_pretrained(model_name, trust_remote_code=True)

# Translate English to Bengali
input_text = "Your rice stock is running low. Please reorder."
# Output: "আপনার চালের স্টক কম হয়ে যাচ্ছে। দয়া করে পুনরায় অর্ডার করুন।"
```

### 2. Sarvam AI ⭐⭐⭐
- **URL:** https://www.sarvam.ai
- **Headquarters:** Bangalore
- **Key Models:**
  - **OpenHathi** - Hindi-focused base model (7B)
  - **Sarvam-1** - Hindi + English commercial model
- **API:** Available for developers
- **Why follow:** Best Indian startup in AI language models

### 3. Krutrim (Ola AI) ⭐⭐
- **URL:** https://www.olakrutrim.com
- **Parent:** Bhavish Aggarwal (Ola founder)
- **Focus:** Hindi-first AI assistant
- **Products:** Krutrim Pro (cloud), Krutrim Si-7B (open source)
- **Note:** India's first full-stack AI company (claimed)

### 4. BharatGPT (CoRover) ⭐⭐
- **URL:** https://bharatgpt.ai
- **Focus:** Multilingual Indian AI
- **Use case:** Government services, banking, healthcare
- **Languages:** 12+ Indian languages

### 5. Dhruva (Bhashini) ⭐⭐⭐
- **URL:** https://bhashini.gov.in/dhruva
- **Organization:** Government of India (MeitY)
- **Free API:** Yes!
- **Features:**
  - ASR (speech recognition) for 22 Indian languages
  - TTS (text-to-speech) for 22 Indian languages
  - Translation between Indian languages
  - OCR for Indian scripts
- **Why special:** Government-backed, free for Indian developers!

```python
# Dhruva API example
import requests

def translate_to_bengali(text: str) -> str:
    """Translate any Indian language to Bengali using Dhruva"""
    url = "https://dhruva-api.bhashini.gov.in/services/inference/pipeline"
    headers = {
        "Authorization": "YOUR_DHRUVA_API_KEY",
        "Content-Type": "application/json"
    }
    payload = {
        "pipelineTasks": [{
            "taskType": "translation",
            "config": {
                "language": {"sourceLanguage": "en", "targetLanguage": "bn"}
            }
        }],
        "inputData": {"input": [{"source": text}]}
    }
    response = requests.post(url, json=payload, headers=headers)
    return response.json()['pipelineResponse'][0]['output'][0]['target']
```

---

## 💻 Indian GPU Providers

### Why Indian GPUs?
```
AWS A100 (us-east-1): ~$3.50/hour = ~₹290/hour = ₹6,960/day
E2E Networks A100 (India): ~₹1,667/hour = ₹4,000/day

Savings: ~43% cheaper!
+ Data stays in India (compliance)
+ Lower latency for Indian users
```

### 1. E2E Networks ⭐⭐⭐
- **URL:** https://www.e2enetworks.com
- **GPUs Available:** A100 80GB, RTX 4090, V100
- **A100 Cost:** ~₹40K/month (dedicated)
- **Spot Instances:** ~₹15K/month (70% off!)
- **Data Centers:** Delhi, Mumbai
- **Why choose:** SEBI/RBI compliant, Indian data residency
- **SmartStock use:** Train SmartStock-7B here

### 2. Yotta Data Services ⭐⭐
- **URL:** https://yotta.com
- **GPUs:** H100, A100
- **Tier IV data center** (highest reliability)
- **Location:** Navi Mumbai
- **Target:** Enterprise customers

### 3. Jarvislabs ⭐⭐⭐
- **URL:** https://jarvislabs.ai
- **GPUs:** A100 80GB, RTX 3090, RTX 4090
- **Pricing:** Very affordable
- **UI:** Developer-friendly
- **Why choose:** Best UI/UX, pre-installed ML frameworks
- **Great for:** Experiments before committing to E2E

### 4. Tata Communications ⭐⭐
- **URL:** https://www.tatacommunications.com
- **Focus:** Enterprise cloud infrastructure
- **Data Centers:** Pan-India
- **AI Services:** Managed AI/ML platform

---

## 📺 Indian YouTube Channels

### Hindi/Bengali ML Education ⭐⭐⭐

| Channel | Language | Content | URL |
|---------|----------|---------|-----|
| **Krish Naik** | English + Hindi | Complete ML/DL | https://www.youtube.com/@krishnaik06 |
| **CampusX** | Hindi | 100 Days ML Course | https://www.youtube.com/@campusx-official |
| **CodeBasics** | English (Indian) | ML Projects | https://www.youtube.com/@codebasics |
| **iNeuron** | English + Hindi | Industry-focused | https://www.youtube.com/@iNeuroniNtelligence |
| **Apna College** | Hindi | Python basics | https://www.youtube.com/@ApnaCollegeOfficial |
| **CodeWithHarry** | Hindi | Programming | https://www.youtube.com/@CodeWithHarry |
| **Telusko** | English | Python, Java | https://www.youtube.com/@Telusko |
| **Aman Kharwal** | English | Data Science projects | Search on YouTube |

---

## 🏛️ Government Programs & Support

### 1. Startup India ⭐⭐⭐
- **URL:** https://www.startupindia.gov.in
- **Benefits:**
  - Tax exemptions (3 years)
  - Self-certification compliance
  - Fast-track patent filing
  - Access to ₹10,000 Cr fund of funds
- **How to register:** Apply online as DPIIT-recognized startup
- **SmartStock relevance:** Register once you have revenue

### 2. IndiaAI Mission ⭐⭐⭐
- **URL:** https://indiaai.gov.in
- **Budget:** ₹10,372 crore ($1.25 billion) allocated
- **Key initiatives:**
  - IndiaAI Compute Capacity (10,000 GPUs!)
  - IndiaAI Datasets Platform
  - IndiaAI Application Development
  - IndiaAI Future Skills
- **How to benefit:** Apply for GPU access, datasets, funding

### 3. MeitY Startup Hub ⭐⭐
- **URL:** https://mshindia.in
- **Benefits:** Incubation, mentoring, funding
- **AI focus:** Special AI startup cohorts

### 4. NASSCOM AI Initiative ⭐⭐
- **URL:** https://nasscom.in/ai
- **Benefits:** Industry connections, certifications, events
- **NASSCOM 10K Startups:** Accelerator program

### 5. iCreate (Ahmedabad) ⭐⭐
- **URL:** https://icreate.org.in
- **Support:** Funding, mentoring for tech startups
- **Government:** DPIIT affiliated

---

## 🌐 India-Specific Datasets

### Bengali Language Datasets
| Dataset | Source | Size | Link |
|---------|--------|------|------|
| Bengali Wikipedia | Wikimedia | ~100K articles | HuggingFace: `wikipedia` (bn) |
| Samanantar | AI4Bharat | 8.5M Bengali-English pairs | HF: `ai4bharat/samanantar` |
| IndicCorp | AI4Bharat | Bengali web corpus | HF: `ai4bharat/indiccorp-v2` |
| BNLPC | Various | Bengali NLP benchmarks | GitHub: Bengali-NLP |

### Indian Business Datasets
| Dataset | Content | Where to find |
|---------|---------|---------------|
| E-commerce India | Flipkart/Amazon products | Kaggle |
| Indian stock market | NSE/BSE data | yfinance (RELIANCE.NS) |
| FMCG sales India | Consumer goods | Kaggle |
| Indian census data | Demographics | census.gov.in |
| GST data | Business transactions | gst.gov.in (some public) |

---

## 💼 Indian AI Companies (For Inspiration + Job Opportunities)

| Company | Focus | HQ |
|---------|-------|-----|
| Sarvam AI | Indic LLMs | Bangalore |
| Mad Street Den (Vue.ai) | Retail AI | Chennai |
| Haptik | Conversational AI | Mumbai |
| Uniphore | Speech AI | Chennai |
| Krutrim (Ola) | AI assistant | Bangalore |
| Yellow.ai | Enterprise chatbots | Bangalore |
| Vernacular.ai | Voice AI | Bangalore |
| CoRover (BharatGPT) | Multilingual AI | Noida |
| Reverie Language | Indic NLP | Bangalore |
| Niki.ai | Conversational commerce | Bangalore |

---

> 💪🇮🇳🚀 **ভারতের AI revolution-এ অংশ নিন! আপনার SmartStock AI এই দেশের কোটি ব্যবসায়ীকে সাহায্য করবে!**
