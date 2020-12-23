# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I blurred the image to reduce white spec noise.  Third, Canny was then applied to the image to get the edges followed by selecting the region of interest. In the fifth and final step,  I applied Houghline to detect lines and used them to draw them on the returned image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by taking several steps to remove outlier lines that are not part of the left or right lane lines.  Once the outliers were removed, I took the longest line available on the left and right lane to use as the line to extend and extrapolate the lane lines.  The steps in removing the outlier included first combining all left and right lane lines, taking their average, and then keeping only slopes that were were within the average slope +/- (average slope * 50%).  With the lines that are left int he left and right lane, do two more filters to first keep only lines within 1 standard deviation of the mean, and then the second filter to keep only lines within 2 standard deviation of the mean.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


It seems that being able to identify the region of interest that conforms to the exact area of the lane can dramatically improve our results in difficult scenaries.  In certain scenes, non-lane edges or even debris on the road can cause our drawn lane line to deviate from the true lane line.  It seems that moving cars are a source of lines that if it feel within our region of interest, can skew our drawn lane line.  Generally speaking, this pipeline's reliance on edge detection and hough lines within an area of interest would easily fail in instances where non-lane objects with the right line profile were to enter into the scene region of interest.  

Another shortcoming is that our pipeline would have a problem detecting roads that use small reflectors as lane dividers instead of drawn lane lines.  Dirt roads that are not drawn with bright lane lines would potentially perform poorly using our pipeline.  

### 3. Suggest possible improvements to your pipeline

The current pipeline outputs movies with lines that are not as stable as the example video.  We can probably improve our stability using average the slope of the line over multiple frames instead of frame by frame.

It is probably possible to write an algorithm that would auto-adjust region of interest calculations to include only the current lane, excluding the side and center, so that we can simplify our pipeline.  

It is probably also possible to remove non-line objects so as to minimize the potential effect of the object' lines on our calculations.