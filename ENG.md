---

# Project Analysis and Computer Vision

## Introduction
The project involves the use of computer vision for image analysis and task automation. One of the key elements is the processing of input data, particularly the application of filters, edge detection, and object recognition.

Each pixel of an image can be represented as an element of the matrix **I** *x, y*, where *x* and *y* are the coordinates of the pixel, and the value **I** *x, y* represents the intensity of color or brightness.

---

## Example Filters for Image Processing
Various kernels (filters) are used for image processing. The main types of filters are:

1. **Edge Detection**
2. **Sharpness**
3. **Blurring**

### 1. Edge Detection (Sobel Operator)
The Sobel operator is a differential filter used to detect edges in an image. It can be expressed using convolution:

![image](https://github.com/user-attachments/assets/925d8774-0f9b-4b3c-811f-139d1feb95e5)

Here, **G**_x_ and **G**_y_ are filters for computing gradients:

![image](https://github.com/user-attachments/assets/3b5bcff1-b4c7-4f8e-8742-708994d6b035)

This allows the detection of sharp intensity changes, thereby identifying object edges in the image.

Code for edge detection:

```python
import cv2

# Load image
image = cv2.imread("input.png")

# Convert to grayscale
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Detect edges
edges = cv2.Canny(gray_image, 50, 150)

# Save result
cv2.imwrite('edges_output.jpg', edges)
```

---

## Task Automation

To automate the process, we use algorithms based on OpenCV and Matplotlib libraries. The main steps are:

1. Read the image.
2. Convert to grayscale.
3. Perform edge detection.
4. Object segmentation.

### Code for Object Segmentation

Image segmentation involves isolating connected components. This can be done using the "flood-fill" method or finding components using a function:

Each contour is described as a set of coordinates *x*, *y* for the pixels belonging to one object.

```python
# Find contours
contours, _ = cv2.findContours(edges, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

# Draw contours
for contour in contours:
    x, y, w, h = cv2.boundingRect(contour)
    cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)

# Save result
cv2.imwrite('segmented_image.jpg', image)
```

---

## Generating Test Images

To test the algorithms, we can create custom image datasets. When generating test images, we can use equations to create geometric shapes.

This means that for each generated rectangle, the coordinates of the top-left and bottom-right corners are determined.

```python
import random
from PIL import Image, ImageDraw
import os

def random_color():
    return tuple(random.randint(0, 255) for _ in range(3))

def generate_test_images(num_images=10, img_size=(300, 300), output_folder="test_images"):
    if not os.path.exists(output_folder):
        os.makedirs(output_folder)

    for i in range(1, num_images + 1):
        img = Image.new("RGB", img_size, color=(255, 255, 255))
        draw = ImageDraw.Draw(img)

        for _ in range(random.randint(10, 50)):
            x1, y1 = random.randint(0, img_size[0] - 50), random.randint(0, img_size[1] - 50)
            x2, y2 = x1 + random.randint(10, 50), y1 + random.randint(10, 50)
            draw.rectangle([x1, y1, x2, y2], fill=random_color(), outline=random_color())

        img.save(os.path.join(output_folder, f"image_{i}.png"))

# Generate images
generate_test_images()
```

---

## Canny Parameters for Edge Detection

The Canny algorithm has two main parameters that affect the result:

- **Threshold1** (lower threshold): defines the minimum gradient intensity for an edge.
- **Threshold2** (upper threshold): defines the maximum gradient intensity for an edge.

The Canny algorithm uses the gradient to detect edges and applies two conditions to select significant edges.

### Example with Different Threshold Values

```python
# Change thresholds
edges_low = cv2.Canny(gray_image, 30, 100)
edges_high = cv2.Canny(gray_image, 100, 200)

# Save results
cv2.imwrite('edges_low.jpg', edges_low)
cv2.imwrite('edges_high.jpg', edges_high)
```

---

## Conclusion
Computer vision opens up numerous possibilities for image analysis, task automation, and improving process efficiency. The application of filters, segmentation algorithms, and visualization of results are key to the success of such projects. Mathematical methods such as gradient filters and convolution methods allow efficient extraction of useful information from images and applying it to solve real-world problems.

---
