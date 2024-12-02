# Sparse Point Cloud Generation

## Overview
Sparse point clouds represent the initial alignment of images based on their overlapping areas and EXIF data.

## Steps
1. **Align Photos**:
   - Go to `Workflow > Align Photos`.
   - Choose alignment parameters:
     - **Accuracy**: Set to "High" or "Highest" for better results.
     - **Key Point Limit**: Default is 40,000; adjust if necessary.
     - **Tie Point Limit**: Default is 4,000; adjust if necessary.
2. Review the generated sparse point cloud for completeness.

## Notes
- Ensure EXIF data is accurate before running alignment.
- Sparse point clouds are critical for generating dense clouds and meshes.

