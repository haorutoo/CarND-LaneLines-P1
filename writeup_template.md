# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/grayscale.jpg "Grayscale"

[image2]: ./test_images_output/remove_horizontal_lines.jpg "Remove_Horizontal_Lines"

[image3]: ./test_images_output/blurred.jpg "Median_Blur"

[image4]: ./test_images_output/canny_edges.jpg "Canny_Edges"

[image5]: ./test_images_output/region_of_interest.jpg "Region_Of_Interest"

[image6]: ./test_images_output/hough_lines.jpg "Hough_Lines"

[image7]: ./test_images_output/pipeline_result.jpg "Pipeline Result"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. 

1) First, I converted the images to grayscale 
2) Then I removed horizontal lines using cv2.morphologyEx()
3) Next, I blurred the image to reduce the effect of salt and pepper noise.  
4) Fourth, Canny was then applied to the image to get the edges 
5) Afterwards, a mask was used to select the region of interest. 
6) In the final step, Houghline is used to detect and draw lines on the image

I modified the draw_lines function in the following ways:
    1) I combined Houghlines from both the left and right lanes to get an average slope
    2) Using the combined average slope, I used it to remove Houghlines in both the left and right lanes which had slopes that were outside a +/- 50% range of the average slope.
    3) After removing the outliers from the left and right lanes in the previous step, I calculated the mean and standard deviation of each lane and removed lines that were more than 1 standard deviation from their lane's mean.  I recalculated the mean and standard deviation again and removed lines that were 2 standard deviations away from their mean. 
    4) After filtering out the outliers, I did a search for the longest line to use for extrapolating the lane lines.  I noticed this did a better job than taking the average of line slopes since the longest line often corresponded with the lane line.  In selecting the longest lane line, I made sure to select lines that were within +/- 20% of the average x1 and x2 of that lane's line to avoid picking outliers.


If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]
[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"


### 2. Identify potential shortcomings with your current pipeline

This pipeline is heavily dependent on edge detection (Canny) and line detection (Houghline).  In situations where non-lane edges are introduced, our pipeline will not perform optimally and perhaps fail to find our lane line.  Things like shadows from trees, moving cars and objects, road dividers, and anything with lines that come into the region of interest will potentially skew our line detection.

And since our pipeline is heavily line dependent, roads that use reflective bumps or roads with no lines will very likely cause our pipeline to fail. 

### 3. Suggest possible improvements to your pipeline

Since our pipeline is so line dependent, an algorithm that can dynamically adjust the region of interest can dramatically improve our results. 

Also, a mask that is only encompasses the two lanes and removes non-lane areas to the left, right, and middle could provide more stable and accurate results. 

In addition, since roads don't change abruptly, using the average slope over multiple frames can lead to smoother output and removes outliers.  

Another possible improvement is to remove non-line objects that are are larger than lane markings so that these objects don't skew our lane line calculation.

Finally, to deal with difficult lighting or shadow situations, we could use masks on cv2 HSV that will detect lanes based on saturation. 