# 📁 Week 3: Files & Modules (মে ১৫-২১, ২০২৬)

> **"Real programs data store করে — files ও databases-এ!"** 💪🇮🇳🚀

[← Week 2](week-02-functions-oop.md) | [← Month 1](README.md)

---

## 🎯 Week 3 Goals

এই সপ্তাহ শেষে আপনি পারবেন:
- [ ] Text files read/write করতে
- [ ] JSON data সংরক্ষণ ও লোড করতে
- [ ] CSV files নিয়ে কাজ করতে
- [ ] pip দিয়ে packages install করতে
- [ ] নিজের modules তৈরি করতে
- [ ] **Project:** To-Do CLI App তৈরি করতে ✅

---

## 📚 Resources

| Resource | Link | কত সময় |
|----------|------|--------|
| ⭐ CS50P Week 6 (File I/O) | https://cs50.harvard.edu/python/2022/weeks/6/ | 1.5 ঘণ্টা |
| Real Python: File I/O | https://realpython.com/read-write-files-python/ | 45 মিনিট |
| Python JSON Tutorial | https://www.w3schools.com/python/python_json.asp | 30 মিনিট |
| Corey Schafer Modules | https://www.youtube.com/watch?v=CqvZ3vGoGs0 | 30 মিনিট |

---

## 📅 Day-by-Day Breakdown

### Day 15 (মে ১৫) — Text File I/O

```python
# file_io.py

# Write to file
with open("sales_log.txt", "w", encoding="utf-8") as f:
    f.write("তারিখ | পণ্য | পরিমাণ | মোট\n")
    f.write("2026-05-15 | চাল | 10 kg | ৳750\n")
    f.write("2026-05-15 | ডাল | 5 kg | ৳600\n")

# Read from file
with open("sales_log.txt", "r", encoding="utf-8") as f:
    content = f.read()
    print(content)

# Append to file
with open("sales_log.txt", "a", encoding="utf-8") as f:
    f.write("2026-05-15 | তেল | 2 litre | ৳360\n")

# Read line by line
with open("sales_log.txt", "r", encoding="utf-8") as f:
    for i, line in enumerate(f, 1):
        print(f"Line {i}: {line.strip()}")
```

---

### Day 16 (মে ১৬) — JSON Files

```python
# json_files.py
import json

# Save inventory to JSON
inventory = {
    "store_name": "করিম স্টোর",
    "last_updated": "2026-05-16",
    "products": [
        {"name": "চাল", "price": 75, "stock": 100, "unit": "kg"},
        {"name": "ডাল", "price": 120, "stock": 50, "unit": "kg"},
        {"name": "তেল", "price": 180, "stock": 30, "unit": "litre"},
    ]
}

# Write JSON
with open("inventory.json", "w", encoding="utf-8") as f:
    json.dump(inventory, f, ensure_ascii=False, indent=2)
print("✅ Inventory saved!")

# Read JSON
with open("inventory.json", "r", encoding="utf-8") as f:
    loaded = json.load(f)

print(f"\nStore: {loaded['store_name']}")
for product in loaded["products"]:
    print(f"  {product['name']}: ৳{product['price']}/{product['unit']} | Stock: {product['stock']}")
```

---

### Day 17 (মে ১৭) — CSV Files

```python
# csv_files.py
import csv

# Write CSV
headers = ["তারিখ", "পণ্য", "পরিমাণ", "ইউনিট", "দাম", "মোট"]
sales_data = [
    ["2026-05-17", "চাল", 10, "kg", 75, 750],
    ["2026-05-17", "ডাল", 5, "kg", 120, 600],
    ["2026-05-17", "তেল", 3, "litre", 180, 540],
]

with open("sales.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerow(headers)
    writer.writerows(sales_data)

# Read CSV
with open("sales.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    total = 0
    for row in reader:
        print(f"{row['তারিখ']} | {row['পণ্য']} | {row['পরিমাণ']} {row['ইউনিট']} | ৳{row['মোট']}")
        total += int(row['মোট'])

print(f"\nমোট বিক্রি: ৳{total}")
```

---

### Day 18 (মে ১৮) — Exception Handling

```python
# exception_handling.py

def safe_divide(a, b):
    try:
        result = a / b
        return result
    except ZeroDivisionError:
        print("❌ Error: শূন্য দিয়ে ভাগ করা যায় না!")
        return None
    except TypeError as e:
        print(f"❌ Type Error: {e}")
        return None
    finally:
        print("Division operation attempted.")


def load_data(filename):
    try:
        with open(filename, "r", encoding="utf-8") as f:
            return json.load(f)
    except FileNotFoundError:
        print(f"❌ File '{filename}' not found!")
        return None
    except json.JSONDecodeError:
        print(f"❌ Invalid JSON in '{filename}'!")
        return None


# Custom Exception
class InsufficientStockError(Exception):
    def __init__(self, item, requested, available):
        self.item = item
        self.requested = requested
        self.available = available
        super().__init__(f"{item}: {requested} চাইলেন, কিন্তু আছে মাত্র {available}")


def sell_item(name, stock, quantity):
    if quantity > stock:
        raise InsufficientStockError(name, quantity, stock)
    return stock - quantity

try:
    new_stock = sell_item("চাল", 5, 10)
except InsufficientStockError as e:
    print(f"❌ Stock Error: {e}")
```

---

### Day 19 (মে ১৯) — Modules & Packages

নিজের module তৈরি করুন:

```python
# utils/calculations.py
def calculate_profit(buy_price, sell_price, quantity):
    return (sell_price - buy_price) * quantity

def calculate_gst(amount, rate=18):
    return amount * rate / 100

def format_currency(amount):
    return f"৳{amount:,.2f}"
```

```python
# utils/validators.py
def is_valid_price(price):
    return isinstance(price, (int, float)) and price > 0

def is_valid_quantity(qty):
    return isinstance(qty, int) and qty > 0
```

```python
# main.py
from utils.calculations import calculate_profit, format_currency
from utils.validators import is_valid_price

profit = calculate_profit(60, 75, 100)
print(f"লাভ: {format_currency(profit)}")
```

---

### Day 20 (মে ২০) — pip & Third-Party Packages

```bash
# Terminal-এ run করুন:
pip install requests colorama tabulate
```

```python
# packages_demo.py
import requests
from tabulate import tabulate
from colorama import Fore, Style, init

init()  # colorama init

# Tabulate দিয়ে সুন্দর table
inventory = [
    ["চাল", 75, 100, "৳7,500"],
    ["ডাল", 120, 50, "৳6,000"],
    ["তেল", 180, 30, "৳5,400"],
]
headers = ["পণ্য", "দাম/ইউনিট", "স্টক", "মোট মূল্য"]
print(tabulate(inventory, headers=headers, tablefmt="grid"))

# Colorama দিয়ে colored output
print(Fore.GREEN + "✅ Stock level OK" + Style.RESET_ALL)
print(Fore.RED + "❌ Low Stock Alert!" + Style.RESET_ALL)
print(Fore.YELLOW + "⚠️ Stock running low" + Style.RESET_ALL)
```

---

### Day 21 (মে ২১) — ✅ Mini Project: To-Do CLI App

```python
# todo_app.py
import json
import os
from datetime import datetime

TODO_FILE = "todos.json"

def load_todos():
    if os.path.exists(TODO_FILE):
        with open(TODO_FILE, "r", encoding="utf-8") as f:
            return json.load(f)
    return []

def save_todos(todos):
    with open(TODO_FILE, "w", encoding="utf-8") as f:
        json.dump(todos, f, ensure_ascii=False, indent=2)

def add_todo(title, priority="medium"):
    todos = load_todos()
    todo = {
        "id": len(todos) + 1,
        "title": title,
        "priority": priority,
        "completed": False,
        "created_at": datetime.now().strftime("%Y-%m-%d %H:%M")
    }
    todos.append(todo)
    save_todos(todos)
    print(f"✅ Added: {title}")

def list_todos(show_completed=False):
    todos = load_todos()
    if not todos:
        print("📋 কোনো কাজ নেই!")
        return
    priority_emoji = {"high": "🔴", "medium": "🟡", "low": "🟢"}
    print("\n📋 আপনার কাজের তালিকা:")
    for todo in todos:
        if not show_completed and todo["completed"]:
            continue
        status = "✅" if todo["completed"] else "⬜"
        emoji = priority_emoji.get(todo["priority"], "⬜")
        print(f"  {status} [{todo['id']}] {emoji} {todo['title']} ({todo['created_at']})")

def complete_todo(todo_id):
    todos = load_todos()
    for todo in todos:
        if todo["id"] == todo_id:
            todo["completed"] = True
            save_todos(todos)
            print(f"✅ Completed: {todo['title']}")
            return
    print(f"❌ ID {todo_id} পাওয়া যায়নি!")

def delete_todo(todo_id):
    todos = load_todos()
    todos = [t for t in todos if t["id"] != todo_id]
    save_todos(todos)
    print(f"🗑️ Deleted todo #{todo_id}")

def main():
    print("=" * 50)
    print("📋 SmartStock To-Do Manager")
    print("=" * 50)

    while True:
        print("\n1. কাজ যোগ করুন\n2. তালিকা দেখুন\n3. সম্পন্ন করুন\n4. মুছুন\n5. বের হন")
        choice = input("\nআপনার choice: ").strip()

        if choice == "1":
            title = input("কাজের বিবরণ: ")
            priority = input("Priority (high/medium/low): ") or "medium"
            add_todo(title, priority)
        elif choice == "2":
            list_todos()
        elif choice == "3":
            list_todos()
            todo_id = int(input("কোন কাজ সম্পন্ন? (ID): "))
            complete_todo(todo_id)
        elif choice == "4":
            list_todos()
            todo_id = int(input("কোনটা মুছবেন? (ID): "))
            delete_todo(todo_id)
        elif choice == "5":
            print("👋 পরে দেখা হবে!")
            break

if __name__ == "__main__":
    main()
```

**GitHub-এ push করুন!** 🚀

---

## ✔️ Week 3 Completion Checklist

- [ ] Text file read/write করা
- [ ] JSON save ও load করা
- [ ] CSV নিয়ে কাজ করা
- [ ] Exception handling করা
- [ ] নিজের module তৈরি করা
- [ ] pip দিয়ে packages install করা
- [ ] To-Do App তৈরি করা
- [ ] GitHub-এ push করা

---

[← Week 2](week-02-functions-oop.md) | [Week 4 →](week-04-advanced-python.md)
