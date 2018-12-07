# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image0]: ./test_images_output/original.png "Original Image"
[image1]: ./test_images_output/blurred.png "Blurred Grayscale Image"
[image2]: ./test_images_output/canny.png "Canny Transformation"
[image3]: ./test_images_output/masked.png "Region Mask Applied"
[image4]: ./test_images_output/extended_lines.png "Average Lane Lines"
[image5]: ./test_images_output/final.png "Final Image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.


The pipeline was built with the extensive use of the OpenCV library in order to annotate the image with any detected lane lines and consists of the following 7 steps.

![Original Image][image0]


1.  The original color image was converted to grayscale.


2.  In order to smooth out any potential noise, a little Gaussian blurring was added using a kernel of 5.

![Blurred Grayscale Image][image1]

3.  A Canny transformation was applied in order to detect edges in the grayscale image.  Good results were obtained using a low threshold of 100 and a high threshold of 200.

![Edge Detection][image2]

4.  Because the lane line are know to always appear within a particular sub-region within the bottom half of the image, a mask was created of this region and the mask was applied to the image.  This removed many edges that were known not to be lane lines.

![Region Masking][image3]

5.  A Hough transformation was applied to find line from the masked edges image.  After a little experimentation, the following parameters were found to yield very good results:  ρ = 1, θ = π/180, threshold = 40, minimum line length = 80, maximum line gap = 80


6.  In order to draw extended lane lines on each the left and right sides of the lane, I modified the draw_lines function by adding a new helper function called draw_extended_lines().  It eliminates any lines that are too vertical or too horizontal by applying a threshold for each.  Treating the left hand lines and rigth hand lines separately, it averages the position of each line to find an average line which it extends (interpolates) from the bottom of the image upwards to cover the bottom 40% of the image.

![Lane Lines][image4]

7. Tthe lane line drawing is merged with the original image using OpenCV's addWeighted() function which allows one to control the transparency of each image.

![Final Image][image5]


### 2. Identify potential shortcomings with your current pipeline

One potential short coming is that this method seems to be susceptable to shadows on the edge of the road (like due to the side of a bridge).  The current pipeline can sometimes mistaken these shadows for lane lines.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to add color detection in order to eliminated unwanted shadows.
