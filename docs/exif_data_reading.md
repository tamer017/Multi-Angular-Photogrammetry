
# EXIF Data Reading

## Table of Contents
1. [Overview](#overview)
2. [Steps](#steps)
3. [Python Code for EXIF Data Inspection](#python-code-for-exif-data-inspection)
4. [Handling Missing EXIF Data](#handling-missing-exif-data)
   - [1. Manually Input Calibration Parameters](#1-manually-input-calibration-parameters)
   - [2. Use Reference Data](#2-use-reference-data)
   - [3. Automate Fallback Parameters](#3-automate-fallback-parameters)
5. [References](#references)

## Overview
EXIF metadata contains crucial information, such as camera parameters (e.g., focal length, pixel size) and GPS coordinates. This information is essential for accurate alignment and reconstruction processes in photogrammetry.

---

## Steps

### **1. Import Images**
- Load images into the project via `Workflow > Add Photos`.

### **2. Open Camera Calibration**
- Navigate to `Tools > Camera Calibration` to inspect the camera settings.

### **3. Verify EXIF Metadata**
- Ensure that parameters such as **Focal Length**, **Sensor Width**, and **Pixel Size** are loaded correctly for each image.
- Check for any discrepancies or missing values.

### **4. Adjust Calibration Settings**
- For missing or incorrect EXIF data:
  - Manually input the correct focal length, sensor width, or other calibration parameters in the **Camera Calibration** dialog.
  - Use an external source (e.g., camera specifications or datasheets) for accurate values.
- Enable **Adaptive Camera Model Fitting** during alignment for datasets with inconsistent EXIF data.

---

## Notes
- **Impact of Missing EXIF Data**:
  - Misalignment or poor-quality sparse point clouds can result from incorrect or missing EXIF metadata.
  - Default assumptions (e.g., 50mm focal length equivalent) may lead to errors if actual camera settings differ significantly.

- **Recommended Practice**:
  - Use cameras with well-documented EXIF metadata.
  - Avoid manual cropping or editing of images that might strip EXIF metadata.

---

## Python Code for EXIF Data Inspection

Below is an example script to read and inspect EXIF metadata using the Metashape Python API:

```python
import Metashape

# Open an existing project
doc = Metashape.Document()
doc.open("path_to_project/project.psz")
chunk = doc.chunk

# Iterate through cameras to inspect EXIF data
for camera in chunk.cameras:
    print(f"Camera: {camera.label}")
    if "Exif/FocalLength" in camera.photo.meta:
        focal_length = camera.photo.meta["Exif/FocalLength"]
        print(f"  Focal Length: {focal_length}")
    else:
        print("  Focal Length: Missing")
    
    if "Exif/GPSLatitude" in camera.photo.meta and "Exif/GPSLongitude" in camera.photo.meta:
        gps_lat = camera.photo.meta["Exif/GPSLatitude"]
        gps_lon = camera.photo.meta["Exif/GPSLongitude"]
        print(f"  GPS: {gps_lat}, {gps_lon}")
    else:
        print("  GPS Data: Missing")
```

---

## Handling Missing EXIF Data

### **1. Manually Input Calibration Parameters**
If EXIF data is missing, manually input values in the **Camera Calibration** dialog:
- **Focal Length**: Refer to the camera's technical specifications.
- **Sensor Width and Height**: Look up sensor dimensions from the manufacturer.

### **2. Use Reference Data**
For GPS coordinates, you can use external GNSS logs or base station data to georeference images:
- Import georeferenced data into the **Reference Pane**.

### **3. Automate Fallback Parameters**
Update the default camera model programmatically using the Python API:

```python
# Assign default calibration parameters for cameras
for sensor in chunk.sensors:
    sensor.calibration.f = 35  # Example focal length in mm
    sensor.calibration.width = 36  # Sensor width in mm
    sensor.calibration.height = 24  # Sensor height in mm
```

---

## References
1. [Agisoft Metashape User Manual (Version 2.1)](https://www.agisoft.com/pdf/metashape_2_1_en.pdf).
2. [Agisoft Metashape Python API Reference (Version 2.1.3)](https://www.agisoft.com/pdf/metashape_python_api_2_1_3.pdf).
