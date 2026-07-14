# Week 02 - Task 1


## Objective

Preparing image images required for *Feature detection*.


## Steps Performed

1. Saved the standard grayscale image `camera.png` using `skimage.data.camera()`.
   - Image size: **512 × 512**
   - Data type: **uint8**

2. Saved the standard grayscale image `coins.png` using `skimage.data.coins()`.
   - Image size: **303 × 384**
   - Data type: **uint8**

3. Captured a real-world image `input/personal_edge.png`.
   - Selected an object containing strong linear structures (building facade / bookshelf / window blinds).
   - Saved the image for testing edge detection and Hough Line Transform on natural scenes.

## Files Produced

- `task01.ipynb`
- `input/personal_edge.png`
- `output/camera.png`
- `output/coins.png`

---
---

# Week 02 - Task 2

## Objective

Generate a image containing line segments with 5 angles ({0°, 15°, 30°, 60°, 90°}), 3 lengths ((50, 120, 250)), 3 thicknesses ({1, 2, 4}). Record the coordinates of every generated line.


## Steps Performed

1. Created an **800 × 600** white canvas using NumPy.
2. Generated endpoints of each line across:
   - **5 orientations:** 0°, 15°, 30°, 60°, and 90°
   - **3 lengths:** 50 px, 120 px, and 250 px
   - **3 thicknesses:** 1 px, 2 px, and 4 px
3. Generated **30 line segments** using OpenCV's `cv2.line()` function.
4. Saved the generated image as **`lines_grid.png`**.
5. Stored the ground-truth information for every line in **`lines_grid_gt.json`**, including:
   - `start`: `(x1, y1)`
   - `end`: `(x2, y2)`
   - `angle`: `<degrees>`
   - `length`: `<pixels>`
   - `thickness`: `<int>`


## Files Produced

- `task02.ipynb`
- `output/lines_grid.png`
- `output/lines_grid_gt.json`
- `WORKLOG.md`

---
---

# Week 02 - Task 3

## Objective

Applied three smoothing filters (Gaussian Blur, Bilateral Filter, and Median Blur) with kernel sizes 3, 7, and 15 on `input/personal_edge.png`.


## Steps Performed

1. Loaded `input/personal_edge.png` using `cv2.imread()`.
2. Applied the three smoothing filters using `results.append()`
3. Compared the filters using Matplotlib.
4. Saved the comparison figure as `output/2_smoothing.png`.

**Which preserves edges best?**

- **Bilateral Filter** preserves edges the best.
- It smooths regions with similar intensities while preventing smoothing across strong intensity changes, so building edges remain sharp even with larger kernel sizes.

**Which removes fine noise best?**

- **Gaussian Blur** removes fine random noise most effectively.
- However, increasing the kernel size also causes noticeable edge blurring.

### Additional Notes

- **Median Blur** is especially effective for salt-and-pepper noise and preserves edges better than Gaussian Blur, but it may remove small image details at larger kernel sizes.
- As the kernel size increases from **3 → 15**, all filters produce stronger smoothing, with Gaussian causing the greatest loss of detail and Bilateral maintaining the clearest structural edges.


## Files Produced

- `task03.ipynb`
- `output/2_smoothing.png`
- `WORKLOG.md`

---
---

# Week 02 - Task 4

## Objective

Apply binary thresholding using four fixed threshold values (**80, 120, 160, 200**) and **Adaptive Gaussian Thresholding** on `camera.png`. Compare how different threshold values affect image segmentation.

## Steps Performed

1. Loaded the grayscale image `output/camera.png` using `cv2.imread()`.
2. Applied **Binary Thresholding** with threshold values:
   - **80**
   - **120**
   - **160**
   - **200**
3. Applied **Adaptive Gaussian Thresholding** using:
   - **Block Size:** 21
   - **Constant (C):** 5
4. Displayed all thresholded outputs side-by-side using Matplotlib.
5. Saved the comparison figure as `output/3_threshold.png`.

## Observations

### Threshold = 80
- Produces a brighter binary image.
- Many background pixels become white, reducing separation between foreground and background.

### Threshold = 120
- Provides better foreground-background separation.
- Preserves more object details while reducing unwanted background.

### Threshold = 160
- Removes more low-intensity pixels.
- Dark regions become more prominent, resulting in clearer object segmentation.

### Threshold = 200
- Produces the strictest segmentation.
- Only the brightest regions remain white, causing loss of finer image details.

### Adaptive Gaussian Thresholding
- Computes a threshold value for each local neighborhood.
- Handles uneven illumination much better than fixed thresholding.
- Preserves local details even when image brightness varies across different regions.

## Conclusion

- Lower threshold values classify more pixels as foreground, producing brighter binary images.
- Higher threshold values retain only high-intensity regions while removing weaker details.
- **Adaptive Gaussian Thresholding** performs best under varying lighting conditions because it computes thresholds locally rather than using a single global threshold.

## Files Produced

- `task04.ipynb`
- `output/3_threshold.png`
- `WORKLOG.md`

---
---

# Week 02 - Task 5

## Objective

Apply the Canny edge detector using different low and high threshold pairs to observe how the thresholds affect edge detection. The task also explores the role of `apertureSize=3`, which specifies the Sobel kernel size used for gradient computation.

## Effect of Different Thresholds

| Low | High | Observation |
|------|-------|-------------|
|30|100|Detects many edges, including noise and weak edges.|
|50|150|Balanced result with good edge coverage.|
|80|200|Suppresses weak edges while preserving major structures.|
|100|300|Only strong object boundaries remain.|
|150|400|Very strict detection; only the strongest edges survive.|

# What does `apertureSize=3` do?

`apertureSize` specifies the size of the Sobel kernel used to compute image gradients inside the Canny algorithm.

With

```python
cv.Canny(image, low, high, apertureSize=3)
```

## Files Produced

- `task05.ipynb`
- `output/4_canny.png`
- `WORKLOG.md`

---
---

# Week 02 – Task 6

## Objective

Generate a binary image by thresholding `camera.png` at **120**, then apply the following morphological operations using rectangular kernels of sizes **2×2**, **3×3**, and **5×5**:

* Erosion
* Dilation
* Opening
* Closing

Save the resulting **4 × 3 mosaic** as `5_morphology.png`.

## Prediction (Before Execution)

### Thresholding

Applying a threshold of **120** will convert the grayscale image into a binary image:

* Pixel intensity > 120 → White (255)
* Pixel intensity ≤ 120 → Black (0)

This binary image will be used as the input for all morphological operations.

### Expected Results

| Operation                        | Expected Effect |
| -------------------------------- | -----------------|
| **Erosion** | White foreground objects will shrink. Thin structures and small white regions may disappear as the kernel size increases.|
| **Dilation** | White foreground objects will expand. Small gaps may close, and nearby objects may merge for larger kernels. |
| **Opening (Erosion → Dilation)** | Small white noise and isolated foreground pixels will be removed while preserving the overall shape of larger objects. Larger kernels will remove more fine details. |
| **Closing (Dilation → Erosion)** | Small black holes and narrow gaps inside foreground objects will be filled. Larger kernels will produce smoother object boundaries and connect nearby regions. |

### Effect of Kernel Size

| Kernel Size | Prediction                                                                                                                              |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **2×2** | Mild morphological changes with most image details preserved. |
| **3×3** | Moderate shrinking/expansion and improved removal of small artifacts. |
| **5×5** | Strongest effect. Fine structures may disappear after erosion/opening, while dilation/closing may noticeably enlarge and merge objects. |


## Observations (After Execution)

* **Erosion** progressively reduced the size of white foreground regions as the kernel size increased.
* **Dilation** expanded foreground objects and filled small gaps, with stronger effects for larger kernels.
* **Opening** successfully removed isolated white noise while preserving the major structures. Larger kernels removed additional fine details.
* **Closing** filled small holes and connected narrow breaks within foreground regions, producing smoother and more continuous shapes.
* The **5×5 kernel** produced the strongest morphological changes, whereas the **2×2 kernel** preserved most of the original object details.


## Conclusion

The experiment demonstrates how the size of the structuring element influences morphological processing. Smaller kernels perform subtle modifications, while larger kernels increasingly alter object boundaries, remove fine details, fill gaps, and smooth binary shapes. The generated mosaic (`5_morphology.png`) clearly illustrates the effect of each operation across different kernel sizes.

## Files Produced

- `task06.ipynb`
- `output/5_morphology.png`
- `WORKLOG.md`

---
---

# Week 02 - Task 7

## Objective

Detect and count coins in `coins.png` using contour detection. The processing pipeline consists of:

1. Convert the image to grayscale.
2. Apply binary thresholding.
3. Perform morphological closing to remove small gaps and connect broken boundaries.
4. Detect external contours.
5. Filter contours based on area to remove noise.
6. Count the detected coins and verify that the result is within **24 ± 2**.
7. Draw the detected contours and save the output as `6_contours.png`.

## Implementation

1. Loaded `coins.png`.
2. Converted the image to grayscale.
3. Applied binary thresholding with a threshold value of **140**.
4. Applied morphological closing using a **6×6 elliptical structuring element**.
5. Detected external contours using:

   * Retrieval mode: `RETR_EXTERNAL`
   * Approximation method: `CHAIN_APPROX_SIMPLE`
6. Filtered contours with an area greater than **50 pixels**.
7. Counted the remaining contours.
8. Verified that the detected count lies within **24 ± 2**.
9. Drew the detected contours on the original image.
10. Saved the final result as `output/6_contours.png`.


## Observations (After Execution)

* Binary thresholding successfully separated the coins from the background.
* Morphological closing removed small gaps along coin boundaries, resulting in cleaner contours.
* External contour retrieval detected one contour for each visible coin.
* Area filtering effectively eliminated small noisy regions.
* The detected coin count satisfied the expected range (**24 ± 2**).
* The generated output image clearly highlighted each detected coin using green contours.


## Conclusion

Contour detection combined with morphological preprocessing provides a simple and effective method for object counting. Thresholding isolates the foreground, morphological closing improves object connectivity, and contour area filtering removes noise, resulting in accurate detection and counting of the coins.

## Files Produced

- `task06.ipynb`
- `output/6_contours.png`
- `WORKLOG.md`

___
___

# Week 02 - Task 8

## Objective

Compare the performance of the Hough Line Transform (`HoughLinesP`) and the Line Segment Detector (LSD) for detecting line segments in a synthetic image containing 30 ground-truth lines.


## Steps Performed

1. Loaded the generated line image `output/lines_grid.png`.
2. Converted the image to grayscale using `cv2.cvtColor()`.
3. Applied the Canny edge detector with thresholds:
   - Lower Threshold: **50**
   - Upper Threshold: **250**
4. Detected line segments using the Probabilistic Hough Transform (`cv2.HoughLinesP`) with:
   - `rho = 1`
   - `theta = 1°`
   - `threshold = 30`
   - `minLineLength = 40`
   - `maxLineGap = 10`
5. Drew all detected Hough line segments on a blank white canvas.
6. Detected line segments using OpenCV's Line Segment Detector (`cv2.createLineSegmentDetector()`).
7. Drew all detected LSD segments on another blank white canvas.
8. Compared:
   - Original image (30 ground-truth lines)
   - HoughLinesP detections
   - LSD detections
9. Saved the comparison figure as `output/7_hough_lsd.png`.
10. Printed the number of detected line segments.


## Observations

The original synthetic image contains **30 line segments**, while the detectors reported:

- Ground Truth Lines : **30**
- HoughLinesP : **59**
- LSD : **85**

Both algorithms detect **edge-based line segments** rather than the original geometric lines.

For thicker lines, the Canny edge detector produces two parallel edges corresponding to the two boundaries of each line. As a result, **HoughLinesP frequently detects duplicate parallel segments** for a single drawn line. Long lines may also be divided into multiple shorter segments depending on the detected edge continuity.

The **Line Segment Detector (LSD)** is more sensitive and detects additional short and fragmented segments. It often splits a single physical line into multiple smaller segments and detects both boundaries of thick lines, resulting in a higher number of detections.

Therefore, the detected line counts are higher than the ground truth and illustrate that both methods typically require **post-processing** (such as merging nearby, collinear, or overlapping segments) to recover the original set of line segments.


## Files Produced

- `task07.ipynb`
- `output/7_hough_lsd.png`
- `WORKLOG.md`