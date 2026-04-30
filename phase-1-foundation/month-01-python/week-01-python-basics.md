# 🐍 Week 1: Python Basics (মে ১-৭, ২০২৬)

> **"প্রতিটি expert-ই একসময় beginner ছিল!"** 💪🇮🇳🚀

[← Month 1](README.md) | [← Phase 1](../README.md)

---

## 🎯 Week 1 Goals

এই সপ্তাহ শেষে আপনি পারবেন:
- [ ] Python install ও VS Code setup করতে
- [ ] Variables, data types ব্যবহার করতে
- [ ] If/else, for, while loops লিখতে
- [ ] Basic functions তৈরি করতে
- [ ] ছোট programs run করতে
- [ ] **Project:** Number Guessing Game তৈরি করতে ✅

---

## 📚 Resources

| Resource | Link | কত সময় |
|----------|------|--------|
| ⭐ CS50P Week 0 | https://cs50.harvard.edu/python/2022/weeks/0/ | 2 ঘণ্টা |
| ⭐ Python Crash Course (YouTube) - Corey Schafer | https://www.youtube.com/watch?v=YYXdXT2l-Gg | 1.5 ঘণ্টা |
| Python Variables - W3Schools | https://www.w3schools.com/python/python_variables.asp | 30 মিনিট |
| CampusX Python Basics (Hindi) | https://www.youtube.com/watch?v=_YnVLSvT_do | 1 ঘণ্টা |
| Automate the Boring Stuff Ch 1-2 | https://automatetheboringstuff.com/2e/chapter1/ | 1 ঘণ্টা |

---

## 📅 Day-by-Day Breakdown

### Day 1 (মে ১) — Setup & Hello World 🚀
**লক্ষ্য:** Python install করুন এবং প্রথম program লিখুন

**করণীয়:**
1. Python download করুন: https://python.org/downloads
2. VS Code install করুন: https://code.visualstudio.com
3. VS Code-এ Python extension install করুন
4. প্রথম program লিখুন:

```python
# hello.py
print("হ্যালো! আমি Python শিখছি!")
print("আজ থেকে আমার AI journey শুরু!")
name = input("আপনার নাম কী? ")
print(f"স্বাগতম, {name}! এটাই শুরু 🚀")
```

5. Terminal-এ `python hello.py` run করুন
6. Daily log লিখুন ✅

**✔️ Checkpoint:** `print("Hello World")` কাজ করলে Day 1 complete!

---

### Day 2 (মে ২) — Variables & Data Types 📦
**লক্ষ্য:** Python-এর বিভিন্ন data types বুঝুন

**Topics:**
- `int`, `float`, `str`, `bool`
- Variable naming rules
- `type()` function
- Type conversion: `int()`, `str()`, `float()`

**Practice Code:**
```python
# data_types.py
age = 31                    # int
height = 5.8                # float
name = "SmartStock AI"      # str
is_learning = True          # bool

print(f"বয়স: {age}, Type: {type(age)}")
print(f"উচ্চতা: {height}, Type: {type(height)}")
print(f"নাম: {name}, Type: {type(name)}")
print(f"শিখছি: {is_learning}, Type: {type(is_learning)}")

# Type conversion
price_str = "100"
price_int = int(price_str)
print(f"দাম: {price_int + 50}")  # 150
```

**Exercise:** নিজের একটি "ব্যবসার তথ্য" store করুন — দোকানের নাম, মালিকের বয়স, আজকের বিক্রি পরিমাণ, দোকান খোলা আছে কি না।

---

### Day 3 (মে ৩) — Strings & Operations 🔤
**লক্ষ্য:** String manipulate করতে শিখুন

**Topics:**
- String methods: `.upper()`, `.lower()`, `.strip()`, `.split()`, `.replace()`
- String formatting: f-strings
- String indexing ও slicing
- `len()` function

**Practice Code:**
```python
# strings.py
product = "  Basmati Rice  "
print(product.strip())          # "Basmati Rice"
print(product.strip().upper())  # "BASMATI RICE"
print(len(product.strip()))     # 12

# f-string formatting
item = "চাল"
price = 60
quantity = 100
total = price * quantity
print(f"পণ্য: {item} | দাম: ৳{price}/কেজি | পরিমাণ: {quantity} কেজি | মোট: ৳{total}")

# String slicing
message = "SmartStock AI"
print(message[0:5])    # Smart
print(message[-2:])    # AI
print(message[::-1])   # IA kcotStramS (reverse)
```

---

### Day 4 (মে ৪) — Lists & Tuples 📋
**লক্ষ্য:** Collections ব্যবহার করতে শিখুন

**Topics:**
- List creation, indexing, slicing
- List methods: `.append()`, `.remove()`, `.sort()`, `.pop()`
- Tuple (immutable list)
- List comprehension (basic)

**Practice Code:**
```python
# lists.py
# দোকানের products list
products = ["চাল", "ডাল", "তেল", "চিনি", "আটা"]

products.append("লবণ")          # Add item
print(f"Products: {products}")
print(f"মোট items: {len(products)}")
print(f"প্রথম পণ্য: {products[0]}")
print(f"শেষ পণ্য: {products[-1]}")

products.remove("ডাল")          # Remove item
products.sort()                  # Sort alphabetically

# List comprehension
prices = [60, 120, 180, 50, 45, 20]
expensive = [p for p in prices if p > 100]
print(f"দামী পণ্যের দাম: {expensive}")
```

---

### Day 5 (মে ৫) — Dictionaries & Sets 🗄️
**লক্ষ্য:** Key-value pairs দিয়ে data store করুন

**Topics:**
- Dictionary creation, access, update
- `.keys()`, `.values()`, `.items()` methods
- Nested dictionaries
- Sets (unique values)

**Practice Code:**
```python
# dictionaries.py
# একটি product-এর তথ্য
product = {
    "name": "Basmati Rice",
    "bangla_name": "বাসমতি চাল",
    "price": 80,
    "unit": "kg",
    "stock": 50,
    "supplier": "করিম এন্টারপ্রাইজ"
}

print(f"পণ্য: {product['bangla_name']}")
print(f"দাম: ৳{product['price']} প্রতি {product['unit']}")

# Update
product["price"] = 85
product["stock"] -= 5

# Nested dictionary
inventory = {
    "চাল": {"price": 60, "stock": 100},
    "ডাল": {"price": 120, "stock": 50},
    "তেল": {"price": 180, "stock": 30},
}

for item, details in inventory.items():
    print(f"{item}: দাম ৳{details['price']}, স্টক {details['stock']} কেজি")
```

---

### Day 6 (মে ৬) — Control Flow (if/else, loops) 🔄
**লক্ষ্য:** Program-এর flow control করুন

**Topics:**
- `if`, `elif`, `else`
- `for` loop (with range, list, dict)
- `while` loop
- `break`, `continue`

**Practice Code:**
```python
# control_flow.py

# Stock Alert System
stock_levels = {
    "চাল": 100,
    "ডাল": 5,    # low!
    "তেল": 30,
    "চিনি": 2    # critical!
}

LOW_STOCK = 10
CRITICAL_STOCK = 5

for item, quantity in stock_levels.items():
    if quantity <= CRITICAL_STOCK:
        print(f"🚨 CRITICAL: {item} মাত্র {quantity} কেজি বাকি! আজই order করুন!")
    elif quantity <= LOW_STOCK:
        print(f"⚠️ LOW: {item} মাত্র {quantity} কেজি বাকি। শীঘ্রই order করুন।")
    else:
        print(f"✅ OK: {item} - {quantity} কেজি আছে")

# While loop example
total_sales = 0
sales = [150, 200, 350, 100, 500]
i = 0
while i < len(sales):
    total_sales += sales[i]
    i += 1
print(f"\nমোট বিক্রি: ৳{total_sales}")
```

---

### Day 7 (মে ৭) — 🎲 Mini Project: Number Guessing Game

**Project Description:**
একটি Number Guessing Game তৈরি করুন যেখানে:
- Computer 1-100 এর মধ্যে একটি random number বেছে নেবে
- User guess করবে
- Computer বলবে "বেশি" বা "কম"
- কত চেষ্টায় মিলিয়েছে তা দেখাবে
- বাংলায় সব message থাকবে

```python
# number_guessing_game.py
import random

def play_game():
    """Number Guessing Game - বাংলায়"""
    print("=" * 40)
    print("🎲 Number Guessing Game স্বাগতম!")
    print("=" * 40)

    secret_number = random.randint(1, 100)
    attempts = 0
    max_attempts = 7

    print(f"আমি ১ থেকে ১০০-এর মধ্যে একটা সংখ্যা ভেবেছি।")
    print(f"আপনার {max_attempts} বার চেষ্টার সুযোগ আছে।\n")

    while attempts < max_attempts:
        try:
            guess = int(input(f"চেষ্টা {attempts + 1}/{max_attempts}: আপনার guess: "))
        except ValueError:
            print("❌ দয়া করে একটি সংখ্যা লিখুন!")
            continue

        attempts += 1

        if guess == secret_number:
            print(f"\n🎉 অসাধারণ! মাত্র {attempts} চেষ্টায় মিলিয়েছেন!")
            if attempts <= 3:
                print("🏆 Excellent! আপনি একজন champion!")
            elif attempts <= 5:
                print("👍 খুব ভালো!")
            else:
                print("😊 শেষ পর্যন্ত পারলেন!")
            return

        elif guess < secret_number:
            remaining = max_attempts - attempts
            print(f"⬆️ আরো বেশি। {remaining} চেষ্টা বাকি।")
        else:
            remaining = max_attempts - attempts
            print(f"⬇️ আরো কম। {remaining} চেষ্টা বাকি।")

    print(f"\n😞 দুঃখিত! সংখ্যাটি ছিল: {secret_number}")
    print("আবার চেষ্টা করুন!")

def main():
    while True:
        play_game()
        play_again = input("\nআবার খেলবেন? (হ্যাঁ/না): ").strip().lower()
        if play_again not in ["হ্যাঁ", "ha", "yes", "y", "হ", "han"]:
            print("\n🙏 ধন্যবাদ! Happy Coding! 🚀")
            break

if __name__ == "__main__":
    main()
```

**GitHub-এ push করুন!** 🚀

---

## ✔️ Week 1 Completion Checklist

- [ ] Python + VS Code install করা
- [ ] Day 1: Hello World program চালানো
- [ ] Day 2: Data types practice করা
- [ ] Day 3: String manipulation করা
- [ ] Day 4: List & Tuple ব্যবহার করা
- [ ] Day 5: Dictionary ব্যবহার করা
- [ ] Day 6: if/else + loops practice করা
- [ ] Day 7: Number Guessing Game complete করা
- [ ] GitHub-এ project push করা
- [ ] ৭টি daily log লেখা

---

## 💡 Week 1 Tips

> **"শুধু পড়লে হবে না, code করতে হবে!"**
>
> প্রতিটি example নিজে type করুন — copy-paste করবেন না।
> ভুল হলে ভালো — ভুল থেকেই শেখা হয়।

- 🔴 **Common Mistake:** IndentationError — Python-এ spacing খুব গুরুত্বপূর্ণ!
- 🟡 **Tip:** VS Code-এ auto-indent on রাখুন
- 🟢 **Pro Tip:** `print()` দিয়ে debug করুন

---

[← Month 1 README](README.md) | [Week 2 →](week-02-functions-oop.md)
