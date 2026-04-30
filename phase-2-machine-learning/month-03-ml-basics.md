# 🤖 Month 3: ML Basics (জুলাই ২০২৬)

> **"Andrew Ng-এর course হলো ML শেখার সবচেয়ে ভালো পথ!"** 💪🇮🇳🚀

[← Phase 2 README](README.md) | [← Main README](../README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | জুলাই ১-৩১, ২০২৬ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | ML fundamentals + first models |
| 💻 মূল Project | House Price Prediction + Email Spam Classifier |
| 📚 মূল Resource | Andrew Ng ML Specialization (Coursera) |

---

## 🗓️ Week-by-Week Plan

### Week 9 (জুলাই ১-৭)
**Topics:**
- Machine Learning কী? Supervised vs Unsupervised
- Linear Regression (one variable)
- Cost function, Gradient Descent
- Scikit-learn দিয়ে প্রথম model

**Practice Code:**
```python
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score
import numpy as np
import matplotlib.pyplot as plt

# Simple house price example
np.random.seed(42)
size = np.random.uniform(500, 3000, 100)
price = 100 * size + np.random.normal(0, 50000, 100)

X = size.reshape(-1, 1)
y = price

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

print(f"R² Score: {r2_score(y_test, y_pred):.3f}")
print(f"RMSE: ৳{np.sqrt(mean_squared_error(y_test, y_pred)):,.0f}")
print(f"Coefficient: {model.coef_[0]:.2f} (price per sqft)")
```

### Week 10 (জুলাই ৮-১৪)
**Topics:**
- Multiple Linear Regression
- Logistic Regression (binary classification)
- Feature scaling (StandardScaler, MinMaxScaler)
- Confusion matrix, Accuracy, Precision, Recall, F1

**Project: Email Spam Classifier**
```python
from sklearn.linear_model import LogisticRegression
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics import classification_report

# Sample spam data
emails = [
    ("Free money! Click now! Win prize!", "spam"),
    ("Meeting tomorrow at 3pm", "ham"),
    ("URGENT: Your account needs verification", "spam"),
    ("Mom's birthday dinner this Saturday?", "ham"),
]

texts, labels = zip(*emails)
vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(texts)
# ... train model
```

### Week 11 (জুলাই ১৫-২১)
**Topics:**
- Regularization: L1 (Lasso), L2 (Ridge), Elastic Net
- Overfitting vs Underfitting
- Bias-Variance tradeoff
- Cross-validation (K-Fold)

### Week 12 (জুলাই ২২-৩১)
**Topics:**
- Polynomial regression
- Kaggle competition এ প্রথম submission
- Monthly project: House Price Prediction
- Model selection ও evaluation best practices

---

## 🏆 Month 3 Major Project: House Price Prediction

**Dataset:** Use Kaggle's "House Prices: Advanced Regression Techniques"

```python
# house_price_prediction.py
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestRegressor
from sklearn.linear_model import Ridge
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import cross_val_score
from sklearn.metrics import mean_squared_error
import warnings
warnings.filterwarnings("ignore")

# Load Kaggle dataset (or create sample)
def create_sample_data(n=1000):
    np.random.seed(42)
    df = pd.DataFrame({
        "size_sqft": np.random.uniform(500, 5000, n),
        "bedrooms": np.random.randint(1, 6, n),
        "bathrooms": np.random.randint(1, 4, n),
        "age_years": np.random.randint(0, 50, n),
        "distance_city": np.random.uniform(1, 50, n),
        "has_parking": np.random.randint(0, 2, n),
    })
    df["price"] = (200 * df["size_sqft"] +
                   50000 * df["bedrooms"] +
                   30000 * df["bathrooms"] -
                   5000 * df["age_years"] -
                   10000 * df["distance_city"] +
                   20000 * df["has_parking"] +
                   np.random.normal(0, 50000, n))
    return df

df = create_sample_data()
X = df.drop("price", axis=1)
y = df["price"]

from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Models
models = {
    "Ridge Regression": Ridge(alpha=1.0),
    "Random Forest": RandomForestRegressor(n_estimators=100, random_state=42),
}

for name, model in models.items():
    model.fit(X_train_scaled, y_train)
    y_pred = model.predict(X_test_scaled)
    rmse = np.sqrt(mean_squared_error(y_test, y_pred))
    cv_scores = cross_val_score(model, X_train_scaled, y_train, cv=5, scoring="r2")
    print(f"\n{name}:")
    print(f"  RMSE: ৳{rmse:,.0f}")
    print(f"  R² (CV): {cv_scores.mean():.3f} ± {cv_scores.std():.3f}")

print("\n✅ House Price Model complete!")
```

---

## ✅ Month 3 Checklist

- [ ] Andrew Ng ML Specialization Week 1-3 সম্পূর্ণ করা
- [ ] Linear Regression নিজে implement করা
- [ ] Logistic Regression দিয়ে spam classifier
- [ ] Kaggle-এ House Price competition submission
- [ ] GitHub-এ projects push করা

---

## 🧠 Self-Assessment Questions

1. Gradient Descent কীভাবে কাজ করে? নিজের ভাষায় বলুন
2. Overfitting কী? কীভাবে prevent করবেন?
3. Precision আর Recall-এর মধ্যে কোনটা কখন বেশি গুরুত্বপূর্ণ?
4. L1 ও L2 regularization-এর মধ্যে পার্থক্য কী?

---

[← Phase 2 README](README.md) | [Month 4 →](month-04-classical-ml.md)
