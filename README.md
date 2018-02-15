# Finding Lane Lines on the Road 

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Skills
---
* Python
* OpenCV


Overview
---

This project was completed as an assignment for Udacity's Self Driving Car Nano Degree [![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive). The jupyter notebook walks step by step through the process of identifying lane lines in a video stream using Canny Edge Detection and the Hough Transform. 


Dependencies
---
This project was built using the anaconda _carnd-term1_ environment available [here](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/doc/configure_via_anaconda.md)


Algorithm
---

1. Convert to a greyscale image

2. Perform Gaussian smoothing on the image 
			
3. Perform Canny edge detection on the blurred image
		
4. Create a trapezoidal mask to define a region of interest and set rest of image to black 

5. Perform Hough transform on masked image
		
6. Draw the lines identified by the Hough transform on the original image:
 
``1. Sorts Hough-identified lines into two arrays by whether the line's slope is positive (right lane boundary) or negative (left).

``2. Performs a linear regression on the endpoints of the hough lines 
	in each array using the np.polyfit function.

``3. y-coordinates of the endpoints of the desired singular lines are
	definated as the top and bottom of the image mask defined in the 
	pipeline above. x-coordinates are solved for using the coefficients 
	from the polyfit regression.

``4. Using the calculated endpoints, 2 lines (right and left) are plotted on the original image. 



