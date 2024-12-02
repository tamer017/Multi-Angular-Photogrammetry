# Image Import

## Overview
The first step in processing multi-angular UAV data is importing images into Agisoft Metashape. This process ensures that all source imagery is correctly loaded for alignment and further photogrammetric processing.

---

## Steps

### **1. Open Metashape and Create a New Project**
- Launch Agisoft Metashape.
- Select `File > New` to start a new project.

### **2. Add Photos**
- Navigate to `Workflow > Add Photos`.
- Select the folder containing your UAV images or manually choose files.
- Click `Open` to import the selected images into the project.

### **3. Inspect Loaded Images**
- Verify all images are loaded in the **Photos pane**.
- Double-click any image to preview and confirm data quality.

### **4. Review Metadata**
- Check for missing or inconsistent EXIF metadata:
  - Sensor pixel size.
  - Focal length.
- If necessary, adjust metadata in the **Camera Calibration** dialog (`Tools > Camera Calibration`).

---

## Python Code for Automation
To automate the import process using the Python API, use the following example script:

```python
import Metashape

# Initialize Metashape application
app = Metashape.Application()

# Create a new project
doc = Metashape.Document()
doc.open("path_to_project/project.psz")

# Add photos to the project
chunk = doc.addChunk()
chunk.addPhotos(["/path/to/images/image1.jpg", "/path/to/images/image2.jpg"])

# Save the project
doc.save("path_to_project/updated_project.psz")
```

This script demonstrates the core workflow:
1. Opening or creating a project.
2. Adding photos to a new chunk.
3. Saving the updated project.

---

## Best Practices

### **Image Preparation**
- Ensure a consistent naming convention across files for easier identification.
- Avoid cropping or geometric modifications to source images.
- Maintain original image resolutions; avoid resizing or rotating images.

### **Data Quality**
- Verify that images have sufficient overlap (e.g., 75% forward and 60% side for aerial surveys).
- Use RAW or minimally compressed formats (e.g., TIFF) to reduce noise.
- Avoid blurry, distorted, or underexposed images to ensure alignment accuracy.

### **Hardware Considerations**
- Use a machine with sufficient RAM to handle large datasets. For instance:
  - 16 GB for ~300-500 images at 10 MP resolution.
  - 32 GB or more for larger datasets.

---

## Notes
- Metashape calculates initial parameters (e.g., focal length, pixel size) using EXIF metadata. Missing or inaccurate metadata can affect results.
- If EXIF data is unreliable or unavailable, manually set calibration parameters in **Camera Calibration**.
- Use the `Reduce Overlap` tool for large datasets to eliminate redundant images and optimize processing time.

---

## References
1. [Agisoft Metashape Python API Reference (Version 2.1.3)](https://www.agisoft.com/pdf/metashape_python_api_2_1_3.pdf).
2. [Agisoft Metashape User Manual (Version 2.1)](https://www.agisoft.com/pdf/metashape_2_1_en.pdf).
```
