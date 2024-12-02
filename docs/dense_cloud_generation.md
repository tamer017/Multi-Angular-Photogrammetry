# Dense Cloud Generation

## Overview
Dense clouds are detailed 3D representations generated from the sparse point cloud and provide the foundation for mesh creation.

## Steps
1. **Generate Dense Cloud**:
   - Go to `Workflow > Build Dense Cloud`.
   - Configure the following parameters:
     - **Quality**: Choose "Medium" for speed, "High" for accuracy.
     - **Depth Filtering**: Use "Aggressive" for reducing noise.
2. **Run the Process**:
   - Start the dense cloud generation.
   - Save the project once the dense cloud is created.

## Notes
- Ensure the sparse point cloud is complete before generating a dense cloud.
- The dense cloud can be exported for external analysis (e.g., in `.las` format).
- Large datasets may require significant computational resources; consider using "Chunking" for large projects.

