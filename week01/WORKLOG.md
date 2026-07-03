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