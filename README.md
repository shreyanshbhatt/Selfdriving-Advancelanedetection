## Writeup
---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image0]: ./output_undistorted_calib1.png
[image1]: ./test_images/test2.jpg "Test2 Original image"
[image2]: ./output_images/test2_calib.jpg "Test2 calibrated"
[image3]: ./output_images/test2_persped.jpg "Test2 Perspective"
[image4]: ./output_images/test2_thresholded_persped.jpg "Test2 thresholded perspectived"
[image5]: ./output_images/test2_final.jpg "Test6 final"
[image6]: ./test_images/test6.jpg "Test6 Original image"
[image7]: ./output_images/test6_calib.jpg "Test6 calibrated"
[image8]: ./output_images/test6_persped.jpg "Test6 Perspective"
[image9]: ./output_images/test6_thresholded_persped.jpg "Test6 thresholded perspectived"
[image10]: ./output_images/test6_final.jpg "Test6 final"

---
### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

I used all the chess board images provided with the project. I used the findChessboardCorners utility of opencv to find corners. Using all the images that were provided, calibration\*.jpg, the system was able to learn the distortion correciton matrix by mapping the corners to corners found in distorted image. An example undistorted image looks like following.
![alt text][image0]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image0]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image. The code for it is in the adv_lane_det.ipynb code block In [842]. I started with using only s channel however I found out that only s channel is not enough to identify both lines. However, it identified yellow lane lines accurately. Next I tried using sobel in x direction on gray scaled image. This identified the white lanes. Combined this with the s channel, I was able to identify the lane lines in binary image. Please refer to the code block In [851] for results of s_channel, sx, combined binary output.
I also tried combination of sobel in x direction, y direction and as well as gradient. However, it did not improve results.

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.
The code for perspective transform is in code block In[844]. I performed the perspective transform each time with each test image assuming that the camera position did not change as well as lane width. I chose 4 corners points (statically for each image). The resulting perspective transformed image looks like following.

![alt text][image1]
![alt text][image3]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I used the histogram and widowing as suggested in the project to identify lines. The code for which is in code block In[849] method window_based_line_det. Following examplifies lane detected in perspective transformed binary image

![alt text][image1]
![alt text][image4]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I calculated the curvature using the left lane points. The x cordinates and y cordinates of left lane points were used for this. The code for which is in the code block In[849]find_curvature. I calculated the vehicle position using the camera mount information provided in the project as well as left and right lane x position for each y position. The code for which is in same code block in method find_position.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

Final result on test image 2 and test image 3 looks like following.

![alt text][image2]
![alt text][image5]
![alt text][image6]
![alt text][image10]

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here is a [link to my video result](./project_video_output.mp4)

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The pipeline was able to work adequately on the project_video.mp4 however it had difficulties on the challenge video specifically with not being able to identify yellow lane markings. It worked well otherwise. The reason for which I believe is s_channel threshold. Also, using s_channel combined with r_channel could improve results. Another thing I would like to try is to use the same pipeline and tuning the thresholds for different weather and/or night time.
