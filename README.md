# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.jpg "solidWhiteCurve"
[image2]: ./test_images_output/solidWhiteRight.jpg "solidWhiteRight"
[image3]: ./test_images_output/solidYellowCurve.jpg "solidYellowCurve"
[image4]: ./test_images_output/solidYellowCurve2.jpg "solidYellowCurve2"
[image5]: ./test_images_output/solidYellowLeft.jpg "solidYellowLeft"
[image6]: ./test_images_output/whiteCarLaneSwitch.jpg "whiteCarLaneSwitch"

---

### Reflection

### 1. Pipeline description and line drawing algoritm.

The pipeline is implemented within the find_lanes method. The pipeline consisted of 8 steps. 
* Color masking. White and yellow pixels where masked
* Gray scale conversion. The image was converted to gray scale
* Image smoothing. A 7x7 pixel Gaussian kernel was applied to the image
* Canny edge detector. Edge detection using the canny algorithm. The ration of the low to high threshold was set to 3 and after trial and error values 50 and 150 where selected for low and high threshold respectively.
* Define a polygon to be used as a region of interest (ROI). Initially a triangular region was used but it was later changed to a trapezoid for improved results. The coordinates where identified by trial and error and then approximated with a constant factor with respect to the size of the image
* A ROI was create to be used in conjunction with the hough lines algorithm, using the previously defined trapezoid. 
* Hough lines detection based on a ROI
* Line drawing on the original image

The initial draw_lines algorithm was fine, but the extended requirement called for line extrapolation, such that each side of the lane is drawn at the maximum possible length. The new algorithm is implemented within the draw_lines_improved function. Unlike the original, that draws lines for all coordinates, the improved version iterates over each line, calculating the slope in order to distinguish between left and right lines. 
A first order polynomial was used to fit a line to each set of coordinates that composed the detected set of hough lines. Points were rejected if there was no line fitted. In addition to that vertical lines were rejected (x1=x2) and a threshold of 0.5 was also set to limit incorrectly fitted lines.
The resulting images can be found in the test_images_output folder.
The resulting videos can be found in the test_videos_output folder.

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

Due to the simplicity of the fitted lines (1st order), the extrapolated line did not always fit the actual road line. 
This was the case when a sharp road turn was visible, particularly when the car was driving on the lane at the opposite side. 

Field of view seems to be an important factor also. The algorithm seemed to work better on wide fields of view.

The position of the camera was also important. We used a trapezoid to select the region of interest based on the assumption that the car is actually moving at the center of this region. In some videos, this was not the case, as parts of the road line were outside this region.

The challenge video had all these characteristics.

### 3. Conclusion and Possible improvement

The requirements of the project where met. The road lines on all images where detected with reasonable accuracy, the road lane where drawn and outliers where rejected. 
However, a polynomial of higher degree could be used for line fitting. This would require extensive testing in order to select the appropriate degree, as this will increase processing time, without necessarily giving better results.
In addition to that, a more complex transformation could also yield better results.
