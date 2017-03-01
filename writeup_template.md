#**Finding Lane Lines on the Road** 
Andrew Parker

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 11 steps in the function called `pipeline1()`.

1. Convert the image to HSV. This makes creating masks for different colors easier.
2. Create a mask for the white pixels.
3. Create a mask for the yellow pixels.
4. Create a combined mask by taking the bitwise OR for the white and yellow masks.
5. Apply the color mask to the original input image.
6. Convert the masked image to grayscale.
7. Apply a slight Gaussian blur.
8. Apply a region mask in the shape of a trapezoid.
9. Apply Canny Edge Detection.
10. Draw interpolated and extrapolated lane lines after running the image through Hough Lines. More on this below. 
11. Combine the final lane lines with the original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by adding the following processing steps:

1. Take the `lines` parameter, which is a single collection of lines, and separate it into two buckets: Left Lines and Right Lines. This is determined purely by looking at the slope of the line. Lines with a positive slope are put into the Left Lines bucket, and otherwise they are put into the Right Lines bucket. (Actually, the slopes have opposite signs, due to the way the pixel coordinates are represented in the image, with increasing values of Y as you go down the image.)

2. For each line in each bucket of lines, extend that line and calculate the coordinates where it intersects the two horizontal lines that correspond to the top and bottom of the trapezoid used in the region mask. We use the helper function `calc_avg_x` which takes a collection of a lines and the Y coordinate for a horizontal line, and outputs the average value of the X coordinate of where the collection of lines (extrapolated out) intersect the given horizontal line.

3. (For movies only): If we are drawing lanes for a movie, then we use the coordinates derived from a moving average of the last several frames. The average is based on the intercept coordinates of lane lines for a given image. Also, I filter out "bad" coordinates that would cause the lane lines to go outside a defined region. 

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
