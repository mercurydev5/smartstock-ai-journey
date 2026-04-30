# 🚀 Month 5: Real-World ML (সেপ্টেম্বর ২০২৬)

> **"Model বানালেই হবে না — deploy করতে জানতে হবে!"** 💪🇮🇳🚀

[← Month 4](month-04-classical-ml.md) | [← Phase 2](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | সেপ্টেম্বর ১-৩০, ২০২৬ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | MLOps basics + Deployment |
| 💻 মূল Project | **Stock Price Predictor Web App** 🚀 |
| 📚 মূল Resource | Streamlit + Optuna + Mlflow |

---

## 🗓️ Week-by-Week Plan

### Week 17 (সেপ্টেম্বর ১-৭) — Model Evaluation Deep Dive

```python
# model_evaluation.py
from sklearn.model_selection import (StratifiedKFold, cross_val_score,
                                       learning_curve)
from sklearn.metrics import (confusion_matrix, roc_curve, auc,
                               precision_recall_curve)
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

# StratifiedKFold for imbalanced data
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

# Confusion Matrix visualization
def plot_confusion_matrix(y_true, y_pred, labels):
    cm = confusion_matrix(y_true, y_pred)
    plt.figure(figsize=(8, 6))
    sns.heatmap(cm, annot=True, fmt="d", cmap="Blues",
                xticklabels=labels, yticklabels=labels)
    plt.title("Confusion Matrix")
    plt.ylabel("Actual")
    plt.xlabel("Predicted")
    plt.tight_layout()
    plt.savefig("confusion_matrix.png")
    print("✅ Saved confusion_matrix.png")
```

### Week 18 (সেপ্টেম্বর ৮-১৪) — Hyperparameter Tuning

```python
# hyperparameter_tuning.py
import optuna
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import cross_val_score
from sklearn.datasets import make_classification

X, y = make_classification(n_samples=1000, random_state=42)

def objective(trial):
    n_estimators = trial.suggest_int("n_estimators", 50, 300)
    max_depth = trial.suggest_int("max_depth", 3, 15)
    min_samples_split = trial.suggest_int("min_samples_split", 2, 20)

    model = RandomForestClassifier(
        n_estimators=n_estimators,
        max_depth=max_depth,
        min_samples_split=min_samples_split,
        random_state=42
    )
    score = cross_val_score(model, X, y, cv=3, scoring="accuracy").mean()
    return score

study = optuna.create_study(direction="maximize")
study.optimize(objective, n_trials=50)

print(f"Best accuracy: {study.best_value:.3f}")
print(f"Best params: {study.best_params}")
```

### Week 19 (সেপ্টেম্বর ১৫-২১) — Feature Engineering

```python
# feature_engineering.py
import pandas as pd
import numpy as np

# Stock data feature engineering
def engineer_stock_features(df):
    """Create technical indicators"""
    # Moving averages
    df["ma_5"] = df["close"].rolling(5).mean()
    df["ma_20"] = df["close"].rolling(20).mean()
    df["ma_50"] = df["close"].rolling(50).mean()

    # Momentum
    df["rsi"] = calculate_rsi(df["close"])
    df["macd"] = df["close"].ewm(span=12).mean() - df["close"].ewm(span=26).mean()

    # Volatility
    df["volatility"] = df["close"].rolling(20).std()

    # Price changes
    df["pct_change_1d"] = df["close"].pct_change(1)
    df["pct_change_5d"] = df["close"].pct_change(5)

    # Volume features
    df["volume_ma"] = df["volume"].rolling(10).mean()
    df["volume_ratio"] = df["volume"] / df["volume_ma"]

    return df.dropna()

def calculate_rsi(prices, period=14):
    delta = prices.diff()
    gain = (delta.where(delta > 0, 0)).rolling(window=period).mean()
    loss = (-delta.where(delta < 0, 0)).rolling(window=period).mean()
    rs = gain / loss
    return 100 - (100 / (1 + rs))
```

### Week 20 (সেপ্টেম্বর ২২-৩০) — Streamlit Deployment

```python
# app.py (Streamlit App)
import streamlit as st
import pandas as pd
import numpy as np
import plotly.graph_objects as go
from sklearn.ensemble import RandomForestRegressor
import yfinance as yf

st.set_page_config(page_title="📈 Stock Price Predictor", layout="wide")

st.title("📈 Stock Price Predictor")
st.markdown("**SmartStock AI** — Powered by Machine Learning")

# Sidebar
with st.sidebar:
    st.header("⚙️ Settings")
    ticker = st.text_input("Stock Symbol", "RELIANCE.NS")
    prediction_days = st.slider("Prediction horizon (days)", 1, 30, 7)
    if st.button("🔄 Load Data"):
        st.session_state.load = True

# Main content
col1, col2, col3 = st.columns(3)

@st.cache_data
def load_stock_data(ticker):
    try:
        data = yf.download(ticker, period="2y")
        return data
    except:
        # Return sample data
        dates = pd.date_range(end=pd.Timestamp.now(), periods=500)
        return pd.DataFrame({
            "Close": 2000 + np.cumsum(np.random.randn(500) * 20),
            "Volume": np.random.randint(1000000, 5000000, 500)
        }, index=dates)

data = load_stock_data(ticker)
current_price = data["Close"].iloc[-1]

with col1:
    st.metric("Current Price", f"₹{current_price:.2f}")
with col2:
    change = current_price - data["Close"].iloc[-2]
    st.metric("Day Change", f"₹{change:.2f}", f"{change/data['Close'].iloc[-2]*100:.2f}%")
with col3:
    st.metric("52W High", f"₹{data['Close'].rolling(252).max().iloc[-1]:.2f}")

# Price chart
fig = go.Figure()
fig.add_trace(go.Scatter(x=data.index, y=data["Close"], name="Price", line=dict(color="blue")))
fig.update_layout(title=f"{ticker} Price History", xaxis_title="Date", yaxis_title="Price (₹)")
st.plotly_chart(fig, use_container_width=True)

st.success("✅ To run: `streamlit run app.py`")
```

---

## 🏆 Month 5 Major Project: Stock Price Predictor

**Deploy on Streamlit Cloud (free):**
```bash
# Setup
pip install streamlit yfinance scikit-learn plotly

# Run locally
streamlit run app.py

# Deploy to Streamlit Cloud:
# 1. Push to GitHub
# 2. Go to share.streamlit.io
# 3. Connect repo → Deploy!
```

---

## ✅ Month 5 Checklist

- [ ] Model evaluation techniques শেখা
- [ ] Optuna দিয়ে hyperparameter tuning করা
- [ ] Feature engineering practice করা
- [ ] Streamlit দিয়ে app তৈরি করা
- [ ] Stock Price Predictor deploy করা ✅
- [ ] **Phase 2 সম্পূর্ণ!** 🎉

---

## 🎓 Phase 2 Complete! What's Next?

**আপনি এখন জানেন:**
✅ Linear/Logistic Regression
✅ Decision Trees, Random Forest, XGBoost
✅ Clustering, PCA
✅ Model evaluation & tuning
✅ Streamlit deployment

**Phase 3: Deep Learning শুরু হচ্ছে!** 🧠

---

[← Month 4](month-04-classical-ml.md) | [Phase 3 →](../phase-3-deep-learning/README.md)
