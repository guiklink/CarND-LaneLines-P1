# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/Processing_Steps/solidWhiteCurve_1_Gray.jpg "Grayscale"
[image2]: ./test_images/Processing_Steps/solidWhiteCurve_2_GrayBlur.jpg "Blur"
[image3]: ./test_images/Processing_Steps/solidWhiteCurve_3_Canny.jpg "Canny"
[image4]: ./test_images/Processing_Steps/solidWhiteCurve_4_Masked.jpg "Mask"
[image5]: ./test_images/Processing_Steps/solidWhiteCurve_5_Lines.jpg "Lines"
[image6]: ./test_images/Processing_Steps/solidWhiteCurve_6_Out.png "Final"

---

### Reflection

### 1. Pipeline

My pipeline consisted of 5 steps.
1. Convert images to grayscale
2. Apply Gaussian Blur
3. Use Canny alghorithm do use the gradient to detect edges
4. Apply a mask to extract an area of interest
5. Detect lines using the Hough transformation
6. Apply the detected lines on the initial image


In order to improve the quality of lines that will be drawn, I used the function **extrapolate_lines**. To do so, first I got all the small segments retrieved from the Hough line detector and divided in two groups accordingly to the value of its slope, hence a positive slope means it is a segment from the left line and negative slope means it is be a segment of the right line of the road. Segments with slope 0 should be noise and not be considered. Once the segments are divided in buckets for the two lines a [polyfit](https://docs.scipy.org/doc/numpy/reference/generated/numpy.polyfit.html) function from the **numpy** library can be used to fit a line to these points. Then, having an equation of the line points are picked on an arbitrary subinterval (default resolution 10), starting at the bottom of the image until the top of the mask and the line is plot.

The images bellow show the pipeline in action:
![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be when another car is changing lanes and therefore tapping the line. In this case depending of the size and distance, the edges of the car changing lanes can create enough noise to jeopardize the line found

Another shortcoming could when a sharp turn is close. None of the images or videos used presented this scenario.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use OpenCV blob detection to remove cars crossing lanes from the detection zone from the pipeline. 

Another potential improvement could be to try to identify high slope variations to ignore edges from sharp curves.
