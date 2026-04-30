# 📊 Week 8: Statistics & Probability (জুন ২২-৩০, ২০২৬)

> **"Statistics ছাড়া ML মানে চোখ বন্ধ করে গাড়ি চালানো!"** 💪🇮🇳🚀

[← Week 7](week-07-linear-algebra.md) | [← Month 2](README.md)

---

## 🎯 Week 8 Goals

এই সপ্তাহ শেষে আপনি বুঝতে পারবেন:
- [ ] Mean, Median, Mode, Variance, Std Dev
- [ ] Normal distribution, Z-score
- [ ] Probability basics
- [ ] Bayes' Theorem (conceptually)
- [ ] Correlation vs Causation
- [ ] Hypothesis testing (basic)
- [ ] **Project:** Business Statistics Report

---

## 📚 Resources

| Resource | Link | কত সময় |
|----------|------|--------|
| ⭐ StatQuest Statistics Playlist | https://www.youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9 | 3 ঘণ্টা |
| Khan Academy Statistics | https://www.khanacademy.org/math/statistics-probability | 2 ঘণ্টা |
| Think Stats (Free Book) | https://greenteapress.com/thinkstats2/ | 1 ঘণ্টা |
| Scipy Stats Tutorial | https://docs.scipy.org/doc/scipy/reference/stats.html | 1 ঘণ্টা |

---

## 📅 Day-by-Day Breakdown

### Day 50-51 (জুন ২২-২৩) — Descriptive Statistics

```python
# descriptive_stats.py
import numpy as np
import pandas as pd
from scipy import stats

# Business data
daily_sales = [1200, 1500, 1100, 2000, 1800, 900, 2500, 1300, 1600, 1400,
               1700, 1200, 2200, 1900, 1000, 1600, 1800, 1400, 2100, 1300,
               1500, 1700, 1900, 1100, 2300, 1600, 1400, 1800, 2000, 1700]

sales = np.array(daily_sales)

print("📊 === Descriptive Statistics ===")
print(f"Count: {len(sales)}")
print(f"Mean (গড়): ৳{sales.mean():.2f}")
print(f"Median (মধ্যমান): ৳{np.median(sales):.2f}")
print(f"Mode (প্রচুরক): ৳{stats.mode(sales, keepdims=True).mode[0]}")
print(f"Std Dev (মান বিচ্যুতি): ৳{sales.std():.2f}")
print(f"Variance (বিভেদ): ৳²{sales.var():.2f}")
print(f"Range (পরিসর): ৳{sales.max() - sales.min()}")
print(f"Min: ৳{sales.min()} | Max: ৳{sales.max()}")
print(f"25th percentile: ৳{np.percentile(sales, 25):.2f}")
print(f"75th percentile: ৳{np.percentile(sales, 75):.2f}")
print(f"IQR: ৳{np.percentile(sales, 75) - np.percentile(sales, 25):.2f}")
```

---

### Day 52-53 (জুন ২৪-২৫) — Probability & Distributions

```python
# probability.py
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

# Normal Distribution
mu, sigma = 1600, 300  # mean sales, std dev

# What is P(sales > 2000)?
prob = 1 - stats.norm.cdf(2000, mu, sigma)
print(f"P(sales > 2000) = {prob:.3f} ({prob:.1%})")

# What sales value is at 90th percentile?
p90 = stats.norm.ppf(0.90, mu, sigma)
print(f"90th percentile sales: ৳{p90:.0f}")

# Z-score
today_sales = 2200
z_score = (today_sales - mu) / sigma
print(f"\nToday's sales: ৳{today_sales}")
print(f"Z-score: {z_score:.2f}")
print(f"This is {z_score:.1f} standard deviations above average")

# Visualize
x = np.linspace(mu - 4*sigma, mu + 4*sigma, 100)
y = stats.norm.pdf(x, mu, sigma)

plt.figure(figsize=(10, 5))
plt.plot(x, y, "b-", linewidth=2)
plt.fill_between(x, y, where=(x > 2000), alpha=0.3, color="red", label="P(>2000)")
plt.axvline(mu, color="green", linestyle="--", label=f"Mean: ৳{mu}")
plt.title("Daily Sales Distribution")
plt.xlabel("Sales (৳)")
plt.legend()
plt.tight_layout()
plt.savefig("sales_distribution.png", dpi=150)
plt.show()
```

---

### Day 54-56 (জুন ২৬-২৮) — Correlation & Hypothesis Testing

```python
# correlation_hypothesis.py
import numpy as np
import pandas as pd
from scipy import stats
import matplotlib.pyplot as plt
import seaborn as sns

# Generate business data
np.random.seed(42)
n = 100
temperature = np.random.uniform(25, 40, n)  # Temperature
cold_drink_sales = 50 + 5 * temperature + np.random.normal(0, 20, n)
hot_tea_sales = 200 - 3 * temperature + np.random.normal(0, 15, n)

df = pd.DataFrame({
    "temperature": temperature,
    "cold_drink_sales": cold_drink_sales,
    "hot_tea_sales": hot_tea_sales
})

# Correlation
corr = df.corr()
print("Correlation Matrix:")
print(corr.round(3))

# Pearson correlation
r_cold, p_cold = stats.pearsonr(temperature, cold_drink_sales)
r_hot, p_hot = stats.pearsonr(temperature, hot_tea_sales)
print(f"\nTemperature vs Cold Drink: r={r_cold:.3f}, p={p_cold:.4f}")
print(f"Interpretation: {'Strong positive' if r_cold > 0.7 else 'Moderate'} correlation")
print(f"Temperature vs Hot Tea: r={r_hot:.3f}, p={p_hot:.4f}")

# Hypothesis Testing
# H0: বৃহস্পতিবার ও শুক্রবারের sales একই
thursday_sales = np.random.normal(1600, 300, 20)
friday_sales = np.random.normal(1900, 350, 20)

t_stat, p_value = stats.ttest_ind(thursday_sales, friday_sales)
print(f"\nT-test: t={t_stat:.3f}, p={p_value:.4f}")
if p_value < 0.05:
    print("✅ Significant! শুক্রবারের sales বেশি (95% confidence)")
else:
    print("❌ Not significant difference")
```

---

### Day 57-60 (জুন ২৯-৩০) — 📋 Project: Business Statistics Report

```python
# business_stats_report.py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
from scipy import stats

print("=" * 60)
print("📊 SmartStock Business Statistics Report")
print("Period: January - May 2026")
print("=" * 60)

# Generate comprehensive business data
np.random.seed(42)
dates = pd.date_range("2026-01-01", "2026-05-31", freq="D")
products = ["চাল", "ডাল", "তেল", "চিনি", "আটা"]

records = []
for date in dates:
    for product in products:
        base_sales = {"চাল": 1500, "ডাল": 800, "তেল": 1200, "চিনি": 600, "আটা": 500}
        sales = np.random.normal(base_sales[product], base_sales[product]*0.2)
        records.append({"date": date, "product": product, "sales": max(0, sales)})

df = pd.DataFrame(records)
df["month"] = df["date"].dt.strftime("%B")
df["day_of_week"] = df["date"].dt.day_name()

# Report sections
print("\n📈 1. Overall Performance")
print(f"   Total Revenue: ৳{df['sales'].sum():,.0f}")
print(f"   Daily Average: ৳{df.groupby('date')['sales'].sum().mean():,.0f}")

print("\n🏆 2. Product Performance")
product_stats = df.groupby("product")["sales"].agg(["sum", "mean", "std"])
for idx, row in product_stats.iterrows():
    cv = row["std"] / row["mean"] * 100
    print(f"   {idx}: Total ৳{row['sum']:,.0f} | Avg ৳{row['mean']:,.0f}/day | CV: {cv:.1f}%")

print("\n📅 3. Monthly Trend")
monthly = df.groupby("month")["sales"].sum()
for month, sales in monthly.items():
    print(f"   {month}: ৳{sales:,.0f}")

print("\n🔍 4. Statistical Tests")
jan_sales = df[df["month"] == "January"].groupby("date")["sales"].sum()
may_sales = df[df["month"] == "May"].groupby("date")["sales"].sum()
t_stat, p_val = stats.ttest_ind(jan_sales, may_sales)
print(f"   Jan vs May t-test: t={t_stat:.2f}, p={p_val:.4f}")
result = "significant growth" if p_val < 0.05 and may_sales.mean() > jan_sales.mean() else "no significant difference"
print(f"   Result: {result}")

print("\n✅ Report Complete! Save as PDF for presentation.")
```

**GitHub-এ push করুন!** 🚀

---

## ✔️ Week 8 Completion Checklist

- [ ] Descriptive statistics বোঝা ও Python-এ calculate করা
- [ ] Normal distribution বোঝা
- [ ] Correlation vs Causation বোঝা
- [ ] Hypothesis testing করা
- [ ] Business Statistics Report তৈরি করা
- [ ] GitHub-এ push করা

## 🎉 Phase 1 Complete!

**আপনি এখন জানেন:**
✅ Python (basics → advanced)
✅ NumPy ও Pandas
✅ Data Visualization
✅ Linear Algebra
✅ Statistics ও Probability

**Phase 2: Machine Learning শুরু হচ্ছে!** 🤖🚀

---

[← Week 7](week-07-linear-algebra.md) | [Phase 2 →](../../phase-2-machine-learning/README.md)
