# 🔁 Month 8: RNNs & Sequences (ডিসেম্বর ২০২৬)

> **"সময়ের সাথে data বোঝার জন্য RNN — এটাই NLP-র ভিত্তি!"** 💪🇮🇳🚀

[← Month 7](month-07-cnn-vision.md) | [← Phase 3](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | ডিসেম্বর ১-৩১, ২০২৬ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | RNN, LSTM, GRU + Time Series |
| 💻 মূল Project | 🎉 Festival Sales Predictor |
| 📚 মূল Resource | Karpathy videos + d2l.ai |

---

## 🗓️ Week-by-Week Plan

### Week 29 — RNN Fundamentals

```python
# rnn_from_scratch.py
import numpy as np

class SimpleRNN:
    """Simple RNN from scratch to understand the concept"""
    def __init__(self, input_size, hidden_size, output_size):
        # Weights
        self.Wxh = np.random.randn(hidden_size, input_size) * 0.01
        self.Whh = np.random.randn(hidden_size, hidden_size) * 0.01
        self.Why = np.random.randn(output_size, hidden_size) * 0.01
        self.bh = np.zeros((hidden_size, 1))
        self.by = np.zeros((output_size, 1))
        self.hidden_size = hidden_size

    def forward(self, inputs, h_prev):
        """
        inputs: list of input vectors
        h_prev: previous hidden state
        """
        hidden_states = {}
        hidden_states[-1] = np.copy(h_prev)
        outputs = []

        for t, x in enumerate(inputs):
            x = x.reshape(-1, 1)
            # h_t = tanh(Wxh*x + Whh*h_prev + bh)
            h = np.tanh(self.Wxh @ x + self.Whh @ hidden_states[t-1] + self.bh)
            # y_t = Why*h + by
            y = self.Why @ h + self.by
            hidden_states[t] = h
            outputs.append(y)

        return outputs, hidden_states

print("✅ RNN from scratch - understanding the math!")
```

### Week 30 — LSTM & GRU in PyTorch

```python
# lstm_pytorch.py
import torch
import torch.nn as nn
import numpy as np
import matplotlib.pyplot as plt

class SalesForecastLSTM(nn.Module):
    def __init__(self, input_size=1, hidden_size=64, num_layers=2, output_size=1):
        super().__init__()
        self.hidden_size = hidden_size
        self.num_layers = num_layers

        self.lstm = nn.LSTM(input_size, hidden_size, num_layers,
                            batch_first=True, dropout=0.2)
        self.fc = nn.Linear(hidden_size, output_size)

    def forward(self, x):
        h0 = torch.zeros(self.num_layers, x.size(0), self.hidden_size)
        c0 = torch.zeros(self.num_layers, x.size(0), self.hidden_size)
        out, _ = self.lstm(x, (h0, c0))
        return self.fc(out[:, -1, :])

# Time series dataset creation
def create_sequences(data, seq_length=30):
    X, y = [], []
    for i in range(len(data) - seq_length):
        X.append(data[i:i+seq_length])
        y.append(data[i+seq_length])
    return np.array(X), np.array(y)

model = SalesForecastLSTM()
print(f"LSTM Parameters: {sum(p.numel() for p in model.parameters()):,}")
print("Ready to predict festival sales!")
```

### Week 31-32 — 🎉 Festival Sales Predictor Project

```python
# festival_sales_predictor.py
import pandas as pd
import numpy as np
import torch
import torch.nn as nn
import matplotlib.pyplot as plt

# Indian festivals that affect sales
FESTIVALS = {
    "Eid-ul-Fitr": [4, 5],
    "Eid-ul-Adha": [7, 8],
    "Durga Puja": [10],
    "Diwali": [10, 11],
    "Christmas": [12],
    "New Year": [1, 12],
    "Holi": [3],
    "Raksha Bandhan": [8],
}

def generate_festival_sales(days=730):
    """Generate 2-year sales data with festival spikes"""
    np.random.seed(42)
    dates = pd.date_range("2024-01-01", periods=days)
    base_sales = 10000 + np.cumsum(np.random.randn(days) * 200)

    # Add festival spikes
    for festival, months in FESTIVALS.items():
        for month in months:
            festival_days = np.where(dates.month == month)[0][:7]
            base_sales[festival_days] *= np.random.uniform(1.5, 2.5)

    # Weekly seasonality
    weekend_mask = dates.dayofweek.isin([4, 5, 6])  # Friday, Saturday, Sunday
    base_sales[weekend_mask] *= 1.3

    return pd.DataFrame({
        "date": dates,
        "sales": np.maximum(0, base_sales),
        "month": dates.month,
        "day_of_week": dates.dayofweek,
        "is_weekend": weekend_mask.astype(int),
    })

df = generate_festival_sales()
print(f"Generated {len(df)} days of sales data")
print(f"Average daily sales: ৳{df['sales'].mean():,.0f}")
print(f"Peak sales day: {df.loc[df['sales'].idxmax(), 'date'].strftime('%Y-%m-%d')}")
print("\n✅ Festival Sales Predictor — train LSTM and forecast next 30 days!")
```

---

## 🎄 ডিসেম্বর ৩১, ২০২৬ — Year 1 Complete! 🎉

**আপনি ২০২৬ সালে শিখেছেন:**
✅ Python (complete)
✅ Math (NumPy, Pandas, Linear Algebra, Stats)
✅ Machine Learning (supervised, unsupervised)
✅ Deep Learning (NN, CNN, RNN)
✅ **১৫+ Projects GitHub-এ**

**এখন ২০২৭ শুরু — NLP & Transformers!** 🚀

---

## ✅ Month 8 Checklist

- [ ] RNN concept বোঝা (from scratch)
- [ ] LSTM PyTorch দিয়ে implement করা
- [ ] Festival Sales Predictor বানানো
- [ ] Phase 3 সম্পূর্ণ! 🎉

---

[← Month 7](month-07-cnn-vision.md) | [Phase 4 →](../phase-4-nlp-transformers/README.md)
