## **Finding Lane Lines on the Road** 

### Writeup by Avi Avidan for CarND-Term1 first project

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]:https://github.com/Avi-avidan/CarND-LaneLines-P1/blob/master/pipeline_imgs/solidWhiteCurve_1_RGB_threshold_edges.jpg
[image2]:https://github.com/Avi-avidan/CarND-LaneLines-P1/blob/master/pipeline_imgs/solidWhiteCurve_2_grayscale.jpg
[image3]:https://github.com/Avi-avidan/CarND-LaneLines-P1/blob/master/pipeline_imgs/solidWhiteCurve_3_blur_gray.jpg
[image4]:https://github.com/Avi-avidan/CarND-LaneLines-P1/blob/master/pipeline_imgs/solidWhiteCurve_4_canny_edges.jpg
[image5]:https://github.com/Avi-avidan/CarND-LaneLines-P1/blob/master/pipeline_imgs/solidWhiteCurve_5_masked_edges.jpg
[image6]:https://github.com/Avi-avidan/CarND-LaneLines-P1/blob/master/pipeline_imgs/solidWhiteCurve_6_right_polyline.jpg
[image7]:https://github.com/Avi-avidan/CarND-LaneLines-P1/blob/master/pipeline_imgs/solidWhiteCurve_7_left_polyline.jpg
[image8]:https://github.com/Avi-avidan/CarND-LaneLines-P1/blob/master/pipeline_imgs/solidWhiteCurve_8_final_img.jpg

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps, as following -
1. RGB threshold (function: 'color_thresh'), basically keeping only pixles with R values over 210

![alt text][image1]

2. Identifying canny edges in a region of interest (function: 'get_masked_edges'), using these steps -
    2.a. function 'grayscale' (cv2.cvtColor) (possibly redundant after color threshold)
![alt text][image2]
    2.b. function 'gaussian_blur' (cv2.GaussianBlur)
![alt text][image3]
    2.c. function 'canny' used to indentify canny edges (cv2.Canny)
![alt text][image4]
    2.d. function 'region_of_interest' used to copy only non-zero pixels from a polygon of interest (cv2.fillPoly than cv2.bitwise_and)
![alt text][image5]
3. Fit a first degree polynome ( function: 'get_polyfit_line'), using these steps -
    3.a. cropping the image for either left or right side for fitting either left or right lane line (function:'get_polyline')
    3.b. using np.nonezero and np.polyfit to get coefficients of a stright line (1st degree polynome) (function:'get_polyline')
    3.c. using coefficients found to draw the line (function: 'get_polyfit_line')
![alt text][image6]
![alt text][image7]
4. drawing the fitted lines on the original image (function: 'weighted_img')
![alt text][image8]


### 2. Identify potential shortcomings with your current pipeline

Shortcomings of this approach -
1. failing to identify lane lines outside area of interest
2. failing to identify lane lines of different colors
3. failing to identify lane lines which are not straight lines

### 3. Suggest possible improvements to your pipeline

before I have decided to submit this very simple implementation, I actually also tried thresholding using various HLS values and without thresholding at all.
for each frame I have tried the 3 approaches (RGB threshold, HLS threshold and none.. before canny edges detection) and chose the best poly fit (with lowest squared error), but the marginal improvement was negligible compared to that I am submitting here.

So, to sum up, possible imporements might include - 
1. identifying actual color of the laneline and than polyfit
2. averaging polyfit over a number of frames
3. encorporating to the projected fit the expected width of lane, so if identification of one lane is good the other could be drawn accurately even if was not detected clearly from the img.  
