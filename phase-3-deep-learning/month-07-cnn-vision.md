# 🔭 Month 7: CNNs & Vision (নভেম্বর ২০২৬)

> **"CNN দিয়ে computer দেখতে শেখে — আমরাও শেখাবো!"** 💪🇮🇳🚀

[← Month 6](month-06-neural-networks.md) | [← Phase 3](README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | নভেম্বর ১-৩০, ২০২৬ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | CNN architecture + Transfer Learning |
| 💻 মূল Project | 🇮🇳 Indian Currency Classifier |
| 📚 মূল Resource | CS231n + fast.ai Part 1 |

---

## 🗓️ Week-by-Week Plan

### Week 25 — CNN Architecture Deep Dive

```python
# cnn_architecture.py
import torch
import torch.nn as nn

class CustomCNN(nn.Module):
    """Understanding CNN layers"""
    def __init__(self, num_classes=10):
        super().__init__()
        # Feature Extraction
        self.features = nn.Sequential(
            # Block 1: 224x224 → 112x112
            nn.Conv2d(3, 32, kernel_size=3, padding=1),  # 3 channels (RGB) → 32 filters
            nn.BatchNorm2d(32),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2, 2),

            # Block 2: 112x112 → 56x56
            nn.Conv2d(32, 64, kernel_size=3, padding=1),
            nn.BatchNorm2d(64),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2, 2),

            # Block 3: 56x56 → 28x28
            nn.Conv2d(64, 128, kernel_size=3, padding=1),
            nn.BatchNorm2d(128),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2, 2),
        )

        # Classifier
        self.classifier = nn.Sequential(
            nn.AdaptiveAvgPool2d((4, 4)),  # Any size → 4x4
            nn.Flatten(),
            nn.Linear(128 * 4 * 4, 256),
            nn.ReLU(),
            nn.Dropout(0.5),
            nn.Linear(256, num_classes)
        )

    def forward(self, x):
        return self.classifier(self.features(x))

model = CustomCNN(num_classes=7)  # 7 Indian currency denominations
print(f"Parameters: {sum(p.numel() for p in model.parameters()):,}")
```

### Week 26 — Transfer Learning

```python
# transfer_learning.py
import torch
import torch.nn as nn
from torchvision import models, transforms
from torch.utils.data import DataLoader, ImageFolder

# Use EfficientNet-B0 (best accuracy/speed tradeoff)
def create_currency_classifier(num_classes=7):
    model = models.efficientnet_b0(pretrained=True)

    # Freeze all layers except last
    for param in model.parameters():
        param.requires_grad = False

    # Replace classifier for our task
    model.classifier = nn.Sequential(
        nn.Dropout(0.3),
        nn.Linear(model.classifier[1].in_features, 256),
        nn.ReLU(),
        nn.Dropout(0.3),
        nn.Linear(256, num_classes)
    )

    # Unfreeze last few layers for fine-tuning
    for param in model.features[-3:].parameters():
        param.requires_grad = True

    return model

model = create_currency_classifier(7)
trainable = sum(p.numel() for p in model.parameters() if p.requires_grad)
total = sum(p.numel() for p in model.parameters())
print(f"Trainable: {trainable:,} / Total: {total:,} ({trainable/total:.1%})")
```

### Week 27-28 — 🇮🇳 Indian Currency Classifier Project

**Dataset collection:**
- ₹10, ₹20, ₹50, ₹100, ₹200, ₹500, ₹2000 notes
- Use Google Images + roboflow for labeling
- Augmentation: rotation, brightness, noise

```python
# indian_currency_classifier.py
import torch
import torch.nn as nn
import torch.optim as optim
from torchvision import models, transforms
from torch.utils.data import DataLoader, ImageFolder
import matplotlib.pyplot as plt

CURRENCY_CLASSES = ["10", "20", "50", "100", "200", "500", "2000"]
IMG_SIZE = 224
BATCH_SIZE = 32
EPOCHS = 20
DEVICE = "cuda" if torch.cuda.is_available() else "cpu"

# Data augmentation
train_transforms = transforms.Compose([
    transforms.Resize((IMG_SIZE, IMG_SIZE)),
    transforms.RandomHorizontalFlip(),
    transforms.RandomRotation(15),
    transforms.ColorJitter(brightness=0.3, contrast=0.3),
    transforms.ToTensor(),
    transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
])

# Training function
def train_epoch(model, loader, optimizer, criterion):
    model.train()
    total_loss = 0
    correct = 0
    for images, labels in loader:
        images, labels = images.to(DEVICE), labels.to(DEVICE)
        optimizer.zero_grad()
        outputs = model(images)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()
        total_loss += loss.item()
        correct += (outputs.argmax(1) == labels).sum().item()
    return total_loss / len(loader), correct / len(loader.dataset)

print("✅ Indian Currency Classifier ready!")
print("Target: 98%+ accuracy on all denominations")
print("This project goes directly on resume/portfolio!")
```

---

## ✅ Month 7 Checklist

- [ ] CNN architecture বোঝা (conv, pooling, stride, padding)
- [ ] Transfer learning implement করা
- [ ] Indian Currency Dataset collect করা
- [ ] Indian Currency Classifier train করা (98%+ accuracy)
- [ ] Gradio demo তৈরি করা
- [ ] HuggingFace Spaces-এ deploy করা ✅
- [ ] GitHub-এ push করা

---

[← Month 6](month-06-neural-networks.md) | [Month 8 →](month-08-rnn-sequences.md)
