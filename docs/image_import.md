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
