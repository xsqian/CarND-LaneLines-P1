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

My pipeline consisted of 5 steps.
1) I converted the images to grayscale
the resulting grayscale image is shown below:
[image2]: ./test_images_output/solidYellowCurveGray.jpg
![alt text2][image2]

2) Applies a Gaussian Noise kernel (5) to filter out some noise

3) Applies the Canny transform to detect the edges
[image3]: ./test_images_output/solidYellowCurveEdges.jpg
![alt text3][image3]

4) Create a mask area with a trapezoid with 60% in the Y direction and mask out the area not interested. The line image after mask is shown below:
[image4]: ./test_images_output/solidYellowCurveMaskedEdges.jpg
![alt text4][image4]

5) Run Hough on edge detected image. Output "lines" is an array containing endpoints of detected line segments using the following parameters:

rho = 2
theta = np.pi/180
threshold = 20     # minimum number of votes (intersections in Hough grid cell)
min_line_length = 40 # minimum number of pixels making up a line
max_line_gap = 20    # maximum gap in pixels between connectable line segments

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first identify a line segments whether it should be an left line or a right line measured by it slope and which side of the the line is in the image. If the slope is negative and it's on the left side of the image, it belongs to the left line. Other wise it belongs to the right line. I average the slopes of the line segments and using the average slope to draw the line from the bottom of the image to 60% of the image in Y direction


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be when the color of the lane line is very light and when it's on a sharp curve, the average slope is change so much from frame to frame, that cause the line on the video to be jumping around.

Another shortcoming could be all those parameters are tuned by try and error.


### 3. Suggest possible improvements to your pipeline

Current solution is frame by frame, the current frame's result doesn't affect the next frame. A possible improvement for the first issue might be improved by keep the previous frame's information (like the slope) for the current calculation. So it will take some kind moving average and will stablize the lines in the video.

Another potential improvement could some kind of training/learning to do the realtime tuning of the paramters.
