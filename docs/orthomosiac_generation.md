### **Resampling Methods**
Resampling is applied when resizing or transforming raster data, adjusting how pixel values are interpolated. The script uses:

1. **Nearest**:
   - **Method**: Chooses the nearest pixel's value.
   - **Pros**: Fast, retains original values (important for categorical data like land cover).
   - **Cons**: Can result in blocky images for continuous data like elevation or imagery.
   - **Best for**: Categorical data or cases where preserving exact original values is essential.

2. **Bilinear**:
   - **Method**: Averages values of the four nearest pixels, weighted by distance.
   - **Pros**: Smooth transitions, good for continuous data.
   - **Cons**: Slightly slower, introduces interpolation artifacts in categorical data.
   - **Best for**: Satellite or aerial imagery with continuous data.

3. **Cubic**:
   - **Method**: Uses 16 nearest pixels to calculate a smooth curve through the data points.
   - **Pros**: High-quality results with smooth gradients.
   - **Cons**: Slowest, may introduce artifacts or smoothing over small, sharp details.
   - **Best for**: High-resolution imagery where detail and smooth gradients are important.

---

### **Blending Methods**
Blending determines how overlapping areas in input rasters are combined:

1. **First**:
   - **Method**: Uses the first pixel encountered in the stack.
   - **Effect**: Prioritizes the order of input datasets.
   - **Best for**: Situations where a primary data source is preferred.
   - **Formula**: 
     $$P_{\text{output}}(x, y) = P_{\text{first}}(x, y)$$
   - **Explanation**: The output pixel value at \((x, y)\) is taken directly from the first raster where data exists.

2. **Last**:
   - **Method**: Uses the last pixel encountered in the stack.
   - **Effect**: Similar to `first` but prioritizes the later datasets.
   - **Best for**: Datasets where newer imagery or specific layers take precedence.
   - **Formula**: 
     $$P_{\text{output}}(x, y) = P_{\text{last}}(x, y)$$
   - **Explanation**: The output pixel value at \((x, y)\) is taken from the last raster where data exists.

3. **Min**:
   - **Method**: Uses the minimum pixel value among overlapping areas.
   - **Effect**: Can emphasize darker or lower-valued areas.
   - **Best for**: Identifying minimum values (e.g., lowest elevation in DEM).
   - **Formula**:
     $$P_{\text{output}}(x, y) = \min(P_{1}(x, y), P_{2}(x, y), \dots, P_{n}(x, y))$$
   - **Explanation**: The output pixel value is the smallest value among overlapping pixels.

4. **Max**:
   - **Method**: Uses the maximum pixel value among overlapping areas.
   - **Effect**: Highlights the brightest or highest values.
   - **Best for**: Applications like cloud height maps or emphasizing brighter features.
   - **Formula**:
     $$P_{\text{output}}(x, y) = \max(P_{1}(x, y), P_{2}(x, y), \dots, P_{n}(x, y))$$
   - **Explanation**: The output pixel value is the largest value among overlapping pixels.

5. **Sum**:
   - **Method**: Adds pixel values from overlapping areas.
   - **Effect**: Produces cumulative results.
   - **Best for**: Applications like intensity mapping or pixel stacking analysis.
   - **Formula**:
     $$P_{\text{output}}(x, y) = \sum_{i=1}^{n} P_{i}(x, y)$$
   - **Explanation**: The output pixel value is the sum of all overlapping pixel values.

6. **Count**:
   - **Method**: Counts the number of overlapping layers for each pixel.
   - **Effect**: Outputs a layer showing coverage density.
   - **Best for**: Analyzing coverage overlap or data density.
   - **Formula**:
     $$P_{\text{output}}(x, y) = n$$
   - **Explanation**: The output pixel value represents the number of overlapping layers contributing to that pixel.

---
### **Example**
![Screenshot 2024-12-11 013853](https://github.com/user-attachments/assets/220ae2ae-3c13-482b-8e50-f3b7bb6d8e78)

For detailed steps, refer to [Orthomosiac Generation](https://github.com/tamer017/Multi-Angular-Photogrammetry/blob/master/docs/dem.md).

---
#### **Resampling Happens Before Blending**
Resampling is applied to adjust the resolution or alignment of individual input rasters. If the input rasters are already perfectly aligned and match the target resolution, resampling becomes redundant. The merge process simply applies the blending method to combine the input rasters, leaving the outputs identical.
