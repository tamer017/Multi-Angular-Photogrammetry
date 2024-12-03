
# Digital Elevation Model (DEM) Creation in Agisoft Metashape

## Table of Contents
1. [Introduction](#introduction)
2. [Key Concepts](#key-concepts)
3. [Workflow in Metashape](#workflow-in-metashape)
4. [How DEM Generation Works](#how-dem-generation-works)
5. [Python Implementation](#python-implementation)
6. [Pseudocode](#pseudocode)
7. [Formulas Used](#formulas-used)
8. [Parameters and Settings](#parameters-and-settings)
9. [Challenges](#challenges)
10. [Comparisons](#comparisons)
11. [References](#references)

---

## Introduction

A **Digital Elevation Model (DEM)** represents the Earth's surface as a grid of elevation values. These models are crucial in topography, hydrology, and terrain analysis. Agisoft Metashape automates DEM creation from images using advanced photogrammetric and computational techniques.

---

## Key Concepts

1. **DEM**: A raster grid of elevation values derived from a 3D surface model.
2. **Dense Reconstruction**: The process of generating highly detailed 3D point clouds from overlapping images.
3. **Interpolation**: Filling gaps in sparse data to create a continuous surface.

---

## Workflow in Metashape

DEM generation involves the following steps:
1. **Image Alignment**  
   Refer to the [Alignment Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/alignment.md).

2. **Dense Point Cloud Creation**  
   Refer to the [Dense Cloud Generation Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/dense_cloud_generation.md).

3. **Mesh Creation**  
   Refer to the [Mesh Creation Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/mesh_creation.md).

4. **DEM Creation**  
   - The focus of this file: converting the 3D surface model into a raster grid.

---

## How DEM Generation Works

### Step 1: Input Data
- **Source**: Mesh or dense cloud.
- **Goal**: Project the 3D surface onto a regular grid (raster) and extract elevation values.

### Step 2: Interpolation
- Interpolation is used to fill gaps in the surface model where data is missing.  
- **Formula**:
  
    $$z_{ij} = \frac{\sum w_k z_k}{\sum w_k}$$
  
  Where:
  - $$z_{ij}$$: Interpolated elevation value for the grid cell.
  - $$z_k$$: Known elevation values from nearby points.
  - $$w_k$$: Weighting factor based on proximity.

### Step 3: Rasterization
- Elevation values are stored in a 2D grid format, aligning the data with the specified projection system (e.g., UTM).

### Step 4: Export
- The raster DEM is exported in formats like GeoTIFF with associated metadata.

---

## Python Implementation

```python
import Metashape

# Initialize project and load existing chunk
doc = Metashape.app.document
chunk = doc.chunk

# Create DEM
chunk.buildDem(
    source_data=Metashape.PointCloudData,  # Use dense cloud or mesh as the source
    interpolation=Metashape.EnabledInterpolation,  # Fill gaps using interpolation
    resolution=0.5,  # Output grid resolution in meters
)

# Save Project
doc.save("project_with_dem.psz")
```

---

## Pseudocode

```plaintext
Start Project
  Load Project
  Select Chunk
  
  # Generate DEM
  Select Source Data (e.g., Dense Cloud or Mesh)
  Interpolate Missing Elevation Data
  Set Output Grid Resolution
  Export DEM
  
  Save Project
End
```

---

## Formulas Used

### Interpolation for DEM
  $$z_{ij} = \frac{\sum w_k z_k}{\sum w_k}$$
- Fills missing values in the grid by weighting nearby points based on proximity.

### DEM Rasterization
Each 3D point is projected onto a 2D grid:
  $$(x, y) \rightarrow (i, j)$$
Where:
- $$(x, y)$$: World coordinates.
- $$(i, j)$$: Corresponding grid cell in the raster.

---

## Parameters and Settings

| Parameter             | Description                                    | Default Value          |
|------------------------|------------------------------------------------|------------------------|
| `Resolution`           | Grid size of the DEM in meters                | Depends on project     |
| `Interpolation`        | Method to fill gaps in the data               | EnabledInterpolation   |
| `Source Data`          | Input data for DEM generation                 | Dense Point Cloud      |
| `Flip X/Y/Z`           | Direction flipping for DEM axes               | False                  |
| `Region`               | Geographic region for DEM extraction          | Auto-defined           |

---

## Challenges

1. **Sparse Data**: Limited coverage in images can lead to incomplete DEMs.
2. **Artifacts**: Misalignment or errors in the dense cloud or mesh can propagate to the DEM.
3. **High Resolution**: While detailed, high-resolution DEMs require significant computational resources.

---

## Comparisons

| Setting              | Impact                        | Recommendation                  |
|-----------------------|-------------------------------|----------------------------------|
| `Resolution`         | Affects detail and file size  | Match project requirements      |
| `Interpolation`       | Impacts smoothness           | Use EnabledInterpolation        |
| `Source Data`         | Point cloud or mesh          | Dense cloud for higher accuracy |

---

## References

1. [Agisoft Metashape User Manual (Version 2.1)](https://www.agisoft.com/pdf/metashape_2_1_en.pdf).
2. [Agisoft Metashape Python API Reference (Version 2.1.3)](https://www.agisoft.com/pdf/metashape_python_api_2_1_3.pdf).
3. [Alignment Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/alignment.md)
4. [Dense Cloud Generation Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/dense_cloud_generation.md)
5. [Mesh Creation Documentation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/mesh_creation.md)

