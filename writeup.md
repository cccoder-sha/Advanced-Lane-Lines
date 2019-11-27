
[//]: # (Image References)

[image1]: ./image/undistorted_mage.png "Undistorted"
[image2]: ./image/pipeline_exampleImage.jpg "Road Transformed"
[image3]: ./image/grad_img.PNG "Binary Example"
[image4]: ./image/wraped_image.PNG "Warp Example"
[image5]: ./image/fit_colot.png "Fit Visual"
[image6]: ./image/pipeline_image.png "Output"
[video1]: ./test_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the third code cell of the IPython notebook located in "./Advanced-Lane-Lines/P2_v2.ipynb"
I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at 5th conde cell in ./Advanced-Lane-Lines/P2_v2.ipynb ).  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `corners_unwarp()`, which in the 4th code cell of the IPython notebook.  The `corners_unwarp()` function takes as inputs an mtx (`mtx`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[540, 488],
    [746, 488],
    [1100, 718],
    [212, 718]])
dst = np.float32(
    [[280, 490],
    [1000, 490],
    [1000, 718],
    [280, 718]])
```

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the 9th code cell and 11th code cell in my code in `P2_v2.ipynb`

In the 9th code cell ,I define a function called measure_curvature_pixels().
The `measure_curvature_pixels()` function takes as inputs an ploty (`mtx`), left_fit (`left_fit`) and right_fit (`right_fit`) . 

In the 11th code cell, I called measure_curvature_pixels() function to calculate the radius of leftand right.
And then take the average of both radius as the final radius.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in the 8th code cell.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./test_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Although I complete the project by detected the Lane-lines.
But I just took the most simple way that just combine the sample code together from the lessons before.
I did not know how to use the Class Line().But it seems useful to improve the process.

Secondly, I did not know well how to debug it.For example, at the  test_videos's 19s-21s, detected lines wobbled a little.
I could not found the reason, but just know the result from the video. So that I could not improve it.

