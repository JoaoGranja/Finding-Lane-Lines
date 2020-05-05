# **Finding Lane Lines on the Road** 

This document is a summary report explaining the approach used on this project. This report is composed by the following sub-sections:
* Project goals which presents the main goals of this project as well as the steps used;
* Pipeline description;
* Idenfitication of potential shortcomings with the pipeline used;
* Suggestion of possible improvements to the pipeline;

---

## Project Goals

This project has as main goal identifying lane lines on the road in images and videos. 
The steps used to approach the project are:

* Build a pipeline that finds lane lines on the road using several images;
* Tuning the pipeline parameters to make it more accurate on the images used;
* Apply the pipeline to the videos provided;
* Change the pipeline so that only a full length left and right lane lines are drew in the region of interest;


[//]: # (Image References)

[original_image]: ./test_images/solidWhiteCurve.jpg 
[final_image]: ./test_images_output/solidWhiteCurve.jpg 

---

### Pipeline Description
The final goal of the pipeline is to take an image and draw the left and right lane lines in the original image.
The pipeline consisted of 6 steps:
**1. Images are converted to grayscale**
    This step converts the image to grayscale to easily perform edge detection on next steps; 
    Function used: "grayscale"
**2. Apply Gaussian smoothing**
    The Gaussian filter has the goal to suppress any image noise and spurious gradient by averaging;
    Function used: "gaussian_blur"
**3. Edges detection using the Canny algorithm**
    This algorithm uses intensity gradient of the image to find edges.
    Function used: "canny"
**4. Define and mask the image with a region of interest** 
    To design the region of interest, it is considered that the front facing camera that took the image is mounted in a fixed position on the car, such that the lane lines will always appear in the same general region of the image.
    A quadrilateral region mask is used, defining the 4 vertices of the polygon.
    Function used: "region_of_interest"
**5. Apply Hough Transform and draw hough lines**
    Hough Transform is a space transformation so that a line is presented by a point in this new space. This transform can be used to find points in the original space that are good candidates to belongs to a line. These lines are draw in a new image called "line_image" which has the same shape as the original one. 
    Function used: "hough_lines"
**6. Combine line_image with the original image**
    Function used: "weighted_img"

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by slipting the points which belows to the left lane and to the right lane where it is considered that the left lane has slope between \[-0.9; -0.5] and the right lane has slope between \[0.5; 0.9]. Then for each lane, I took the average slope and intersection of the line and draw the left and right lane line taking into account the region of intereset. It was considered that any line can be calculated as "y=mx+b", where "m" is the slope and "b" the intersection. 

An example of the pipeline work is presented below. The original image is:

![alt text][original_image]

And the image with the left and right lanes drew is:

![alt text][final_image]


---
### Potential shortcomings with the current pipeline


One potential shortcoming would be what would happen when the slope of the left or right lane changes too much (outside of the considered range in the draw_lines function), for example in a road curve. In that case can be possible that the two lanes have both a positive or negative slope and the draw_lines function would not work properly.

Another shortcoming could be when the car cross a lane line and three lines are visible on the image. The pipeline was designed considering just two lanes.

---
### Suggestion of possible improvements to the pipeline

A possible improvement would be to better split the left and right lane using other condition than slope. For example taking into account the points positions. 

Another potential improvement could be to smooth the slope variation between consecutive images on a video to supress some noise on lane lines. It is expected that the lane line direction change smoothy and slowly. 
