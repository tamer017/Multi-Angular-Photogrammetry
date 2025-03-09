# Multi-Angular Photogrammetry Knowledge Compendium

This repository documents workflows and processing steps for multi-angular UAV image data using **Agisoft Metashape**. Its aim is to establish a **transparent and reproducible framework** for photogrammetry workflows, with a focus on multi-angular data analysis in plant disease studies.

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Repository Structure](#repository-structure)
3. [Documentation](#documentation)
4. [Getting Started](#getting-started)
5. [Contributing](#contributing)
6. [Contact](#contact)

---

## Project Overview

### Objectives
- **Document Key Processing Steps**: Provide a detailed guide for all major processes in Agisoft Metashape related to multi-angular UAV data.
- **Address Gaps in Transparency**: Highlight and resolve steps that are under-documented in Metashape’s official materials.
- **Recommend Alternatives**: Explore and suggest efficient algorithms or workflows.

### Expected Outcomes
- A well-documented repository featuring:
  - Comprehensive guides for Metashape processes.
  - Transparent alternatives for undocumented workflows.
  - Recommendations for optimizing multi-angular data processing.

---

## Repository Structure

```plaintext
Multi-Angular-Photogrammetry-master
├── .gitignore
├── README.md
├── algorithms
│   ├── feature_matching.md
│   └── SGM.md
├── data
│   ├── image.png
│   └── brandenburg_gate_images
│       └── jpg images
├── docs
│   ├── .gitkeep
│   ├── alignment.md
│   ├── camera.md
│   ├── DEM.md
│   ├── dense_cloud_generation.md
│   ├── exif_data_reading.md
│   ├── image_import.md
│   ├── mesh_creation.md
│   ├── orthomosiac_generation.md
│   ├── orthophoto_generation.md
│   ├── overall_workflow.md
│   ├── pixel_selection_strategies.md
│   ├── sparse_point_cloud.md
│   └── uav_multispectral_data.md
├── examples
│   └── .gitkeep
└── scripts
    ├── .gitkeep
    ├── feature_matching.ipynb
    └── orhomosaic_generation.ipynb
```

---

## Documentation

### Core Steps
- **Overall Workflow**: [overall_workflow.md](docs/overall_workflow.md) – High-level summary of the photogrammetry process.
- **Image Import**: [image_import.md](docs/image_import.md) – Guidance on importing UAV images into Metashape.
- **EXIF Data Reading**: [exif_data_reading.md](docs/exif_data_reading.md) – Verification and usage of metadata.
- **Alignment**: [alignment.md](docs/alignment.md) – Steps for aligning images to form a sparse point cloud.
- **Sparse Point Cloud**: [sparse_point_cloud.md](docs/sparse_point_cloud.md) – Details on generating the initial point cloud.
- **Dense Cloud Generation**: [dense_cloud_generation.md](docs/dense_cloud_generation.md) – Creating a detailed 3D point cloud.
- **Mesh Creation**: [mesh_creation.md](docs/mesh_creation.md) – Converting the dense cloud into a 3D mesh.
- **Orthophoto Generation**: [orthophoto_generation.md](docs/orthophoto_generation.md) – Creating georeferenced orthophotos.
- **Digital Elevation Model (DEM)**: [DEM.md](docs/DEM.md) – Steps to generate a DEM from processed data.

### Additional Documentation
- **Camera Details**: [camera.md](docs/camera.md) – Specifications of the multispectral camera.
- **UAV Multispectral Data**: [uav_multispectral_data.md](docs/uav_multispectral_data.md) – Key parameters of UAV-acquired data.
- **Pixel Selection Strategies**: [pixel_selection_strategies.md](docs/pixel_selection_strategies.md) – Approaches for selecting optimal pixels during processing.
- **Orthomosiac Generation**: [orthomosiac_generation.md](docs/orthomosiac_generation.md) – Alternative workflow for generating orthomosaics.

### Algorithms
- **Feature Matching**: [feature_matching.md](algorithms/feature_matching.md) – Discussion on feature matching techniques.
- **SGM (Semi-Global Matching)**: [SGM.md](algorithms/SGM.md) – Explanation and justification of the SGM algorithm.

---

## Getting Started

To start using this repository:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/tamer017/Multi-Angular-Photogrammetry.git
   cd Multi-Angular-Photogrammetry-master
   ```
2. **Explore the documentation:**  
   Detailed instructions for each processing step are available in the `docs/` directory.
3. **Test the workflows:**  
   Use the provided data in the `data/` folder and examples in the `examples/` directory.

---

## Contributing

We welcome contributions to enhance the repository! Here’s how you can contribute:

1. Fork the repository.
2. Create a new feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Make your changes and commit:
   ```bash
   git commit -m "Describe your changes here"
   ```
4. Push your branch to GitHub and create a pull request.

---

## Contact

For questions, feedback, or collaboration, please reach out:
- **Ahmed Tamer Assy**: [ahmed.assy@stud.uni-goettingen.de](mailto:ahmed.assy@stud.uni-goettingen.de)
- **Supervisor Dr. Rene Heim**: [heim@ifz-goettingen.de](mailto:heim@ifz-goettingen.de)


