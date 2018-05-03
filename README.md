# Finding Lane Lines
All code can be found in finding_lane_lines.ipynb

[//]: # (Image References)
[detected]: ./detected_lanes.png "Detected Lanes"

## Lane Detection Pipeline
The lane finding pipeline begins by converting the image to grayscale, applying Gaussian Smoothing, then performing Canny Edge Detection on the smoothed grayscale image. Everything outside of the region of interest (a trapezoidal region, centered horizontally) is converted to black, and a Hough line transform is computed on the result. The result of these operations can be seen below, with the detected lines overlayed on the slightly transparent original image.

![alt text][detected]

The Hough line transform
