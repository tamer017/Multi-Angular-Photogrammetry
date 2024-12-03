# Complete Workflow for Agisoft Metashape

## Table of Contents
1. [Introduction](#introduction)
2. [Workflow Steps](#workflow-steps)
   - [Image Import](#1-image-import)
   - [EXIF Data Reading](#2-exif-data-reading)
   - [Image Alignment](#3-image-alignment)
   - [Sparse Point Cloud Generation](#4-sparse-point-cloud-generation)
   - [Dense Point Cloud Generation](#5-dense-point-cloud-generation)
   - [Mesh Creation](#6-mesh-creation)
   - [Digital Elevation Model (DEM) Generation](#7-digital-elevation-model-dem-generation)
   - [Orthophoto and Orthomosaic Generation](#8-orthophoto-and-orthomosaic-generation)
3. [Workflow Diagram](#workflow-diagram)
4. [References](#references)

---

## Introduction

This document outlines the complete photogrammetry workflow using Agisoft Metashape, detailing each step from input to output and the algorithms used.

---

## Workflow Steps

### 1. Image Import
- **Description**: This step involves adding digital images into Metashape for processing. The software validates and organizes the input images.
- **Input**: Digital images in supported formats (JPEG, TIFF, etc.).
- **Output**: Imported images listed in the Metashape workspace.
- **Algorithm**: Metadata extraction and validation.
- **Details**: [Image Import Documentation](./image_import.md)

---

### 2. EXIF Data Reading
- **Description**: Validates and processes metadata embedded in images, such as focal length and GPS coordinates, for camera calibration.
- **Input**: Metadata from imported images.
- **Output**: Validated camera parameters for alignment.
- **Algorithm**: EXIF parsing and initial calibration.
- **Details**: [EXIF Data Documentation](./exif_data_reading.md)

---

### 3. Image Alignment
- **Description**: Aligns images by detecting features and estimating camera positions, generating a sparse point cloud.
- **Input**: Imported images with validated metadata.
- **Output**: Sparse point cloud and initial camera positions.
- **Algorithm**: SIFT (Scale-Invariant Feature Transform), Bundle Adjustment.
- **Details**: [Image Alignment Documentation](./alignment.md)

---

### 4. Sparse Point Cloud Generation
- **Description**: Creates a 3D representation of the scene by optimizing the alignment results and extracting tie points.
- **Input**: Aligned images and tie points.
- **Output**: Sparse 3D point cloud.
- **Algorithm**: Triangulation and optimization.
- **Details**: [Sparse Point Cloud Documentation](./sparse_point_cloud.md)

---

### 5. Dense Point Cloud Generation
- **Description**: Enhances the sparse point cloud by adding fine details using dense stereo matching.
- **Input**: Sparse point cloud and camera positions.
- **Output**: Dense point cloud with finer details.
- **Algorithm**: Dense stereo matching.
- **Details**: [Dense Point Cloud Documentation](./dense_cloud_generation.md)

---

### 6. Mesh Creation
- **Description**: Converts the dense point cloud into a polygonal mesh for a detailed surface representation.
- **Input**: Dense point cloud or depth maps.
- **Output**: Polygonal mesh model.
- **Algorithm**: Surface reconstruction.
- **Details**: [Mesh Creation Documentation](./mesh_creation.md)

---

### 7. Digital Elevation Model (DEM) Generation
- **Description**: Produces a grid-based elevation model derived from the dense cloud or mesh.
- **Input**: Dense cloud or mesh.
- **Output**: Digital Elevation Model (DEM).
- **Algorithm**: Interpolation and rasterization.
- **Details**: [DEM Documentation](./dem.md)

---

### 8. Orthophoto and Orthomosaic Generation
- **Description**: Creates orthorectified images and mosaics corrected for geometric distortions.
- **Input**: DEM or dense cloud.
- **Output**: Orthophotos, Orthomosaics.
- **Algorithm**: Orthorectification and mosaicking.
- **Details**: [Orthophoto Documentation](./orthophoto_generation.md)

---

## Workflow Diagram

![Workflow Diagram](photogrammetry_workflow.png)

---

## References
1. [Agisoft Metashape User Manual](./metashape_user_manual.pdf)
2. [Agisoft Metashape Python API Reference](./metashape_python_api_reference.pdf)
