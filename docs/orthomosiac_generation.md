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

2. **Last**:
   - **Method**: Uses the last pixel encountered in the stack.
   - **Effect**: Similar to `first` but prioritizes the later datasets.
   - **Best for**: Datasets where newer imagery or specific layers take precedence.

3. **Min**:
   - **Method**: Uses the minimum pixel value among overlapping areas.
   - **Effect**: Can emphasize darker or lower-valued areas.
   - **Best for**: Identifying minimum values (e.g., lowest elevation in DEM).

4. **Max**:
   - **Method**: Uses the maximum pixel value among overlapping areas.
   - **Effect**: Highlights the brightest or highest values.
   - **Best for**: Applications like cloud height maps or emphasizing brighter features.

5. **Sum**:
   - **Method**: Adds pixel values from overlapping areas.
   - **Effect**: Produces cumulative results.
   - **Best for**: Applications like intensity mapping or pixel stacking analysis.

6. **Count**:
   - **Method**: Counts the number of overlapping layers for each pixel.
   - **Effect**: Outputs a layer showing coverage density.
   - **Best for**: Analyzing coverage overlap or data density.

---
