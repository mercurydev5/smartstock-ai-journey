# 🧠 Month 6: Neural Networks (অক্টোবর ২০২৬)

> **"Neural Networks মানে math + code + magic!"** 💪🇮🇳🚀

[← Phase 3](README.md) | [← Main README](../README.md)

---

## 📋 Month Overview

| বিষয় | বিবরণ |
|------|-------|
| 📅 সময়কাল | অক্টোবর ১-৩১, ২০২৬ |
| ⏰ ঘণ্টা | ~৬০ ঘণ্টা |
| 🎯 লক্ষ্য | NN fundamentals + PyTorch |
| 💻 মূল Project | MNIST Digit Classifier (99%+) |
| 📚 মূল Resource | Karpathy NN Zero to Hero + fast.ai |

---

## 🗓️ Week-by-Week Plan

### Week 21 — NN from Scratch (NumPy only!)

```python
# neural_network_scratch.py
import numpy as np

class NeuralNetwork:
    def __init__(self, layer_sizes):
        """Initialize weights & biases"""
        self.weights = []
        self.biases = []
        for i in range(len(layer_sizes) - 1):
            W = np.random.randn(layer_sizes[i], layer_sizes[i+1]) * 0.01
            b = np.zeros((1, layer_sizes[i+1]))
            self.weights.append(W)
            self.biases.append(b)

    def sigmoid(self, z):
        return 1 / (1 + np.exp(-np.clip(z, -500, 500)))

    def sigmoid_derivative(self, a):
        return a * (1 - a)

    def relu(self, z):
        return np.maximum(0, z)

    def forward(self, X):
        self.activations = [X]
        current = X
        for W, b in zip(self.weights, self.biases):
            z = current @ W + b
            current = self.relu(z) if len(self.activations) < len(self.weights) else self.sigmoid(z)
            self.activations.append(current)
        return current

    def backward(self, X, y, learning_rate=0.01):
        m = X.shape[0]
        delta = self.activations[-1] - y
        for i in range(len(self.weights) - 1, -1, -1):
            dW = self.activations[i].T @ delta / m
            db = delta.mean(axis=0, keepdims=True)
            self.weights[i] -= learning_rate * dW
            self.biases[i] -= learning_rate * db
            if i > 0:
                delta = (delta @ self.weights[i].T) * self.sigmoid_derivative(self.activations[i])

    def train(self, X, y, epochs=1000, lr=0.01):
        losses = []
        for epoch in range(epochs):
            pred = self.forward(X)
            loss = -np.mean(y * np.log(pred + 1e-8) + (1-y) * np.log(1 - pred + 1e-8))
            self.backward(X, y, lr)
            if epoch % 100 == 0:
                print(f"Epoch {epoch}: Loss = {loss:.4f}")
            losses.append(loss)
        return losses

# XOR problem
X = np.array([[0,0],[0,1],[1,0],[1,1]])
y = np.array([[0],[1],[1],[0]])

nn = NeuralNetwork([2, 4, 1])
losses = nn.train(X, y, epochs=1000, lr=0.1)
predictions = nn.forward(X)
print("\nXOR Predictions:")
for xi, yi, pred in zip(X, y, predictions):
    print(f"  {xi} → {pred[0]:.3f} (true: {yi[0]})")
```

### Week 22 — PyTorch Basics

```python
# pytorch_basics.py
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader, TensorDataset

# Neural Network in PyTorch
class SimpleNN(nn.Module):
    def __init__(self, input_size, hidden_size, output_size):
        super().__init__()
        self.network = nn.Sequential(
            nn.Linear(input_size, hidden_size),
            nn.ReLU(),
            nn.Dropout(0.3),
            nn.Linear(hidden_size, hidden_size // 2),
            nn.ReLU(),
            nn.Linear(hidden_size // 2, output_size)
        )

    def forward(self, x):
        return self.network(x)

# Training loop
def train_model(model, train_loader, val_loader, epochs=10):
    optimizer = optim.Adam(model.parameters(), lr=0.001)
    criterion = nn.CrossEntropyLoss()
    device = "cuda" if torch.cuda.is_available() else "cpu"
    model.to(device)

    for epoch in range(epochs):
        model.train()
        train_loss = 0
        for X_batch, y_batch in train_loader:
            X_batch, y_batch = X_batch.to(device), y_batch.to(device)
            optimizer.zero_grad()
            output = model(X_batch)
            loss = criterion(output, y_batch)
            loss.backward()
            optimizer.step()
            train_loss += loss.item()

        print(f"Epoch {epoch+1}/{epochs} | Loss: {train_loss/len(train_loader):.4f}")

print("✅ PyTorch Neural Network ready!")
```

### Week 23 — MNIST Classifier

```python
# mnist_classifier.py
import torch
import torch.nn as nn
from torchvision import datasets, transforms
from torch.utils.data import DataLoader

# Load MNIST
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.1307,), (0.3081,))
])

train_data = datasets.MNIST("./data", train=True, download=True, transform=transform)
test_data = datasets.MNIST("./data", train=False, transform=transform)

train_loader = DataLoader(train_data, batch_size=64, shuffle=True)
test_loader = DataLoader(test_data, batch_size=1000)

# CNN for MNIST
class MNISTNet(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv = nn.Sequential(
            nn.Conv2d(1, 32, 3, padding=1),
            nn.ReLU(),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 3, padding=1),
            nn.ReLU(),
            nn.MaxPool2d(2),
        )
        self.fc = nn.Sequential(
            nn.Linear(64 * 7 * 7, 128),
            nn.ReLU(),
            nn.Dropout(0.5),
            nn.Linear(128, 10)
        )

    def forward(self, x):
        return self.fc(self.conv(x).view(x.size(0), -1))

model = MNISTNet()
print(f"Parameters: {sum(p.numel() for p in model.parameters()):,}")
print("Train this model → Target: 99%+ accuracy!")
```

### Week 24 — Monthly Project Complete

---

## ✅ Month 6 Checklist

- [ ] NN from scratch (NumPy) implement করা
- [ ] Backpropagation manually বোঝা
- [ ] PyTorch basics শেখা
- [ ] MNIST 99%+ accuracy অর্জন করা
- [ ] Karpathy NN:Zero to Hero video 1-3 দেখা
- [ ] GitHub-এ push করা

---

[← Phase 3 README](README.md) | [Month 7 →](month-07-cnn-vision.md)
