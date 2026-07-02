# Week 01 - Task 1


## Objective

Learn the OpenCV image data model by loading an image, inspecting its properties, and comparing different image representations.


## Steps Performed

1. Loaded `input/chelsea.png` using `cv2.imread()`.
2. Saved `input/chelsea.png` as `output/chelsea.png` using `cv2.imwrite()`.
3. Converted the image from BGR to grayscale and saved it as `output/chelsea_gray.png`.
4. Printed:
   - Shape : (300, 451, 3)
   - Data Type : uint8
   - Minimum Pixel Value : 0
   - Maximum Pixel Value : 255
5. Compared BGR, RGB and Grayscale using Matplotlib.
6. Saved the comparison figure as `output/1_channels.png`.


## BGR vs RGB Observation

When the original OpenCV image is displayed directly using Matplotlib, the colors appear incorrect because OpenCV stores color channels in **Blue-Green-Red (BGR)** order, while Matplotlib expects **Red-Green-Blue (RGB)** order.

For the Chelsea cat image, this causes the cat's warm orange-brown fur to appear bluish or cyan-tinted, making the overall image look unnatural. After converting the image to RGB using `cv2.cvtColor()`, the fur returns to its natural orange-brown appearance, and the image displays correctly.


## Files Produced

- `task01.ipynb`
- `output/chelsea.png`
- `output/chelsea_gray.png`
- `output/1_channels.png`
- `WORKLOG.md`