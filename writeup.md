# **Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image_original]: ./examples/original.jpg "HLS"
[image_hls]: ./examples/hls.jpg "HLS"
[image_yellow]: ./examples/yellow.jpg "Yellow"
[image_white]: ./examples/white.jpg "White"
[image_yellow_white]: ./examples/yellow_white.jpg "Yellow and White"
[image_blur]: ./examples/blur.jpg "Gaussian Blur"
[image_canny]: ./examples/canny.jpg "Canny Edge Detection"
[image_roi]: ./examples/roi.jpg "Region of Interest"
[image_hough]: ./examples/hough.jpg "Hough Transform"
[image_result]: ./examples/result.jpg "Result"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 10 steps.

| Steps | Images |
| ---- | ------ |
| 1. Read RGB image. | ![alt text][image_original] |
| 2. Convert image from RGB to HLS. | ![alt text][image_hls] |
| 3. Use `cv2.inRange` to filter yellow color from HLS image. | ![alt text][image_yellow] |
| 4. Use `cv2.inRange` to filter white color from HLS image. | ![alt text][image_white] |
| 5. Combine yellow and white color masks. | ![alt text][image_yellow_white] |
| 6. Apply Gaussian Blur on combined yellow and white masks. | ![alt text][image_blur] |
| 7. Use Canny Edge Detection on blurred image to find edges. | ![alt text][image_canny] |
| 8. Mask detected edges with Region of Interest. | ![alt text][image_roi] |
| 9. Use Hough Transform for line detection and draw_lines() to extrapolate the lanes. | ![alt text][image_hough] |
| 10. Overlay extrapolated lanes on original image. | ![alt text][image_result] |

In order to draw a single line on the left and right lanes, I modified the draw_lines() function to:

1. Calculate the slope and length for each line detected.
1. Calculate the new x1 and x2 to extrapolate the lines to reach the region of interest.
1. Filter all lines with negative slope and extrapolated x1 and x2 in the left 60% of the image as potential left lane lines.
1. Filter all lines with positive slope and extrapolated x1 and x2 in the right 60% of the image as potential right lane lines.
1. Find the longest left and right lane lines from the potential lines and draw them on the image.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the car is changing lane. The position of the left and right lane lines will not be in the left or right 60% of the image.

Another shortcoming could be what would happen when the road is having a sharp turn. Extrapolating from the longest straight line may not be useful in that situation.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use linear regression to find a straight line that best matches all the points of lines with negative slopes as the left lane. And find a straight line that best matches all the points of lines with positive slopes as the right lane.

Another potential improvement could be to take the detected lanes from previous frame into account. Lanes in real world are continuous, considering the previous frame would provide more consistent lanes.
