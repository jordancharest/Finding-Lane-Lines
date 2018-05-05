# Finding Lane Lines
All code can be found in finding_lane_lines.ipynb

[//]: # (Image References)
[detected]: ./detected_lanes.png "Detected Lanes"
[extrapolated]: ./extrapolated_lanes.png "Extrapolated Lanes"

## Lane Detection Pipeline
The lane finding pipeline begins by converting the image to grayscale, applying Gaussian Smoothing, then performing Canny Edge Detection on the smoothed grayscale image. Everything outside of the region of interest (a trapezoidal region, centered vertically) is converted to black, and a Hough line transform is computed on the result. The result of these operations can be seen below, with the detected lines overlayed on the slightly transparent original image.

![alt text][detected]

The Hough line transform results in a vector of line segments, where each segment is denoted by two sets of x and y coordinates, the two endpoints. My pipeline sorts these points by rising x value, then divides the new points vector into 2 different vectors, choosing the minimum y value as the cutoff, and then throwing away a few points adjacent to the cutoff, to try to prevent overlap. This results in two vectors of points, one for each lane. The average slope of each lane is calculated, a unit vector is defined along each lane using their respective slopes, and points are appended to each of the respective vectors along the defined unit vector until the bottom of the image is reached, starting from the lowest point on the frame (highest y-value) that a line was detected.

This augmented vector of points is then fit with a quadratic polynomial and drawn on the image, resulting in the image below.

![alt text][extrapolated]

This process is repeated for every frame in a video. My average laptop can process the videos using this lane detector at an average of around 35 frames/second. The challenge video tends to process around 20-25 frames/second, but I have not profiled the code to see where the bottleneck occurs. Processed videos can be found [here.](./test_video_output)

## Pipeline Shortcomings and Possible Improvements
Although the above image looks well-fit, there are a few shortcomings with the pipeline, which you will notice when watching the video.
1) My method for dividing the lanes is not very robust. Occasionally there will be crossover between the two. An improvement here would mean developing a new method of dividing the lanes that is more robust. I avoided using slopes because, as you may see in the challenge video, lanes do not have a consistent slope, and the left lane does not always have negative slope and the right lane does not always have positive slope.

2) Fitting a quadratic polynomial tends to work very well when many line segments are detected, however, when the lane markers are sparse, it tends to diverge, sometimes significantly. I chose to use a quadratic fit because I was attempting to perform well on the challenge video, but that attempt was in vain. This sensitivity to the number of points manifests as noise/jitter in the highlighted lanes shown in the video. I think that including the points from the previous frame or two would significantly reduce the noise, and give much better output, since the lane does not change significantly from frame to frame. This would need to be implemented intelligently to not severely impact performance.

3) Better lane detection in shadows/variable lighting conditions is needed. In the challenge vidoe it is clear that the lanes are not being detected well on the white concrete road or under the shadows from the trees. In order to sovle this problem, I think the best course of action would be to implement color detection. First converting to HSV color space would be best to be able to handle color detection under variable lighting conditions. I initially avoided color detection since it appeared to have no effect on the easier conditions, but it later became cler that the pipline could benefit from using this technique.


Overall, the project was a great learning experience. I was a little bit disappointed in how noisy my lane detector turned out, but I think the above three suggestions would result in significant improvement.
