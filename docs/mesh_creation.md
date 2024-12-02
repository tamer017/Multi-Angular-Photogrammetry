# Mesh Creation

## Overview
A 3D mesh is created by connecting points in the dense cloud to form a surface representation of the scene.

## Steps
1. **Build Mesh**:
   - Navigate to `Workflow > Build Mesh`.
   - Choose the source data:
     - **Dense Cloud**: For more detail.
     - **Sparse Cloud**: For faster processing.
2. **Configure Parameters**:
   - **Surface Type**: Choose "Arbitrary" for complex shapes or "Height Field" for flat terrain.
   - **Face Count**: Adjust based on detail requirements and performance constraints.
3. **Run the Process**:
   - Start the mesh generation and save the project.

## Notes
- Meshes are essential for visualizing surfaces and creating orthophotos.
- Export meshes in formats like `.obj` or `.ply` for use in external software.
- Check for holes or artifacts in the mesh and refine settings if needed.

