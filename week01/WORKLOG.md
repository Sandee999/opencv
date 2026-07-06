# Week 01 - Task 1


## Objective

Learn the OpenCV image data model by loading an image, inspecting its properties, and comparing different image representations.


## Steps Performed

1. Loaded `input/chelsea.png` using `cv2.imread()`.
2. Saved `input/chelsea.png` as `output/chelsea.png` using `cv2.imwrite()`.
3. Converted the image from BGR to grayscale and saved it as `output/chelsea_gray.png`.
4. Printed:
   - Shape : (300, 451, 3) *(height, width, channels )*
   - Data Type : uint8  *(each pixel value occupies 8 bits)*
   - Numerical value of each pixel ranges between 0–255
5. Compared BGR, RGB and Grayscale using Matplotlib.
6. Saved the comparison figure as `output/1_channels.png`.


## BGR vs RGB Observation

OpenCV stores color images in **Blue-Green-Red (BGR)** channel order, whereas Matplotlib expects images in **Red-Green-Blue (RGB)** order.

When a BGR image is displayed directly using `matplotlib.pyplot.imshow()`, the red and blue channels are interpreted incorrectly, causing the colors to appear unnatural. In the Chelsea cat image, the cat's orange-brown fur appears bluish or cyan-tinted.

After converting the image from BGR to RGB using `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)`, the channel order is corrected, and the image is displayed with its natural colors.

## Files Produced

- `task01.ipynb`
- `output/chelsea.png`
- `output/chelsea_gray.png`
- `output/1_channels.png`
- `WORKLOG.md`

---
---

# Week 01 - Task 2

## Objective

Learn OpenCV drawing functions, explore the HSV color space, and perform color-based segmentation using binary masks.

## Steps Performed

1. Created a 600×600 white canvas using NumPy.
2. Drew the following shapes using OpenCV drawing functions:
   - Filled red circle using `cv2.circle()`
   - Filled green square using `cv2.rectangle()`
   - Filled blue triangle using `cv2.fillPoly()`
   - Magenta arrow using `cv2.arrowedLine()`
   - Black diagonal line using `cv2.line()`
3. Saved the generated image as `output/shapes.png`.
4. Converted the image from BGR to HSV using `cv2.cvtColor()`.
5. Split the HSV image into Hue, Saturation, and Value channels using `cv2.split()`.
6. Combined the H, S, and V channels into a tiled image and saved it as `output/3_hsv.png`.
7. Created binary masks for the red circle, green square, and blue triangle using `cv2.inRange()`.
8. Combined the three masks into a tiled image and saved it as `output/4_masks.png`.
9. Verified that each mask contained between **8,000** and **30,000** non-zero pixels using `cv2.countNonZero()`.

## HSV Observation

The HSV color space separates **color information (Hue)** from **color intensity (Saturation)** and **brightness (Value)**.

- **Hue (H)** represents the actual color and makes it easier to distinguish different colored objects.
- **Saturation (S)** represents the purity or intensity of the color.
- **Value (V)** represents the brightness of each pixel.

Unlike the BGR color space, HSV allows colors to be segmented by defining ranges of Hue, Saturation, and Value, making it well suited for color detection and image segmentation.

## Color Mask Observation

Binary masks were generated using `cv2.inRange()` by specifying lower and upper HSV bounds for each color.

- Pixels whose HSV values fall within the specified range are assigned **255 (white)**.
- All other pixels are assigned **0 (black)**.

The generated masks successfully isolated the red circle, green square, and blue triangle while excluding the remaining shapes and background. The number of non-zero pixels in each mask satisfied the assignment requirement of **8,000–30,000** pixels.

## Files Produced

- `task02.ipynb`
- `output/shapes.png`
- `output/3_hsv.png`
- `output/4_masks.png`
- `WORKLOG.md`