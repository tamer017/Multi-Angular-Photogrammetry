# **Semi-Global Matching (SGM) Algorithm for Disparity and Depth Map Calculation
## Table of Contents  
1. [**Introduction**](#introduction)  
2. [**Disparity Definition**](#disparity-definition)  
3. [**SGM Algorithm Overview**](#sgm-algorithm-overview)  
   - 3.1 [Cost Computation](#cost-computation)  
   - 3.2 [Disparity Estimation](#disparity-estimation)  
   - 3.3 [Optimization](#optimization)  
   - 3.4 [Disparity Refinement](#disparity-refinement)  
4. [**SGM Cost Function**](#sgm-cost-function)  
   - 4.1 [Matching Cost](#matching-cost)  
   - 4.2 [Cost Volume](#cost-volume)  
5. [**SGM Algorithm Pseudocode**](#sgm-algorithm-pseudocode)  
   - 5.1 [Step-by-Step Explanation](#step-by-step-explanation)  
6. [**Optimization with Dynamic Programming**](#optimization-with-dynamic-programming)  
   - 6.1 [Dynamic Programming along Multiple Directions](#dynamic-programming-along-multiple-directions)  
   - 6.2 [Minimizing the Cost Function](#minimizing-the-cost-function)  
7. [**Disparity to Depth Conversion**](#disparity-to-depth-conversion)  
   - 7.1 [Depth Calculation Formula](#depth-calculation-formula)  
8. [**Disparity Map Refinement**](#disparity-map-refinement)  
   - 8.1 [Post-Processing Techniques](#post-processing-techniques)  
9. [**Key Concepts in SGM**](#key-concepts-in-sgm)  
   - 9.1 [Global Optimization](#global-optimization)  
   - 9.2 [Penalizing Disparity Changes](#penalizing-disparity-changes)  
10. [**Visualizing Disparity and Depth**](#visualizing-disparity-and-depth)  
11. [**Conclusion**](#conclusion)  
12. [**References**](#references)
---

## Introduction
Stereo vision and depth map generation are essential for understanding 3D environments. Disparity refers to the difference in pixel positions between corresponding points in the left and right images of a stereo pair. Semi-Global Matching (SGM) is an advanced stereo matching algorithm that provides accurate disparity maps by minimizing a cost function across multiple scanlines, which considers smoothness constraints and global consistency.

## 2Disparity Definition
Disparity $$d(x, y)$$ at pixel position $$(x, y)$$ in the left image is defined as:

  $$d(x, y) = x_L(x, y) - x_R(x, y)$$

Where:
- $$x_L(x, y)$$ is the pixel coordinate of the corresponding point in the left image.
- $$x_R(x, y)$$ is the pixel coordinate in the right image for the same point.

The disparity increases as objects approach the cameras. The larger the disparity, the closer the object is to the camera, and vice versa.

## 3SGM Algorithm Overview
The SGM algorithm involves several key stages to compute the disparity map, each designed to minimize errors and improve accuracy. These stages include cost computation, disparity estimation, optimization, and refinement.

### Cost Computation
For each pixel $$(x, y)$$ in the left image, the matching cost $$C(x, y, d)$$ for each disparity $$d$$ is computed by comparing the pixel intensities between the left and right images. A commonly used cost function is the **Sum of Absolute Differences (SAD)**:

  $$C(x, y, d) = |I_L(x, y) - I_R(x + d, y)|$$

Where:
- $$I_L(x, y)$$ is the intensity at pixel $$(x, y)$$ in the left image.
- $$I_R(x + d, y)$$ is the intensity at the corresponding pixel in the right image at disparity $$d$$.

###Disparity Estimation
For each pixel, the algorithm searches for the disparity $$d$$ in the right image that minimizes the cost function. The disparity is calculated by selecting the pixel with the smallest matching cost:

  $$d(x, y) = \argmin_{d} \{C(x, y, d)\}$$

### *Optimization
SGM optimizes the cost function along multiple scanlines or paths in the image. Instead of optimizing locally for each pixel, dynamic programming is used to propagate the cost along rows and other directions. This improves global consistency and reduces errors in low-texture areas.

### 3.4 **Disparity Refinement
To refine the disparity map, post-processing techniques such as median filtering are applied. This helps to remove small disparities and reduce noise, improving the overall quality of the depth map.

---

## 4SGM Cost Function
The cost function evaluates the disparity by comparing the pixel intensities in the left and right images. The goal is to find the disparity that minimizes the cost.

### 4.1 **Matching Cost
For each pixel, the matching cost function is:

  $$C(x, y, d) = |I_L(x, y) - I_R(x + d, y)|$$

This cost function measures how well a pixel in the left image matches the corresponding pixel in the right image at disparity $$d$$.

### 4.2 **Cost Volume
A **cost volume** is a 3D array where each element represents the matching cost for a pixel at position $$(x, y)$$ and disparity $$d$$. This volume stores the cost for all possible disparities and helps in selecting the optimal disparity for each pixel.

---

## 5SGM Algorithm Pseudocode
Below is the pseudocode for implementing the SGM algorithm:

```plaintext
# Step 1: Initialize Disparity Range and Cost Volume
Initialize disparity range [d_min, d_max] (e.g., 0 to 64)
Initialize a cost volume C(x, y, d) to store matching costs

# Step 2: Compute Matching Costs for Each Pixel
For each pixel (x, y) in the left image:
    For each disparity d in [d_min, d_max]:
        Compute the matching cost C(x, y, d) using Sum of Absolute Differences (SAD):
        C(x, y, d) = |I_L(x, y) - I_R(x + d, y)|

# Step 3: Minimize Cost Function with Dynamic Programming
For each row in the image:
    Initialize a 1D cost array D(d) for each disparity d
    
    For each disparity d in [d_min, d_max]:
        D(d) = C(x, y, d) + min(D(d-1), D(d+1), D(d))

# Step 4: Perform Multi-Path Optimization
For each scanline, perform dynamic programming along multiple directions (4 or 8 directions)
Optimize the cost volume by considering disparity smoothness constraints

# Step 5: Select Disparity with Minimum Cost
For each pixel (x, y):
    d(x, y) = argmin_d {C(x, y, d)}

# Step 6: Refinement (Optional)
Apply post-processing like median filtering to remove small disparities

# Step 7: Output the Final Disparity Map
The disparity map d(x, y) represents the distance of each pixel from the camera
```

---

## 6Optimization with Dynamic Programming
The optimization process involves minimizing the cost function along multiple directions using dynamic programming.

### 6.1 **Dynamic Programming along Multiple Directions
SGM performs dynamic programming along several directions (e.g., left-to-right, right-to-left, top-to-bottom, bottom-to-top) to propagate the cost and ensure global consistency. This helps in reducing errors from local matching.

### 6.2 **Minimizing the Cost Function
For each row, the dynamic programming step propagates the cost to neighboring pixels. The final cost for each pixel $$(x, y)$$ is computed by minimizing the cost along neighboring disparities:

  $$D(d) = C(x, y, d) + \min(D(d-1), D(d+1), D(d))$$

---

## 7Disparity to Depth Conversion
Once the disparity map is generated, it can be used to compute the depth map.

### 7.1 **Depth Calculation Formula
The depth $$Z(x, y)$$ at pixel $$(x, y)$$ is calculated using the following formula:

  $$\text{Depth}(x, y) = \frac{B \cdot f}{d(x, y)}$$

Where:
- $$B$$ is the baseline (the distance between the two cameras).
- $$f$$ is the focal length of the camera.
- $$d(x, y)$$ is the disparity at pixel $$(x, y)$$.

The depth is inversely proportional to disparity: the closer an object is to the camera, the larger its disparity and the smaller its depth.

---

## 8Disparity Map Refinement
Post-processing techniques are used to improve the quality of the disparity map.

### 8.1 **Post-Processing Techniques
Median filtering is commonly used to reduce noise and remove small disparities in the disparity map, improving its smoothness.

---

## 9Key Concepts in SGM
### 9.1 **Global Optimization
SGM uses dynamic programming across multiple directions to optimize the disparity map globally. This allows the algorithm to consider both local matching costs and smoothness constraints, ensuring consistent disparity estimation.

### 9.2 **Penalizing Disparity Changes
SGM incorporates a penalty for large disparity changes between neighboring pixels, which helps in reducing errors in low-texture areas and maintaining smooth transitions in the disparity map.

---

## 10Visualizing Disparity and Depth
- **Stereo Image Pair**: Two images taken from slightly different viewpoints.
- **Disparity Map**: A 2D representation of the disparity for each pixel.
- **Depth Map**: A 2D representation of the depth (distance from the camera) for each pixel, calculated from the disparity map.

## 11Conclusion
SGM is a powerful and accurate method for stereo

 vision and depth map generation. By using dynamic programming for optimization and penalizing large disparity changes, it produces accurate disparity maps with smooth transitions. Refinement techniques further improve the quality of the depth map, making SGM a widely used algorithm in 3D scene reconstruction.

## 12References
1. Hirschmüller, H. (2008). "Stereo Processing by Semi-Global Matching and Mutual Information." *IEEE Transactions on Pattern Analysis and Machine Intelligence*, 30(2), 328-341.
2. M. Pollefeys, D. Nister, and S. Schaffalitzky. (2004). "Visual modeling with a handheld camera." *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR)*, 2004.
3. Szeliski, R. (2010). *Computer Vision: Algorithms and Applications*. Springer.
4. Zhang, L., & Kosecka, J. (2008). "Stereo matching using belief propagation." *IEEE Transactions on Pattern Analysis and Machine Intelligence*, 30(8), 1540-1549.
