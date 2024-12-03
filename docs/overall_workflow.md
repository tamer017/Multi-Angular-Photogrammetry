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

This document outlines the complete photogrammetry workflow using Agisoft Metashape, detailing each step from input to output, algorithms used, and applicable references.

---

## Workflow Steps

### 1. Image Import
- **Input**: Digital images in supported formats (JPEG, TIFF, etc.).
- **Output**: Imported images listed in the Metashape workspace.
- **Algorithm**: Metadata extraction and validation.
- **Steps**:
  1. Add images to the project via the `Workflow > Add Photos` option.
  2. Review image metadata in the Photos panel.
- **References**: [Metashape Manual, Chapter 3](13).

---

### 2. EXIF Data Reading
- **Input**: Metadata from imported images (focal length, sensor size, GPS data).
- **Output**: Validated camera parameters for alignment.
- **Algorithm**: EXIF parsing and initial calibration.
- **Steps**:
  1. Extract EXIF data from images.
  2. Calibrate cameras based on metadata or manual inputs.
- **References**: [Metashape Manual, Chapter 3](13).

---

### 3. Image Alignment
- **Input**: Imported images with validated metadata.
- **Output**: Sparse point cloud and initial camera positions.
- **Algorithm**: SIFT (Scale-Invariant Feature Transform), Bundle Adjustment.
- **Steps**:
  1. Align photos using `Workflow > Align Photos`.
  2. Adjust parameters like accuracy and key point limit.
- **References**: [Metashape Manual, Chapter 3](13).

---

### 4. Sparse Point Cloud Generation
- **Input**: Aligned images and tie points.
- **Output**: Sparse 3D point cloud.
- **Algorithm**: Triangulation and optimization.
- **Steps**:
  1. Extract tie points to generate a sparse cloud.
  2. Optimize the sparse cloud using bundle adjustment.
- **References**: [Metashape Manual, Chapter 3](13).

---

### 5. Dense Point Cloud Generation
- **Input**: Sparse point cloud and camera positions.
- **Output**: Dense point cloud with finer details.
- **Algorithm**: Dense stereo matching.
- **Steps**:
  1. Generate depth maps.
  2. Build the dense cloud using `Workflow > Build Dense Cloud`.
- **References**: [Metashape Manual, Chapter 3](13).

---

### 6. Mesh Creation
- **Input**: Dense point cloud or depth maps.
- **Output**: Polygonal mesh model.
- **Algorithm**: Surface reconstruction.
- **Steps**:
  1. Use `Workflow > Build Mesh`.
  2. Specify surface type and interpolation parameters.
- **References**: [Metashape Manual, Chapter 3](13).

---

### 7. Digital Elevation Model (DEM) Generation
- **Input**: Dense cloud or mesh.
- **Output**: Elevation grid (DEM).
- **Algorithm**: Interpolation and rasterization.
- **Steps**:
  1. Build DEM using `Workflow > Build DEM`.
  2. Choose the source data and set resolution.
- **References**: [Metashape Manual, Chapter 3](13).

---

### 8. Orthophoto and Orthomosaic Generation
- **Input**: DEM or dense cloud.
- **Output**: Orthorectified imagery (orthophotos, orthomosaics).
- **Algorithm**: Orthorectification and mosaicking.
- **Steps**:
  1. Use `Workflow > Build Orthomosaic`.
  2. Adjust blending and resolution settings.
- **References**: [Metashape Manual, Chapter 3](13).

---

## Workflow Diagram

![Workflow Diagram](photogrammetry_workflow.png)

---

## References
1. Agisoft Metashape User Manual - Standard Edition, Version 2.1 [PDF](13).
2. Agisoft Metashape Python API Reference, Version 2.1.3 [PDF](12).

