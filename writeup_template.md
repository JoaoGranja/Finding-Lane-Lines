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
* Change the pipeline so that only a full length left and right lane line is drew in the region of interest;

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Pipeline Description
The final goal of the pipeline is to take an image draw segments of lane lines in the original image.
The pipeline consisted of 6 steps:
1. Images are converted to grayscale;
    This step convert the image to grayscale and it is necessary to easily perform edge detection on next steps; 
    Function used: "grayscale"
2. Apply Gaussian smoothing;
    The Gaussian filter has the goal to suppress any image noise and spurious gradient by averaging;
    Function used: "gaussian_blur"
3. Edges detection using the Canny algorithm. 
    This algorithm uses intensity gradient of the image to find edges.
    Function used: "canny"
4. Define and mask the image with a region of interest. 
    To design the region of interest, it is considered that the front facing camera that took the image is mounted in a fixed position on the car, such that the lane lines will always appear in the same general region of the image.
    A quadrilateral region mask is used, defining the 4 vertices of the polygon.
    Function used: "region_of_interest"
5. Apply Hough Transform and draw hough lines. 
    Hough Transform is a space transformation so that a line is presented by a point in this new space and can be used to find points in the original space that are good candidates to below to a line. This lines are draw in a new image "line_image" with the same shape as the original one. 
    Function used: "hough_lines"
6. Combine line_image with the original image
    Function used: "weighted_img"

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by slipting the points which belows to the left lane and to the right lane where it is considered that the left lane has slope between \[-0.9; -0.5] and the right lane has slope between \[0.5; 0.9]. Then I took the average slope and intersection of the line and draw the left and right lane line taking into account the region of intereset. It was considered that any line can be calculated and y=mx+b, m is the slope and b the intersection. 


![alt text][image1]


### Potential shortcomings with the current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### Suggestion of possible improvements to the pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
