
# Alignment

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
  \[
  \text{Image Size} = \frac{\text{Original Resolution}}{\text{Downscale Factor}}
  \]

- **Key Point Limit**: Maximum feature points detected per image (default is 40,000).
- **Tie Point Limit**: Maximum tie points retained after filtering (default is 4,000).
- **Adaptive Camera Model Fitting**: Enables optimization of camera parameters for varying image quality or EXIF inconsistencies.

### **3. Run the Alignment**
- Click **OK** to initiate alignment.
- Review the resulting sparse point cloud in the **Model View**.
- Inspect alignment errors in the **Reference Pane** if GPS data is available.

---

## Notes
- Ensure a minimum of 60% side overlap and 80% forward overlap for aerial datasets.
- Check for misaligned cameras or sparse point clouds caused by:
  - Poor image overlap.
  - Low-quality images (blur, noise, overexposure).
  - Inadequate key or tie points.

---

## Formulas and Metrics

### **Reprojection Error**
Reprojection error measures how accurately 3D points project back into the 2D image plane:
\[
\text{Reprojection Error} = \sqrt{\frac{1}{N} \sum_{i=1}^N \left( (x_i - \hat{x}_i)^2 + (y_i - \hat{y}_i)^2 \right)}
\]
Where:
- \( N \): Total number of tie points.
- \( (x_i, y_i) \): Observed tie point coordinates.
- \( (\hat{x}_i, \hat{y}_i) \): Reprojected tie point coordinates.

### **Tie Points**
The software matches feature points across images and forms tie points for 3D reconstruction:
\[
\text{Tie Points} = \text{Key Points Matched Across Multiple Images}
\]
Higher tie point limits improve alignment but increase processing time and memory usage.

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

---

## References
1. [Agisoft Metashape User Manual (Version 2.1)](https://www.agisoft.com/pdf/metashape_2_1_en.pdf).
2. [Agisoft Metashape Python API Reference (Version 2.1.3)](https://www.agisoft.com/pdf/metashape_python_api_2_1_3.pdf).
