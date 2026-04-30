# 🚀 Week 4: Advanced Python (মে ২২-৩১, ২০২৬)

> **"Advanced Python মানে professional-level programming!"** 💪🇮🇳🚀

[← Week 3](week-03-files-modules.md) | [← Month 1](README.md)

---

## 🎯 Week 4 Goals

এই সপ্তাহ শেষে আপনি পারবেন:
- [ ] Decorators বুঝতে ও তৈরি করতে
- [ ] Generators ও iterators ব্যবহার করতে
- [ ] Context managers তৈরি করতে
- [ ] Comprehensions (list, dict, set) efficiently ব্যবহার করতে
- [ ] `requests` library দিয়ে web scraping করতে
- [ ] **Project:** Web Scraper তৈরি করতে ✅

---

## 📚 Resources

| Resource | Link | কত সময় |
|----------|------|--------|
| ⭐ Corey Schafer Decorators | https://www.youtube.com/watch?v=FsAPt_9Bf3U | 30 মিনিট |
| ⭐ Corey Schafer Generators | https://www.youtube.com/watch?v=bD05uGo_sVI | 30 মিনিট |
| Real Python Decorators | https://realpython.com/primer-on-python-decorators/ | 1 ঘণ্টা |
| Beautiful Soup Tutorial | https://www.youtube.com/watch?v=XVv6mJpFOb0 | 45 মিনিট |

---

## 📅 Day-by-Day Breakdown

### Day 22 (মে ২২) — Decorators

```python
# decorators.py
import time
import functools

# Simple decorator
def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"⏱️ {func.__name__} ran in {end-start:.4f}s")
        return result
    return wrapper

def logger(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"📝 Calling {func.__name__}({args}, {kwargs})")
        result = func(*args, **kwargs)
        print(f"✅ {func.__name__} returned: {result}")
        return result
    return wrapper

@timer
@logger
def process_sales_data(items):
    total = sum(items)
    return total

result = process_sales_data([100, 200, 150, 300, 250])
print(f"Total: ৳{result}")
```

---

### Day 23 (মে ২৩) — Generators

```python
# generators.py

def sales_report_generator(sales_file):
    """Memory-efficient way to process large files"""
    with open(sales_file, "r") as f:
        for line in f:
            yield line.strip()

def fibonacci():
    """Infinite Fibonacci generator"""
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Generator expression (like list comprehension but lazy)
prices = [75, 120, 180, 60, 45]
discounted = (p * 0.9 for p in prices if p > 100)  # 10% discount
for price in discounted:
    print(f"Discounted: ৳{price:.1f}")
```

---

### Day 24-26 (মে ২৪-২৬) — Web Scraping Project

```python
# web_scraper.py
import requests
from bs4 import BeautifulSoup
import json
import time

def scrape_product_prices(url):
    """Scrape product prices from a website"""
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
    }

    try:
        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()
        soup = BeautifulSoup(response.text, "html.parser")
        return soup
    except requests.RequestException as e:
        print(f"❌ Error: {e}")
        return None

# Practice: Scrape quotes from a practice site
def scrape_quotes():
    url = "http://quotes.toscrape.com"
    soup = scrape_product_prices(url)

    if not soup:
        return []

    quotes = []
    for quote in soup.find_all("div", class_="quote"):
        text = quote.find("span", class_="text").text
        author = quote.find("small", class_="author").text
        quotes.append({"text": text, "author": author})

    return quotes

quotes = scrape_quotes()
for q in quotes[:3]:
    print(f"💬 \"{q['text'][:50]}...\" — {q['author']}")

# Save to JSON
with open("scraped_quotes.json", "w", encoding="utf-8") as f:
    json.dump(quotes, f, ensure_ascii=False, indent=2)
print(f"\n✅ Saved {len(quotes)} quotes!")
```

---

## ✔️ Week 4 Completion Checklist

- [ ] Decorators বানানো ও ব্যবহার করা
- [ ] Generators দিয়ে কাজ করা
- [ ] Context managers বোঝা
- [ ] Web scraper তৈরি করা
- [ ] GitHub-এ push করা
- [ ] Month 1 complete! 🎉

## 🏆 Month 1 Complete!

**আপনি শিখেছেন:**
✅ Python basics (variables, loops, conditions)
✅ Functions & OOP (classes, inheritance)
✅ File I/O & JSON (data persistence)
✅ Advanced Python (decorators, generators)
✅ 4টি projects GitHub-এ আছে!

**এখন আপনি Phase 1 Month 2-তে যাওয়ার জন্য ready!** 🚀

---

[← Week 3](week-03-files-modules.md) | [Month 2 →](../month-02-math-tools/README.md)
