# Stereo-Camera-Calibration

## Project Overview

This project involves the calibration of a single camera and the creation of a stereo vision system. The tasks are divided into two main sections:

1. **Single Camera Calibration**
2. **Pipeline for Creating a Stereo Vision System**

### 1. Single Camera Calibration

**Task**: Perform single camera calibration using either a chessboard or circular pattern.

#### Method:

**A) Parameter Initialization:**

- **Square Chessboard Size**: 10x7 (10 squares by 7 squares)
- **Frame Size**: (640, 480) pixels
- **Termination Criteria**: 
  - **Maximum Iterations**: 30
  - **Minimum Accuracy**: 0.001
  - **Flags**: `cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER`
    - `cv2.TERM_CRITERIA_EPS`: Desired accuracy or change in parameters for stopping.
    - `cv2.TERM_CRITERIA_MAX_ITER`: Maximum number of iterations for stopping.
- **Size of Chessboard Square**: 20 mm

**B) Object Points:**

- **obj_p**: Zero array with shape (number of corners, 3) where the number of corners is chessboard_size[0] * chessboard_size[1] (70 corners), representing x, y, z coordinates in 3D space.
- **Meshgrid**: Created using `numpy.mgrid` for x and y coordinates, reshaped, and scaled according to the real size of the chessboard squares.

**C) List for Storing Points:**

- **objpts**: List for storing object points.
- **imgpts**: List for storing image points.

**D) Loading Images:**

- Using `cv2.imread()` to load all the required images.

**E) Iterating over Each Image:**

- **Loop Over Each Image**: Load each image in grayscale and attempt to find chessboard corners using `cv2.findChessboardCorners()`.
  - If corners are found, refine the image points using `cv2.cornerSubPix()` and add to `imgpts`. Add object points to `objpts`.

**F) Camera Calibration:**

- **cv2.calibrateCamera()**: Compute camera matrix, distortion coefficients, rotation, and translation vectors if both `objpts` and `imgpts` are non-empty.
  - Calculate reprojection errors using Euclidean distance between observed corners (`imgpts`) and projected points.

**G) Undistorting the Image and Visualization:**

- **Undistort an Image**: Calculate a new camera matrix optimized for the undistorted image using `cv2.getOptimalNewCameraMatrix()`. Undistort using `cv2.undistort()`.
  - Convert original and undistorted images to RGB, visualize with detected and reprojected points.
  - Create plots showing detected corners on the original image and reprojected points on the undistorted image using `matplotlib`.

**H) Error Analysis:**

- Generate a plot of reprojection errors across all images to evaluate calibration quality.

### 2. Pipeline for Creating a Stereo Vision System

Each dataset contains two images of the same scene captured from different camera perspectives, along with a `calib.txt` file detailing camera matrices, baseline, image size, and other parameters.

#### Calibration:

**a.** Identify matching features between the two images using feature matching algorithms.

**b.** Estimate the Fundamental matrix using the RANSAC method based on the matched features.

**c.** Compute the Essential matrix from the Fundamental matrix considering calibration parameters.

**d.** Decompose the Essential matrix into rotation and translation matrices.

#### Rectification:

**a.** Apply perspective transformation to rectify images and ensure horizontal epipolar lines.

**b.** Print the homography matrices (H1 and H2) for rectification.

**c.** Visualize epipolar lines and feature points on both rectified images.

#### Compute Depth Image:

**a.** Calculate the disparity map representing pixel-wise differences between the two images.

**b.** Rescale the disparity map and save it as grayscale and color images using heat map conversion.

**c.** Utilize the disparity information to compute depth values for each pixel.

**d.** Generate a depth image representing the spatial dimensions of the scene and save it as grayscale and color using heat map conversion for visualization.

#### OUTPUT: 
### 1) Perform single camera calibration using either a chessboard or circular pattern.
![Rerojected Points](https://github.com/Iaryan-21/Stereo-Camera-Calibration/blob/main/re_proje_img.png) 
### 2) Pipeline for Creating a Stereo Vision System
a) Epipolar Lines
![Epipolar Lines](https://github.com/Iaryan-21/Stereo-Camera-Calibration/blob/main/epi_lines.png)
b) Depth and Disparity Visualization
![disparity](https://github.com/Iaryan-21/Stereo-Camera-Calibration/blob/main/download.png)
