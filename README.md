#**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

This project was started as an assignment as a part of Udacity's Self Driving Car Nano Degree. The project aim is to identify lane boundaries in a dash cam video stream. This repository contains a jupyter notebook containing such an algorithm, designed for use with example videos also in this repository. 

***project built in miniconda "carnd-term1" environment, built by Udacity

Contents
---

1.) examples
		images and videos used for reference as ideal outputs of algorithm
2.) output_images
		annotated images produced by algorithm
3.) test_images
		images used to test algorithm
4.) test_videos
		videos used to test algorithm
5.) test_videos_output
		annotated videos produced by algorithm
6.) P1.ipynb
		jupyter notebook containing 
7.) writeup.md


Algorithm
---

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


Shortcomings
---

As currently implemented, this pipeline relies heavily on a relatively narrow image mask. Sharp turns in the road would result in lane lines outside of this region. They would not be found.  

My modifications to the draw_lines function give equal weight to all identified hough lines, regardless of size, since all are defined by only two endpoints. Very small lines (perhaps triggered by trash or other artifacts in the road) then have the ability to throw off the regression. For instance, a negatively sloped line (normally associated with a left lane boundary) caused by roadkill on the right side of the road would temporarily cause a significant disturbance to the left lane line because of how it would get sorted, and its equal weight in the regression. 


Opportunities
---

More careful tuning of the Hough transform parameters could alleviate some of the reliance on the narrow image mask. 

A size-weighted average, or some kind of signal smoothing could be used to prevent some of the jitters that result from outlier-lines throwing off the regression as described above. "# CarND-LaneLines-P1" 
