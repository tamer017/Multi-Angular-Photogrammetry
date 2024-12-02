# Alignment

## Overview
Alignment is the process of matching corresponding points across multiple images to generate a sparse point cloud.

## Steps
1. **Start Alignment**:
   - Navigate to `Workflow > Align Photos`.
   - Select the desired alignment parameters.
2. **Configure Parameters**:
   - **Accuracy**: Set to "High" or "Highest" (trade-off with speed).
   - **Key Point Limit**: Default is 40,000; adjust based on dataset size.
   - **Tie Point Limit**: Default is 4,000; lower for faster processing.
   - Enable **Adaptive Camera Model Fitting**.
3. **Run the Alignment**:
   - Start the alignment process and review the resulting sparse point cloud.

## Notes
- Proper alignment is critical for downstream processing (dense cloud, mesh).
- Misaligned points indicate insufficient overlap or poor image quality.
- Use the **Reference Pane** to verify alignment accuracy with GPS data.

