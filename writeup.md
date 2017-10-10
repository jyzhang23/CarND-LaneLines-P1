# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.
Here is an example test_image from the project
![test_image](https://github.com/jyzhang23/CarND-LaneLines-P1/blob/master/test_images/whiteCarLaneSwitch.jpg "whiteCarLaneSwitch")

My pipeline consisted of the following 7 steps: 
1. Convert image to grayscale
![gray_image](https://github.com/jyzhang23/CarND-LaneLines-P1/blob/master/test_images_output/gray.jpg "grayscale")

2. Apply Gaussian blur
![blur_image](https://github.com/jyzhang23/CarND-LaneLines-P1/blob/master/test_images_output/blur.jpg "blurred")

3. Apply Canny edge detection
![edge_image](https://github.com/jyzhang23/CarND-LaneLines-P1/blob/master/test_images_output/edge.jpg "edge image")

4. Apply quadrilateral ROI mask
![roi_image](https://github.com/jyzhang23/CarND-LaneLines-P1/blob/master/test_images_output/roi.jpg "roi edges")

5. Perform Hough transform to extract lines

6. Extract lanes by separating all Hough lines into positive and negative arrays, and taking the median to generate 2 sets of lane lines
![lane_image](https://github.com/jyzhang23/CarND-LaneLines-P1/blob/master/test_images_output/lane.jpg "lanes")

7. Overlay lanes on original image
![final_image](https://github.com/jyzhang23/CarND-LaneLines-P1/blob/master/test_images_output/whiteCarLaneSwitch.jpg "lane overlay")

In order to draw a single line on the left and right lanes, I separated the output of the Hough transform function into positive and negative arrays. I then took the median over both arrays to get a slope and intercept for each lane. I then converted each set of slope/intercept into 2 points x1,y1,x2,y2 by using the line equation for the edges of the ROI. This is done in the calculate_lane_lines(lines) function. The function returns 4 cartesion points, which describe the endpoints of the extracted lane lines. These points are passed to the draw_lines() function, which draws the lines in a blank image.

### 2. Identify potential shortcomings with your current pipeline

Some potential shortcomings to the pipeline could occur when:
1. There is low contrast between the lane lines and the road, such as with faded lines or external lighting differences
2. The road bends more sharply and passes outside the ROI
3. There are vehicles or other objects inside the ROI

These cases are present in the challenge video, which this pipeline did not work on

### 3. Suggest possible improvements to your pipeline

Some possible improvements to the pipeline:
1. Operate on different color channels to specifically look for yellow and white objects to detect
2. Allow curved lane lines by fitting both lines and parabolas to the Hough lines
