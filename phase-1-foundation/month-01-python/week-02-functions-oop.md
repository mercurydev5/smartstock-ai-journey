# 🔧 Week 2: Functions & OOP (মে ৮-১৪, ২০২৬)

> **"Functions মানে code-এর super power!"** 💪🇮🇳🚀

[← Week 1](week-01-python-basics.md) | [← Month 1](README.md)

---

## 🎯 Week 2 Goals

এই সপ্তাহ শেষে আপনি পারবেন:
- [ ] Functions define ও call করতে
- [ ] Arguments, default values, *args, **kwargs বুঝতে
- [ ] Classes ও Objects তৈরি করতে
- [ ] Inheritance implement করতে
- [ ] **Project:** Library Management System তৈরি করতে ✅

---

## 📚 Resources

| Resource | Link | কত সময় |
|----------|------|--------|
| ⭐ CS50P Week 1-2 | https://cs50.harvard.edu/python/2022/weeks/1/ | 2 ঘণ্টা |
| ⭐ Corey Schafer OOP (Part 1-6) | https://www.youtube.com/playlist?list=PL-osiE80TeTsqhIuOqKhwlXsIBIdSeYtc | 3 ঘণ্টা |
| Real Python Functions | https://realpython.com/defining-your-own-python-function/ | 1 ঘণ্টা |
| CampusX OOP (Hindi) | https://www.youtube.com/watch?v=qiSCMNBIP2g | 1.5 ঘণ্টা |

---

## 📅 Day-by-Day Breakdown

### Day 8 (মে ৮) — Functions Basics 🔧

**Topics:** def keyword, parameters, return values, scope

```python
# functions_basics.py

def calculate_profit(buy_price, sell_price, quantity):
    """
    পণ্যের লাভ হিসাব করে
    Args:
        buy_price: ক্রয়মূল্য প্রতি ইউনিট
        sell_price: বিক্রয়মূল্য প্রতি ইউনিট
        quantity: পরিমাণ
    Returns:
        মোট লাভ
    """
    profit_per_unit = sell_price - buy_price
    total_profit = profit_per_unit * quantity
    return total_profit

# Function call
rice_profit = calculate_profit(60, 75, 100)
print(f"চাল বিক্রিতে লাভ: ৳{rice_profit}")

# Default parameter
def greet_customer(name, language="bengali"):
    if language == "bengali":
        return f"স্বাগতম, {name} ভাই! আজকে কী নেবেন?"
    else:
        return f"Welcome, {name}! What would you like today?"

print(greet_customer("রহিম"))
print(greet_customer("John", "english"))
```

---

### Day 9 (মে ৯) — Advanced Functions (*args, **kwargs, Lambda)

```python
# advanced_functions.py

# *args - variable number of arguments
def calculate_total(*prices):
    """যেকোনো সংখ্যক দামের যোগফল"""
    return sum(prices)

total = calculate_total(60, 120, 45, 200, 80)
print(f"মোট: ৳{total}")  # ৳505

# **kwargs - keyword arguments
def create_product(**details):
    """নতুন product তৈরি করে"""
    product = {key: value for key, value in details.items()}
    return product

rice = create_product(name="চাল", price=60, unit="kg", stock=100)
print(rice)

# Lambda function
discount = lambda price, pct: price * (1 - pct/100)
print(f"১০% ছাড়ে ১০০৳: {discount(100, 10)}৳")  # 90.0৳

# Higher-order functions
prices = [100, 50, 200, 75, 150]
expensive = list(filter(lambda p: p > 100, prices))
doubled = list(map(lambda p: p * 2, prices))
print(f"দামী পণ্য: {expensive}")
print(f"দ্বিগুণ দাম: {doubled}")
```

---

### Day 10 (মে ১০) — Classes & Objects Basics

```python
# classes_basics.py

class Product:
    """একটি দোকানের পণ্য"""

    # Class variable
    currency = "৳"

    def __init__(self, name, buy_price, sell_price, stock, unit="kg"):
        """Constructor"""
        self.name = name
        self.buy_price = buy_price
        self.sell_price = sell_price
        self.stock = stock
        self.unit = unit

    def profit_per_unit(self):
        """প্রতি unit-এ লাভ"""
        return self.sell_price - self.buy_price

    def total_value(self):
        """মোট stock-এর মূল্য"""
        return self.sell_price * self.stock

    def sell(self, quantity):
        """পণ্য বিক্রি করুন"""
        if quantity > self.stock:
            print(f"❌ মাত্র {self.stock} {self.unit} আছে!")
            return False
        self.stock -= quantity
        revenue = self.sell_price * quantity
        profit = self.profit_per_unit() * quantity
        print(f"✅ {quantity} {self.unit} {self.name} বিক্রি হয়েছে")
        print(f"   আয়: {self.currency}{revenue} | লাভ: {self.currency}{profit}")
        return True

    def __str__(self):
        return f"{self.name} | দাম: {self.currency}{self.sell_price}/{self.unit} | স্টক: {self.stock} {self.unit}"


# Objects তৈরি করুন
rice = Product("বাসমতি চাল", 60, 75, 100)
dal = Product("মসুর ডাল", 100, 120, 50)
oil = Product("সয়াবিন তেল", 160, 180, 30, "litre")

print(rice)
print(dal)
print()
rice.sell(10)
print(f"লাভ প্রতি কেজি: {rice.currency}{rice.profit_per_unit()}")
```

---

### Day 11 (মে ১১) — Inheritance & Polymorphism

```python
# inheritance.py

class Person:
    """Base class"""
    def __init__(self, name, phone):
        self.name = name
        self.phone = phone

    def contact_info(self):
        return f"{self.name}: {self.phone}"


class Customer(Person):
    """Customer inherits from Person"""
    def __init__(self, name, phone, credit_limit=0):
        super().__init__(name, phone)
        self.credit_limit = credit_limit
        self.outstanding = 0
        self.purchase_history = []

    def purchase(self, item, amount):
        self.purchase_history.append({"item": item, "amount": amount})
        print(f"✅ {self.name} কিনলেন: {item} (৳{amount})")

    def credit_purchase(self, item, amount):
        if self.outstanding + amount > self.credit_limit:
            print(f"❌ Credit limit exceed! বাকি {self.credit_limit - self.outstanding}৳ credit আছে")
            return
        self.outstanding += amount
        print(f"📝 Credit entry: {self.name} - {item} ৳{amount} (বাকি: ৳{self.outstanding})")

    def pay_credit(self, amount):
        self.outstanding = max(0, self.outstanding - amount)
        print(f"✅ {self.name} পেমেন্ট: ৳{amount} | বাকি: ৳{self.outstanding}")


class Supplier(Person):
    """Supplier inherits from Person"""
    def __init__(self, name, phone, company):
        super().__init__(name, phone)
        self.company = company
        self.products = []

    def add_product(self, product):
        self.products.append(product)

    def __str__(self):
        return f"Supplier: {self.name} ({self.company}) | Products: {', '.join(self.products)}"


# Usage
rahim = Customer("রহিম", "01711000001", credit_limit=5000)
rahim.purchase("চাল", 600)
rahim.credit_purchase("ডাল", 1200)
rahim.pay_credit(500)

karim_supplier = Supplier("করিম ভাই", "01822000002", "করিম এন্টারপ্রাইজ")
karim_supplier.add_product("চাল")
karim_supplier.add_product("ডাল")
print(karim_supplier)
```

---

### Day 12 (মে ১২) — Special Methods & Properties

```python
# special_methods.py

class Inventory:
    def __init__(self):
        self._products = {}

    def add_product(self, name, price, stock):
        self._products[name] = {"price": price, "stock": stock}

    def __len__(self):
        """len(inventory) কাজ করবে"""
        return len(self._products)

    def __contains__(self, item):
        """'চাল' in inventory কাজ করবে"""
        return item in self._products

    def __getitem__(self, name):
        """inventory['চাল'] কাজ করবে"""
        return self._products.get(name, None)

    def __repr__(self):
        return f"Inventory({len(self._products)} products)"

    @property
    def total_value(self):
        """inventory.total_value (getter)"""
        return sum(p["price"] * p["stock"] for p in self._products.values())

    @property
    def low_stock_items(self):
        """৫-এর কম stock"""
        return [name for name, p in self._products.items() if p["stock"] < 5]


# Usage
inv = Inventory()
inv.add_product("চাল", 75, 100)
inv.add_product("ডাল", 120, 4)  # low stock
inv.add_product("তেল", 180, 30)

print(f"মোট items: {len(inv)}")  # __len__
print(f"চাল আছে? {'চাল' in inv}")  # __contains__
print(f"চালের info: {inv['চাল']}")  # __getitem__
print(f"মোট মূল্য: ৳{inv.total_value}")  # property
print(f"কম স্টক: {inv.low_stock_items}")  # property
print(inv)  # __repr__
```

---

### Day 13 (মে ১৩) — Review & Practice

আজকের কাজ:
1. Week-এর সব concepts review করুন
2. কোনো অংশ বুঝতে সমস্যা হলে YouTube-এ দেখুন
3. Project শুরু করুন (Library System)

---

### Day 14 (মে ১৪) — 📚 Mini Project: Library Management System

```python
# library_system.py

class Book:
    def __init__(self, title, author, isbn, copies=1):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.total_copies = copies
        self.available_copies = copies

    def __str__(self):
        status = "✅ Available" if self.available_copies > 0 else "❌ Not Available"
        return f"📚 {self.title} by {self.author} | ISBN: {self.isbn} | {status} ({self.available_copies}/{self.total_copies})"


class Member:
    def __init__(self, name, member_id):
        self.name = name
        self.member_id = member_id
        self.borrowed_books = []

    def __str__(self):
        return f"👤 {self.name} (ID: {self.member_id}) | Books: {len(self.borrowed_books)}"


class Library:
    def __init__(self, name):
        self.name = name
        self.books = {}
        self.members = {}

    def add_book(self, book):
        self.books[book.isbn] = book
        print(f"✅ Book added: {book.title}")

    def register_member(self, member):
        self.members[member.member_id] = member
        print(f"✅ Member registered: {member.name}")

    def borrow_book(self, member_id, isbn):
        if isbn not in self.books:
            print("❌ Book not found!")
            return
        if member_id not in self.members:
            print("❌ Member not found!")
            return

        book = self.books[isbn]
        member = self.members[member_id]

        if book.available_copies <= 0:
            print(f"❌ '{book.title}' currently not available!")
            return
        if len(member.borrowed_books) >= 3:
            print(f"❌ {member.name} already has 3 books!")
            return

        book.available_copies -= 1
        member.borrowed_books.append(isbn)
        print(f"✅ {member.name} borrowed '{book.title}'")

    def return_book(self, member_id, isbn):
        if isbn not in self.books or member_id not in self.members:
            print("❌ Invalid book or member!")
            return

        member = self.members[member_id]
        if isbn not in member.borrowed_books:
            print(f"❌ {member.name} didn't borrow this book!")
            return

        self.books[isbn].available_copies += 1
        member.borrowed_books.remove(isbn)
        print(f"✅ {member.name} returned '{self.books[isbn].title}'")

    def search_books(self, keyword):
        results = [b for b in self.books.values()
                   if keyword.lower() in b.title.lower() or keyword.lower() in b.author.lower()]
        if results:
            print(f"\n🔍 '{keyword}' খুঁজে পেলাম:")
            for book in results:
                print(f"  {book}")
        else:
            print(f"❌ '{keyword}' পাওয়া যায়নি।")

    def show_all_books(self):
        print(f"\n📚 {self.name} - সব বই:")
        for book in self.books.values():
            print(f"  {book}")


# Demo
library = Library("স্মার্ট পাবলিক লাইব্রেরি")

# Add books
library.add_book(Book("Python Crash Course", "Eric Matthes", "978-1-59327-928-8", 2))
library.add_book(Book("Deep Learning", "Goodfellow", "978-0262035613", 1))
library.add_book(Book("AI সহজ ভাষায়", "বাংলা লেখক", "BN-001", 3))

# Register members
library.register_member(Member("রহিম সাহেব", "M001"))
library.register_member(Member("করিমা বেগম", "M002"))

# Operations
library.show_all_books()
library.borrow_book("M001", "978-1-59327-928-8")
library.borrow_book("M002", "978-0262035613")
library.search_books("Python")
library.return_book("M001", "978-1-59327-928-8")
library.show_all_books()
```

**GitHub-এ push করুন!** 🚀

---

## ✔️ Week 2 Completion Checklist

- [ ] Functions (basic + advanced) শেখা
- [ ] Classes ও Objects তৈরি করা
- [ ] Inheritance implement করা
- [ ] Special methods বোঝা
- [ ] Library Management System তৈরি করা
- [ ] GitHub-এ project push করা
- [ ] ৭টি daily log লেখা

---

[← Week 1](week-01-python-basics.md) | [Week 3 →](week-03-files-modules.md)
