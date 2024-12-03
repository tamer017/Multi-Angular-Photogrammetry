
# UAV Multispectral Data for Plant Pathology Studies

## Table of Contents
1. [Introduction](#introduction)
2. [Electromagnetic Signals](#electromagnetic-signals)
3. [Camera and Sensor System](#camera-and-sensor-system)
4. [Spectral Bands and Wavelengths](#spectral-bands-and-wavelengths)
5. [Data Structure](#data-structure)
6. [Data Preprocessing](#data-preprocessing)
7. [Applications of UAV Multispectral Data](#applications-of-uav-multispectral-data)
8. [Challenges and Considerations](#challenges-and-considerations)
9. [ON Cerco Project: Background and Goals](#on-cerco-project-background-and-goals)

---

## Introduction
UAV multispectral data has revolutionized remote sensing by providing precise, efficient, and versatile tools for data acquisition. Equipped with advanced multispectral cameras like the **MicaSense RedEdge-P™**, UAVs can collect detailed information essential for industries such as agriculture, forestry, and environmental monitoring. This specific dataset is being used to advance plant disease modeling by leveraging multi-angular UAV data for studying plants under pathogen stress.

---

## Electromagnetic Signals
Electromagnetic (EM) signals are the foundation of remote sensing. They are emitted or reflected by objects on the Earth's surface and provide critical information about the properties of these objects. In the case of multispectral data, different materials interact with electromagnetic radiation in distinct ways, depending on their physical and chemical properties.

The **MicaSense RedEdge-P™** camera captures electromagnetic signals across various wavelengths, which allows the differentiation of plant health, disease symptoms, and other environmental factors. In this context, multispectral data is essential for analyzing spectral signatures that reflect stress caused by pathogens, especially when captured from multiple angles.

![Integration Example](../images/science-electromagnetic-spectrum-diagram-free-vector.jpg)

---


## Camera and Sensor System

### MicaSense RedEdge-P™
The **MicaSense RedEdge-P™** is a high-performance multispectral camera designed for UAV-based remote sensing. Key features include:
- **Five Narrow Spectral Bands**: Captures data with precision across specific wavelengths (detailed in the next section).
- **High Resolution**: Provides professional-grade imagery for detailed analysis.
- **Global Shutter**: Ensures distortion-free images, even during rapid UAV movement.
- **Fast Capture Rate**: Capable of acquiring images at high speeds for efficient data collection.

For more details on the **MicaSense RedEdge-P™**, visit the [camera documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/edit/master/docs/camera.md).

### DLS 2 (Downwelling Light Sensor)
The **DLS 2** system measures ambient light conditions during data capture. It:
- Adjusts for changes in sunlight to improve reflectance calibration.
- Includes an integrated GPS for geotagging images.

---

### Placeholder for Example Image  
`![MicaSense Data Example](path_to_image/micasense_data_example.jpg)`

---

## Spectral Bands and Wavelengths

The **RedEdge-P™** camera captures data across five spectral bands, as detailed below:

| **Spectral Band** | **Wavelength Range (nm)** | **Applications**                                   |
|--------------------|---------------------------|---------------------------------------------------|
| Blue               | 475 ± 20                 | Water body monitoring, vegetation discrimination  |
| Green              | 560 ± 20                 | Vegetation health, plant vigor                    |
| Red                | 668 ± 10                 | Chlorophyll absorption, soil boundaries          |
| Red Edge           | 717 ± 10                 | Early stress detection in plants                 |
| Near-Infrared (NIR)| 842 ± 26                 | Vegetation health (NDVI), biomass estimation     |

### Placeholder for Spectral Bands Visualization  
`![Spectral Bands Visualization](path_to_image/spectral_bands_rededgep.jpg)`

---

## Data Structure

### Suggested Folder Structure
To organize data collected during flights, the following folder structure is recommended. Each flight contains six images per channel (one per spectral band, named 1-6).

```
project_name/
│
├── flight_1/
│   ├── raw_data/
│   │   ├── 1.tif  # Blue channel
│   │   ├── 2.tif  # Green channel
│   │   ├── 3.tif  # Red channel
│   │   ├── 4.tif  # Red Edge channel
│   │   ├── 5.tif  # Near-Infrared channel
│   │   └── 6.tif  # Optional extra image (e.g., reflectance panel or calibration image)
│   ├── calibration/
│   │   ├── dls_reading.json
│   │   └── reflectance_panel.tif
│   └── metadata.json
│
├── flight_2/
│   ├── raw_data/
│   │   ├── 1.tif
│   │   ├── 2.tif
│   │   ├── 3.tif
│   │   ├── 4.tif
│   │   ├── 5.tif
│   │   └── 6.tif
│   ├── calibration/
│   │   ├── dls_reading.json
│   │   └── reflectance_panel.tif
│   └── metadata.json
│
└── examples/
    └── stitched_mosaic.jpg
```

Each image is named based on the corresponding spectral band:
- **1.tif**: Blue channel (475 ± 20 nm)
- **2.tif**: Green channel (560 ± 20 nm)
- **3.tif**: Red channel (668 ± 10 nm)
- **4.tif**: Red Edge channel (717 ± 10 nm)
- **5.tif**: Near-Infrared channel (842 ± 26 nm)
- **6.tif**: Optional extra image (e.g., a calibration panel image)

### Metadata Fields
Each flight should include metadata:
- **Camera Model**: MicaSense RedEdge-P™  
- **Light Sensor**: DLS 2  
- **Date and Time**: For temporal analysis.  
- **Flight Altitude**: Ensures reproducibility.  
- **GPS Coordinates**: Geolocation of each image.  

---

## Data Preprocessing

### Steps:
1. **Noise Reduction**:  
   - Apply filters to remove sensor and environmental noise (e.g., haze, motion blur).  

2. **Radiometric Calibration**:  
   - Use DLS 2 data and reflectance panels to normalize reflectance values for consistent lighting.

3. **Geometric Corrections**:  
   - Correct distortions due to camera tilt or UAV motion.  

4. **Image Stitching**:  
   - Combine overlapping images into a seamless orthomosaic for analysis.

---

## Applications of UAV Multispectral Data

1. **Agriculture**:  
   - Use NDVI or other vegetation indices for crop health monitoring.  
   - Optimize irrigation and nutrient management.

2. **Forestry**:  
   - Assess forest canopy health and identify stressed vegetation.  

3. **Environmental Monitoring**:  
   - Track changes in water quality or detect pollutants.  

4. **Urban Planning**:  
   - Identify vegetation patterns and urban heat islands.  

---

## Challenges and Considerations

1. **Lighting Conditions**:  
   - Variations in sunlight require robust calibration using DLS 2.  

2. **Data Volume**:  
   - Large datasets demand efficient storage and high-performance processing tools.

3. **Complex Preprocessing**:  
   - Requires specialized software (e.g., Pix4D, Agisoft Metashape) for orthomosaic generation and analysis.

---

## ON Cerco Project: Background and Goals

### Background
The **ON Cerco Project** is a research initiative funded by the German Research Foundation (DFG), hosted by the Institute for Sugar Beet Research and the Stachniss Lab for Photogrammetry & Robotics at the University of Bonn. The project aims to enhance plant disease modeling by leveraging multi-angular UAV data. It focuses on studying plants under pathogen stress, particularly in crops like sugar beet, where the leaves grow upright and rosette-oriented. This innovative approach breaks new ground by capturing **multi-angular reflectance data** from both nad

ir and oblique observation angles.

The inclusion of **oblique angles** allows for a more comprehensive evaluation of the **pathosystem** (pathogen + host + environment), enabling the capture of disease patterns on both the upper and lower leaf surfaces. This comprehensive data acquisition could significantly improve disease quantification models and provide new insights into plant disease dynamics.

### Goal
- Develop an image processing approach that:
  - Allows for unbiased radiometric properties.
  - Extracts spectral data from all available zenith and azimuth angles, enhancing the analysis of plant pathogens.

### Tasks
1. Understand the difference between spectral signatures from **nadir** (straight-down) and **oblique** (angled) observation angles.
2. Develop a pre-processing workflow that addresses biases in spectral data.
3. Extract spectral data from all available observation angles and optimize runtime for processing large datasets (> 1 million rows).
4. **Optional**: Obtain a UAV license to collect multi-angular data for optimizing current flight patterns in vegetation science.

---

## Conclusion
The **MicaSense RedEdge-P™** camera, together with multi-angular data collection, offers significant advancements in remote sensing for plant pathology. The **ON Cerco Project** is an example of how these technologies are being used to revolutionize plant disease modeling by capturing disease dynamics from multiple angles. The data generated will help improve disease management strategies and enhance agricultural practices.

---

### Additional Resources
- [MicaSense RedEdge-P™ Overview](https://www.micasense.com/rededge-p)
- [Reflectance Calibration Techniques](https://example.com/reflectance-calibration)
- [NDVI and Vegetation Indices](https://example.com/ndvi-guide)
