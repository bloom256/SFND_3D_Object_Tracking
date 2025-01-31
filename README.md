# SFND 3D Object Tracking

Welcome to the final project of the camera course. By completing all the lessons, you now have a solid understanding of keypoint detectors, descriptors, and methods to match them between successive images. Also, you know how to detect objects in an image using the YOLO deep-learning framework. And finally, you know how to associate regions in a camera image with LIDAR points in 3D space. Let's take a look at our program schematic to see what we already have accomplished and what's still missing.

<img src="images/course_code_structure.png" width="779" height="414" />

In this final project, you will implement the missing parts in the schematic. To do this, you will complete four major tasks: 
1. First, you will develop a way to match 3D objects over time by using keypoint correspondences. 
2. Second, you will compute the TTC based on LIDAR measurements. 
3. You will then proceed to do the same using the camera, which requires to first associate keypoint matches to regions of interest and then to compute the TTC based on those matches. 
4. And lastly, you will conduct various tests with the framework. Your goal is to identify the most suitable detector/descriptor combination for TTC estimation and also to search for problems that can lead to faulty measurements by the camera or LIDAR sensor. In the last course of this Nanodegree, you will learn about the Kalman filter, which is a great way to combine the two independent TTC measurements into an improved version which is much more reliable than a single sensor alone can be. But before we think about such things, let us focus on your final project in the camera course. 

## Dependencies for Running Locally
* cmake >= 2.8
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* Git LFS
  * Weight files are handled using [LFS](https://git-lfs.github.com/)
* OpenCV >= 4.1
  * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors.
  * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

* PCL>=1.7
  * sudo apt-get install libpcl-dev

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level project directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./3D_object_tracking`.



## FP.1 Match 3D Objects

matchBoundingBoxes() function is implemented. Each bounding box is assigned the match candidate with the highest number of occurrences.



## FP.2 Compute LIDAR-based TTC

computeTTCLidar() function is implemented. Outliers removing is implemented in RemoveLidarOutliers() function, which do Euclidean clustering on LIDAR points, and removes small clusters.



## FP.3 Associate Keypoint Correspondences with Bounding Boxes

 clusterKptMatchesWithROI() function is implemented.



## FP.4 Compute Camera-based TTC

computeTTCCamera() function is implemented.  Median distance ratio is used to deal with outliers.



## FP.5 Performance Evaluation 1

<img src="images/LidarFluct.png" width="779" height="414" />

Small fluctuations in LIDAR data, just a few centimeters, can lead to wrong results when relative speed between ego vehicle and tracked vehicle is small. On the graph above we see the wrong estimation at frame 7. Estimated distance delta (frame 6 and 7) is 2cm, but real distance delta is about 6cm. 4cm distance error leads to 20s TTC error.
The same thing happens when vehicles are not moving - LIDAR estimated min distance is a little different every frame, which leads to the wrong TTC. For example, at frame 56 estimated TTC is 27s when real TTC is infinite. Again 5cm distance error leads to wrong TTC estimation.  



## FP.6 Performance Evaluation 2

All combinations of detector/descriptor were tried.
The following graph show the results.

<img src="images/TTCChartAll.png" width="779" height="414" />

There are a lot of the wrong TTC estimations. It caused by two main reasons. The first one is a small number of keypoints. And the second one is the outliers.

But there are a set of combinations that gives pretty good results. We can see it in the following chart.

<img src="images/TTCChartBest.png" width="779" height="414" />

We can choose any robust detector/descriptor combination to fuse it with LIDAR TTC. For example, AKAZE-BRIEF looks like a good choice.

<img src="images/TTCAkazeBrief.png" width="779" height="414" />