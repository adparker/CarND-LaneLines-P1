#**Finding Lane Lines on the Road** 
Andrew Parker
March 1st, 2017

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

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

1. Take the `lines` parameter, which is a collection of line segments, and separate it into two buckets: *Left Lines* and *Right Lines*. This is determined by calculating the line segment's slope. Lines with a positive slope are put into the Left Lines bucket, and otherwise they are put into the Right Lines bucket. (Actually, the slopes have opposite signs, due to the way the pixel coordinates are represented in the image, with increasing values of Y as you go down the image.)

2. For each bucket of line segments, I calculate the average location of the top and bottom of the right and left buckets when the line segments are extrapolated out. 

3. (For movies only): If drawing lanes for a movie, then I use the moving average of left and right lanes based on the last several movie frames. Also, I filter out "bad" lane lines for a given image; lines that are nearly horizontal or are sloped the wrong way.


###2. Identify potential shortcomings with your current pipeline

One major potential shortcoming is that it does not handle curves or turns in the road. The current pipeline assumes the lanes are straight. With more work, we could attempt to fit a spline or something else. 

Another shortcoming is if the car were to change lanes. The pipeline assumes the camera is mostly centered between two lanes, and would be become very confused during the lane shift.


###3. Suggest possible improvements to your pipeline

A possible improvement would be to take advantage of the fact that the camera images are 2D representations of a 3D world, and the video is a result of the car's "smooth" or continuous traversal through this world. 

Some ideas:

1. Put more weight on information that is available closer to the camera (towards the bottom of the image). The line segments nearer the bottom should have more influence on where we believe the lanes are. 

2. Detect the general "flow" of stationary objects in the movie. Lane lines should be parallel to this flow if the car is not turning.

###4. Thoughts and Reflection

I'm really impressed by how well the current pipeline works, since it is treating the image as a purely 2D object and using pretty straight-forward (but clever) image processing transforms. It's tempting to impose more and more domain knowledge on the problem (further restricting the region mask, color filters, etc.), but you run the risk of making it more brittle and increasing the execution time of the video processing, which is a real concern if we are looping into a real-time control system. 
