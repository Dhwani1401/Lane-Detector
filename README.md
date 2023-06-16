# Lane-Detector
## Introduction
This project employs the OpenCV library to detect lanes as viewed through a vehicle camera. While it is handy for straight lane detections, this cannot be applied to curvature lanes. The entire building process can be broken down into four processes. This project was done with the help of an online tutorial provided by David Chuan-En Lin at https://towardsdatascience.com/tutorial-build-a-lane-detector-679fd8953132.
## Brief Break-Down of the Code
### 1. Canny Detector- Multi-Stage Edge Detection Algorithm
This corresponds to the do_canny function, which can be applied directly with the OpenCV library. Here is a brief explanation of the algorithm- It can be broken into four segments:
#### A. Noise Reduction
Noise may lead to false detection. Thus, a 5x5 Gaussian Filter is applied to smoothen the image to lower the detector's sensitivity to the noise. This is done using a kernel containing normally distributed numbers to run across the entire image, setting each pixel value equal to the weighted average of its neighboring pixels.
#### B. Intensity Gradient
The smoothened image is then applied with a Sobel kernel along the x and y axes to detect whether the edges are horizontal, vertical, or diagonal.
#### C. Non-Maximum Suppression
The NMS is applied to thin out and sharpen the edges. The algorithm goes through all the points on the gradient intensity matrix and finds the pixels with the maximum value in the edge directions. If the specific point is a maximum, non-maximum suppression is applied to the next point in the same edge direction. Otherwise, the pixel value of that point is set to zero, and it is suppressed.
#### D. Hysteresis Thresholding
The non-maximum suppression confirms the strong pixels in the final mapped image. However, we must analyze the weak pixels to determine whether they constitute edge or noise. We select two pre-defined threshold values- minVal and maxVal. We set that any pixel with an intensity gradient higher than maxVal are edges, and those lower than minVal are noise. Further, pixels with intensity gradient values between minVal and maxVal are only considered edges if they are connected to a pixel with an intensity gradient above maxVal.
### 2. Segmenting Lane Area
We establish a triangular mask to segment the lane area and discard the irrelevant areas in the frame for better detection, i.e., implementing the "do_segment" function.
### 3. Hough Transform
We can represent a straight line as a single point in Hough Space by plotting b against m for a straight line given as
$$𝑦=𝑚𝑥+𝑏$$
 The point of intersection in Hough space represents the m and b values that pass through all the points in the series.
Our frame passed through the Canny Detector may be interpreted simply as a series of white points representing the edges in our image space. We can identify which of these points are connected to the same line, and if they are connected, what its equation would be to plot it in the Hough frame.
We use Polar coordinates to eliminate the infinite gradient problem, and thus, we will plot 'r' against $\theta$.
 
For our implementation, we will define a minimum threshold number of intersections in Hough space to detect a line. If the number of intersections exceeds a defined threshold, we identify a line with the corresponding $\theta$ and r parameters.
We apply Hough Transform to identify two straight lines- the left and right lane boundaries.
 ### 4. Visualization
 The lane is visualized as two green, linearly fitted polynomials which will be overlayed on our input frame.
 


