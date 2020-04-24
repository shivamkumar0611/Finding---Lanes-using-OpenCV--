# Finding---Lanes-using-OpenCV--
Identifying lanes of the road is very common task that human driver performs. This is important to keep the vehicle in the constraints of the lane. This is also very critical task for an autonomous vehicle to perform. And very simple Lane Detection pipeline is possible with simple Computer Vision techniques. This article will describe simple pipeline that can be used for simple lane detection using Python and OpenCV. This exercise is done as part of “Udacity Self driving Car nanodegree”.

Lane Detection Pipeline:
Convert original image to grayscale.
Darkened the grayscale image (this help in reducing contrast from discoloured regions of road)
Convert original image to HLS colour space.
Isolate yellow from HLS to get yellow mask. ( for yellow lane markings)
Isolate white from HLS to get white mask. (for white lane markings)
Bit-wise OR yellow and white masks to get common mask.
Bit-wise AND mask with darkened image .
Apply slight Gaussian Blur.
Apply canny Edge Detector (adjust the thresholds — trial and error) to get edges.
Define Region of Interest. This helps in weeding out unwanted edges detected by canny edge detector.
Retrieve Hough lines.
Consolidate and extrapolate the Hough lines and draw them on original image.

Convert to grayscale
Converting original image to grayscale has its benefit. We have to find yellow and white lanes , and converting original image to grayscale increases the contrast of lanes with respect to road.

Darken the grayscale image
This is done with the intent of reducing contrast of discoloured patches of the road.

Convert original image to HLS colour space
Original images are in RGB, but we should also explore other colour spaces like HSV and HLS. When looked at side-by-side, one can easily see that we can get better colour contrast in HLS colour space from road. This may help in better colour selection and in turn lane detection.

Colour Selection
Here we use OpenCV’s inRange to get mask between thresh hold value. After some trial and error, we can find out range for threshold.
For yellow mask:
Hue value was used between 10 and 40.
We use higher saturation value(100–255) to avoid yellow from hills.
For white mask:
We use higher lightness value (200–255) for white mask.
We bit-wise OR both mask to get combined mask.

Apply Canny Edge Detection
Now we apply Canny edge detection to these Gaussian blurred images. Canny Edge Detection is algorithm that detects edges based on gradient change. Not that the first step of Canny Edge detection is image smoothing with default kernel size 5, we still apply explicit Gaussian blur in previous step. The other steps in Canny Edge detection include:
Finding Intensity Gradient of the Image
Non-maximum Suppression
Hysteresis Thresholding

Select Region of Interest
Even after applying Canny Edge Detection, there are still many edges that are detected which are not lanes. Region of Interest is a polygon that defines area in the image, from where edges we are interested.
Note that , the co-ordinate origin in the image is top-left corner of image. rows co-ordinates increase top-down and column co-ordinates increase left-right.
Assumption here is camera remains in constant place and lanes are flat, so that we can “guess” region of interest.


Hough Transformation Lines Detection
Hough Transform is the technique to find out lines by identifying all points on the line. This is done by representing a line as point. And points are represented as lines/sinusoidal(depending on Cartesian / Polar co-ordinate system). If multiple lines/sinusoidal pass through the point , we can deduce that these points lie on the same line.

Consolidation and extrapolation of the Hough lines
We need to trace complete lane markings. For this the first thing we need to distinguish between left lane and right lane. There is easy way of identifying left lane and right lane.
Left Lane: As the values of column co-ordinate increases , the values of rows co-ordinate decreases. So the gradient must be negative.
Right Lane: As the values of column co-ordinate increases , the values of rows co-ordinate increases. So the gradient must be positive.
We’ll ignore the vertical lines.
After identifying left lane and right lane Hough lines , we’ll extrapolate these lines. There are 2 things we do:
There are many lines detected for Lane, we’ll average the lines
There are some partially detected lines , we’ll extrapolate them.


Applying Pipeline to videos
Now let’s apply this pipeline for videos.
Pipeline works pretty good for straight lanes.
