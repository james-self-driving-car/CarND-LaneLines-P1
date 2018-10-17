# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

Submissions
---
1. Writeup (this file)
2. Submitted Ipython notebook with code ([Alt. Github URL](https://github.com/james-self-driving-car/CarND-LaneLines-P1/blob/master/P1.ipynb))

Additionally, I have included a [https://github.com/james-self-driving-car/CarND-LaneLines-P1/blob/master/For-Writeup.ipynb](Jupyter notebook) that I used to generate images to explain some of the cases below.


Pipeline (Image)
---
Here, we have built the pipeline to process an image through a sequence of image transformations for lane detection. 

The pipeline consists of below image transformations:  

<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/01.png"/><br/>
<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/02.png"/><br/>
<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/03.png"/><br/>
<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/05.png"/><br/>
<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/06.png"/><br/>
<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/07.png"/><br/>
<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/08.png"/><br/>
<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/09.png"/><br/>


## Pipeline (video)
In case of videos, the same image transformation steps are carried out for each image frame in the video. Some of the output videos can be seen here on my Github: [https://github.com/james-self-driving-car/CarND-LaneLines-P1/tree/master/test_videos_output](link) <br/>
**Youtube video**<br/>
<a href="https://youtu.be/u_ykyQnfLqM" target="self-driving-car-video"><img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/10.jpg"></a>

## Discussion
1. **Why did I introduce filter for yellow and white colors in the pipeline?**  
With the steps mentioned in the course, I was able to get the 5 images and the 2 videos to render without much issues. But in case of the challenge video, there was a significant number of interfering agents in the Canny transformed image such as shadow, road condition etc. With just the approach taught in the chapters (original image -> grayscale -> blur -> canny -> hough), I was getting a lot of noise in the image at each step.

<img src="https://raw.githubusercontent.com/james-self-driving-car/CarND-LaneLines-P1/master/writeup-resources/noisy.jpg"><br/>
<img src="https://raw.githubusercontent.com/james-self-driving-car/CarND-LaneLines-P1/master/writeup-resources/noisy2.png"><br/>
<img src="https://raw.githubusercontent.com/james-self-driving-car/CarND-LaneLines-P1/master/writeup-resources/noisy3.png"><br/>
<img src="https://raw.githubusercontent.com/james-self-driving-car/CarND-LaneLines-P1/master/writeup-resources/noisy4.png"><br/>
<img src="https://raw.githubusercontent.com/james-self-driving-car/CarND-LaneLines-P1/master/writeup-resources/noisy5.png"><br/>

So I introduced a filter for white/yellow colors in the original image first. With that, the images mostly contained the lane lines in the intermediate steps, hence making lane detection much easier.

2. **Why was Otsu thresholding introduced?**  
Ref: [https://docs.opencv.org/3.4/d7/d4d/tutorial_py_thresholding.html](Image Thresholding)  
The Canny edge detection function requires low and high thresholds. But it's hard and time-consuming to find an optimum value by trial and error. Otsu, on the other hand calculates a threshold value from image histogram for a bimodal image, and returns an a binarized image. Once we find Otsu threshold, it makes it easier to find the low and high thresholds for Canny. I have provided two images below in the context of lane detection. They look pretty similar, but the one with Otsu transformation looks a bit less noisy comparatively.

<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/otsu1.png"><br/>
<img src="https://github.com/james-self-driving-car/CarND-LaneLines-P1/raw/master/writeup-resources/otsu2.png"><br/>

3. How are the two lane lines derived from Hough lines?
