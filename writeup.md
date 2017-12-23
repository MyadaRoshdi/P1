# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect my work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/grayscale.png "Grayscale"
[image2]: ./test_images_output/Canny-Edge.png "CannyEdge"
[image3]: ./test_images_output/Hough-transform.png "HoughTransform"
[image4]: ./test_images_output/test1_before_pipeline.png "test1"
[image5]: ./test_images_output/test1_after_pipeline.png "test1_pipeline"
[image6]: ./test_images_output/test2_before_pipeline.png "test2"
[image7]: ./test_images_output/test2_after_pipeline.png "test2_pipeline"

---

### Reflection

first, here is an example of original image before applying any modifications:
![test1_img][image4]
### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:

**a) Convert image to grayscale**
  Here  I used the function (`cv2.cvtColor(image, cv2.COLOR_RGB2GRAY)`), for grayscale conversion. 
The code for this step is contained in the code cell In[4], line#12 of the IPython notebook located in "Find_Lane_Lines_P1_Self_Driving.ipynb"

Here's an example of my output for this step. 

![Grayscale_img][image1]

**b) Apply Canny-Edge**
 Here  I first did Gaussian smoothing as suggested in the lesson, then applied Canny-Edge.Canny Edge algorithm will first detect strong edge (strong gradient) pixels above the high_threshold, and reject pixels below the low_threshold. Next, pixels with values between the low_threshold and high_threshold will be included as long as they are connected to strong edges. The output edges is a binary image with white pixels tracing out the detected edges and black everywhere else. . 
The code for this step is contained in the code cell In[4], in lines #14 through #21  of the IPython notebook located in "Find_Lane_Lines_P1_Self_Driving.ipynb"

Here's an example of my output for this step. 

![CannyEdge_img][image2]

**c) Select only region of interest**
  As shown in above image, the image has alot of detailes outside the lanes (e.g. trees, left and right sides of the image), these noise could be misleading in lane lines detection, so it is better to cut of the image all these unwanted details. My region of interest is just a trapeziod shape cut out of the middle of the image. 
The code for this step is contained in the code cell In[4],in  lines#18 through #43 of the IPython notebook located in "Find_Lane_Lines_P1_Self_Driving.ipynb"

**d) Apply Hough transform**
 Using Hough Transform to find lines from Canny Edge. Now, I have an image of white dots as output from Canny Edge, looking for a method to connect these dots to find the lines( Since here dots could be connected looking for any shape), so I will use the line mathematical modeling for that (Y=aX+b), since I am looking for lines.
The code for this step is contained in the code cell In[4],in  lines#49 through #62 of the IPython notebook located in "Find_Lane_Lines_P1_Self_Driving.ipynb"

Here's an example of my output for this step. 

![HoughTransform_img][image3]

**e) Draw 2-continous lines for left and right lane-lines**
In the above Image, I already drew all the lines found from Hough through the selected region, but now I want only two lines. One on the left and one on the right. First, I will the info that the slope for left line is negative and the slope for the right line is positive. (When x increase and y increase, the slope is positive). The used Algorithm is :

  1) Loop though all the lines and find all the slopes

  2) Make an array of left slops and right slops.

  3) Put the positive and negative number into the correct array. Note: Since some lines don't belong to left or right, so consider a        slope with difference margin 0.5 from left or right, otherwise, ignore.

  4) Calculate left and right slope average.

  5) For every line, Calculate the y- intersection, so we can use the point with the minimum y-value as the 2nd point to draw the line.   Since the line we draw must start at the botton edge of the image( subistitute in the equation Y=mX+b).

Note: to draw the line from 2- points I used the line mathematical formula Y= mX+b, In the equation of a straight line (when the equation is written as "y = mx + b"), the slope is the number "m" ( which is calculated in my Algorithm for both left and right lines), and "b" is the y-intercept (that is, the point where the line crosses the vertical y-axis).

The code for this step is contained in the code cell In[4],in  lines#65 through #168 of the IPython notebook located in "Find_Lane_Lines_P1_Self_Driving.ipynb"


**Then, I appled on all test images and test videos**
Here's 2- examples of  images before and after the pipeline. 

**Example 1:**

![test1_before_pipeline img][image4]
![test1_after_pipeline img][image5]

**Example 2:**

![test2_before_pipeline img][image6]
![test2_after_pipeline img][image7]

**And finally, apply on test Videos**

Here's a [link to my 1st video result](https://github.com/MyadaRoshdi/P1/blob/master/test_videos_output/solidWhiteRight.mp4)

Here's a [link to my 2nd video result](https://github.com/MyadaRoshdi/P1/blob/master/test_videos_output/solidYellowLeft.mp4)

### 2. Identify potential shortcomings with your current pipeline

This pipeline is only tested on the testing images and videos supported, so it might fail when tested on images taken in differenct conditions (e.g. different lighting conditions ( night, vogue ..etc), rains, snow, crossing pedestrians, shadows, sharp edges, faded lane lines...etc).


### 3. Suggest possible improvements to your pipeline

1* I'd suggest as first step to do Camera calibration and undistortion of all images.

2* Capture myself more images to test over on different conditions.

3* Instead of using the current algorthim, I can invest more time to get a better algorithm that divide the image as sliding windows, where lines detected from bottom windows could be used as a starting point fo the next window.
