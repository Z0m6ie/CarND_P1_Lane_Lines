# **Finding Lane Lines on the Road**

## Project 1

### Using Canny edge detection, image masking and Hough transform through OpenCV to detect and extrapolate lane lines.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Describe the process and outline any issues


[//]: # (Image References)

[image1]: ./report_images_output/original.png "Original"
[image2]: ./report_images_output/gray.png "Grayscale"
[image3]: ./report_images_output/blur.png "Blur"
[image4]: ./report_images_output/canny.png "Canny"
[image5]: ./report_images_output/hough.png "Hough"
[image6]: ./report_images_output/final.png "final"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline to detect the lane lines consisted of 6 key steps.
1) Create a copy of the original image, to ensure any manipulation does not alter the original
![alt text][image1]

2) Convert the image to grayscale all pixels are between 0 & 255
![alt text][image2]

3) Blur image
![alt text][image3]

4) Use canny edge detection to find edges in image
![alt text][image4]

5) Mask area and use Hough transform to find lines in the image
![alt text][image5]

6) merge Hough transform image with the original image to show lines
![alt text][image6]

#### draw_lines() function
In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first separating the lines in each frame by its gradient using the equation slope = y2-y1/x2-x1.
Right lanes can roughly be assumed to have a positive gradient and Left lanes a negative gradient.

In addition I split the points of the line in to rightx, righty, leftx, lefty this enables me to use these points to train a linear regressor.

For this I imported sklearns LinearRegression() and used fit and predict to train on each frames data and predict the extrapolated line.

To extrapolate the line I predicted on the y value that represents the bottom of the image (540) and my preferred end point (350).

The only additional trick I used was to use a deque which holds points from previous frames to train and predict the line. As lane lines are fairly consistent this means a single frame which finds and anomalous lane line does not effect the extrapolated lines too much.




### 2. Identify potential shortcomings with your current pipeline


This pipeline suffers from a few issues because of the fixed variables which have been set up for images with good lane markings on a clear bright day.

If the lane markings were worn away or it was not a bright sunny day, there is a good change that the lines would not be picked up well by the current pipeline. evidence of this can be seen in the challenge video where my pipeline does not provide a satisfactory result.

In addition in my pipeline some information carries on from one frame to another. Most of the time this is good as it helps prevent anomalous data impact our lane markings but it also would struggle with a sudden sharp turn.




### 3. Suggest possible improvements to your pipeline

This pipeline could be improved several ways.

1) improved masking to eliminate distracting lines.

2) improve slope filtering of lines. help reduce erroneous lines such as horizontals

3) Assess if grayscale helps in all situations, may mute the yellow lane lines and make them harder to find.
