# Finding Lane Lines on the Road

The goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

## Table of Contents

1. [Directory Structure](#directory-structure)
1. [Usage](#usage)
1. [General Approach](#general-approach)
1. [Reflection](#reflection)
1. [Potential shortcomings with the current pipeline](#potential-shortcomings-with-the-current-pipeline)
1. [Possible improvements to the current pipeline](#possible-improvements-to-the-current-pipeline)
1. [About](#about)

## Directory Structure

```plain
│   P1.ipynb                            <- Project Notebook
│   README.md
│   writeup.md                          <- Project Writeup
│   writeup_template.md
│
├───examples
│       grayscale.jpg
│       laneLines_thirdPass.jpg
│       line-segments-example.jpg
│       P1_example.mp4
│       raw-lines-example.mp4
│
├───test_images
│       solidWhiteCurve.jpg
│       solidWhiteRight.jpg
│       solidYellowCurve.jpg
│       solidYellowCurve2.jpg
│       solidYellowLeft.jpg
│       whiteCarLaneSwitch.jpg
│
└───test_videos
        challenge.mp4
        solidWhiteRight.mp4
        solidYellowLeft.mp4
```

## Usage

You just need to run the `P1.ipynb` notebook. Don't forget to activate the [carnd-term1](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) environment beforehand.

```bash
source activate carnd-term1
jupyter notebook P1.ipynb
```

## General Approach

I took a *pythonic* approach to the problem. My goal was to find a consistent, general yet simple solution.
As such, the challenge video lane detection isn't perfect but still very good considering the approach.

The benefits of such an approach is that it focuses on understanding how to adjust the parameters of each very essential OpenCV and Numpy functions in order to find lane line within *poor* conditions.

In the future, I would be able to iterate on this solution to create a better one but without having to add too many custom filtering for specific use cases.

## Reflection

### Pipeline Description

The lane lines detection pipeline consists of 7 steps:

1. Grayscale conversion
1. Noise reduction using GaussianBlur filter
1. Edges detection using Canny filter
1. Selecting Region of Interest (ROI) using fillPoly
1. Lines detection using HoughLinesP filter
1. Finding lane lines in the detected lines with the custom draw_lines() function (explained in [Drawing Lines](#drawing-lines))
1. Adding found lines to initial image using addWeigthed

In the 2 following subsections, I detail the cropping of the ROI and the draw_lines() function which are the only parts that includes changes of my own.

### Cropping Region of Interest

The Region of Interest (ROI) is the region comprised of the road lane and lateral lines. This region is calculated from the shape of the image. Hence, it can handles different size of image. However, the camera needs to be placed with the same position and orientation in the car as in this project test videos.

### Drawing Lines

The HoughLinesP filter returns many detected lines, some belongs to road marker edges, others to undesired asperities from the road. This is where the function draw\_lines() comes in. With the draw_lines() function, we find the lane lines and we draw a line over each of them. These are the steps used:

1. Calculate lines slope
1. Get rid of outlier lines according to their slope deviation from a slope reference (remove lines of the road asperities)
1. Seperate lines between left (negative slope <0) and right (positive slope >0)
1. Fit a polynomial of degree 1 (linear regression) to the left and right lines
1. Draw the 2 lane lines using the left and right polynomial

The step 2 is very usefull to improve the lane detection in the challenge video.

As you can see, the solution is very basic but efficient.

## Potential shortcomings with the current pipeline

First, there will always be shortcomings as long as the script has not been tested on a very big amount of road scenes.

That being said, there are some obvious limitations in this solution that we can already specify:

* Bad edge detection in hard conditions (night, etc.)
* Inaccurate lane detection in very curved road

## Possible improvements to the current pipeline

* Use HSV colors and different filters that improve edge detection according to road conditions (night, etc.)
* Fit a polynomial of higher degree to improve the accuracy of the lane detection for curved roads

## About

### Authors

* **Julien Grave** - Udacity SDCND student - [JulienGrv](https://github.com/JulienGrv)

### Aknowledgements

* https://github.com/davidawad/Lane-Detection/blob/master/notebook.ipynb
* https://peteris.rocks/blog/extrapolate-lines-with-numpy-polyfit/
