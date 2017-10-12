







# **Finding Lane Lines on the Road** 


## Writeup



---


**Finding Lane Lines on the Road**


The goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road

* Reflect on your work in a written report




---


## Reflection



###  1. Pipeline Description

#### Pipeline Summary
My pipeline consists of the following steps:

* Color Selection 
* Canny Edge Detection
* Region of Interest Selection
* Hough Lines Detection

The pipeline_process_image() function implements the pipeline and
calls multiple provided helper functions and new functions I have added.

#### Overall Strategy
To start with, I completely focussed on one image that had a solid white line on the right and segmented white lines on the
left. I executed the pipeline step by step. For each step, I displayed the image and made sure that step works as expected.
Once the pipeline produced the correct result, I started working on another image that had both yellow and white lines. 
After detecting the yellow lines correctly, I tested all the test images in the test_images directory. 
Once the test images are verified, I focussed on the videos.


#### Pipeline Details 

##### Color Selection
Intially, I wrote a function using the RGB color space as was done in the classroom example.
But this function didn't detect the yellow lines. Realized that the HLS color space provided 
better color selection thresholds. I converted the image to HLS and used HLS thresholds to
detect white and yellow lines.
This is captured in the select_colors_white_yellow()
![alt text](test_images\colorselected.jpg)



##### Canny Edge Detection
This consists of multiple sub-steps

* Converted the image to gray scale.
![alt text](test_images\grayscale.jpg)


* Applied Gaussian Blur to smooth out the edges.
![alt text](test_images\gaussianBlur.jpg)


* Used the helper function canny() to detect the edges.
  As recommended in the lesson, kept the ratio between low and high thresholds to 3.
![alt text](test_images\canny.jpg)



##### Region of Interest
This step needed some experimentation to exactly define the needed area of interest Polygon.
![alt text](test_images\roi.jpg)



##### Hough Lines Detection
A lot of experimentation went in to defining the values for the parameters.
Especially notable is the min_line_len, which I kept it as 5 since I wanted to detect
even the smallest white segment in the image. 

Instead of modifying the provided helper function draw_lines(), I created a new function
called draw_unbroken_lines() and called it from a new function hough_lines_continuous().
The reason for this is that I wanted to keep the original helper functions intact so that
I can go back to them if needed.

The hough_lines_continuous() function called a new function called get_complete_lanes_from_segments()
which took the individual segments, calculated their slope and intercept and returned the coordinates of
the extrapolated lines.
Finally, the new function draw_unbroken_lines() drew the extrapolated lines on a blank image.
![alt text](test_images\houghLines.jpg)


In the main function, the hough lines returned by the pipeline_process_image()
was added to the original image using the weighted_img() function.
![alt text](test_images\finalImage.jpg)



### 2. Potential Shortcomings 
This is a very simple lane detection program and I believe this pipeline 
won't work under the following scenarios:

* The road does not have a white or yellow lane at the edge of the road (rural areas)

* There is a sharp turn in the road.

* The road dips down sharply or goes up at  steep angle 

* During night, under yellow color sodium/mercury lights, the lane colors will not be yellow and white.

* There is a 2 line (no zebra markings) diagonal cross walk at an intersection. 

* There is a black bus ahead and it has 2 white lines painted coinciding with the white lanes on the road!



### 3. Possible Improvements. 

* To address many of the shortcomings, I believe the Region of Interest needs to be more flexible and not fixed.

* To improve night time lane detection, more colors must be detected.

* Steep lanes or sharp dips require different slope calculations.



### **Meeting the Rubric Requirements**

* The pipeline takes images from a video stream and creates an annotated video stream as output.

* The pipeline used the helper functions as well as newly added functions, identifies white and yellow continuous and segmented lanes
  and annotates the video stream with continuous left and right lanes. 


    
## **Final Thoughts**
It is just one week since the course started and I feel I have already learned a lot about Self Driving cars. 
I am very much impressed with the quality of the lessons. The quizzes and the project have been very cleverly 
designed so that students end up exploring and in the process learn the concepts well. 
Can't wait to go through the rest of the course.
