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

My pipeline consists of: 

	1.) Converting to a greyscale image

	2.) Perform Gaussian smoothing on the image 
			Kernel Size = 5

	3.) Perform Canny edge detection on the blurred image
			low threshold = 50
			high threshold = 150

	4.) Create a trapezoidal mask to define a region of interest and set 	rest of image to black 
			bottom left = 	(x = .08*image width, y = image height)
			top left = 		(x = .47*image width, y = .6*image height)
			top right = 	(x = .56*image width, y = .6*image height)
			bottom right = 	(x = .95*image width, y = image height)

	5.) Perform Hough transform on masked image with following parameters
			rho = 2                
    		theta = np.pi/180      
    		threshold = 15         
    		min_line_len = 40   
    		max_line_gap = 20

	6.) Draw the lines identified by the Hough transform on the original 	image

In order to draw single lines defining the left and right lane boundaries,
I modified the draw_lines() function which is called within the hough_lines() function. The new funtion: 

	1.) Sorts Hough-identified lines into two arrays by whether the line's slope is positive (right lane boundary) or negative (left).

	2.) Performs a linear regression on the endpoints of the hough lines 
	in each array using the np.polyfit function.

	3.) y-coordinates of the endpoints of the desired singular lines are
	definated as the top and bottom of the image mask defined in the 
	pipeline above. x-coordinates are solved for using the coefficients 
	from the polyfit regression.

	4.) Using the calculated endpoints, 2 lines (right and left) are plotted on the original image. 


### 2. Identify potential shortcomings with your current pipeline
Shortcomings: 

As currently implemented, this pipeline relies heavily on a relatively narrow image mask. Sharp turns in the road would result in lane lines outside of this region. They would not be found.  

My modifications to the draw_lines function give equal weight to all identified hough lines, regardless of size, since all are defined by only two endpoints. Very small lines (perhaps triggered by trash or other artifacts in the road) then have the ability to throw off the regression. For instance, a negatively sloped line (normally associated with a left lane boundary) caused by roadkill on the right side of the road would temporarily cause a significant disturbance to the left lane line because of how it would get sorted, and its equal weight in the regression. 

### 3. Suggest possible improvements to your pipeline
Opportunities: 

More careful tuning of the Hough transform parameters could alleviate some of the reliance on the narrow image mask. 

A size-weighted average, or some kind of signal smoothing could be used to prevent some of the jitters that result from outlier-lines throwing off the regression as described above. 