# Canny Edge Detection

**Canny Edge Detection** is a multi-stage edge detection algorithm proposed by **John F. Canny (1986)**. It is designed to satisfy three optimality criteria:

- Good detection (low false positives and false negatives)
- Good localization (detected edges are close to true edges)
- Single response per edge

## Overview of the Pipeline

1. Gaussian smoothing (noise reduction)
2. Gradient computation (magnitude and direction)
3. Non-maximum suppression
4. Double thresholding
5. Edge tracking by hysteresis

Each stage refines the result of the previous one.

## Step 1: Gaussian Smoothing (Noise Reduction)

Edge detection involves derivatives, and derivatives amplify noise. Therefore, the image is first smoothed using a Gaussian filter.

### Continuous 2D Gaussian Function

Plain mathematical form:

$G(x, y) = exp( - (x² + y²) / (2σ²) )$

Let the input image be f(x, y). The smoothed image is obtained by convolution:

$f_s(x, y) = G(x, y) * f(x, y)$

where * denotes convolution.

## Predefined Discrete Gaussian Kernels

In practice, discrete kernels are used as approximations of the continuous Gaussian.

### 3×3 Gaussian Kernel (σ ≈ 1)

```
1   2   1
2   4   2   × 1/16
1   2   1
```

### 5×5 Gaussian Kernel (σ ≈ 1)

```
1   4   7   4   1
4  16  26  16   4
7  26  41  26   7   × 1/273
4  16  26  16   4
1   4   7   4   1
```

These kernels are normalized so that the sum of all elements equals 1.

## Step 2: Gradient Computation

After smoothing, edges are detected by measuring intensity changes using image gradients.

### Partial Derivatives (Discrete Approximation)

$g_x(x, y) = ∂f_s(x, y) / ∂x$

$g_y(x, y) = ∂f_s(x, y) / ∂y$

In digital images, these derivatives are approximated using convolution kernels.

## Predefined Gradient Kernels (Sobel Operator)

### Horizontal Gradient Kernel (Gx)

```
-1   0   1
-2   0   2
-1   0   1
```

### Vertical Gradient Kernel (Gy)

```
-1  -2  -1
 0   0   0
 1   2   1
```

## Gradient Magnitude (Edge Strength)

The gradient magnitude is computed as:

$M(x, y) = sqrt( g_x²(x, y) + g_y²(x, y) )$

Large values of M(x, y) correspond to strong edges.

## Gradient Direction (Edge Orientation)

The gradient direction is computed as:

$α(x, y) = arctan( g_y(x, y) / g_x(x, y) )$

This angle represents the direction of maximum intensity change.

## Step 3: Non-Maximum Suppression (Edge Thinning)

After gradient computation, edges appear thick. Non-maximum suppression thins edges to one-pixel width.

### Procedure

For each pixel:

1. Determine the gradient direction
2. Compare the pixel magnitude with its two neighbors along that direction
3. Keep the pixel only if it is a local maximum
4. Otherwise, suppress it (set to zero)

### Gradient Direction Quantization

To simplify comparison, gradient directions are quantized into four cases:

- 0° (horizontal)
- 45° (diagonal)
- 90° (vertical)
- 135° (diagonal)

## Step 4: Double Thresholding

After non-maximum suppression, some edges are strong, some are weak, and some are noise.

Two thresholds are used:

- High threshold: T_H
- Low threshold: T_L (T_L < T_H)

### Classification

Strong edge:

$M(x, y) ≥ T_H$

Weak edge:

$T_L ≤ M(x, y) < T_H$

Non-edge:

$M(x, y) < T_L$

## Step 5: Edge Tracking by Hysteresis

Weak edges are preserved only if they are connected to strong edges.

### Rule

A weak edge pixel is kept if and only if it is connected (8-connectivity) to a strong edge pixel. Otherwise, it is discarded.

### Purpose

- Removes isolated noise responses
- Preserves continuous real edges

## Final Output

The output of the Canny edge detector is a binary edge image:

- White pixels represent edges
- Black pixels represent non-edge regions

The result contains thin, well-localized, and noise-resistant edges.

## Exam-Oriented Summary

- Gaussian smoothing reduces noise before differentiation
- Gradient magnitude measures edge strength
- Gradient direction guides non-maximum suppression
- Double thresholding separates strong and weak edges
- Hysteresis ensures edge continuity

This step-by-step design is what makes Canny superior to single-kernel edge detectors.