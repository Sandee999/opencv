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
