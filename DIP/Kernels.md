# Edge Detection Kernels: Laplacian, Prewitt, and Sobel

## 1. Kernel (Quick Recap)

A **kernel** (or filter/mask) is a small matrix used in **convolution** to extract or enhance specific image features such as edges, smoothing, or sharpening.

---

## 2. Laplacian Operator

### What is Laplacian

The **Laplacian** is a **second-order derivative** operator. It detects regions of **rapid intensity change**, which correspond to edges and fine details.

Mathematically (2D):

∇²f(x, y) = ∂²f/∂x² + ∂²f/∂y²

Flat regions → output ≈ 0  
Edges → large positive or negative values

---

### 2.1 4-neighbor Laplacian

Kernel:

```
 0  -1   0
-1   4  -1
 0  -1   0
```

• Uses only **up, down, left, right** neighbors  
• Strong for **horizontal & vertical edges**  
• Less sensitive to noise  
• Produces moderate sharpening

---

### 2.2 8-neighbor Laplacian

Kernel:

```
-1  -1  -1
-1   8  -1
-1  -1  -1
```

• Uses **all 8 neighbors (including diagonals)**  
• Detects edges in **all directions**  
• Stronger edge response  
• More noise sensitive

---

### 2.3 Key intuition (4 vs 8 neighbor)

The Laplacian asks:

> “How different is the center pixel from its neighbors?”

• 4-neighbor → compares with 4 pixels → milder response  
• 8-neighbor → compares with 8 pixels → stronger response

That’s why the center weight is **4 or 8**.

---

### 2.4 Laplacian for sharpening

If kernel center is positive:

g(x, y) = f(x, y) − ∇²f(x, y)

If kernel center is negative:

g(x, y) = f(x, y) + ∇²f(x, y)

This boosts high-frequency components (edges).

---

### 2.5 Noise issue

Because Laplacian is a **second derivative**, it is **very sensitive to noise**.

Common solution:  
**Gaussian smoothing + Laplacian = LoG (Laplacian of Gaussian)**

---

## 3. Prewitt Operator

### What is Prewitt

**Prewitt** is a **first-order derivative** operator. It estimates the **gradient**, giving both **edge strength and direction**.

---

### 3.1 Prewitt kernels

Horizontal gradient (Gx) – detects vertical edges:

```
-1  0  1
-1  0  1
-1  0  1
```

Vertical gradient (Gy) – detects horizontal edges:

```
 1  1  1
 0  0  0
-1 -1 -1
```

Edge magnitude:

|Gx| + |Gy| (or √(Gx² + Gy²))

---

### 3.2 Intuition

Prewitt compares **average intensity on one side of a pixel with the other side**.

• Large difference → edge  
• Small difference → flat region

It also performs **slight smoothing** due to averaging.

---

### 3.3 Characteristics

• First derivative  
• Directional (horizontal & vertical)  
• Less sensitive to noise than Laplacian  
• Produces smoother, thicker edges

---

## 4. Sobel Operator

### What is Sobel

**Sobel** is also a **first-order derivative** operator, similar to Prewitt, but with **more emphasis on the center pixels**, giving better noise suppression.

---

### 4.1 Sobel kernels

Horizontal gradient (Gx):

```
-1  0  1
-2  0  2
-1  0  1
```

Vertical gradient (Gy):

```
 1  2  1
 0  0  0
-1 -2 -1
```

---

### 4.2 Why Sobel is better than Prewitt

• Center row/column has **weight 2**  
• Stronger edge response  
• Better noise suppression  
• More commonly used in practice

---

## 5. Laplacian vs Prewitt vs Sobel (Exam Table)

|Feature|Laplacian|Prewitt|Sobel|
|---|---|---|---|
|Derivative order|Second|First|First|
|Direction info|No|Yes|Yes|
|Kernels used|1|2 (Gx, Gy)|2 (Gx, Gy)|
|Noise sensitivity|High|Low–medium|Medium|
|Edge strength|Very strong|Moderate|Strong|
|Edge thickness|Thin, noisy|Thicker|Balanced|
|Common use|Sharpening, fine edges|Simple edge detection|Practical edge detection|

---

## 6. When to use which

### Use Laplacian when

• Image is smooth or pre-filtered  
• You want sharpening or fine detail enhancement

### Use Prewitt when

• You want simple, fast edge detection  
• Direction matters  
• Noise level is moderate

### Use Sobel when

• You want reliable edge detection  
• Noise is present  
• Practical computer vision tasks

---

## 7. One-line memory trick (for exams)

• **Prewitt & Sobel** → slope (first derivative)  
• **Laplacian** → change of slope (second derivative)  
• **Sobel = weighted Prewitt**

---

End of note