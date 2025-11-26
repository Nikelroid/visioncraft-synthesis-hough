
# VisionCraft: Hough Transform, Texture Synthesis & Image Completion

![Python](https://img.shields.io/badge/python-3.8%2B-blue?logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/numpy-1.24%2B-013243?logo=numpy&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-4.5%2B-red?logo=opencv&logoColor=white)



## Description
This repository contains advanced image processing algorithms implemented from scratch in Python. The project focuses on three distinct computer vision challenges: identifying geometric structures using the Hough Transform, generating large textures from small samples, and intelligently filling missing parts of images (inpainting).

The solutions utilize fundamental computer vision techniques such as gradient-based edge detection, accumulation matrices, template matching, and minimum error boundary cuts to achieve seamless visual results without relying on deep learning frameworks.


## Features
* **Hough Transform from Scratch:** A custom implementation of the Hough Transform to detect lines in polar coordinates without using `cv2.HoughLines`.
* **Chessboard Corner Detection:** An algorithm that filters Hough lines to specifically identify the grid structure and internal corners of a chessboard.
* **Texture Synthesis:** Generates high-resolution textures from small source patches using a patch-based approach.
* **Minimum Error Boundary Cut:** Implements a dynamic programming approach to find the optimal jagged path between overlapping image patches, ensuring seamless stitching.
* **Image Inpainting:** Fills holes or masked objects in images by synthesizing texture from the surrounding valid regions.
* **Gradient Blending:** Options for smooth transitions between synthesized patches.

## File Descriptions & Implementation Details

### 1. `q1.py` - Hough Transform & Corner Detection
This script implements the Hough Transform to detect lines and finds the corners of a chessboard.
* **Preprocessing:** Uses Canny edge detection (`cv2.Canny`) to create a binary edge map.
* **Accumulator:** Maps edge points to sinusoidal curves in the Hough Space ($\rho, \theta$). Intersections in this space represent lines in the Cartesian space.
* **Filtering:** Applies custom filters (`filter1`, `filter2`) to the accumulator matrix to suppress noise and merge close parallel lines, isolating the strongest grid lines.
* **Corner Finding:** Calculates the intersections of the detected vertical and horizontal lines to pinpoint the chessboard corners.

### 2. `q2.py` - Texture Synthesis
This script takes a small sample texture and expands it into a larger image.
* **Template Matching:** Uses `cv2.matchTemplate` to find the best matching patch in the original sample that aligns with the currently synthesized region.
* **Boundary Cut:** Instead of pasting patches as rectangles (which creates visible seams), this script calculates a "Minimum Error Boundary Cut." It finds a path through the pixels where the difference between the old and new patch is minimal (using a dynamic programming energy minimization approach similar to Seam Carving).
* **Synthesis Logic:** It fills the image row-by-row and column-by-column, blending the "L" shaped overlaps.


### 3. `q3.py` - Image Completion (Inpainting)
This script removes unwanted objects (masked in black) from an image and fills the hole plausibly.
* **Mask Detection:** Automatically identifies black rectangular regions (`[0,0,0]`) in the input image.
* **Patch Filling:** Adapts the texture synthesis logic from `q2.py`. It searches the valid parts of the image for patches that best match the boundary of the hole.
* **Smoothing:** Applies a post-processing smoothing filter to the boundaries of the filled region to remove high-frequency artifacts caused by the stitching process.

### 4. `Report.pdf`
A detailed report (in Persian) explaining the mathematical theory, algorithmic steps, and parameter choices (e.g., threshold values, accumulator size) used in the code.

## Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/nikelroid/image-processing-3.git](https://github.com/nikelroid/image-processing-3.git)
    cd image-processing-3
    ```

2.  **Install dependencies:**
    This project requires Python 3.x and the following libraries:
    ```bash
    pip install numpy opencv-python
    ```

## Usage

Ensure your input images are located in the same directory as the scripts or update the file paths within the code.

### Running Hough Transform (Q1)
Expects an input image (e.g., `im02.jpg`) containing a chessboard.
```bash
python q1.py
````

*Output:* Generates images showing the Canny edges, Hough space, detected lines, and final corners (`res09-corners.jpg`).

### Running Texture Synthesis (Q2)

Expects a small texture sample (e.g., `tx5.png`).

```bash
python q2.py
```

*Output:* Generates a large synthesized texture file (`res.jpg`). You can toggle the `gradient` variable in the script to `True` for smoother blending.

### Running Image Inpainting (Q3)

Expects an image with black holes/masks (e.g., `im04.png`).

```bash
python q3.py
```

*Output:* Generates the completed image (`res16.jpg`) with the black regions filled.

## Contributing

Contributions are welcome\! Please follow these steps:

1.  Fork the repository.
2.  Create a new branch (`git checkout -b feature/AmazingFeature`).
3.  Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4.  Push to the branch (`git push origin feature/AmazingFeature`).
5.  Open a Pull Request.

## License

This project is open-source and available under the **MIT License**.

## Contact

For questions regarding the algorithms or implementation, please open an issue in this repository.
