## Advanced Lane Finding Project

---

**Goals**

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

[image1]: ./output_images/undistorted/calibration3.jpg "Undistorted Calibration Image"
[image2]: ./output_images/undistorted/test5.jpg "Undistorted Road Image"
[image3]: ./output_images/thresh_binary/test5.jpg "Binary Thresholded Road Image"
[image4]: ./output_images/warped/test5.jpg "Warped Binary Road Image"
[image5]: ./output_images/warped_lanes/test5.jpg "Warped Binary Road Image with Lanes"
[image6]: ./output_images/final_output/test5.jpg "Final Output Road Image"
[video1]: ./project_video_output.mp4 "Project Video Output"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./P2.ipynb"

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![Undistorted Calibration Image][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![Undistorted Road Image][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps in "./P2.ipynb" in Section 3. Gradient and Color Thresholds to Create a Binary Image).  Here's an example of my output for this step.

![Binary Thresholded Road Image][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform is in P2.ipynb in Section 4. Perspective Transform.  I chose to hardcode the source and destination points in the following manner:

```python
src_points = np.float32([[596,449],[684,449],[1054,678],[253,678]]) # clockwise starting from top left
dst_points = np.float32([[300,0],[930,0],[930,y_max],[300,y_max]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 596, 449      | 300, 0        | 
| 684, 449      | 930, 0        |
| 1054, 678     | 930, 720      |
| 253, 678      | 300, 720      |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![Warped Binary Road Image][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

In Sections 5.1 and 5.2 of P2.ipynb, I coded the sliding windows method and search around method respectively. Here's an output image from this step. Please note that the sliding windows and the search around area have not been indicated here for easier usage in later steps and for the video processing.

![Warped Binary Road Image with Lanes][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

Section 6.1 of P2.ipynb has the functions for these calculations.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in Section 6.2 of P2.ipynb in the function `draw_lanes_orig()`.  Here is an example of my result on a test image:

![Final Output Road Image][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [Project Video Output][video1]

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

**Problems faced / Potential Failure Scenarios**
Slow performance. It took about 5 minutes on my laptop to process a 50 second video. In real life scenarios, this is obviously unacceptable because video feed needs to be processed in real time for an actual car. I did make several performance improvements to bring it to this level, some of which have been described in comments in the code in Sections 5.1 and 5.2.

**Potential Improvements**
Use deep learning based solutions to speed up lane detection.