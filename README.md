# Finding Lane Lines
All code can be found in finding_lane_lines.ipynb

[//]: # (Image References)
[detected]: ./detected_lanes.png "Detected Lanes"
[extrapolated] : ./extrapolated_lanes.png "Extrapolated Lanes"

## Lane Detection Pipeline
The lane finding pipeline begins by converting the image to grayscale, applying Gaussian Smoothing, then performing Canny Edge Detection on the smoothed grayscale image. Everything outside of the region of interest (a trapezoidal region, centered horizontally) is converted to black, and a Hough line transform is computed on the result. The result of these operations can be seen below, with the detected lines overlayed on the slightly transparent original image.

![alt text][detected]

The Hough line transform results in a vector of line segments denoted by two sets of x and y coordinates, the two endpoints. My pipeline sorts these points by rising x value, then divides the new points vector into 2 different vectors, choosing the minimum y value as the cutoff, and then throwing away a few points adjacent to the cutoff, to try to prevent overlap. This results in two vectors of points, one for each lane. The average slope of each lane is calculated, a unit vector is defined along each lane using their respective slopes, and points are appended to each of the respective vectors along the defined unit vector until the bottom of the image is reached, starting from the lowest point on the frame (highest y-value) that a line was detected.

This augmented vector of points is then fit with a quadratic polynomial, and drawn on the image, resulting in the image below.

![alt text][extrapolated]

