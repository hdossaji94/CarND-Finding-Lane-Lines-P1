# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of several different steps. First, I converted the images to grayscale, then I defined my kernel size for Gaussian smoothing for the grayscaled imaged. Then I defined my paramaters (low threshold and high threshold) to include canny edge detection on the Gaussian smoothed image. Once these modifications are done, I created a polygon mask for the region of interest/intersect and was returned a image only where mask pixels are nonzero. Before doing Hough Transformation, I had to define the associated paramaters such as rho, theta, threshold, etc. This then led me to the first hough_lines function which took in the masked_edges images and outputted an image with hough lines drawn. A weighted img is then applied to the output of the hough lines image which results in the first lane lines image of all the images in the test_images folder.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function (draw_lines2) by first creating global variables to store previous frames information from the video section. Then i created left line and right line arrays which will store information on the slope, intercept and length for each line segment in the image. The line segments are classified as being right or left by their slope values. Once all line segments were catagorized in the right array list, the aveage slope and intercept for each line was calculated. Since longer line segments are much more useful then the shorter ones, I used the line lenghts as weights to dominate the average. Going down the function, and taking into account that going from frame to frame in a video file may result in a loss of relevant data. Therefore I implemented a conditional where we can use the information from the line in the previous frame to calculate a corresponding line to the next frame. Finally using the calculated line slopes and intercepts we are able to draw a solid line from the bottom of the lane closest to the car to the to top where it meets the horizon. 


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when we encounter line changing lines or shadows in the video like the challenge video given. There would be many different mishaps in the line finding algorithm that could cause the car to lose track of the road or pick up a different line.

Another shortcoming could be that if the angle of the mounted camera was slightly higher or lower, it could severly mess up the min_y pixel value which determines where the horizon or where the extent to which the line should extrapolate should end. Also if the camera was more to the left or right, this could also mess up the masked region to which we have defined. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to add additional color filtering and procedures to identify shadows on the road and be able to identify color changing lanes so the car camera would not lose track of the lane.

Another potential improvement could be to keep track of the position and angle of the mounted camera in respect to the car. That way we would not have to define specific pixels in the frame to which we would use our masking. 
