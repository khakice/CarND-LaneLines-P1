#**Finding Lane Lines on the Road** 

##Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/result_solidWhiteCurve.jpg "solidWhiteCurve"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.

1. I converted the images to grayscale.
2. I applied gaussian blur to the gray image with a kernal size 5.
3. I found edges using canny function with a low/high threshold 50 and 150.
4. I masked edges using trapezoid to select only lanes that I concern.
5. I applied hough with these parameters:
 - rho: 2, theta: pi/180, threshold: 15, min_line_len: 40 max_line_gap: 20

In order to draw a single line on the left and right lanes, I modified the draw_lines() function like this.

I assumped if the slope sign of the line is positive, it should be the left lane, right lane if negative. If the slope is zero and x value is smaller than 'img.shape[1] / 2', it should be the left lane, otherwise right lane.

Here's the example images that recognizes left and right lanes correctly:

![alt_text][image1]


###2. Identify potential shortcomings with your current pipeline

Here's potential shortcomings that can happen:

1. If the lane length or gap between lines doesn't meet with my parameter values, then my code would fail to draw a line.
2. My code assumped that nothing should be in the trapezoid zone. Thus, if another the car in front of my car is close enough to be in the trapezoid zone, the line drawings would be messed.
3. If the line is a so big curve that the slope sign couldn't say whether left or right, my code would fail.
4. We solve the lines for each image independently, adjacent images could draw very different lanes for left and right lanes.


###3. Suggest possible improvements to your pipeline

Here's the possible improvement for each shortcomings:

1. We should learn parameters with more images to fine tune the value.
2, 3, 4. We can set a prior knowledge about left and right lanes since the location of these lanes are not a random, but very likely predictable. So if we met very difficult situation, we can use the prior knowledge and the previous lane location to predict the current lanes location.
