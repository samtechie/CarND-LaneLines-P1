
**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: grayscale.jpg "Grayscale"
[image2]: edges.jpg "Canny"
[image3]: region_of_interest.jpg "Region of Interest"
[image4]: final_result.jpg "Final Result"

---

### Reflection

### 1. My Pipeline.

My pipeline consisted of the following steps. I got all the the test images and iterated over them as I applied each transformation.
The first thing I did was apply the Grayscale transform.
![alt text][image1]

I then applied the Gaussian blur to reduce some of the noise in the images since we are interested in lines that standout.

I then get the edges by using the Canny transform which creates a binary image by looking at the strength of the Pixels

![alt text][image2]

I then need to crop out the areas that I don't expect to find lane markings using the region_of_interest() function

![alt text][image3]

The next step is to apply the hough_lines() function which takes a while to understand particularly with regard to the parameters. With hough transform we are trying to find lines. The canonical equation y = mx + c represents a line. This doesn't quite work well because if we have a vertical line m will be infinity. We instead use the parametric form which is rho = xcostheta + ysintheta where rho is the distance from the line to the origin and theta is the angle between the line and the origin. The hough algorithm creates a grid rows denoted by rho and columns denoted by theta.

Our Hough transform iterates through our binary image with only 0s and 1s and anytime it finds a 1 it iterates through all the row resolutions. Because we want a high resolution for theta we pick 1 degree. We convert this to radians and this becomes our theta resolution. Our maximum rho resolution measured in pixels is 1. The algorithm will go through all the xs and ys in our binary image and keep testing theta values upto 360 degrees to calculate our rho. This is like a voting system on the best theta and rho values. The best pair can actually be converted to line ie from hough space to image space.

![alt text][image4]


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by
Calculating the right and left slopes for both the x any y values for the two lines. I iterate through all the points of the line and compare with the slope. If the slope is less than 0, I assign it to the left line else I assign to the right line. I then get the mean for the x and y values for each of the lines such that am left with two points for both the left and right line. I then estimate the the distance to the top of my image and to the bottom. I then use these two points and the calculated slope to extrapolate the lane lines


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the images are in darker environments and the lane markings are not clear. This pipeline will clearly not work.

Another shortcoming could be with the the smooth corners because our Hough transform only recognizes lines.
How would this even work when a car is in traffic and you can't see the Lane Lines


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use to use linear regression to calculate the average slope

Another potential improvement could be to the parameter selection which is now mainly guessing.  I would overlay the lines generated by hough transform which would assist me in choosing the parameters.
