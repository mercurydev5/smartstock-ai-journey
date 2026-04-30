# 📈 Week 6: Data Visualization (জুন ৮-১৪, ২০২৬)

> **"একটি ভালো chart হাজার row data-র চেয়ে বেশি বলে!"** 💪🇮🇳🚀

[← Week 5](week-05-numpy-pandas.md) | [← Month 2](README.md)

---

## 🎯 Week 6 Goals

এই সপ্তাহ শেষে আপনি পারবেন:
- [ ] Matplotlib দিয়ে basic charts তৈরি করতে
- [ ] Seaborn দিয়ে beautiful statistical plots করতে
- [ ] Plotly দিয়ে interactive charts তৈরি করতে
- [ ] **Project:** COVID Dashboard তৈরি করতে ✅

---

## 📚 Resources

| Resource | Link | কত সময় |
|----------|------|--------|
| ⭐ Matplotlib Official Tutorial | https://matplotlib.org/stable/tutorials/index.html | 2 ঘণ্টা |
| Seaborn Tutorial | https://seaborn.pydata.org/tutorial.html | 1.5 ঘণ্টা |
| Plotly Express Guide | https://plotly.com/python/plotly-express/ | 1 ঘণ্টা |
| Krish Naik Data Visualization | https://www.youtube.com/@krishnaik06 | YouTube |

---

## 📅 Day-by-Day Breakdown

### Day 36-37 (জুন ৮-৯) — Matplotlib

```python
# matplotlib_basics.py
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# Line Chart — Monthly Sales
months = ["Jan", "Feb", "Mar", "Apr", "May"]
sales = [45000, 52000, 48000, 61000, 58000]

plt.figure(figsize=(10, 5))
plt.plot(months, sales, marker="o", color="blue", linewidth=2, markersize=8)
plt.fill_between(months, sales, alpha=0.1, color="blue")
plt.title("Monthly Sales Trend 2026", fontsize=14, fontweight="bold")
plt.xlabel("Month")
plt.ylabel("Sales (৳)")
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig("monthly_sales.png", dpi=150)
plt.show()

# Bar Chart — Product comparison
products = ["চাল", "ডাল", "তেল", "চিনি", "আটা"]
revenues = [75000, 36000, 54000, 22000, 25000]

plt.figure(figsize=(10, 6))
bars = plt.bar(products, revenues, color=["#FF6B6B", "#4ECDC4", "#45B7D1", "#96CEB4", "#FFEAA7"])
plt.title("Product Revenue Comparison", fontsize=14, fontweight="bold")
plt.xlabel("Product")
plt.ylabel("Revenue (৳)")
for bar, rev in zip(bars, revenues):
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 500,
             f"৳{rev:,}", ha="center", va="bottom", fontsize=10)
plt.tight_layout()
plt.savefig("product_revenue.png", dpi=150)
plt.show()
```

---

### Day 38-39 (জুন ১০-১১) — Seaborn

```python
# seaborn_viz.py
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

sns.set_theme(style="whitegrid")

# Generate data
np.random.seed(42)
data = pd.DataFrame({
    "product": np.random.choice(["চাল", "ডাল", "তেল", "চিনি"], 200),
    "sales": np.random.randint(100, 2000, 200),
    "month": np.random.choice(["Jan", "Feb", "Mar", "Apr", "May"], 200),
    "profit_margin": np.random.uniform(10, 30, 200)
})

# Box Plot
plt.figure(figsize=(10, 6))
sns.boxplot(data=data, x="product", y="sales", palette="husl")
plt.title("Sales Distribution by Product", fontsize=14)
plt.tight_layout()
plt.savefig("boxplot.png", dpi=150)
plt.show()

# Heatmap
pivot = data.pivot_table(values="sales", index="product", columns="month", aggfunc="sum")
plt.figure(figsize=(10, 5))
sns.heatmap(pivot, annot=True, fmt=".0f", cmap="YlOrRd", linewidths=0.5)
plt.title("Sales Heatmap: Product vs Month", fontsize=14)
plt.tight_layout()
plt.savefig("heatmap.png", dpi=150)
plt.show()
```

---

### Day 40-42 (জুন ১২-১৪) — 🦠 Project: COVID Dashboard

```python
# covid_dashboard.py
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
import seaborn as sns

# Download COVID data from: https://github.com/owid/covid-19-data
# Or create sample data:
dates = pd.date_range("2021-01-01", "2021-12-31", freq="W")
india_cases = [10000 + i*500 + (i%4)*2000 for i in range(len(dates))]
india_deaths = [int(c * 0.012) for c in india_cases]

df = pd.DataFrame({
    "date": dates,
    "new_cases": india_cases,
    "new_deaths": india_deaths,
    "total_cases": pd.Series(india_cases).cumsum(),
})

# Dashboard with subplots
fig = plt.figure(figsize=(16, 10))
fig.suptitle("🦠 COVID-19 India Dashboard 2021", fontsize=16, fontweight="bold", y=1.02)

gs = gridspec.GridSpec(2, 3, figure=fig, hspace=0.4, wspace=0.3)

# Plot 1: Daily Cases Trend
ax1 = fig.add_subplot(gs[0, :2])
ax1.fill_between(df["date"], df["new_cases"], alpha=0.4, color="red")
ax1.plot(df["date"], df["new_cases"], color="red", linewidth=1.5)
ax1.set_title("📈 Daily New Cases")
ax1.set_ylabel("Cases")
ax1.grid(True, alpha=0.3)

# Plot 2: Deaths
ax2 = fig.add_subplot(gs[0, 2])
ax2.bar(df["date"], df["new_deaths"], color="darkred", alpha=0.7, width=5)
ax2.set_title("💀 Daily Deaths")
ax2.grid(True, alpha=0.3)

# Plot 3: Cumulative
ax3 = fig.add_subplot(gs[1, :])
ax3.plot(df["date"], df["total_cases"], color="orange", linewidth=2)
ax3.fill_between(df["date"], df["total_cases"], alpha=0.2, color="orange")
ax3.set_title("📊 Cumulative Cases")
ax3.grid(True, alpha=0.3)

plt.savefig("covid_dashboard.png", dpi=150, bbox_inches="tight")
plt.show()
print("✅ Dashboard saved as covid_dashboard.png")
```

**GitHub-এ push করুন!** 🚀

---

## ✔️ Week 6 Completion Checklist

- [ ] Matplotlib দিয়ে line, bar, scatter charts করা
- [ ] Seaborn দিয়ে heatmap, boxplot করা
- [ ] Plotly দিয়ে interactive chart করা
- [ ] COVID Dashboard project complete করা
- [ ] GitHub-এ push করা

---

[← Week 5](week-05-numpy-pandas.md) | [Week 7 →](week-07-linear-algebra.md)
