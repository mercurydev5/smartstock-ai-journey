# 🌱 Beginner Projects (Phase 1-2)

> **"প্রতিটি expert ছিলেন একজন beginner। আপনার যাত্রা শুরু হোক এখানে!"**

[← Projects README](./README.md) | [← Main README](../README.md)

---

## Overview

| | Details |
|---|---|
| **Phase** | 1-2 (Month 1-6) |
| **Projects** | 12 projects |
| **Skills** | Python, Data Analysis, ML Basics |
| **Estimated Time** | 6 months (2 hrs/day) |

---

## 📦 Project 1: Number Guessing Game

### 🎯 Objective
Python এর basics ব্যবহার করে একটি interactive game বানানো।

### 📚 Skills Practiced
- Variables, data types
- `if/elif/else` conditionals
- `while` loops
- Functions
- `random` module
- User input handling

### 💻 Starter Code Outline
```python
import random

def number_guessing_game():
    """
    Computer একটি 1-100 এর মধ্যে সংখ্যা বেছে নেয়।
    User ৭টি chance পায় guess করতে।
    প্রতি guess-এ hint দেওয়া হয়।
    """
    secret = random.randint(1, 100)
    attempts = 0
    max_attempts = 7

    print("🎮 সংখ্যা অনুমান করুন! (১-১০০ এর মধ্যে)")

    while attempts < max_attempts:
        # TODO: User input নিন
        # TODO: Guess check করুন
        # TODO: "বেশি" বা "কম" hint দিন
        # TODO: জিতলে congratulate করুন
        pass

    # TODO: Game over message

if __name__ == "__main__":
    number_guessing_game()
```

### ✅ Completion Criteria
- [ ] Game সঠিকভাবে start ও end হয়
- [ ] Hint সঠিক (higher/lower)
- [ ] Max attempts logic কাজ করে
- [ ] Score tracking আছে
- [ ] Replay option আছে

### 🚀 Bonus Challenges
1. Difficulty levels add করুন (Easy: 10 attempts, Hard: 3 attempts)
2. High score leaderboard তৈরি করুন (file-এ save করুন)
3. Bengali/English bilingual messages যোগ করুন
4. GUI version বানান (tkinter দিয়ে)

---

## 🔢 Project 2: Calculator App

### 🎯 Objective
Basic arithmetic থেকে scientific functions পর্যন্ত calculator।

### 📚 Skills Practiced
- Functions এবং modules
- Exception handling (try/except)
- While loops
- String formatting
- Math module

### 💻 Starter Code Outline
```python
import math

def calculate(num1: float, operator: str, num2: float) -> float:
    """
    দুটি সংখ্যার উপর operation করুন।
    Operators: +, -, *, /, %, **, //
    """
    operations = {
        '+': lambda a, b: a + b,
        '-': lambda a, b: a - b,
        '*': lambda a, b: a * b,
        '/': lambda a, b: a / b if b != 0 else None,
        # TODO: বাকি operators যোগ করুন
    }
    # TODO: Operation execute করুন
    pass

def scientific_calc(num: float, function: str) -> float:
    """Scientific functions: sqrt, log, sin, cos, tan"""
    # TODO: Implement করুন
    pass

def main():
    """Main calculator loop"""
    history = []  # Calculation history

    while True:
        # TODO: User input নিন
        # TODO: Calculate করুন
        # TODO: Result দেখান
        # TODO: History-তে save করুন
        pass
```

### ✅ Completion Criteria
- [ ] সব basic operations কাজ করে
- [ ] Division by zero handle করে
- [ ] Calculation history দেখানো যায়
- [ ] Scientific functions আছে (sqrt, log, trig)
- [ ] Error messages user-friendly

### 🚀 Bonus Challenges
1. Unit converter যোগ করুন (kg↔lbs, km↔miles, ₹↔BDT)
2. Expression parser বানান (`2 + 3 * 4` directly input করা যাবে)
3. Tkinter GUI বানান
4. Currency converter with live rates (API ব্যবহার করুন)

---

## 📚 Project 3: Library Management System

### 🎯 Objective
Books, members, borrowing track করার system। OOP practice।

### 📚 Skills Practiced
- Object-Oriented Programming (classes)
- File I/O (JSON/CSV)
- List/Dictionary operations
- Date handling
- Search and filter

### 💻 Starter Code Outline
```python
import json
from datetime import datetime, timedelta
from dataclasses import dataclass, asdict
from typing import List, Optional

@dataclass
class Book:
    isbn: str
    title: str
    author: str
    available: bool = True
    borrowed_by: Optional[str] = None
    due_date: Optional[str] = None

@dataclass
class Member:
    member_id: str
    name: str
    phone: str
    borrowed_books: List[str] = None

class Library:
    def __init__(self, data_file: str = "library.json"):
        self.data_file = data_file
        self.books: List[Book] = []
        self.members: List[Member] = []
        self.load_data()

    def add_book(self, book: Book) -> bool:
        """নতুন book যোগ করুন"""
        # TODO: ISBN check করুন (duplicate না হলে add করুন)
        pass

    def borrow_book(self, isbn: str, member_id: str) -> bool:
        """Member book borrow করবে"""
        # TODO: Book available? Member exists? Check করুন
        # TODO: Due date = আজ + 14 দিন
        pass

    def return_book(self, isbn: str) -> dict:
        """Book return, late fine calculate"""
        # TODO: Fine = ৳5 per day late
        pass

    def search_books(self, query: str) -> List[Book]:
        """Title বা author দিয়ে search"""
        # TODO: Case-insensitive search
        pass

    def save_data(self):
        """JSON file-এ save করুন"""
        # TODO: Serialize এবং save করুন
        pass

    def load_data(self):
        """File থেকে load করুন"""
        # TODO: Deserialize এবং load করুন
        pass
```

### ✅ Completion Criteria
- [ ] Book add/remove/search করা যায়
- [ ] Member registration
- [ ] Borrow/return system
- [ ] Late fine calculation
- [ ] Data JSON-এ persist হয়
- [ ] Overdue books list দেখানো যায়

### 🚀 Bonus Challenges
1. CSV export/import feature
2. Email notification for overdue (fake SMTP)
3. CLI with rich library (colorful tables)
4. Web UI with Flask/Streamlit

---

## ✅ Project 4: To-Do App with File Save

### 🎯 Objective
Task management app যা data save করে রাখে।

### 📚 Skills Practiced
- File I/O (JSON)
- CRUD operations
- Date/time handling
- Sorting and filtering
- CLI design

### 💻 Starter Code Outline
```python
import json
import os
from datetime import datetime
from typing import List, Dict

TASKS_FILE = "tasks.json"

def load_tasks() -> List[Dict]:
    if os.path.exists(TASKS_FILE):
        with open(TASKS_FILE, 'r', encoding='utf-8') as f:
            return json.load(f)
    return []

def save_tasks(tasks: List[Dict]):
    with open(TASKS_FILE, 'w', encoding='utf-8') as f:
        json.dump(tasks, f, ensure_ascii=False, indent=2)

def add_task(title: str, priority: str = "medium", due_date: str = None) -> Dict:
    """নতুন task তৈরি করুন"""
    task = {
        "id": generate_id(),
        "title": title,
        "priority": priority,  # low, medium, high
        "due_date": due_date,
        "completed": False,
        "created_at": datetime.now().isoformat()
    }
    # TODO: Save করুন
    return task

def complete_task(task_id: str) -> bool:
    """Task complete mark করুন"""
    # TODO: ID দিয়ে find করুন, completed=True করুন
    pass

def list_tasks(filter_by: str = "all") -> List[Dict]:
    """Tasks list করুন"""
    # Filters: "all", "pending", "completed", "high-priority", "today"
    pass

def delete_task(task_id: str) -> bool:
    """Task delete করুন"""
    pass
```

### ✅ Completion Criteria
- [ ] Task add/complete/delete করা যায়
- [ ] Priority levels (high/medium/low)
- [ ] Due date support
- [ ] Filter by status/priority
- [ ] Data persists between sessions
- [ ] Statistics (completed %, overdue count)

### 🚀 Bonus Challenges
1. Category/tag system
2. Recurring tasks
3. Streamlit web interface
4. Bengali language support

---

## 🌐 Project 5: Web Scraper (BeautifulSoup)

### 🎯 Objective
Wikipedia বা news site থেকে data extract করুন।

### 📚 Skills Practiced
- `requests` library
- `BeautifulSoup` HTML parsing
- CSS selectors
- Data cleaning
- CSV/JSON export
- Rate limiting (polite scraping)

### 💻 Starter Code Outline
```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time
import csv

def scrape_quotes():
    """
    http://quotes.toscrape.com থেকে quotes scrape করুন।
    Practice site - scraping-friendly!
    """
    base_url = "http://quotes.toscrape.com"
    quotes_data = []
    page = 1

    while True:
        url = f"{base_url}/page/{page}/"
        response = requests.get(url)

        if response.status_code != 200:
            break

        soup = BeautifulSoup(response.text, 'html.parser')
        quotes = soup.find_all('div', class_='quote')

        if not quotes:
            break

        for quote in quotes:
            # TODO: text, author, tags extract করুন
            pass

        page += 1
        time.sleep(1)  # Rate limiting - be polite!

    return quotes_data

def scrape_book_prices():
    """
    https://books.toscrape.com থেকে book prices scrape করুন
    """
    # TODO: Implement করুন
    # Extract: title, price, rating, availability
    pass

def save_to_csv(data: list, filename: str):
    """Data CSV-তে save করুন"""
    df = pd.DataFrame(data)
    df.to_csv(filename, index=False)
    print(f"Saved {len(data)} records to {filename}")
```

### ✅ Completion Criteria
- [ ] Successfully extracts data from quotes.toscrape.com
- [ ] Handles pagination (multiple pages)
- [ ] Saves data to CSV
- [ ] Rate limiting implemented
- [ ] Error handling (network errors, missing elements)
- [ ] Data analysis with Pandas

### 🚀 Bonus Challenges
1. Flipkart product prices scraper (educational only)
2. Wikipedia article scraper with categories
3. News headlines aggregator
4. Selenium for dynamic JavaScript pages

---

## 📊 Project 6: Sales Data Analysis (Pandas)

### 🎯 Objective
E-commerce sales dataset analyze করে business insights বের করুন।

### 📚 Skills Practiced
- Pandas DataFrame operations
- GroupBy, Pivot tables
- Data cleaning (null values, duplicates)
- Statistical analysis
- Matplotlib/Seaborn visualization

### 💻 Starter Code Outline
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime

# Dataset: https://www.kaggle.com/datasets/carrie1/ecommerce-data
df = pd.read_csv('ecommerce_data.csv', encoding='latin1')

def clean_data(df: pd.DataFrame) -> pd.DataFrame:
    """Data cleaning করুন"""
    # TODO: null values handle করুন
    # TODO: duplicate rows remove করুন
    # TODO: date columns parse করুন
    # TODO: negative quantities remove করুন
    return df

def analyze_sales(df: pd.DataFrame) -> dict:
    """Key metrics calculate করুন"""
    analysis = {
        # TODO: মোট revenue
        # TODO: Best selling products (top 10)
        # TODO: Revenue by country
        # TODO: Revenue by month
        # TODO: Average order value
    }
    return analysis

def visualize_trends(df: pd.DataFrame):
    """Charts তৈরি করুন"""
    fig, axes = plt.subplots(2, 2, figsize=(15, 10))

    # TODO: Monthly revenue line chart
    # TODO: Top 10 products bar chart
    # TODO: Country-wise revenue pie chart
    # TODO: Sales heatmap (day × hour)

    plt.tight_layout()
    plt.savefig('sales_analysis.png')
    plt.show()
```

### ✅ Completion Criteria
- [ ] Dataset load এবং clean করা
- [ ] Revenue, orders, products analysis
- [ ] Time-series trend analysis
- [ ] Top customers identified
- [ ] 4+ visualizations
- [ ] Actionable business insights written

### 🚀 Bonus Challenges
1. Customer segmentation (RFM analysis)
2. Cohort analysis
3. Interactive dashboard with Plotly
4. Use Indian retail dataset

---

## 🦠 Project 7: COVID-19 Dashboard (Plotly)

### 🎯 Objective
COVID-19 data visualize করে interactive dashboard বানানো।

### 📚 Skills Practiced
- Plotly Express & Graph Objects
- Plotly Dash (web dashboard)
- API data fetching
- Geographic visualization (choropleth maps)
- Time-series animation

### 💻 Starter Code Outline
```python
import plotly.express as px
import plotly.graph_objects as go
from dash import Dash, dcc, html, Input, Output
import pandas as pd
import requests

def fetch_covid_data():
    """disease.sh API থেকে data fetch করুন (free!)"""
    url = "https://disease.sh/v3/covid-19/countries"
    response = requests.get(url)
    data = response.json()
    df = pd.DataFrame(data)
    return df

def create_world_map(df: pd.DataFrame):
    """World choropleth map"""
    fig = px.choropleth(
        df,
        locations="countryInfo",
        color="cases",
        hover_name="country",
        color_continuous_scale="Reds",
        title="Global COVID-19 Cases"
    )
    return fig

# Dash App
app = Dash(__name__)

app.layout = html.Div([
    html.H1("🦠 COVID-19 Global Dashboard"),
    # TODO: Dropdown for country selection
    # TODO: Date range picker
    # TODO: Key metrics cards
    # TODO: Interactive charts
    # TODO: Data table
])

@app.callback(...)
def update_charts(country, date_range):
    # TODO: Filter data and update charts
    pass

if __name__ == '__main__':
    app.run_server(debug=True)
```

### ✅ Completion Criteria
- [ ] Live data from API
- [ ] World map visualization
- [ ] Country comparison charts
- [ ] Time-series trends
- [ ] Interactive controls (dropdown, date range)
- [ ] Deployed on Heroku/Render (free tier)

### 🚀 Bonus Challenges
1. India state-wise data
2. Vaccination progress tracker
3. Prediction model overlay
4. Dark mode toggle

---

## 🏠 Project 8: House Price Prediction (Linear Regression)

### 🎯 Objective
Machine learning দিয়ে house price predict করুন।

### 📚 Skills Practiced
- Scikit-learn basics
- Train/test split
- Feature engineering
- Linear, Ridge, Lasso regression
- Model evaluation (RMSE, R²)
- Cross-validation

### 💻 Starter Code Outline
```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.linear_model import LinearRegression, Ridge, Lasso
from sklearn.metrics import mean_squared_error, r2_score
import matplotlib.pyplot as plt

# Dataset: https://www.kaggle.com/datasets/harlfoxem/housesalesprediction
df = pd.read_csv('house_sales.csv')

def engineer_features(df: pd.DataFrame) -> pd.DataFrame:
    """Feature engineering করুন"""
    # TODO: Age of house (current_year - yr_built)
    # TODO: Renovated or not (binary)
    # TODO: Price per sqft
    # TODO: High-value neighborhood flag
    return df

def preprocess(df: pd.DataFrame):
    """Data preprocessing"""
    # TODO: Handle missing values
    # TODO: Encode categorical features
    # TODO: Scale numerical features
    # TODO: Train/test split (80/20)
    pass

def train_and_evaluate(X_train, X_test, y_train, y_test):
    """Multiple models train এবং compare করুন"""
    models = {
        'Linear Regression': LinearRegression(),
        'Ridge': Ridge(alpha=1.0),
        'Lasso': Lasso(alpha=1.0)
    }

    results = {}
    for name, model in models.items():
        model.fit(X_train, y_train)
        y_pred = model.predict(X_test)
        results[name] = {
            'RMSE': np.sqrt(mean_squared_error(y_test, y_pred)),
            'R2': r2_score(y_test, y_pred)
        }
    return results
```

### ✅ Completion Criteria
- [ ] EDA (10+ visualizations)
- [ ] Feature engineering done
- [ ] 3+ models trained and compared
- [ ] RMSE < ₹50,000 (or $50K for USA dataset)
- [ ] Feature importance chart
- [ ] Simple prediction function (input → price)

### 🚀 Bonus Challenges
1. Use Indian property dataset (Bangalore/Mumbai)
2. XGBoost/RandomForest comparison
3. Streamlit deployment with interactive inputs
4. SHAP explanations (explainable AI)

---

## 📧 Project 9: Email Spam Classifier

### 🎯 Objective
Naive Bayes এবং NLP দিয়ে spam detect করুন।

### 📚 Skills Practiced
- Text preprocessing (tokenization, stopwords, stemming)
- TF-IDF vectorization
- Naive Bayes classifier
- Precision, Recall, F1 score
- Confusion matrix

### 💻 Starter Code Outline
```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import classification_report, confusion_matrix
import re
import nltk

# Dataset: SMS Spam Collection Dataset (Kaggle)
df = pd.read_csv('spam.csv', encoding='latin-1')[['v1', 'v2']]
df.columns = ['label', 'text']

def clean_text(text: str) -> str:
    """Text preprocessing"""
    text = text.lower()
    text = re.sub(r'[^a-zA-Z\s]', '', text)
    # TODO: Remove stopwords
    # TODO: Apply stemming/lemmatization
    return text

def build_classifier():
    """Train spam classifier"""
    # 1. Clean text
    df['cleaned'] = df['text'].apply(clean_text)

    # 2. TF-IDF features
    vectorizer = TfidfVectorizer(max_features=5000, ngram_range=(1, 2))
    X = vectorizer.fit_transform(df['cleaned'])
    y = (df['label'] == 'spam').astype(int)

    # 3. Split
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

    # 4. Train
    model = MultinomialNB()
    model.fit(X_train, y_train)

    # 5. Evaluate
    y_pred = model.predict(X_test)
    print(classification_report(y_test, y_pred))

    return model, vectorizer

def predict_spam(text: str, model, vectorizer) -> dict:
    """Single email classify করুন"""
    cleaned = clean_text(text)
    features = vectorizer.transform([cleaned])
    prediction = model.predict(features)[0]
    probability = model.predict_proba(features)[0]
    return {
        "is_spam": bool(prediction),
        "spam_probability": float(probability[1]),
        "label": "🚫 SPAM" if prediction else "✅ HAM"
    }
```

### ✅ Completion Criteria
- [ ] Accuracy > 97%
- [ ] Precision > 95% for spam class
- [ ] Confusion matrix visualization
- [ ] Example predictions (5 ham, 5 spam)
- [ ] Word cloud for spam vs ham
- [ ] Streamlit demo app

### 🚀 Bonus Challenges
1. Deep learning model (LSTM) comparison
2. Bengali spam classifier
3. Multilingual spam detection
4. Real email integration (Gmail API)

---

## 🔄 Project 10: Customer Churn Prediction

### 🎯 Objective
Telecom customers এর churn predict করুন।

### 📚 Skills Practiced
- Imbalanced dataset handling (SMOTE)
- Random Forest, XGBoost
- Feature importance
- Business metrics (precision vs recall tradeoff)
- ROC-AUC curve

### 💻 Starter Code Outline
```python
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from xgboost import XGBClassifier
from sklearn.metrics import roc_auc_score, classification_report
from imblearn.over_sampling import SMOTE
import shap

# Dataset: Telco Customer Churn (Kaggle)
df = pd.read_csv('WA_Fn-UseC_-Telco-Customer-Churn.csv')

def preprocess_churn(df):
    """Feature engineering for churn"""
    # TODO: Encode categorical features
    # TODO: TotalCharges to numeric (has spaces)
    # TODO: Create: charges_per_month, tenure_group
    # TODO: Handle imbalanced classes with SMOTE
    pass

def train_churn_model(X_train, y_train):
    """Train multiple models"""
    models = {
        'RandomForest': RandomForestClassifier(n_estimators=100),
        'XGBoost': XGBClassifier(use_label_encoder=False)
    }
    # TODO: Train, evaluate, compare
    pass

def explain_churn(model, X_test):
    """SHAP দিয়ে why customer churned explain করুন"""
    explainer = shap.TreeExplainer(model)
    shap_values = explainer.shap_values(X_test)
    shap.summary_plot(shap_values, X_test)
```

### ✅ Completion Criteria
- [ ] ROC-AUC > 0.85
- [ ] SMOTE দিয়ে imbalance handle
- [ ] SHAP explanation plot
- [ ] Business report: "Top 5 reasons for churn"
- [ ] Retention strategy suggestions

### 🚀 Bonus Challenges
1. Indian telecom dataset
2. Real-time prediction API (FastAPI)
3. Automated email alerts for high-risk customers
4. LTV (Lifetime Value) prediction

---

## 👥 Project 11: Customer Segmentation (K-Means)

### 🎯 Objective
Mall customers segment করুন এবং marketing strategy বানান।

### 📚 Skills Practiced
- K-Means clustering
- Elbow method for optimal k
- PCA for visualization
- RFM analysis (Recency, Frequency, Monetary)
- Business interpretation

### 💻 Starter Code Outline
```python
import pandas as pd
import numpy as np
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt
import seaborn as sns

# Dataset: Mall Customer Segmentation (Kaggle)
df = pd.read_csv('Mall_Customers.csv')

def find_optimal_k(X: np.ndarray, max_k: int = 10) -> int:
    """Elbow method দিয়ে best k খুঁজুন"""
    inertias = []
    for k in range(1, max_k + 1):
        kmeans = KMeans(n_clusters=k, random_state=42)
        kmeans.fit(X)
        inertias.append(kmeans.inertia_)

    # TODO: Plot elbow curve
    # TODO: Find elbow point
    pass

def segment_customers(df: pd.DataFrame) -> pd.DataFrame:
    """K-Means দিয়ে segment করুন"""
    # Features: Annual Income, Spending Score, Age
    scaler = StandardScaler()
    X = scaler.fit_transform(df[['Annual Income (k$)', 'Spending Score (1-100)']])

    optimal_k = find_optimal_k(X)
    kmeans = KMeans(n_clusters=optimal_k, random_state=42)
    df['Segment'] = kmeans.fit_predict(X)

    return df, kmeans

def name_segments(df: pd.DataFrame) -> dict:
    """Segments-এর business names দিন"""
    # Analyze each cluster and name them:
    # e.g., "High-Value Premium Shoppers", "Budget Conscious", "Impulse Buyers"
    segment_profiles = {}
    for seg in df['Segment'].unique():
        subset = df[df['Segment'] == seg]
        segment_profiles[seg] = {
            'avg_income': subset['Annual Income (k$)'].mean(),
            'avg_spending': subset['Spending Score (1-100)'].mean(),
            'count': len(subset)
        }
    return segment_profiles
```

### ✅ Completion Criteria
- [ ] Optimal k found with elbow method
- [ ] 5 distinct customer segments
- [ ] 2D scatter plot with cluster colors
- [ ] Business name for each segment
- [ ] Marketing strategy for each segment
- [ ] Streamlit deployment

### 🚀 Bonus Challenges
1. RFM-based segmentation
2. Hierarchical clustering comparison
3. DBSCAN (finds non-circular clusters)
4. Indian retail customer dataset

---

## 📈 Project 12: Stock Price Predictor ⭐ (Streamlit Deployment!)

### 🎯 Objective
ML দিয়ে stock price predict করুন এবং Streamlit-এ deploy করুন।

### 📚 Skills Practiced
- `yfinance` for free stock data
- Time-series feature engineering
- Random Forest for regression
- Technical indicators (MA, RSI, MACD)
- Streamlit deployment
- Model serialization (joblib)

### 💻 Starter Code Outline
```python
import yfinance as yf
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_percentage_error
import streamlit as st
import joblib
import plotly.graph_objects as go

def fetch_stock_data(ticker: str, period: str = "2y") -> pd.DataFrame:
    """Yahoo Finance থেকে stock data নামান"""
    stock = yf.Ticker(ticker)
    df = stock.history(period=period)
    return df

def add_technical_indicators(df: pd.DataFrame) -> pd.DataFrame:
    """Technical indicators যোগ করুন"""
    # Moving Averages
    df['MA_5'] = df['Close'].rolling(window=5).mean()
    df['MA_20'] = df['Close'].rolling(window=20).mean()
    df['MA_50'] = df['Close'].rolling(window=50).mean()

    # TODO: RSI (Relative Strength Index)
    # TODO: MACD
    # TODO: Bollinger Bands
    # TODO: Volume moving average
    # TODO: Price momentum (1-day, 5-day returns)

    return df.dropna()

def train_model(df: pd.DataFrame):
    """Model train করুন"""
    feature_cols = ['MA_5', 'MA_20', 'MA_50', 'Volume',
                    'RSI', 'MACD', 'momentum_1d', 'momentum_5d']
    X = df[feature_cols]
    y = df['Close'].shift(-1).dropna()  # Next day price

    # Train/test split (80/20, no shuffle for time-series!)
    split = int(len(X) * 0.8)
    X_train, X_test = X[:split], X[split:]
    y_train, y_test = y[:split], y[split:]

    model = RandomForestRegressor(n_estimators=100, random_state=42)
    model.fit(X_train, y_train)

    return model, X_test, y_test

# Streamlit App
def main():
    st.title("📈 Stock Price Predictor")
    st.markdown("*Powered by Machine Learning*")

    # Sidebar
    ticker = st.sidebar.text_input("Stock Ticker", "RELIANCE.NS")
    # RELIANCE.NS, TCS.NS, INFY.NS for Indian stocks!

    if st.button("Predict"):
        with st.spinner("Fetching data and training model..."):
            df = fetch_stock_data(ticker)
            df = add_technical_indicators(df)
            model, X_test, y_test = train_model(df)

            # Display results
            st.metric("Prediction Accuracy",
                      f"{(1 - mean_absolute_percentage_error(y_test, model.predict(X_test)))*100:.1f}%")

            # Plot
            fig = go.Figure()
            fig.add_trace(go.Scatter(y=y_test.values, name="Actual"))
            fig.add_trace(go.Scatter(y=model.predict(X_test), name="Predicted"))
            st.plotly_chart(fig)

if __name__ == "__main__":
    main()
```

### ✅ Completion Criteria
- [ ] Works with any stock ticker (especially Indian: .NS suffix)
- [ ] 5+ technical indicators
- [ ] MAPE < 5% on test set
- [ ] Beautiful Streamlit interface
- [ ] **Deployed on Streamlit Community Cloud** (free!)
- [ ] GitHub repo with README
- [ ] Share link on LinkedIn!

### 🚀 Bonus Challenges
1. Multiple stock comparison
2. Portfolio optimization (Sharpe ratio)
3. News sentiment + price prediction
4. LSTM model comparison
5. Paper trading simulation

---

## 📊 Progress Tracker

| # | Project | Status | GitHub | Deployed | LinkedIn Post |
|---|---------|--------|--------|----------|---------------|
| 1 | Number Guessing Game | ⬜ | ⬜ | N/A | ⬜ |
| 2 | Calculator | ⬜ | ⬜ | N/A | ⬜ |
| 3 | Library System | ⬜ | ⬜ | N/A | ⬜ |
| 4 | To-Do App | ⬜ | ⬜ | N/A | ⬜ |
| 5 | Web Scraper | ⬜ | ⬜ | N/A | ⬜ |
| 6 | Sales Analysis | ⬜ | ⬜ | N/A | ⬜ |
| 7 | COVID Dashboard | ⬜ | ⬜ | ⬜ Heroku | ⬜ |
| 8 | House Price ML | ⬜ | ⬜ | ⬜ Streamlit | ⬜ |
| 9 | Spam Classifier | ⬜ | ⬜ | ⬜ Streamlit | ⬜ |
| 10 | Churn Prediction | ⬜ | ⬜ | ⬜ Streamlit | ⬜ |
| 11 | Customer Segmentation | ⬜ | ⬜ | ⬜ Streamlit | ⬜ |
| 12 | Stock Predictor ⭐ | ⬜ | ⬜ | ⬜ Streamlit | ⬜ |

---

> 💪🇮🇳🚀 **Phase 1-2 Projects শেষ করুন! আপনি ML-এর জগতে প্রবেশ করে ফেলেছেন!**
