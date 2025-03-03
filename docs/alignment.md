
# Alignment

## Table of Contents
- [Overview](#overview)
- [Steps](#steps)
- [Configuration Parameters Comparison](#configuration-parameters-comparison)
- [Python Code for Automation](#python-code-for-automation)
- [Detailed Workflow Explanation](#detailed-workflow-explanation)
- [Key Best Practices](#key-best-practices)
- [References](#references)
## Overview
Alignment is the process of matching corresponding points across multiple images to generate a sparse point cloud. This critical step establishes the geometry and relative positions of cameras and prepares the data for further photogrammetric processing.

---

## Steps

### **1. Start Alignment**
- Navigate to `Workflow > Align Photos`.
- Select the desired alignment parameters from the **Align Photos** dialog box.

### **2. Configure Parameters**
- **Accuracy**: Controls image resolution used during alignment:
  - **Highest**: Full-resolution images (most accurate, slowest).
  - **High**: Images downscaled by a factor of 2 (default for balance).
  - **Medium**: Images downscaled by a factor of 4.
  - **Low**: Images downscaled by a factor of 8.
  - **Lowest**: Images downscaled by a factor of 16 (fastest, least accurate).
  
  Formula for downscaling:
  
  $$\text{Image Size} = \frac{\text{Original Resolution}}{\text{Downscale Factor}}$$

- **Key Point Limit**: Maximum feature points detected per image (default is 40,000).
- **Tie Point Limit**: Maximum tie points retained after filtering (default is 4,000).
- **Adaptive Camera Model Fitting**: Enables optimization of camera parameters for varying image quality or EXIF inconsistencies.

### **3. Run the Alignment**
- Click **OK** to initiate alignment.
- Review the resulting sparse point cloud in the **Model View**.
- Inspect alignment errors in the **Reference Pane** if GPS data is available.

---

## Configuration Parameters Comparison

| Parameter                  | High Accuracy                     | Low Accuracy                     |
|----------------------------|------------------------------------|-----------------------------------|
| **Image Resolution**       | Full resolution                   | Downscaled resolution            |
| **Processing Speed**       | Slow                              | Fast                             |
| **Memory Usage**           | High                              | Low                              |
| **Key Point Limit**        | Higher recommended (>40,000)      | Default or lower (e.g., 10,000)  |
| **Tie Point Limit**        | Higher recommended (>4,000)       | Default or lower                 |
| **Ideal Use Case**         | Small, high-detail datasets       | Large or low-detail datasets     |

---

## Python Code for Automation

Automate the alignment process using the Metashape Python API:

```python
import Metashape

# Initialize project
doc = Metashape.Document()
doc.open("path_to_project/project.psz")

# Access the active chunk
chunk = doc.chunk

# Configure alignment parameters
chunk.matchPhotos(
    downscale=1,  # Highest accuracy
    generic_preselection=True,
    reference_preselection=False
)

chunk.alignCameras()

# Save the project with updated alignment
doc.save("path_to_project/aligned_project.psz")
```

### **Pseudo Code for Alignment Workflow**

```python
# Pseudo Code: Alignment Workflow in Metashape

function align_photos(image_list, parameters):
    # Step 1: Initialize Sparse Point Cloud
    sparse_point_cloud = []
    
    # Step 2: Detect Key Points
    for image in image_list:
        keypoints[image] = detect_features(image, parameters['key_point_limit'])
    
    # Step 3: Match Features Across Images
    for image1, image2 in image_pairs(image_list):
        matches = match_features(keypoints[image1], keypoints[image2])
        if matches:
            tie_points.append(matches)
    
    # Step 4: Build Tie Point Cloud
    sparse_point_cloud = create_sparse_cloud(tie_points)
    
    # Step 5: Perform Bundle Adjustment
    camera_parameters = initial_camera_parameters(image_list)
    optimized_parameters = bundle_adjustment(camera_parameters, sparse_point_cloud)
    
    # Step 6: Refine Results
    if parameters['adaptive_camera_model_fitting']:
        optimized_parameters = refine_camera_model(optimized_parameters)
    
    # Step 7: Save Results
    save_sparse_cloud(sparse_point_cloud, optimized_parameters)
    
    return sparse_point_cloud, optimized_parameters

function detect_features(image, key_point_limit):
    # Use SIFT or similar feature detection algorithm
    features = sift(image)
    return features[:key_point_limit]

function match_features(keypoints1, keypoints2):
    # Perform Nearest Neighbor Search to find matches
    matches = nearest_neighbors(keypoints1, keypoints2)
    return matches

function bundle_adjustment(camera_params, sparse_cloud):
    # Use Levenberg-Marquardt optimization
    optimized_params = levenberg_marquardt(camera_params, sparse_cloud)
    return optimized_params
```
![image](https://github.com/user-attachments/assets/9cd606b1-c767-4e6c-93e8-581ddbe22f3a)

---

### **Detailed Workflow Explanation**

#### 1. **Key Point Detection**:
   - Detect features in each image using SIFT or equivalent methods.
   - Limit the number of detected features per image using the **Key Point Limit** parameter.

#### 2. **Feature Matching**:
   - Match features across overlapping image pairs.
   - Use nearest neighbor search with filtering to ensure reliability.

#### 3. **Sparse Point Cloud Generation**:
   - Triangulate matched features to create a sparse 3D point cloud.
   - Retain only reliable tie points based on the **Tie Point Limit**.

#### 4. **Bundle Adjustment**:

   - Refine camera parameters iteratively to minimize reprojection error:
     -  **Reprojection Error**
        Reprojection error measures how accurately 3D points project back into the 2D image plane:
    
        $$\text{Reprojection Error} = \sqrt{\frac{1}{N} \sum_{i=1}^N \left( (x_i - \hat{x}_i)^2 + (y_i - \hat{y}_i)^2 \right)}$$
        
        Where:
        
        - $$N$$: Total number of tie points.
        - $$(x_i, y_i)$$: Observed tie point coordinates.
        - $$(\hat{x}_i, \hat{y}_i)$$: Reprojected tie point coordinates.
    
     - **Tie Points**
        The software matches feature points across images and forms tie points for 3D reconstruction:
        
        $$\text{Tie Points} = \text{Key Points Matched Across Multiple Images}$$
        
        Higher tie point limits improve alignment but increase processing time and memory usage.
#### 5. **Adaptive Camera Model Fitting**:
   - Optimize intrinsic parameters like focal length and distortion coefficients for datasets with varying image quality.

#### 6. **Review and Save Results**:
   - Visualize the sparse point cloud in the Model View.
   - Inspect alignment quality using reprojection error metrics.

---
### **Core Algorithms in Metashape Alignment**

1. **Feature Detection**:
   - **Algorithm**: Scale-Invariant Feature Transform (SIFT) or optimized equivalent.
   - **Purpose**: Extract distinctive key points in images that are invariant to scale and rotation.

2. **Feature Matching**:
   - **Algorithm**: Nearest Neighbor Search using KD-Trees or FLANN (Fast Library for Approximate Nearest Neighbors).
   - **Purpose**: Identify corresponding features between image pairs.

3. **Bundle Adjustment**:
   - **Algorithm**: Levenberg-Marquardt optimization.
   - **Purpose**: Refine camera positions and orientations while minimizing reprojection error.

4. **Camera Model Optimization**:
   - Adjusts intrinsic and extrinsic parameters using adaptive fitting for improved accuracy with varying image quality.

---



### **Key Best Practices**

- **High Accuracy Settings**:
  - Use for detailed projects with small datasets.
  - Ensure high-quality images with sufficient overlap.
- **Low Accuracy Settings**:
  - Suitable for large datasets with less focus on precision.
  - Optimize for speed and memory efficiency.

- **Overlap Requirements**:
  - Aerial: 60–80% overlap.
  - Close-range: Avoid blind spots and ensure object coverage.

- **Pre-Calibration**:
  - Use predefined lens calibration for fisheye or ultra-wide-angle lenses to improve stability.

---
More details about the feature-matching techniques can be found in [feature-matching](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/algorithms/feature_matching.md)

## References
1. [Agisoft Metashape User Manual (Version 2.1)](https://www.agisoft.com/pdf/metashape_2_1_en.pdf).
2. [Agisoft Metashape Python API Reference (Version 2.1.3)](https://www.agisoft.com/pdf/metashape_python_api_2_1_3.pdf).
