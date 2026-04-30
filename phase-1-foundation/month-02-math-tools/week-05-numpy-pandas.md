# 🔢 Week 5: NumPy & Pandas (জুন ১-৭, ২০২৬)

> **"NumPy + Pandas = Data Science-এর দুই হাত!"** 💪🇮🇳🚀

[← Month 2](README.md) | [← Phase 1](../README.md)

---

## 🎯 Week 5 Goals

এই সপ্তাহ শেষে আপনি পারবেন:
- [ ] NumPy arrays তৈরি ও manipulate করতে
- [ ] Pandas DataFrame দিয়ে data analyze করতে
- [ ] CSV load করে data explore করতে
- [ ] Data filter, sort, group করতে
- [ ] **Project:** Sales Data Analysis তৈরি করতে ✅

---

## 📚 Resources

| Resource | Link | কত সময় |
|----------|------|--------|
| ⭐ Kaggle Pandas Course | https://www.kaggle.com/learn/pandas | 4 ঘণ্টা |
| NumPy Quickstart | https://numpy.org/doc/stable/user/quickstart.html | 1 ঘণ্টা |
| CampusX Pandas Tutorial | https://www.youtube.com/watch?v=RhEjmHeDNoA | 2 ঘণ্টা |
| Keith Galli Pandas | https://www.youtube.com/watch?v=vmEHCJofslg | 1 ঘণ্টা |

---

## 📅 Day-by-Day Breakdown

### Day 29 (জুন ১) — NumPy Basics

```bash
pip install numpy pandas
```

```python
# numpy_basics.py
import numpy as np

# Arrays
arr = np.array([1, 2, 3, 4, 5])
prices = np.array([60, 75, 120, 180, 45])
quantities = np.array([100, 80, 50, 30, 200])

# Operations - সব element-এ একসাথে
revenue = prices * quantities
print(f"Revenue per product: {revenue}")
print(f"Total revenue: ৳{revenue.sum()}")
print(f"Average price: ৳{prices.mean():.2f}")
print(f"Max revenue item index: {revenue.argmax()}")

# 2D Arrays (Matrix)
sales_matrix = np.array([
    [100, 80, 90],   # Week 1
    [120, 95, 110],  # Week 2
    [90, 100, 85],   # Week 3
    [130, 120, 140], # Week 4
])
print(f"\nMonthly sales shape: {sales_matrix.shape}")
print(f"Weekly totals: {sales_matrix.sum(axis=1)}")
print(f"Product totals: {sales_matrix.sum(axis=0)}")
```

---

### Day 30 (জুন ২) — Pandas Series & DataFrame

```python
# pandas_basics.py
import pandas as pd
import numpy as np

# Series
products = pd.Series([75, 120, 180, 60, 45],
                      index=["চাল", "ডাল", "তেল", "চিনি", "লবণ"],
                      name="price")
print("Product Prices:")
print(products)
print(f"\nচালের দাম: ৳{products['চাল']}")
print(f"দামী পণ্য (>100):\n{products[products > 100]}")

# DataFrame
data = {
    "product": ["চাল", "ডাল", "তেল", "চিনি", "আটা"],
    "buy_price": [60, 100, 160, 45, 35],
    "sell_price": [75, 120, 180, 55, 42],
    "stock": [100, 50, 30, 80, 120],
    "unit": ["kg", "kg", "litre", "kg", "kg"]
}

df = pd.DataFrame(data)
print("\n=== Product Inventory ===")
print(df)
print(f"\nSummary:\n{df.describe()}")

# Calculated columns
df["profit_per_unit"] = df["sell_price"] - df["buy_price"]
df["total_stock_value"] = df["sell_price"] * df["stock"]
df["profit_margin"] = (df["profit_per_unit"] / df["sell_price"] * 100).round(1)

print("\n=== With Calculations ===")
print(df[["product", "profit_per_unit", "total_stock_value", "profit_margin"]])
```

---

### Day 31 (জুন ৩) — Data Loading & Exploration

```python
# data_exploration.py
import pandas as pd

# Create sample sales CSV
import random
from datetime import datetime, timedelta

dates = pd.date_range("2026-01-01", "2026-05-31", freq="D")
products = ["চাল", "ডাল", "তেল", "চিনি", "আটা", "লবণ"]
data = []
for date in dates:
    for _ in range(random.randint(3, 8)):
        product = random.choice(products)
        quantity = random.randint(1, 20)
        price = {"চাল": 75, "ডাল": 120, "তেল": 180, "চিনি": 55, "আটা": 42, "লবণ": 25}[product]
        data.append({"date": date, "product": product, "quantity": quantity,
                     "unit_price": price, "total": quantity * price})

sales_df = pd.DataFrame(data)
sales_df.to_csv("sales_data.csv", index=False)

# Load and explore
df = pd.read_csv("sales_data.csv")
print("Shape:", df.shape)
print("\nFirst 5 rows:")
print(df.head())
print("\nData types:")
print(df.dtypes)
print("\nMissing values:")
print(df.isnull().sum())
print("\nBasic stats:")
print(df["total"].describe())
```

---

### Day 32 (জুন ৪) — Data Manipulation

```python
# data_manipulation.py
import pandas as pd

df = pd.read_csv("sales_data.csv")
df["date"] = pd.to_datetime(df["date"])
df["month"] = df["date"].dt.month
df["day_of_week"] = df["date"].dt.day_name()

# Filter
rice_sales = df[df["product"] == "চাল"]
big_sales = df[df["total"] > 1000]

# Sort
top_sales = df.nlargest(5, "total")[["date", "product", "quantity", "total"]]
print("Top 5 Sales:")
print(top_sales)

# GroupBy
monthly_sales = df.groupby("month")["total"].sum()
print("\nMonthly Sales:")
print(monthly_sales)

product_analysis = df.groupby("product").agg(
    total_sold=("quantity", "sum"),
    total_revenue=("total", "sum"),
    avg_transaction=("total", "mean")
).round(2)
print("\nProduct Analysis:")
print(product_analysis.sort_values("total_revenue", ascending=False))
```

---

### Day 33-35 (জুন ৫-৭) — 📊 Project: Sales Data Analysis

```python
# sales_analysis_project.py
import pandas as pd
import numpy as np

def generate_sample_data():
    """Generate realistic sales data"""
    import random
    from datetime import datetime, timedelta
    # ... (same as above)
    pass

class SalesAnalyzer:
    def __init__(self, filepath):
        self.df = pd.read_csv(filepath)
        self.df["date"] = pd.to_datetime(self.df["date"])
        print(f"✅ Loaded {len(self.df)} sales records")

    def summary(self):
        print("\n📊 === Sales Summary ===")
        print(f"Period: {self.df['date'].min()} → {self.df['date'].max()}")
        print(f"Total Records: {len(self.df)}")
        print(f"Total Revenue: ৳{self.df['total'].sum():,.0f}")
        print(f"Average Transaction: ৳{self.df['total'].mean():.2f}")

    def top_products(self, n=5):
        print(f"\n🏆 Top {n} Products by Revenue:")
        top = self.df.groupby("product")["total"].sum().nlargest(n)
        for product, revenue in top.items():
            print(f"  {product}: ৳{revenue:,.0f}")

    def monthly_trend(self):
        print("\n📅 Monthly Sales Trend:")
        monthly = self.df.groupby(self.df["date"].dt.strftime("%Y-%m"))["total"].sum()
        for month, revenue in monthly.items():
            bar = "█" * int(revenue / 5000)
            print(f"  {month}: {bar} ৳{revenue:,.0f}")

    def daily_avg(self):
        print("\n📆 Average by Day of Week:")
        self.df["day"] = self.df["date"].dt.day_name()
        day_avg = self.df.groupby("day")["total"].mean()
        days_order = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
        for day in days_order:
            if day in day_avg:
                print(f"  {day}: ৳{day_avg[day]:.2f}")

    def generate_report(self):
        self.summary()
        self.top_products()
        self.monthly_trend()
        self.daily_avg()
        print("\n✅ Report Complete! 🎉")

# Run
analyzer = SalesAnalyzer("sales_data.csv")
analyzer.generate_report()
```

**GitHub-এ push করুন!** 🚀

---

## ✔️ Week 5 Completion Checklist

- [ ] NumPy install ও array operations করা
- [ ] Pandas DataFrame তৈরি ও explore করা
- [ ] CSV data load করে analyze করা
- [ ] GroupBy, filter, sort করা
- [ ] Sales Analysis project complete করা
- [ ] GitHub-এ push করা

---

[← Month 2 README](README.md) | [Week 6 →](week-06-visualization.md)
