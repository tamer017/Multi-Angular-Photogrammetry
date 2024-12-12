
## Workflow Explanation

1. **Input**: The script reads all `.tif` orthophotos from a directory.
2. **Processing**:
   - Combines the input rasters with varying **resampling** and **blending** methods.
   - Updates metadata (e.g., size, transform) based on the merged raster.
3. **Output**: Each resampling-blending combination is saved as a separate GeoTIFF file.
4. **Closing Datasets**: Ensures all resources are released properly.

## Comparison and Selection

- **Resampling** affects the visual quality and computational cost:
  - Use **nearest** for categorical data or large datasets needing quick results.
  - Use **bilinear** for smoother results without heavy computational cost.
  - Use **cubic** for the highest quality when detail matters.

- **Blending** affects how overlaps are resolved:
  - Use **first/last** for ordered priority.
  - Use **min/max** for emphasizing extremes.
  - Use **sum/count** for analytical purposes or visualization.

By generating outputs for all combinations, this script allows the user to compare results and choose the best approach for their specific application.

## Blending Formulas

1. **First**
   - **Formula**: 
     $$P_{\text{output}}(x, y) = P_{\text{first}}(x, y)$$
   - **Explanation**: The output pixel value at \((x, y)\) is taken directly from the first raster where data exists.

2. **Last**
   - **Formula**: 
     $$P_{\text{output}}(x, y) = P_{\text{last}}(x, y)$$
   - **Explanation**: The output pixel value at \((x, y)\) is taken from the last raster where data exists.

3. **Min**
   - **Formula**: 
     $$P_{\text{output}}(x, y) = \min(P_{1}(x, y), P_{2}(x, y), \dots, P_{n}(x, y))$$
   - **Explanation**: The output pixel value is the smallest value among overlapping pixels.

4. **Max**
   - **Formula**: 
     $$P_{\text{output}}(x, y) = \max(P_{1}(x, y), P_{2}(x, y), \dots, P_{n}(x, y))$$
   - **Explanation**: The output pixel value is the largest value among overlapping pixels.

5. **Sum**
   - **Formula**: 
     $$P_{\text{output}}(x, y) = \sum_{i=1}^{n} P_{i}(x, y)$$
   - **Explanation**: The output pixel value is the sum of all overlapping pixel values.

6. **Count**
   - **Formula**: 
     $$P_{\text{output}}(x, y) = n$$
   - **Explanation**: The output pixel value represents the number of overlapping layers contributing to that pixel.

## Why Max Blending Can Exceed Sum Blending

Under normal circumstances, the sum of pixel values should always be greater than or equal to the maximum pixel value for overlapping layers. However, there are specific scenarios where `max` blending appears to have higher pixel values than `sum` blending:

1. **No Overlap in Certain Areas**
2. **Normalization or Scaling in Inputs**
3. **Masking or No-Data Areas**
4. **Artifacts in Data Sources**

## Example

Consider two overlapping rasters:

| Raster 1 Pixel Value | Raster 2 Pixel Value | Sum | Max |
|-----------------------|----------------------|-----|-----|
| 100                   | 200                  | 300 | 200 |
| 150                   | NoData               | 150 | 150 |
| 50                    | 300                  | 350 | 300 |

- At \((1,1)\), `max` and `sum` are consistent because both rasters contribute.
- At \((2,1)\), the `max` is higher than the `sum` because only one raster contributes.
- At \((3,1)\), both methods show significant values, but `max` is limited to the highest contributor.

In this way, discrepancies between blending methods depend on overlap, anomalies, and data preparation.
