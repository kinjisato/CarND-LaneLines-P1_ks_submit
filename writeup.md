# **Finding Lane Lines on the Road** 

## Writeup

### Kinji Sato 10/June/2018

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

My pipeline consisted of 5 steps.

First, I converted the images to grayscale.

2nd, I applied Gaussian smoothing for the gray clorred pictures. The kernel_size was 5 as the same as lecture. 

3rd, I applied Canny edge detection. low_threshold = 50, high_threshold = 150, those also have been carried from lecture. I could not find better value than this combination of low and high.

4th, I masked picture with the vertices,
vertices = np.array([[(0,imshape[0]),(450, 320), (490, 320), (imshape[1],imshape[0])]], dtype=np.int32)
I choosed Y = 320, because, if this was smaller than this, many pipes (not R and L line) were detected. And comparing with the exmaple movies, I decided Y=320 should be enough for R and L pipe lines.

5th, I applied Hough transformation. Helper function 'hough_lines' were calling 'draw_lines()' in its function. So, no need to call draw_lines() separatelly from 'hough_lines()'. The parameter tuning of Hoguh transform was difficult. rho = 2, theta = np.pi/180 were the same as lecture. I choose threshold = 50 to eliminate other pipes those were not related main R and L lines. And I choose ming_line_length = 10, and max_line_gap = 20, these combination gave me good results.

6th, finaly, I combined the R and L linen from Hough transrom (draw_line) and original image.





### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
