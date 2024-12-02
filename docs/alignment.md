
# Alignment

## Overview
Alignment is the process of matching corresponding points across multiple images to generate a sparse point cloud. It is a critical step in photogrammetric processing as it defines the foundation for further stages like dense cloud generation, mesh building, and texture mapping.

---

## Steps

### **1. Start Alignment**
- Navigate to `Workflow > Align Photos`.
- Select the desired alignment parameters from the **Align Photos** dialog box.

### **2. Configure Parameters**
- **Accuracy**: Controls the downscaling factor applied to images during alignment:
  - **Highest**: Uses original resolution (slower but more precise).
  - **High**: Downscales images by 2×.
  - **Medium**: Downscales images by 4× (faster but less precise).
  - **Low**: Downscales images by 8×.
  - **Lowest**: Downscales images by 16× (suitable for very large datasets).
  
  **Formula for downscaling**:
  \[
  \text{Image Size (pixels)} = \frac{\text{Original Size}}{\text{Downscale Factor}}
  \]

- **Key Point Limit**: Specifies the maximum number of feature points detected per image:
  - Default is 40,000.
  - Increase for detailed datasets or large, textured areas.
  - Reduce for simpler datasets or low-memory systems.

- **Tie Point Limit**: Specifies the maximum number of tie points retained after filtering:
  - Default is 4,000.
  - Higher values improve alignment accuracy but increase computation time.

- **Adaptive Camera Model Fitting**: Optimizes camera parameters during alignment.
  - Recommended for datasets with variable image quality or EXIF inconsistencies.

### **3. Run the Alignment**
- Click **OK** to start the alignment process.
- Monitor progress in the **Console Pane**.
- Review the resulting sparse point cloud in the **Model View** window.

---

## Notes
- **Overlap**: Ensure at least 60% side overlap and 80% forward overlap in aerial datasets.
- Misaligned points often result from:
  - Poor overlap between images.
  - Low image quality (blurriness, overexposure, noise).
  - Insufficient key points or incorrect camera calibration.

---

## Formulas

### **Feature Matching**
Feature matching is based on the **Scale-Invariant Feature Transform (SIFT)** algorithm:
\[
\text{Similarity} = \frac{\sum (f_1 - \mu_1) (f_2 - \mu_2)}{\sqrt{\sum (f_1 - \mu_1)^2} \sqrt{\sum (f_2 - \mu_2)^2}}
\]
Where:
- \( f_1, f_2 \): Feature vectors for two images.
- \( \mu_1, \mu_2 \): Mean of feature vectors.

### **Alignment Error (Reprojection Error)**
Alignment quality is measured by reprojection error:
\[
\text{Reprojection Error} = \sqrt{\frac{1}{n} \sum_{i=1}^n \left( p_i - p_i' \right)^2}
\]
Where:
- \( p_i \): Observed tie point position.
- \( p_i' \): Reprojected position of the tie point.
- \( n \): Total number of tie points.

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


### Updates
1. **Formulas**: Added reprojection error and SIFT similarity formulas.
2. **Configuration Comparison**: Detailed table for understanding trade-offs.
3. **Python Script**: Provided a practical script for aligning photos programmatically.

