## Advanced Lane Finding

The Project
---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step can be found here: [Compute Camera Calibration Matrix and Distortion Coefficients](<./P2 Advanced Lane Finding.ipynb> ) and [`image.calibrate_camera()`](./output_images/ChessboardCorners/calibration2.jpg)

I loaded the calibration images and converted them to grayscale.  I specified that there are 9 interior corners in the x direction (`nx = 9`) and 6 interior corners in the y direction (`ny = 6`) in these calibration images.  I used the function `cv2.findChessboardCorners()` to find these points in the images, and when they were successfully found I appended the checkerboard indices to the array `objpts` and appended their corresponding pixel coordinates to the array `imgpts`.  I used `cv2.calibrateCamera()` to compute the calibration and distortion coefficients. 
Then I displayed Provided image and Undistorted Images side by side.

### Helper Functions
1. abs_sobel_thrshld: This Function Defines and calculates absolute value of Sobel Threshold.
2. color_thrshld: This Function Defines and calculates color threshold.
3. window_msk: This Function works with the window mask.
4. window_cntrds: This Function Defines and calculates  Centroids.
5. poly_fit: This Function Defines and calculates the polinomial curve fitting.
6. poly_search_area: This Function decides the polinomial and visulizes the area of interest.
For Detailed image analysis I did, please look at this [Folder](./output_images)

### Generating the Pipeline

This Function, as the name suggests generates the pipeline and displays on the image, this function is using all the helper functions mentioned above and applies them to the image, including undistorting the images and drawas the lane lines and borders.
For detailed explaination please see the code [here](<./P2 Advanced Lane Finding.ipynb>)  and search for "Generating the Pipeline"

### Check the images from pipeline
Here I have displayed and checked
1. Undistorted Images
2. Binary Images
3. Perspective Transformed Images
4. Sliding Window Images
5. Images with fitted Lane Lines
6. Images with Curvature displayed

### Modify the pipeline and apply to the video
In this part of notebook, I have defined `img_processing()` Function to modify the pipeline I generated earlier to fit out video needs and Finally apply it to the Pipeline to generate the video.
Here's a [link to my video result](./Advanced-Lane_Finding_achived.mp4)

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

**Challenges**

Getting good quality binary images was more difficult than I expected, and I still feel that there is room for improvement on this aspect of the project.  The difficulty was detecting the lanes but not detecting other edges, such as those due to shadows or the edge of the road.  

**Where the pipeline struggles**

As can be seen in the [video](./Advanced-Lane_Finding_achived.mp4), this pipeline struggles when the car bounces and when the slope of the road changes.  This is because the hard-coded perspective transform parameters are no longer correct.  A better approach might involve finding the vanishing point of the images; that is, the point to which all straight lines converge.  

**Where the pipeline might fail**

A couple scenarios that could cause this pipeline to fail include:

1. **The car is not driving within the lane lines.**  This pipeline assumes that the car is driving more or less in the center of the lane.  It would likely fail if the car were to change lanes because the model is simply not designed to handle a complex case such as this, especially considering that I used a mask which zeroed out the left, right, and center of the image in order to obtain the binary thresholded image.  

2. **The binary thresholding portion of the pipeline fails to eliminate edges which are not lane lines.**  While I did not work with the challenge video, a previous version of my pipeline performed poorly on that video due to the line in the middle of the lane between the light and dark asphalt.  The model might struggle with words or signs painted on the road, such as "SLOW" or a crosswalk.  In order to accurately fit a polynomial to the lane lines, we need to have good data points with minimal noisy data points.  

And Finally I would like to Thank [Jeff Irion](https://jefflirion.github.io/udacity/index.html#self-driving-car) for providing deep insights and a [great writeup](https://github.com/JeffLIrion/udacity_car_nanodegree_project04/blob/master/README.md)