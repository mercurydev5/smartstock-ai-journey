# 🏗️ Month 18: Product Development (October 2027)

> **"এখন code লেখার সময় - আপনার স্বপ্নকে software-এ রূপান্তরিত করুন!"**

[← Phase 7 README](./README.md) | [← Main README](../README.md)

---

## 📅 Month Overview

| | Details |
|---|---|
| **Month** | 18 of 20 |
| **Timeline** | October 2027 |
| **Theme** | 🏗️ Full-Stack Product Development |
| **Goal** | SmartStock AI MVP সম্পূর্ণ তৈরি করা |

---

## 🗓️ Week-by-Week Plan

### Week 1: Backend Foundation (Oct 1-7)
- [ ] FastAPI project setup & architecture
- [ ] PostgreSQL database design
- [ ] JWT authentication system
- [ ] REST API endpoints (inventory CRUD)
- [ ] Celery task queue setup
- [ ] Redis caching layer
- [ ] Unit tests for APIs

### Week 2: AI Engine Integration (Oct 8-14)
- [ ] SmartStock-7B API integration
- [ ] LangChain RAG pipeline
- [ ] Qdrant vector database setup
- [ ] Whisper speech-to-text integration
- [ ] ElevenLabs TTS integration
- [ ] Bengali/Hindi language processing
- [ ] AI response caching

### Week 3: Frontend Development (Oct 15-21)
- [ ] Next.js 14 project setup
- [ ] Dashboard UI design
- [ ] Inventory management screens
- [ ] Sales analytics charts
- [ ] AI chat interface
- [ ] Responsive design (mobile-first)
- [ ] Razorpay payment integration

### Week 4: Mobile & WhatsApp (Oct 22-31)
- [ ] React Native app setup (Expo)
- [ ] Cross-platform inventory screens
- [ ] Push notifications
- [ ] WhatsApp bot (Twilio/Meta API)
- [ ] Voice command via WhatsApp
- [ ] End-to-end testing
- [ ] Docker containerization

---

## 🖥️ FastAPI Backend

### Project Structure
```
smartstock-backend/
├── app/
│   ├── main.py              # FastAPI app entry point
│   ├── config.py            # Settings & environment variables
│   ├── database.py          # SQLAlchemy + PostgreSQL
│   ├── auth/
│   │   ├── router.py        # Auth endpoints
│   │   ├── models.py        # User models
│   │   └── utils.py         # JWT helpers
│   ├── inventory/
│   │   ├── router.py        # Inventory CRUD APIs
│   │   ├── models.py        # Inventory models
│   │   └── schemas.py       # Pydantic schemas
│   ├── ai/
│   │   ├── router.py        # AI chat endpoints
│   │   ├── rag.py           # RAG pipeline
│   │   └── tts.py           # TTS integration
│   ├── payments/
│   │   └── razorpay.py      # Payment processing
│   └── tasks/
│       └── celery_app.py    # Background tasks
├── tests/
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
```

### FastAPI App Setup
```python
# app/main.py
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from app.auth.router import router as auth_router
from app.inventory.router import router as inventory_router
from app.ai.router import router as ai_router
from app.payments.router import router as payment_router

app = FastAPI(
    title="SmartStock AI",
    description="AI-powered inventory management for kirana stores",
    version="1.0.0"
)

# CORS for frontend
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000", "https://smartstockai.com"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Include routers
app.include_router(auth_router, prefix="/api/auth", tags=["auth"])
app.include_router(inventory_router, prefix="/api/inventory", tags=["inventory"])
app.include_router(ai_router, prefix="/api/ai", tags=["ai"])
app.include_router(payment_router, prefix="/api/payments", tags=["payments"])

@app.get("/")
async def root():
    return {
        "message": "SmartStock AI API",
        "version": "1.0.0",
        "status": "running"
    }

@app.get("/health")
async def health_check():
    return {"status": "healthy"}
```

### Database Design (PostgreSQL)
```sql
-- Users table
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    phone_number VARCHAR(15) UNIQUE NOT NULL,
    name VARCHAR(100),
    store_name VARCHAR(200),
    store_type VARCHAR(50),  -- 'kirana', 'pharmacy', 'electronics'
    language_preference VARCHAR(10) DEFAULT 'bn',  -- 'bn', 'hi', 'en'
    subscription_tier VARCHAR(20) DEFAULT 'free',  -- 'free', 'basic', 'pro'
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Products table
CREATE TABLE products (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    name VARCHAR(200) NOT NULL,
    name_bn VARCHAR(200),  -- Bengali name
    barcode VARCHAR(50),
    category VARCHAR(100),
    current_stock INTEGER DEFAULT 0,
    min_stock INTEGER DEFAULT 5,   -- reorder threshold
    max_stock INTEGER DEFAULT 100,
    purchase_price DECIMAL(10,2),
    selling_price DECIMAL(10,2),
    unit VARCHAR(20) DEFAULT 'piece',  -- 'kg', 'litre', 'piece'
    supplier VARCHAR(200),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Transactions table
CREATE TABLE transactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    product_id UUID REFERENCES products(id),
    transaction_type VARCHAR(10),  -- 'sale', 'purchase', 'adjustment'
    quantity INTEGER,
    unit_price DECIMAL(10,2),
    total_amount DECIMAL(10,2),
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- AI Conversations table
CREATE TABLE ai_conversations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    session_id VARCHAR(100),
    message TEXT,
    role VARCHAR(10),  -- 'user', 'assistant'
    language VARCHAR(10),
    input_type VARCHAR(10) DEFAULT 'text',  -- 'text', 'voice'
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Inventory API
```python
# app/inventory/router.py
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session
from typing import List
from app.database import get_db
from app.auth.utils import get_current_user
from app.inventory.models import Product
from app.inventory.schemas import ProductCreate, ProductUpdate, ProductResponse

router = APIRouter()

@router.get("/products", response_model=List[ProductResponse])
async def get_products(
    db: Session = Depends(get_db),
    current_user = Depends(get_current_user)
):
    """Get all products for current user's store"""
    products = db.query(Product).filter(
        Product.user_id == current_user.id
    ).all()
    return products

@router.post("/products", response_model=ProductResponse)
async def add_product(
    product_data: ProductCreate,
    db: Session = Depends(get_db),
    current_user = Depends(get_current_user)
):
    """Add new product to inventory"""
    product = Product(**product_data.dict(), user_id=current_user.id)
    db.add(product)
    db.commit()
    db.refresh(product)
    return product

@router.get("/low-stock")
async def get_low_stock_alerts(
    db: Session = Depends(get_db),
    current_user = Depends(get_current_user)
):
    """Get products below minimum stock threshold"""
    low_stock = db.query(Product).filter(
        Product.user_id == current_user.id,
        Product.current_stock <= Product.min_stock
    ).all()
    return {
        "count": len(low_stock),
        "products": low_stock,
        "message_bn": f"আপনার {len(low_stock)}টি পণ্যের stock কম!"
    }
```

---

## ⚡ AI Chat API
```python
# app/ai/router.py
from fastapi import APIRouter, Depends, UploadFile, File
from app.ai.rag import SmartStockRAG
from app.ai.tts import TextToSpeech
from app.auth.utils import get_current_user
import whisper

router = APIRouter()
rag = SmartStockRAG()
tts = TextToSpeech()

@router.post("/chat")
async def chat(
    message: str,
    language: str = "bn",
    current_user = Depends(get_current_user)
):
    """Text chat with SmartStock AI"""
    response = await rag.generate_response(
        query=message,
        user_id=str(current_user.id),
        language=language
    )
    return {
        "response": response,
        "language": language
    }

@router.post("/voice-chat")
async def voice_chat(
    audio: UploadFile = File(...),
    current_user = Depends(get_current_user)
):
    """Voice chat - Whisper STT → AI → ElevenLabs TTS"""
    # 1. Transcribe audio with Whisper
    model = whisper.load_model("base")
    result = model.transcribe(audio.file, language="bn")
    text = result["text"]

    # 2. Get AI response
    ai_response = await rag.generate_response(
        query=text,
        user_id=str(current_user.id),
        language="bn"
    )

    # 3. Convert response to audio
    audio_bytes = await tts.synthesize(ai_response, language="bn")

    return {
        "transcribed_text": text,
        "ai_response": ai_response,
        "audio": audio_bytes.hex()
    }
```

---

## 🌐 Next.js Frontend

### Project Structure
```
smartstock-frontend/
├── app/
│   ├── (auth)/
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   ├── dashboard/
│   │   ├── page.tsx          # Main dashboard
│   │   ├── inventory/page.tsx
│   │   ├── sales/page.tsx
│   │   └── ai-chat/page.tsx
│   └── layout.tsx
├── components/
│   ├── inventory/
│   │   ├── ProductCard.tsx
│   │   ├── AddProductModal.tsx
│   │   └── LowStockAlert.tsx
│   ├── charts/
│   │   ├── SalesChart.tsx
│   │   └── StockChart.tsx
│   └── ai/
│       ├── ChatInterface.tsx
│       └── VoiceButton.tsx
├── lib/
│   ├── api.ts                # API client
│   └── store.ts              # Zustand state
└── public/
```

### Dashboard Page
```typescript
// app/dashboard/page.tsx
"use client";

import { useState, useEffect } from "react";
import { SalesChart } from "@/components/charts/SalesChart";
import { LowStockAlert } from "@/components/inventory/LowStockAlert";
import { api } from "@/lib/api";

interface DashboardStats {
  totalProducts: number;
  lowStockCount: number;
  todaySales: number;
  monthlyRevenue: number;
}

export default function Dashboard() {
  const [stats, setStats] = useState<DashboardStats | null>(null);

  useEffect(() => {
    api.get("/inventory/stats").then(res => setStats(res.data));
  }, []);

  return (
    <div className="p-6 bg-gray-50 min-h-screen">
      <h1 className="text-2xl font-bold text-gray-800 mb-6">
        🏪 আপনার দোকান - SmartStock AI
      </h1>

      {/* Stats Grid */}
      <div className="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
        <StatCard
          title="মোট পণ্য"
          value={stats?.totalProducts || 0}
          icon="📦"
          color="blue"
        />
        <StatCard
          title="কম স্টক"
          value={stats?.lowStockCount || 0}
          icon="⚠️"
          color="red"
        />
        <StatCard
          title="আজকের বিক্রয়"
          value={`₹${stats?.todaySales || 0}`}
          icon="💰"
          color="green"
        />
        <StatCard
          title="মাসিক আয়"
          value={`₹${stats?.monthlyRevenue || 0}`}
          icon="📈"
          color="purple"
        />
      </div>

      {/* Charts and Alerts */}
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <SalesChart />
        <LowStockAlert />
      </div>
    </div>
  );
}
```

---

## 📱 React Native App (Expo)

### Setup
```bash
npx create-expo-app smartstock-mobile
cd smartstock-mobile
npx expo install expo-camera expo-av expo-notifications
npm install @react-navigation/native @react-navigation/stack
npm install zustand axios
```

### Main App Structure
```typescript
// App.tsx
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import LoginScreen from './screens/LoginScreen';
import DashboardScreen from './screens/DashboardScreen';
import InventoryScreen from './screens/InventoryScreen';
import AIChatScreen from './screens/AIChatScreen';

const Stack = createStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Login">
        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Dashboard" component={DashboardScreen} />
        <Stack.Screen name="Inventory" component={InventoryScreen} />
        <Stack.Screen name="AIChat" component={AIChatScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

### AI Chat Screen (Voice Support)
```typescript
// screens/AIChatScreen.tsx
import React, { useState } from 'react';
import { View, Text, TouchableOpacity, TextInput, ScrollView } from 'react-native';
import { Audio } from 'expo-av';
import { api } from '../lib/api';

export default function AIChatScreen() {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [isRecording, setIsRecording] = useState(false);
  const [recording, setRecording] = useState(null);

  const startRecording = async () => {
    const { status } = await Audio.requestPermissionsAsync();
    if (status !== 'granted') return;

    const { recording } = await Audio.Recording.createAsync(
      Audio.RecordingOptionsPresets.HIGH_QUALITY
    );
    setRecording(recording);
    setIsRecording(true);
  };

  const stopRecordingAndSend = async () => {
    await recording.stopAndUnloadAsync();
    const uri = recording.getURI();
    setIsRecording(false);

    // Send audio to API
    const formData = new FormData();
    formData.append('audio', { uri, type: 'audio/m4a', name: 'voice.m4a' });

    const response = await api.post('/ai/voice-chat', formData);
    addMessage('user', response.data.transcribed_text);
    addMessage('assistant', response.data.ai_response);
  };

  const sendTextMessage = async () => {
    if (!input.trim()) return;
    addMessage('user', input);
    const response = await api.post('/ai/chat', { message: input, language: 'bn' });
    addMessage('assistant', response.data.response);
    setInput('');
  };

  const addMessage = (role, text) => {
    setMessages(prev => [...prev, { role, text, time: new Date() }]);
  };

  return (
    <View style={{ flex: 1, backgroundColor: '#f5f5f5' }}>
      <ScrollView style={{ flex: 1, padding: 16 }}>
        {messages.map((msg, i) => (
          <View key={i} style={{
            alignSelf: msg.role === 'user' ? 'flex-end' : 'flex-start',
            backgroundColor: msg.role === 'user' ? '#007AFF' : '#fff',
            padding: 12, borderRadius: 16, marginBottom: 8, maxWidth: '80%'
          }}>
            <Text style={{ color: msg.role === 'user' ? '#fff' : '#333' }}>
              {msg.text}
            </Text>
          </View>
        ))}
      </ScrollView>

      {/* Input Row */}
      <View style={{ flexDirection: 'row', padding: 16, alignItems: 'center' }}>
        <TextInput
          style={{ flex: 1, borderWidth: 1, borderColor: '#ddd', borderRadius: 24,
            padding: 12, marginRight: 8, backgroundColor: '#fff' }}
          placeholder="বাংলায় লিখুন..."
          value={input}
          onChangeText={setInput}
        />
        <TouchableOpacity onPress={sendTextMessage} style={{
          backgroundColor: '#007AFF', padding: 12, borderRadius: 24
        }}>
          <Text style={{ color: '#fff' }}>📤</Text>
        </TouchableOpacity>
        <TouchableOpacity
          onPress={isRecording ? stopRecordingAndSend : startRecording}
          style={{ backgroundColor: isRecording ? '#FF3B30' : '#34C759',
            padding: 12, borderRadius: 24, marginLeft: 8 }}
        >
          <Text style={{ color: '#fff' }}>{isRecording ? '⏹️' : '🎤'}</Text>
        </TouchableOpacity>
      </View>
    </View>
  );
}
```

---

## 📱 WhatsApp Bot Integration

### Twilio Setup
```python
# app/whatsapp/bot.py
from twilio.twiml.messaging_response import MessagingResponse
from fastapi import APIRouter, Request, Form
from app.ai.rag import SmartStockRAG
import re

router = APIRouter()
rag = SmartStockRAG()

@router.post("/webhook")
async def whatsapp_webhook(
    Body: str = Form(...),
    From: str = Form(...),
    MediaUrl0: str = Form(None)
):
    """Handle incoming WhatsApp messages"""
    phone = From.replace("whatsapp:", "")
    message = Body.strip()

    # Language detection
    language = detect_language(message)

    # Generate AI response
    ai_response = await rag.generate_response(
        query=message,
        user_phone=phone,
        language=language
    )

    # Send WhatsApp reply via Twilio
    resp = MessagingResponse()
    resp.message(ai_response)
    return str(resp)

def detect_language(text: str) -> str:
    """Detect if text is Bengali, Hindi, or English"""
    bengali_pattern = re.compile(r'[\u0980-\u09FF]')
    hindi_pattern = re.compile(r'[\u0900-\u097F]')

    if bengali_pattern.search(text):
        return "bn"
    elif hindi_pattern.search(text):
        return "hi"
    return "en"
```

### Example WhatsApp Interactions
```
User: "আমার চালের stock কত আছে?"
Bot: "🌾 আপনার চাল (Basmati): ২৫ কেজি বাকি আছে।
     ⚠️ Minimum stock: ৩০ কেজি
     📦 Reorder করুন! Supplier: রাজু এন্টারপ্রাইজ
     📞 তাকে call করতে চান? Reply করুন: 'হ্যাঁ'"

User: "আজকে কত বিক্রয় হয়েছে?"
Bot: "📊 আজকের বিক্রয় (৩০ অক্টোবর ২০২৭):
     💰 মোট: ₹৩,৪৫০
     📦 ২৮টি পণ্য বিক্রয়
     🏆 Top item: আটা (৫ কেজি)
     📈 গতকালের তুলনায় +১৫%"
```

---

## 💳 Razorpay Payment Integration

```python
# app/payments/razorpay.py
import razorpay
from fastapi import APIRouter, HTTPException
from pydantic import BaseModel
import os

router = APIRouter()
client = razorpay.Client(
    auth=(os.getenv("RAZORPAY_KEY_ID"), os.getenv("RAZORPAY_KEY_SECRET"))
)

PLANS = {
    "basic": {"amount": 29900, "name": "SmartStock Basic"},   # ₹299/month
    "pro": {"amount": 79900, "name": "SmartStock Pro"},       # ₹799/month
}

class CreateOrderRequest(BaseModel):
    plan: str
    user_id: str

@router.post("/create-order")
async def create_order(request: CreateOrderRequest):
    """Create Razorpay order for subscription"""
    if request.plan not in PLANS:
        raise HTTPException(status_code=400, detail="Invalid plan")

    plan = PLANS[request.plan]
    order = client.order.create({
        "amount": plan["amount"],
        "currency": "INR",
        "payment_capture": 1,
        "notes": {
            "plan": request.plan,
            "user_id": request.user_id
        }
    })
    return {
        "order_id": order["id"],
        "amount": plan["amount"],
        "currency": "INR",
        "plan_name": plan["name"]
    }

@router.post("/verify-payment")
async def verify_payment(
    razorpay_order_id: str,
    razorpay_payment_id: str,
    razorpay_signature: str
):
    """Verify Razorpay payment signature"""
    try:
        client.utility.verify_payment_signature({
            "razorpay_order_id": razorpay_order_id,
            "razorpay_payment_id": razorpay_payment_id,
            "razorpay_signature": razorpay_signature
        })
        # Upgrade user subscription
        return {"status": "success", "message": "Payment verified!"}
    except Exception:
        raise HTTPException(status_code=400, detail="Payment verification failed")
```

---

## 🐳 Docker Setup

```yaml
# docker-compose.yml
version: '3.8'

services:
  backend:
    build: ./smartstock-backend
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/smartstock
      - REDIS_URL=redis://redis:6379
      - RAZORPAY_KEY_ID=${RAZORPAY_KEY_ID}
      - RAZORPAY_KEY_SECRET=${RAZORPAY_KEY_SECRET}
    depends_on:
      - db
      - redis

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: smartstock
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine

  frontend:
    build: ./smartstock-frontend
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_API_URL=http://backend:8000

volumes:
  postgres_data:
```

---

## ✅ Month 18 Checklist

```
Backend:
[ ] FastAPI app with all endpoints
[ ] PostgreSQL with proper schema
[ ] JWT authentication working
[ ] Inventory CRUD APIs
[ ] AI chat endpoint
[ ] Voice chat endpoint
[ ] Razorpay payment integration
[ ] Docker setup

Frontend (Next.js):
[ ] Login / Registration page
[ ] Dashboard with stats
[ ] Inventory management UI
[ ] Sales analytics charts
[ ] AI chat interface
[ ] Payment/subscription page

Mobile (React Native):
[ ] Login screen
[ ] Dashboard screen
[ ] Inventory screen
[ ] AI voice chat screen
[ ] Push notifications

WhatsApp Bot:
[ ] Twilio webhook setup
[ ] Language detection
[ ] Inventory queries working
[ ] Sales reports via WhatsApp
[ ] Voice message support
```

---

> 💪🇮🇳🚀 **Month 18 শেষ করুন - আপনার product প্রায় ready! পরের মাসে beta testing!**
