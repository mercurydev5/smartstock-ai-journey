# 🔥 Intermediate Projects (Phase 3-4)

> **"আপনি আর beginner নন! এখন deep learning এর জগতে ডুব দেওয়ার সময়!"**

[← Projects README](./README.md) | [← Main README](../README.md)

---

## Overview

| | Details |
|---|---|
| **Phase** | 3-4 (Month 7-12) |
| **Projects** | 7 projects |
| **Skills** | PyTorch, CNN, LSTM, NLP, Transformers |
| **Estimated Time** | 6 months (2 hrs/day) |

---

## 🧠 Project 13: Neural Network from Scratch (NumPy only)

### 🎯 Objective
PyTorch বা TensorFlow ছাড়া, শুধু NumPy দিয়ে neural network বানিয়ে MNIST classify করুন।

### 📚 Skills Practiced
- Backpropagation math (chain rule)
- Gradient descent implementation
- Activation functions (ReLU, Sigmoid, Softmax)
- Weight initialization
- Batch training
- Loss functions (Cross-entropy, MSE)

### 💻 Starter Code Outline
```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import fetch_openml
from sklearn.preprocessing import OneHotEncoder

class Layer:
    def forward(self, x): raise NotImplementedError
    def backward(self, grad): raise NotImplementedError
    def update(self, lr): pass

class Linear(Layer):
    def __init__(self, in_features: int, out_features: int):
        # He initialization
        self.W = np.random.randn(in_features, out_features) * np.sqrt(2.0 / in_features)
        self.b = np.zeros((1, out_features))
        self.dW = None
        self.db = None
        self.x = None  # Cache for backprop

    def forward(self, x: np.ndarray) -> np.ndarray:
        self.x = x
        # TODO: y = xW + b
        pass

    def backward(self, grad: np.ndarray) -> np.ndarray:
        # TODO: Compute dW, db, dx using chain rule
        # dL/dW = x^T · grad
        # dL/dx = grad · W^T
        pass

    def update(self, lr: float):
        # TODO: SGD update
        pass

class ReLU(Layer):
    def forward(self, x): pass  # TODO
    def backward(self, grad): pass  # TODO

class Softmax(Layer):
    def forward(self, x): pass  # TODO

def cross_entropy_loss(y_pred: np.ndarray, y_true: np.ndarray) -> tuple:
    """Returns (loss, gradient)"""
    # TODO: Compute CE loss and its gradient
    pass

class NeuralNetwork:
    def __init__(self, layers: list):
        self.layers = layers

    def forward(self, x: np.ndarray) -> np.ndarray:
        for layer in self.layers:
            x = layer.forward(x)
        return x

    def backward(self, grad: np.ndarray):
        for layer in reversed(self.layers):
            grad = layer.backward(grad)

    def train(self, X, y, epochs=100, lr=0.01, batch_size=32):
        losses = []
        for epoch in range(epochs):
            # TODO: Mini-batch training loop
            pass
        return losses

# Architecture: 784 → 256 → 128 → 10
model = NeuralNetwork([
    Linear(784, 256),
    ReLU(),
    Linear(256, 128),
    ReLU(),
    Linear(128, 10),
    Softmax()
])
```

### ✅ Completion Criteria
- [ ] Network trains and loss decreases
- [ ] MNIST accuracy > 95%
- [ ] Training loss curve plotted
- [ ] Confusion matrix shown
- [ ] Visualization of wrong predictions
- [ ] Written explanation of backprop (in Bengali or English)

### 🚀 Bonus Challenges
1. Add momentum, Adam optimizer
2. Batch Normalization layer
3. Dropout regularization
4. Hyperparameter tuning comparison

---

## 💸 Project 14: Indian Currency Note Classifier (CNN + Transfer Learning)

### 🎯 Objective
ResNet-18 এর transfer learning দিয়ে ₹10, ₹20, ₹50, ₹100, ₹200, ₹500, ₹2000 classify করুন।

### 📚 Skills Practiced
- PyTorch CNN
- Transfer learning (ResNet)
- Data augmentation
- Fine-tuning pre-trained models
- Custom dataset creation
- Grad-CAM visualization

### 💻 Starter Code Outline
```python
import torch
import torch.nn as nn
import torchvision.transforms as transforms
import torchvision.models as models
from torch.utils.data import DataLoader, Dataset
from torchvision.datasets import ImageFolder
import matplotlib.pyplot as plt
import numpy as np
from PIL import Image

# Data transforms
train_transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.RandomHorizontalFlip(),
    transforms.RandomRotation(10),
    transforms.ColorJitter(brightness=0.2, contrast=0.2),
    transforms.ToTensor(),
    transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
])

val_transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
])

def build_currency_classifier(num_classes: int = 7):
    """ResNet-18 with custom head"""
    model = models.resnet18(weights='IMAGENET1K_V1')

    # Freeze all layers except last
    for param in model.parameters():
        param.requires_grad = False

    # Replace final classifier
    num_features = model.fc.in_features
    model.fc = nn.Sequential(
        nn.Linear(num_features, 256),
        nn.ReLU(),
        nn.Dropout(0.4),
        nn.Linear(256, num_classes)
    )
    return model

def train_epoch(model, loader, optimizer, criterion, device):
    model.train()
    total_loss, correct = 0, 0
    for imgs, labels in loader:
        imgs, labels = imgs.to(device), labels.to(device)
        optimizer.zero_grad()
        outputs = model(imgs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()
        total_loss += loss.item()
        correct += (outputs.argmax(1) == labels).sum().item()
    return total_loss / len(loader), correct / len(loader.dataset)

def grad_cam(model, img_tensor, target_class):
    """Visualize what the model focuses on"""
    # TODO: Implement Grad-CAM
    # Hook the gradients of the last conv layer
    pass

# Training
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
model = build_currency_classifier().to(device)
optimizer = torch.optim.Adam(model.fc.parameters(), lr=1e-3)
criterion = nn.CrossEntropyLoss()
```

### ✅ Completion Criteria
- [ ] Dataset created (100+ images per class)
- [ ] Transfer learning with ResNet-18
- [ ] Accuracy > 95%
- [ ] Grad-CAM heatmaps showing what model sees
- [ ] Real camera demo (OpenCV)
- [ ] Model deployed on Streamlit

### 🚀 Bonus Challenges
1. Support Bangladeshi Taka notes too
2. Damaged/torn note detection
3. Mobile deployment (TFLite/ONNX)
4. Denomination sum calculator

---

## 🎊 Project 15: Festival Sales Predictor (LSTM)

### 🎯 Objective
LSTM দিয়ে Diwali, Eid, Durga Puja-তে demand forecast করুন।

### 📚 Skills Practiced
- LSTM/GRU architecture
- Sequence modeling
- Time-series preprocessing
- Seasonal decomposition
- Prophet vs LSTM comparison
- Multi-step forecasting

### 💻 Starter Code Outline
```python
import torch
import torch.nn as nn
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.preprocessing import MinMaxScaler

class SalesForecastLSTM(nn.Module):
    def __init__(self, input_size=1, hidden_size=64, num_layers=2,
                 output_size=1, dropout=0.2):
        super().__init__()
        self.lstm = nn.LSTM(
            input_size=input_size,
            hidden_size=hidden_size,
            num_layers=num_layers,
            batch_first=True,
            dropout=dropout
        )
        self.attention = nn.MultiheadAttention(hidden_size, num_heads=4)
        self.fc = nn.Sequential(
            nn.Linear(hidden_size, 32),
            nn.ReLU(),
            nn.Linear(32, output_size)
        )

    def forward(self, x):
        # x shape: (batch, seq_len, features)
        lstm_out, _ = self.lstm(x)

        # Attention mechanism
        attn_out, _ = self.attention(
            lstm_out.permute(1, 0, 2),
            lstm_out.permute(1, 0, 2),
            lstm_out.permute(1, 0, 2)
        )
        out = self.fc(attn_out[-1])
        return out

def add_festival_features(df: pd.DataFrame) -> pd.DataFrame:
    """Indian festival dates as features"""
    df['is_diwali_week'] = 0
    df['is_eid_week'] = 0
    df['is_durga_puja_week'] = 0
    df['is_christmas_week'] = 0
    df['is_new_year_week'] = 0

    # TODO: Mark festival dates for 2024-2027
    # Diwali: Nov 1 2024, Oct 21 2025, Nov 9 2026, Oct 29 2027
    # Eid ul-Fitr: Apr 11 2024, Mar 30 2025, Mar 19 2026, Mar 9 2027

    df['day_of_week'] = df['date'].dt.dayofweek
    df['month'] = df['date'].dt.month
    df['quarter'] = df['date'].dt.quarter
    return df

def create_sequences(data: np.ndarray, seq_length: int = 30):
    """30 days দিয়ে পরের ৭ দিন predict"""
    X, y = [], []
    for i in range(len(data) - seq_length - 7):
        X.append(data[i:i + seq_length])
        y.append(data[i + seq_length:i + seq_length + 7])
    return np.array(X), np.array(y)
```

### ✅ Completion Criteria
- [ ] LSTM trains correctly
- [ ] MAPE < 10% for normal days
- [ ] Festival demand spikes captured
- [ ] 7-day ahead forecast
- [ ] Comparison with Prophet library
- [ ] Business alert: "Stock up before Diwali!"

### 🚀 Bonus Challenges
1. Multivariate (price, weather, events as features)
2. Ensemble: LSTM + XGBoost
3. WhatsApp alert for predicted stockouts
4. SmartStock AI integration

---

## 🔍 Project 16: Time Series Anomaly Detector

### 🎯 Objective
Autoencoder দিয়ে server metrics বা sales data-তে anomalies detect করুন।

### 📚 Skills Practiced
- Autoencoder architecture
- Unsupervised learning
- Reconstruction error threshold
- Rolling statistics
- Isolation Forest comparison

### 💻 Starter Code Outline
```python
import torch
import torch.nn as nn
import numpy as np
import pandas as pd

class TimeSeriesAutoencoder(nn.Module):
    def __init__(self, seq_len: int = 50, latent_dim: int = 8):
        super().__init__()
        # Encoder: compress time series to latent representation
        self.encoder = nn.Sequential(
            nn.Linear(seq_len, 32),
            nn.ReLU(),
            nn.Linear(32, 16),
            nn.ReLU(),
            nn.Linear(16, latent_dim)
        )
        # Decoder: reconstruct original sequence
        self.decoder = nn.Sequential(
            nn.Linear(latent_dim, 16),
            nn.ReLU(),
            nn.Linear(16, 32),
            nn.ReLU(),
            nn.Linear(32, seq_len)
        )

    def forward(self, x):
        encoded = self.encoder(x)
        decoded = self.decoder(encoded)
        return decoded

def detect_anomalies(model, data: np.ndarray,
                     threshold_multiplier: float = 3.0) -> np.ndarray:
    """Reconstruction error > threshold = anomaly"""
    model.eval()
    with torch.no_grad():
        reconstruction = model(torch.FloatTensor(data))
    errors = ((data - reconstruction.numpy()) ** 2).mean(axis=1)
    threshold = errors.mean() + threshold_multiplier * errors.std()
    return errors > threshold, errors, threshold
```

### ✅ Completion Criteria
- [ ] Autoencoder trains successfully
- [ ] Known anomalies detected correctly
- [ ] False positive rate < 5%
- [ ] Real-time monitoring dashboard
- [ ] Alert system for detected anomalies

---

## 🗣️ Project 17: Bengali Sentiment Analyzer

### 🎯 Objective
AI4Bharat embeddings ব্যবহার করে Bengali text-এর sentiment analyze করুন।

### 📚 Skills Practiced
- Hugging Face Transformers
- Fine-tuning pre-trained models
- Bengali text preprocessing
- IndicBERT / MuRIL embeddings
- Transfer learning for NLP

### 💻 Starter Code Outline
```python
from transformers import AutoTokenizer, AutoModel
import torch
import torch.nn as nn
from torch.utils.data import Dataset, DataLoader

# AI4Bharat's IndicBERT - trained on 12 Indian languages!
MODEL_NAME = "ai4bharat/indic-bert"

class BengaliSentimentDataset(Dataset):
    def __init__(self, texts, labels, tokenizer, max_length=128):
        self.texts = texts
        self.labels = labels
        self.tokenizer = tokenizer
        self.max_length = max_length

    def __len__(self):
        return len(self.texts)

    def __getitem__(self, idx):
        encoding = self.tokenizer(
            self.texts[idx],
            max_length=self.max_length,
            padding='max_length',
            truncation=True,
            return_tensors='pt'
        )
        return {
            'input_ids': encoding['input_ids'].squeeze(),
            'attention_mask': encoding['attention_mask'].squeeze(),
            'label': torch.tensor(self.labels[idx], dtype=torch.long)
        }

class BengaliSentimentClassifier(nn.Module):
    def __init__(self, num_classes: int = 3):
        super().__init__()
        self.bert = AutoModel.from_pretrained(MODEL_NAME)
        self.dropout = nn.Dropout(0.3)
        self.classifier = nn.Linear(768, num_classes)

    def forward(self, input_ids, attention_mask):
        outputs = self.bert(input_ids=input_ids, attention_mask=attention_mask)
        pooled = outputs.pooler_output  # [CLS] token
        out = self.dropout(pooled)
        return self.classifier(out)

# Sample Bengali texts
sample_texts = [
    "এই পণ্যটি অসাধারণ! খুব ভালো quality।",  # Positive
    "দোকান থেকে যা কিনেছি সব খারাপ।",          # Negative
    "ঠিক আছে, তেমন ভালো না।",                  # Neutral
]
labels = [2, 0, 1]  # 0=negative, 1=neutral, 2=positive
```

### ✅ Completion Criteria
- [ ] IndicBERT fine-tuned on Bengali sentiment data
- [ ] Accuracy > 85% on test set
- [ ] Handles mixed Bengali-English (Banglish)
- [ ] Product review classifier demo
- [ ] Streamlit app with live prediction

### 🚀 Bonus Challenges
1. Aspect-based sentiment (product quality vs delivery)
2. Sarcasm detection
3. WhatsApp review collector + analyzer
4. SmartStock integration: "কোন product-এর review negative?"

---

## 🤖 Project 18: Mini-Transformer from Scratch

### 🎯 Objective
"Attention Is All You Need" paper implement করুন। Bengali ↔ English translation।

### 📚 Skills Practiced
- Multi-head self-attention
- Positional encoding
- Encoder-decoder architecture
- Teacher forcing
- Beam search decoding
- BLEU score evaluation

### 💻 Starter Code Outline
```python
import torch
import torch.nn as nn
import math

class MultiHeadAttention(nn.Module):
    def __init__(self, d_model: int, num_heads: int):
        super().__init__()
        assert d_model % num_heads == 0
        self.d_k = d_model // num_heads
        self.num_heads = num_heads
        self.W_q = nn.Linear(d_model, d_model)
        self.W_k = nn.Linear(d_model, d_model)
        self.W_v = nn.Linear(d_model, d_model)
        self.W_o = nn.Linear(d_model, d_model)

    def scaled_dot_product_attention(self, Q, K, V, mask=None):
        scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        attn = torch.softmax(scores, dim=-1)
        return torch.matmul(attn, V), attn

    def forward(self, query, key, value, mask=None):
        batch_size = query.size(0)
        Q = self.W_q(query).view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        K = self.W_k(key).view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        V = self.W_v(value).view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        x, attention = self.scaled_dot_product_attention(Q, K, V, mask)
        x = x.transpose(1, 2).contiguous().view(batch_size, -1, self.num_heads * self.d_k)
        return self.W_o(x)

class PositionalEncoding(nn.Module):
    def __init__(self, d_model: int, max_len: int = 5000):
        super().__init__()
        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len).unsqueeze(1).float()
        div_term = torch.exp(torch.arange(0, d_model, 2).float() * (-math.log(10000.0) / d_model))
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        self.register_buffer('pe', pe.unsqueeze(0))

    def forward(self, x):
        return x + self.pe[:, :x.size(1)]

class TransformerBlock(nn.Module):
    def __init__(self, d_model: int, num_heads: int, ff_dim: int, dropout: float = 0.1):
        super().__init__()
        self.attention = MultiHeadAttention(d_model, num_heads)
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.ff = nn.Sequential(
            nn.Linear(d_model, ff_dim),
            nn.ReLU(),
            nn.Linear(ff_dim, d_model)
        )
        self.dropout = nn.Dropout(dropout)

    def forward(self, x, mask=None):
        attn = self.attention(x, x, x, mask)
        x = self.norm1(x + self.dropout(attn))
        ff_out = self.ff(x)
        x = self.norm2(x + self.dropout(ff_out))
        return x
```

### ✅ Completion Criteria
- [ ] Transformer encoder + decoder implemented
- [ ] Bengali ↔ English dataset used (AI4Bharat Samanantar)
- [ ] Training loss decreases correctly
- [ ] BLEU score > 15 on test set
- [ ] Attention visualization plots
- [ ] Blog post explaining the paper

### 🚀 Bonus Challenges
1. Byte Pair Encoding (BPE) tokenizer
2. Label smoothing
3. Learning rate warmup scheduler
4. Scale up to production-grade model

---

## 💬 Project 19: Bengali Customer Service Chatbot

### 🎯 Objective
IndicBERT fine-tune করে kirana store customer service chatbot বানানো।

### 📚 Skills Practiced
- Instruction fine-tuning
- Question-answering with transformers
- Intent classification
- Entity extraction
- Dialogue state management
- Deployment (FastAPI + React)

### 💻 Starter Code Outline
```python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM
from datasets import Dataset
import torch

# Use AI4Bharat's IndicBART for Bengali generation
MODEL_NAME = "ai4bharat/indicbart"

# Training data format
training_data = [
    {
        "instruction": "আপনি SmartStock AI-এর customer service assistant।",
        "input": "আমার দোকানে চালের stock কম, কি করবো?",
        "output": "আপনার চালের stock কম হলে এখনই reorder করুন। "
                  "আমাদের app-এ 'Stock Alert' section-এ যান এবং "
                  "supplier-কে order দিন। প্রতিদিন minimum stock "
                  "check করার জন্য reminder set করুন।"
    },
    {
        "instruction": "আপনি SmartStock AI-এর customer service assistant।",
        "input": "Payment কিভাবে করবো?",
        "output": "আপনি Razorpay দিয়ে UPI, card, বা net banking-এ "
                  "payment করতে পারবেন। Settings > Subscription > "
                  "Upgrade Plan-এ যান।"
    },
    # Add 100+ examples for better performance
]

def prepare_dataset(data: list) -> Dataset:
    """Fine-tuning dataset তৈরি করুন"""
    formatted = []
    for item in data:
        prompt = f"নির্দেশ: {item['instruction']}\nপ্রশ্ন: {item['input']}\nউত্তর:"
        formatted.append({
            "input": prompt,
            "target": item["output"]
        })
    return Dataset.from_list(formatted)

def fine_tune_chatbot():
    """IndicBART fine-tune করুন"""
    from transformers import Seq2SeqTrainer, Seq2SeqTrainingArguments

    tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME)
    model = AutoModelForSeq2SeqLM.from_pretrained(MODEL_NAME)

    # Training arguments
    training_args = Seq2SeqTrainingArguments(
        output_dir="./bengali-chatbot",
        num_train_epochs=5,
        per_device_train_batch_size=8,
        warmup_steps=100,
        learning_rate=3e-5,
        predict_with_generate=True,
        save_strategy="epoch"
    )
    # TODO: Train model
```

### ✅ Completion Criteria
- [ ] 100+ training examples created
- [ ] Model fine-tuned and responds correctly
- [ ] Bengali, English, mixed input handled
- [ ] Intent classification (stock/payment/report/help)
- [ ] API endpoint with FastAPI
- [ ] Simple chat UI

### 🚀 Bonus Challenges
1. Voice input with Whisper
2. WhatsApp integration
3. Fallback to GPT-4 API for unknown queries
4. Conversation history memory

---

## 📊 Progress Tracker

| # | Project | Status | GitHub | Deployed | Blog Post |
|---|---------|--------|--------|----------|-----------|
| 13 | Neural Net from Scratch | ⬜ | ⬜ | N/A | ⬜ |
| 14 | Indian Currency CNN | ⬜ | ⬜ | ⬜ Streamlit | ⬜ |
| 15 | Festival Sales LSTM | ⬜ | ⬜ | ⬜ Streamlit | ⬜ |
| 16 | Anomaly Detector | ⬜ | ⬜ | N/A | ⬜ |
| 17 | Bengali Sentiment | ⬜ | ⬜ | ⬜ HF Spaces | ⬜ |
| 18 | Mini-Transformer | ⬜ | ⬜ | N/A | ⬜ |
| 19 | Bengali Chatbot | ⬜ | ⬜ | ⬜ HF Spaces | ⬜ |

---

> 💪🇮🇳🚀 **Phase 3-4 শেষ করুন! আপনি এখন একজন deep learning practitioner!**
