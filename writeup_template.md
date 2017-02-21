#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale. After that, I applied a guassian blur of kernel size 7. Canny edge detection was followed, and I noticed that the 50 and 150 as low and high thresholds, respectively did quite good. After that I applied selection for region of interest as doing that before edge detection would have detected the boundary lines as edges as well. Then the hough line transform were applied. Hough line parameters were selected so that, even a small line should be detected(minimum number of votes were 10), but length was constrained not to be too small. The images are as follows:

![output_solidWhiteCurve.jpg][test_images/output_solidWhiteCurve.jpg],
![output_solidWhiteRight.jpg][test_images/output_solidWhiteRight.jpg],
![output_solidYellowCurve.jpg][test_images/output_solidYellowCurve.jpg],
![output_solidYellowCurve2.jpg][test_images/output_solidYellowCurve2.jpg],
![output_solidYellowLeft.jpg][test_images/output_solidYellowLeft.jpg],
![output_whiteCarLaneSwitch.jpg][test_images/output_whiteCarLaneSwitch.jpg],

In order to draw a single line on the left and right lanes, I first made the draw_lines()_ modular by adding an extra parameter named extrapolate (=0, by default). So that I could switch between naive lines and extrapolated lines to draw. Then in order to extrapolate I first separated the left lanes from right lanes by using **slope** as a criterian, that is negative slope for left lane and positive slope for right lane to be specific. In between some check for outlier values was done and unlikely points were eliminated. 

Here is how to whole pipeline works shown for a single image

#### Grayscale
![output_gray_solidWhiteCurve.jpg][test_images/output_gray_solidWhiteCurve.jpg],

#### Edge filter
![output_edge_solidWhiteCurve.jpg][test_images/output_edge_solidWhiteCurve.jpg],

#### Region of interest
![output_roi_solidWhiteCurve.jpg][test_images/output_roi_solidWhiteCurve.jpg],

#### Lane Lines over edges
![output_line_image_solidWhiteCurve.jpg][test_images/output_line_image_solidWhiteCurve.jpg],

#### Lane lines over original image
![output_solidWhiteCurve.jpg][test_images/output_solidWhiteCurve.jpg],

#### Extrapolated lane lines
![output_solidWhiteCurve.jpg][test_images/output_solidWhiteCurve.jpg],

###2. Identify potential shortcomings with your current pipeline

Well there are several potential shortcomings of my pileline. 

1. The region of interest is pretty hardcoded, so this might fail if we ever go out of our lane.
2. If there are other lines resembling straight lines on the image near to our lane, for example a divider or an obstacle placed in front of camera that might be taken as a lane.
3. I am assuming that our lane lines are straight but that might not always be the case for example on sharp turns,  lane lines are almost never straight. My pipeline is certain to fail there.


###3. Suggest possible improvements to your pipeline

There can be several possible improvements.

1. The drawlines can be improved significantly to remove outliers. For example, there is a line identified as a lane line deviates significantly most of the lines that can be removed by seeing if its parameters (slope, for example) does not fit with the data. 

2. The region of interest coded right now is a quadrilateral. Other more specific non-convex polygons can be tried with 8-10 vertices to pinpoint the lane lines more accurately.