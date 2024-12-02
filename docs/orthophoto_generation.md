# Orthophoto Generation

## Overview
Orthophotos are georeferenced images generated from the mesh or dense cloud. They are used for accurate 2D analysis.

## Steps
1. **Generate Orthophoto**:
   - Go to `Workflow > Build Orthomosaic`.
   - Choose the source data:
     - **Mesh**: For more detailed orthophotos.
     - **Dense Cloud**: If no mesh is available.
2. **Configure Parameters**:
   - **Projection**: Ensure the correct map projection is set (e.g., UTM or WGS84).
   - **Blending Mode**: Use "Mosaic" for the most natural results.
3. **Run the Process**:
   - Start the orthophoto generation.
   - Review and export the orthophoto in formats like `.tiff`.

## Notes
- Orthophotos are critical for GIS applications and precision agriculture.
- Use `Tools > Raster Calculator` for band-specific analyses in multispectral data.

