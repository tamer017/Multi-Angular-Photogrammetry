# Feature Matching and Image Alignment Evaluation

This repository provides a comprehensive framework for evaluating various feature detectors and matching algorithms on a subset of the Oxford Buildings dataset. In our experiments, we focus on the `all_souls` image series (ranging from `all_souls_000000.jpg` to `all_souls_000220.jpg`) and compare detectors such as **SIFT**, **ORB**, **AKAZE**, **BRISK**, and **KAZE** using both **Brute-Force (BF)** and **FLANN-based** matchers.

Our evaluation metrics include:
- **Number of Matches:** The number of valid correspondences detected between image pairs.
- **Reprojection Error:** The average error (in pixels) when mapping keypoints using the computed homography.

---

## Table of Contents

- [Introduction](#introduction)
- [Definitions and Formulas](#definitions-and-formulas)
- [Methodology](#methodology)
  - [Feature Detectors](#feature-detectors)
  - [Feature Matchers](#feature-matchers)
  - [Homography and Reprojection Error](#homography-and-reprojection-error)
  - [Handling Unknown Overlap](#handling-unknown-overlap)
- [Experimental Results](#experimental-results)
  - [Aggregated Summary](#aggregated-summary)
  - [Conclusion](#conclusion)
- [Usage](#usage)
  - [Script Execution](#script-execution)
  - [Data Samples and Links](#data-samples-and-links)
- [References](#references)
- [License](#license)

---

## Introduction

In many computer vision tasks such as panoramic stitching, 3D reconstruction, and object recognition, robust feature matching and image alignment are critical. However, when the exact overlap between images is unknown, a dynamic and robust evaluation method is required. This project evaluates several popular feature detectors and matchers on the Oxford Buildings dataset—specifically on the `all_souls` series—to determine the optimal combination for reliable image alignment.

---

## Definitions and Formulas

### Key Terms

- **Feature Detector:** An algorithm used to identify salient points (keypoints) in an image. Examples include SIFT, ORB, AKAZE, BRISK, and KAZE.
- **Descriptor:** A vector that encodes the local appearance around a keypoint.
- **Matcher:** An algorithm that compares descriptors between images to establish correspondences.
- **Homography:** A 3×3 matrix$$H$$ that maps points from one plane (image) to another, typically used to align two images.
- **Reprojection Error:** A quantitative measure of alignment accuracy, calculated as the average distance between keypoints in the second image and the projected keypoints from the first image using the homography$$H$$.

### Formula for Reprojection Error

Given$$N$$ inlier matches, where$$\mathbf{p}_i$$ are keypoints in the first image and$$\mathbf{p}'_i$$ are their corresponding points in the second image, the average reprojection error is defined as:

$$
\text{Average Error} = \frac{1}{N} \sum_{i=1}^{N} \left\| \mathbf{p}'_i - H \mathbf{p}_i \right\|
$$

---

## Methodology

### Feature Detectors

1. **SIFT (Scale-Invariant Feature Transform):**  
   - *Description:* Detects keypoints invariant to scale, rotation, and moderate illumination changes.  
   - *Advantages:* High robustness and repeatability; widely used in many applications.  
   - *Disadvantages:* Computationally intensive.

2. **ORB (Oriented FAST and Rotated BRIEF):**  
   - *Description:* Combines the FAST detector with a modified BRIEF descriptor (incorporating orientation).  
   - *Advantages:* Fast and computationally efficient; free and open-source.  
   - *Disadvantages:* May yield fewer or less distinctive matches.

3. **AKAZE:**  
   - *Description:* Utilizes nonlinear diffusion filtering for keypoint detection and computes binary descriptors.  
   - *Advantages:* Robust to noise and offers good performance with binary descriptors.  
   - *Disadvantages:* Results can be variable across different datasets.

4. **BRISK (Binary Robust Invariant Scalable Keypoints):**  
   - *Description:* Detects keypoints and computes binary descriptors that are invariant to scale and rotation.  
   - *Advantages:* Very high match density and speed.  
   - *Disadvantages:* Matching precision can be lower than with methods like SIFT.

5. **KAZE:**  
   - *Description:* Operates in nonlinear scale space to detect keypoints and compute descriptors, similar to AKAZE but without linear approximations.  
   - *Advantages:* Good balance between match density and alignment accuracy.  
   - *Disadvantages:* More computationally expensive than binary methods.

### Feature Matchers

- **Brute-Force (BF) Matcher:**  
  - *Description:* Matches each descriptor in one image with all descriptors in another using a cross-check for symmetry.  
  - *Advantages:* Simple and typically yields a high number of matches.  
  - *Disadvantages:* Slower for very large descriptor sets.

- **FLANN (Fast Library for Approximate Nearest Neighbors):**  
  - *Description:* Uses an approximate nearest neighbor search combined with Lowe's ratio test to filter matches.  
  - *Advantages:* Faster than BF on large datasets.  
  - *Disadvantages:* For binary descriptors, FLANN may be overly conservative, sometimes resulting in no valid matches.

### Homography and Reprojection Error

After matching, the keypoint correspondences are used to compute a homography$$H$$ using the RANSAC algorithm. The reprojection error, as defined above, quantifies the alignment accuracy.

### Handling Unknown Overlap

Since the exact overlap between images is unknown, the evaluation is performed on every consecutive image pair (filtered by the prefix `all_souls`). By checking the number of matches and reprojection error for each pair, we can:
- Determine whether the overlap is sufficient.
- Filter out pairs with inadequate overlap.
- Adjust thresholds dynamically based on actual data.

---

## Experimental Results

### Aggregated Summary

After processing 131 image pairs (from the `all_souls` series), the aggregated results are:

| Detector | Matcher | # Pairs Processed | Avg. Reprojection Error (pixels) | Avg. Number of Matches |
|----------|---------|-------------------|----------------------------------|------------------------|
| SIFT     | BF      | 131               | 8.03                             | 882.4                  |
| SIFT     | FLANN   | 131               | 18.34                            | 66.4                   |
| ORB      | BF      | 131               | 14.75                            | 248.2                  |
| ORB      | FLANN   | 0                 | -                                | -                      |
| AKAZE    | BF      | 131               | 3.61                             | 669.4                  |
| AKAZE    | FLANN   | 0                 | -                                | -                      |
| BRISK    | BF      | 131               | 6.64                             | 1646.4                 |
| BRISK    | FLANN   | 0                 | -                                | -                      |
| KAZE     | BF      | 131               | 2.77                             | 585.2                  |
| KAZE     | FLANN   | 127               | 8.21                             | 38.0                   |

### Conclusion

- **BF Matching Outperforms FLANN:**  
  BF matching produced a significantly higher number of valid matches and lower reprojection errors across all detectors.
  
- **Best Performing Combinations (with BF):**  
  - **KAZE with BF**: Lowest average reprojection error (2.77 pixels) with a moderate match count.
  - **AKAZE with BF**: Low reprojection error (3.61 pixels) with a robust number of matches.
  - **BRISK with BF**: Very high number of matches, though the error is moderately higher (6.64 pixels).
  - **SIFT with BF**: High number of matches but higher reprojection error (8.03 pixels).
  - **ORB with BF**: Fewer matches and higher error, making it less suitable for this dataset.
  
- **FLANN Matching Limitations:**  
  FLANN matching was too conservative for binary descriptors (ORB, AKAZE, BRISK), leading to zero valid pairs. Even for SIFT and KAZE, FLANN produced significantly fewer matches and higher errors.

**Overall Recommendation:**  
For images in the `all_souls` series, **BF matching is preferable**. Among the detectors, **KAZE and AKAZE with BF matching** provide the best balance between match density and alignment accuracy.

---

## Usage

### Script

The main processing script is available at:  
[link-to-your-script.py](https://github.com/yourusername/your-repo/blob/main/script.py)

### Data Samples

The dataset used in this evaluation is the Oxford Buildings dataset (filtered to only include images with the prefix `all_souls`). You can download the dataset from:  
[Oxford Buildings Dataset](http://www.robots.ox.ac.uk/~vgg/data/oxbuildings/oxbuild_images.tgz)

---

## References

1. Lowe, D. G. "Distinctive Image Features from Scale-Invariant Keypoints." *International Journal of Computer Vision*, 2004.
2. Rublee, E., Rabaud, V., Konolige, K., & Bradski, G. "ORB: an efficient alternative to SIFT or SURF." 2011.
3. Alcantarilla, P. F., Nuevo, J., & Bartoli, A. "Fast explicit diffusion for accelerated features in nonlinear scale spaces." BMVC, 2012.
4. Brown, M., & Lowe, D. G. "Automatic panoramic image stitching using invariant features." *International Journal of Computer Vision*, 2007.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
