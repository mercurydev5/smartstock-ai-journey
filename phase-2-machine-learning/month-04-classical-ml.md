# 🌲 Month 4: Classical ML (আগস্ট ২০২৬)

> **"Classical ML algorithms এখনো industry-র backbone!"** 💪🇮🇳🚀

[← Month 3](month-03-ml-basics.md) | [← Phase 2](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | আগস্ট ১-৩১, ২০২৬ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | Tree models + Ensemble + Clustering |
| 💻 মূল Project | Customer Churn Prediction + Customer Segmentation |
| 📚 মূল Resource | StatQuest + Hands-on ML Book |

---

## 🗓️ Week-by-Week Plan

### Week 13 (আগস্ট ১-৭) — Decision Trees & Random Forest

```python
# decision_tree_example.py
from sklearn.tree import DecisionTreeClassifier, export_text
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import make_classification

X, y = make_classification(n_samples=1000, n_features=10, random_state=42)

# Decision Tree
dt = DecisionTreeClassifier(max_depth=5, random_state=42)
dt.fit(X[:800], y[:800])
print("Decision Tree accuracy:", dt.score(X[800:], y[800:]))

# Random Forest (ensemble of trees)
rf = RandomForestClassifier(n_estimators=100, random_state=42)
rf.fit(X[:800], y[:800])
print("Random Forest accuracy:", rf.score(X[800:], y[800:]))

# Feature importance
import pandas as pd
feature_imp = pd.Series(rf.feature_importances_,
                         index=[f"feature_{i}" for i in range(10)])
print("\nTop 5 Features:")
print(feature_imp.nlargest(5))
```

### Week 14 (আগস্ট ৮-১৪) — XGBoost & LightGBM

```python
# gradient_boosting.py
import xgboost as xgb
import lightgbm as lgb
from sklearn.model_selection import cross_val_score

# XGBoost — Kaggle competitions-এর king
xgb_model = xgb.XGBClassifier(n_estimators=200, learning_rate=0.1,
                                max_depth=5, random_state=42)
xgb_scores = cross_val_score(xgb_model, X, y, cv=5, scoring="accuracy")
print(f"XGBoost CV: {xgb_scores.mean():.3f} ± {xgb_scores.std():.3f}")

# LightGBM — faster than XGBoost
lgb_model = lgb.LGBMClassifier(n_estimators=200, learning_rate=0.1, random_state=42)
lgb_scores = cross_val_score(lgb_model, X, y, cv=5, scoring="accuracy")
print(f"LightGBM CV: {lgb_scores.mean():.3f} ± {lgb_scores.std():.3f}")
```

### Week 15 (আগস্ট ১৫-২১) — Clustering & PCA

```python
# clustering.py
from sklearn.cluster import KMeans
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

# Customer Segmentation using K-Means
data = {
    "monthly_spend": [5000, 15000, 8000, 25000, 3000, 18000, 12000, 30000],
    "visit_frequency": [15, 8, 12, 5, 20, 7, 10, 4],
    "avg_basket_size": [300, 2000, 700, 5000, 150, 2500, 1200, 7500],
}
import pandas as pd
df = pd.DataFrame(data)

from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(df)

kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
df["segment"] = kmeans.fit_predict(X_scaled)

segment_names = {0: "Economy Shoppers", 1: "Regular Customers", 2: "Premium Buyers"}
df["segment_name"] = df["segment"].map(segment_names)
print(df[["monthly_spend", "visit_frequency", "segment_name"]])
```

### Week 16 (আগস্ট ২২-৩১) — SVMs + Month Project

---

## 🏆 Month 4 Major Project: Customer Churn Prediction

**Problem:** কোন customer দোকানে আর আসবে না predict করো

```python
# customer_churn.py
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report, roc_auc_score
from sklearn.preprocessing import LabelEncoder

# Create realistic customer data
np.random.seed(42)
n = 1000
df = pd.DataFrame({
    "customer_id": range(1, n+1),
    "months_since_last_visit": np.random.exponential(3, n),
    "total_purchases": np.random.randint(1, 100, n),
    "avg_spend_per_visit": np.random.uniform(100, 5000, n),
    "days_as_customer": np.random.randint(30, 1000, n),
    "complaints": np.random.randint(0, 5, n),
    "discount_used": np.random.randint(0, 2, n),
})

# Churn = haven't visited in 30+ days
df["churned"] = ((df["months_since_last_visit"] > 2) &
                 (df["total_purchases"] < 20)).astype(int)

print(f"Churn rate: {df['churned'].mean():.1%}")

X = df.drop(["customer_id", "churned"], axis=1)
y = df["churned"]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = RandomForestClassifier(n_estimators=100, random_state=42, class_weight="balanced")
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

print("\nClassification Report:")
print(classification_report(y_test, y_pred, target_names=["Retained", "Churned"]))
print(f"ROC-AUC: {roc_auc_score(y_test, model.predict_proba(X_test)[:, 1]):.3f}")

# Feature importance
feat_imp = pd.Series(model.feature_importances_, index=X.columns).nlargest(5)
print("\nTop Churn Indicators:")
print(feat_imp)
```

---

## ✅ Month 4 Checklist

- [ ] Decision Tree ও Random Forest implement করা
- [ ] XGBoost দিয়ে Kaggle competition
- [ ] K-Means clustering করা
- [ ] Customer Churn Prediction project
- [ ] Customer Segmentation project
- [ ] GitHub-এ push করা

---

[← Month 3](month-03-ml-basics.md) | [Month 5 →](month-05-real-world-ml.md)
