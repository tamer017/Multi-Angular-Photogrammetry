# Orthorectification, Orthomosaics, and Orthophotos in Agisoft Metashape

## Table of Contents
1. [Introduction](#introduction)
2. [Key Concepts](#key-concepts)
3. [Role of DEM](#role-of-dem)
4. [Comparisons](#comparisons)
5. [Workflow in Metashape](#workflow-in-metashape)
6. [How It Works](#how-it-works)
7. [Python Implementation](#python-implementation)
8. [Pseudocode](#pseudocode)
9. [Formulas Used](#formulas-used)
10. [Parameters and Settings](#parameters-and-settings)
11. [Challenges](#challenges)
12. [References](#references)

---

## Introduction

**Orthorectification**, **Orthomosaics**, and **Orthophotos** are critical outputs in geospatial analysis.  
- **Orthorectification** corrects distortions caused by terrain relief and camera tilt.  
- **Orthophoto** represents a single corrected image.  
- **Orthomosaic** is a seamless composite of multiple orthophotos.

These outputs are crucial in GIS, precision mapping, and land-use analysis.

---

## Key Concepts

1. **Orthorectification**: Aligns each pixel in an image to real-world coordinates using terrain elevation and camera data.  
2. **Orthophotos**: Single georeferenced images derived from orthorectification.  
3. **Orthomosaics**: A seamless combination of multiple orthophotos covering larger areas.  
4. **DEM**: Provides terrain elevation data to improve accuracy during orthorectification.

---

## Role of DEM

### Does Orthophoto/Orthomosaic Generation Use DEM?
Yes, orthophoto/orthomosaic generation can use DEMs, but it is not mandatory.

### When DEM Is Used:
- DEM is a key input for **orthorectification**, as it accounts for terrain elevation.  
- If no DEM is explicitly provided, **dense cloud** or **mesh** is used to derive elevation.

### Why Calculate DEM Separately?
1. **Reusable Output**: DEMs are valuable in external GIS workflows for hydrology, slope analysis, and more.  
2. **Efficiency**: Pre-computed DEMs simplify workflows by avoiding recalculation of elevation data.  
3. **Integration**: External DEMs can replace photogrammetrically derived terrain models when available.  

For detailed steps on DEM generation, refer to the [DEM Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/dem.md).

---

## Comparisons

| Feature               | Orthorectification            | Orthophotos                  | Orthomosaics                  |
|-----------------------|-------------------------------|------------------------------|-------------------------------|
| **Definition**        | Corrects distortions          | Single corrected image       | Blended image from multiple orthophotos |
| **Use Case**          | Intermediate step             | Small areas                  | Large-scale mapping            |
| **Output**            | Georeferenced image           | One image                    | Seamless composite            |
| **File Size**         | Medium                       | Smaller                      | Larger                        |
| **Processing Time**   | Short                        | Moderate                     | Long                          |

---

## Workflow in Metashape

1. **Image Alignment**  
   Refer to the [Alignment Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/alignment.md).

2. **Dense Cloud Generation**  
   Refer to the [Dense Cloud Generation Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/dense_cloud_generation.md).

3. **Mesh Creation**  
   Refer to the [Mesh Creation Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/mesh_creation.md).

4. **DEM Generation** (Optional)  
   Refer to the [DEM Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/dem.md).

5. **Orthophoto and Orthomosaic Creation**  
   - Select the source data: **Dense Cloud**, **Mesh**, or **DEM**.  
   - Configure blending and projection settings.  
   - Generate orthophoto or orthomosaic.

---

## How It Works

### Orthorectification
- **Input**: Dense Cloud, Mesh, or DEM.  
- **Process**:  
  1. Corrects geometric distortions due to terrain relief and sensor tilt.  
  2. Maps each pixel to real-world coordinates based on projection systems like UTM or WGS84.

### Orthophotos
- Derived from individual orthorectified images.  
- Best suited for small or isolated areas.  

### Orthomosaics
- Combines multiple orthophotos seamlessly.  
- Uses blending modes to remove overlapping artifacts and balance radiometry.

---

## Python Implementation

### Orthophoto and Orthomosaic Creation
```python
import Metashape

# Initialize project and load existing chunk
doc = Metashape.app.document
chunk = doc.chunk

# Generate Orthophoto or Orthomosaic
chunk.buildOrthomosaic(
    surface_data=Metashape.ModelData,  # Use mesh as the source, or DenseCloudData for broader areas
    blending_mode=Metashape.MosaicBlending,  # Use Mosaic for natural results
    projection=chunk.crs,  # Set map projection (e.g., UTM, WGS84)
    resolution_x=0.1,  # Horizontal resolution in meters
    resolution_y=0.1   # Vertical resolution in meters
)

# Save Project
doc.save("project_with_orthomosaic.psz")
```

---

## Pseudocode

```plaintext
Start Project
  Load Project
  Select Chunk
  
  # Generate Orthophoto/Orthomosaic
  Choose Source Data (Dense Cloud, Mesh, or DEM)
  Apply Orthorectification
  Configure Blending Mode (e.g., Mosaic)
  Set Output Projection System
  Specify Resolution
  Export Results
  
  Save Project
End
```

---

## Formulas Used

1. **Orthorectification**  
   Orthorectification uses the DEM (or equivalent) to compute corrected pixel locations:

   $$P' = P \cdot T^{-1}$$
   
   Where:
   - $$P$$: Original pixel coordinates.
   - $$T$$: Transformation matrix accounting for terrain relief and projection.

3. **Blending for Orthomosaics**  
   Combines overlapping images using radiometric weights:

   $$I_{final}(x, y) = \frac{\sum w_k I_k(x, y)}{\sum w_k}$$
   
   Where:
   - $$I_k(x, y)$$: Intensity value from image $$k$$.  
   - $$w_k$$: Weight for blending based on proximity or angle.

---

## Parameters and Settings

| Parameter            | Description                                          | Default Value          |
|-----------------------|------------------------------------------------------|------------------------|
| `Surface Data`        | Input source for orthophoto/orthomosaic generation   | Mesh or Dense Cloud    |
| `Blending Mode`       | Method for merging overlapping areas                | MosaicBlending         |
| `Projection`          | Coordinate system for spatial alignment             | Auto-detected          |
| `Resolution X/Y`      | Spatial resolution in meters                        | Depends on input data  |
| `Fill Holes`          | Enable/disable filling of missing data in output     | True                   |

---

## Challenges

1. **Projection Consistency**: Misaligned projections can distort orthophotos or orthomosaics.  
2. **Blending Artifacts**: Incorrect blending may result in visible seams.  
3. **Large Areas**: High-resolution orthomosaics of extensive regions can strain computational resources.  

---

## References

1. [Alignment Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/alignment.md)  
2. [Dense Cloud Generation Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/dense_cloud_generation.md)  
3. [Mesh Creation Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/mesh_creation.md)  
4. [DEM Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/dem.md)  
5. [Agisoft Metashape User Manual (Version 2.1)](https://www.agisoft.com/pdf/metashape_2_1_en.pdf).
6. [Agisoft Metashape Python API Reference (Version 2.1.3)](https://www.agisoft.com/pdf/metashape_python_api_2_1_3.pdf).


