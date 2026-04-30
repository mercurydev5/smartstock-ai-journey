# 🔢 Week 7: Linear Algebra (জুন ১৫-২১, ২০২৬)

> **"Linear Algebra না বুঝলে Deep Learning বোঝা অসম্ভব!"** 💪🇮🇳🚀

[← Week 6](week-06-visualization.md) | [← Month 2](README.md)

---

## 🎯 Week 7 Goals

এই সপ্তাহ শেষে আপনি বুঝতে পারবেন:
- [ ] Vectors ও scalar operations
- [ ] Matrix multiplication
- [ ] Dot product ও cross product
- [ ] Matrix determinant, inverse, transpose
- [ ] Eigenvalues ও eigenvectors (basic)
- [ ] AI/ML-এ এগুলো কোথায় ব্যবহার হয়

---

## 📚 Resources

| Resource | Link | কত সময় |
|----------|------|--------|
| ⭐⭐⭐ 3Blue1Brown Essence of Linear Algebra | https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab | 3 ঘণ্টা |
| ⭐ Khan Academy Linear Algebra | https://www.khanacademy.org/math/linear-algebra | 2 ঘণ্টা |
| Mathematics for ML Book Ch 2 | https://mml-book.github.io/ | 1 ঘণ্টা |
| StatQuest SVD | https://www.youtube.com/watch?v=FgakZw6K1QQ | 30 মিনিট |

---

## 📅 Day-by-Day Breakdown

### Day 43-44 (জুন ১৫-১৬) — Vectors

```python
# vectors.py
import numpy as np
import matplotlib.pyplot as plt

# Vectors in AI context
word_cat = np.array([0.9, 0.1, 0.8])    # cat = [fluffy, barks, claws]
word_dog = np.array([0.8, 0.9, 0.3])    # dog = [fluffy, barks, claws]
word_fish = np.array([0.0, 0.0, 0.0])   # fish = [fluffy, barks, claws]

# Similarity using dot product (cosine similarity)
def cosine_similarity(a, b):
    return np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))

print(f"cat-dog similarity: {cosine_similarity(word_cat, word_dog):.3f}")    # High!
print(f"cat-fish similarity: {cosine_similarity(word_cat, word_fish):.3f}")  # Low

# Vector operations
v1 = np.array([2, 3])
v2 = np.array([1, -1])
print(f"\nAddition: {v1 + v2}")       # [3, 2]
print(f"Subtraction: {v1 - v2}")     # [1, 4]
print(f"Dot product: {np.dot(v1, v2)}")  # 2*1 + 3*(-1) = -1
print(f"Magnitude of v1: {np.linalg.norm(v1):.3f}")  # sqrt(4+9)
```

---

### Day 45-46 (জুন ১৭-১৮) — Matrices

```python
# matrices.py
import numpy as np

# Matrix operations — Neural Network-এ weights
W1 = np.array([[0.1, 0.2, 0.3],
               [0.4, 0.5, 0.6]])  # 2x3 weight matrix

x = np.array([1, 2, 3])           # 3-dim input vector

# Forward pass (matrix-vector multiplication)
output = W1 @ x    # or np.dot(W1, x)
print(f"Neural network output: {output}")  # [1.4, 3.2]

# Matrix operations
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(f"\nA:\n{A}")
print(f"A^T (transpose):\n{A.T}")
print(f"A*B (element-wise):\n{A * B}")
print(f"A@B (matrix multiply):\n{A @ B}")
print(f"det(A): {np.linalg.det(A):.1f}")
print(f"inv(A):\n{np.linalg.inv(A)}")
print(f"A @ inv(A):\n{(A @ np.linalg.inv(A)).round()}")  # Identity matrix
```

---

### Day 47-49 (জুন ১৯-২১) — Eigenvalues + Practice

```python
# eigenvalues.py
import numpy as np

# PCA (Principal Component Analysis) uses eigenvalues
# Covariance matrix
data = np.random.multivariate_normal([0, 0], [[3, 1.5], [1.5, 1]], 100)

cov_matrix = np.cov(data.T)
print(f"Covariance Matrix:\n{cov_matrix}\n")

eigenvalues, eigenvectors = np.linalg.eig(cov_matrix)
print(f"Eigenvalues: {eigenvalues}")
print(f"Eigenvectors:\n{eigenvectors}")

# Explained variance ratio (used in PCA)
explained_variance = eigenvalues / eigenvalues.sum()
print(f"\nExplained variance: {explained_variance}")
print(f"First component explains {explained_variance[0]:.1%} of variance")
```

**Practice Problems:**
```python
# practice.py
import numpy as np

# Q1: Solve system of equations: 2x + 3y = 13, x - y = 1
A = np.array([[2, 3], [1, -1]])
b = np.array([13, 1])
x = np.linalg.solve(A, b)
print(f"Solution: x={x[0]}, y={x[1]}")

# Q2: Find rank of matrix
M = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(f"Rank: {np.linalg.matrix_rank(M)}")  # 2 (singular matrix)

# Q3: Image as matrix (grayscale)
import matplotlib.pyplot as plt
image = np.random.randint(0, 255, (8, 8))
plt.imshow(image, cmap="gray")
plt.title("8x8 'Image' as Matrix")
plt.colorbar()
plt.savefig("image_matrix.png")
print("✅ Image saved!")
```

---

## ✔️ Week 7 Completion Checklist

- [ ] 3Blue1Brown playlist শেষ করা (সবচেয়ে গুরুত্বপূর্ণ!)
- [ ] Vectors ও operations বোঝা
- [ ] Matrix multiplication করা
- [ ] Eigenvalues concept বোঝা
- [ ] Practice problems solve করা

---

[← Week 6](week-06-visualization.md) | [Week 8 →](week-08-statistics.md)
