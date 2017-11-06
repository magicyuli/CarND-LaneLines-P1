# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[image1]: ./writeup/region.png "Region of interest"

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.
1. Convert image to gray scale
2. Apply Gaussian Blur with kernel size equal to 5
3. Apply Canny Edge Detection
4. Compute the vertices of region of interest base on the shape of the image. I'm using an inverted "V" shaped region to better eliminate noises.

   ![Region of interest][image1]
5. Mask the edges image to just keep the region of interest
6. Apply Hough Transformation to the masked image with parameters (rho = 1, theta = np.pi/360, threshold=20, min_line_len=20, max_line_gap=5) and get back a set of lines
7. Filter out the lines by slopes, since lines with certain slopes are not likely to be part of lane lines
8. Cluster the remaining lines by slopes and distances and pick one cluster with positive slope and one cluster with negative slope, both with largest sums of lengths of lines
9. Compute the average slopes and intercepts for those two lines (weighted by lengths of lines), and compute the end points of them.
10. Draw the two lines on top of the original image.


### 2. Identify potential shortcomings with your current pipeline

Although I've made a lot effort to make the whole algorithm as general as possible, e.g. image size and environment agnostic, but it's still making many assumptions (although not very strong), and I can imagine situations where it won't work well, e.g. when there are other lines parellel and close to the lane lines, or the color of lane lines are close to the color of the road, or even a very different view point of the camera.


### 3. Suggest possible improvements to your pipeline

More sophisticated computer vision or object detection with machine learning probably will help here, for example to find more accurate edges of the region of interest, or make the lane lines contrast more to the road. Also some CNN might make the algorithm camera view point agnostic.
