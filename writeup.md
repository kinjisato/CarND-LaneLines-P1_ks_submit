# **Finding Lane Lines on the Road** 

## Writeup

### Kinji Sato 10/June/2018

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report




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

** Modification of draw_lines() funtion **
To get averae slope (gradient) of R and L pipes, as first, I computed the slopes (gradient) from each set of x1,y1,x2 and y2 as suggested in the code. I clipped the min and max value of slope, to eliminate the slopes those not related to main R and L pipes. Clipping slope worked like filters. Then, after takeing the average of slope, single R and L pipe were extrapolated to the top of region area (Y=320) and bottom of image.

I defined 'ave' parameter in draw_lines. If that is False, the code works as originaly difined. If that is True, the code works with my modification above.


### 2. Identify potential shortcomings with your current pipeline

The shortcoming is the stability of the detection of pipelines after draw_lines. There is no time domain filter in the code,just only clipping the slope (gradient), so if Hough transformation do not find any pipes or incorret lines, maybe starility of the pipeline would be worse.


### 3. Suggest possible improvements to your pipeline

To improve, I might need to tune the parameters for Hough transformartion. These were a litte difficult for me, and due to the deadline for submit, I choosed curret values. And draw_lines also might need to be modified more. Because, to make smooth slope from the lines after hoguh, I made current computing way. But, if Hough transformation give better result than current my code, draw_lines() will be better, and if some time domain filter in the function, slope would be more stable like example movie.


