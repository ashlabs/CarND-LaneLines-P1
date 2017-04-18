#**Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

First of all, the base notebook was re-structured. My preference is to have one function in each cell of the notebook. All import statements and constants/configuration items grouped at the start.

My pipeline is contained in the function "`lane_detection_pipeline`". It consists of the following steps:

1. Convert to grayscale image
2. Smoothen the image using gaussian blur with a kernel size of 5
3. Detect edges using canny edge detection
4. Define a region of view that we need to focus on
5. Extract hough lines that fall within the region of view
6. Filter out the detected lines (discard any horizontal lines)
7. Segregate the detected lines into left lane lines and right lane lines based on positive/negative slopes
8. Compute the slope and intercept for each of these groups (y = mx + c)
9. Using the above values generate the co-ordinates (x1,y1,x2,y2) for the 2 lines that will define the left and right lane
10. Draw the lines on a blank image
11. Overlay the line image over the original image

I made a couple of modifications to the functions that were initially provided:

* "Hough Line" - Instead of returning an image with the lines drawn on it, I modified it to return just the lines so that I can do further processing before imprinting them onto an image
* "draw_lines" - I created an alternative function to draw a single line accepting parameters x1,x2,x3,x4. This was to avoid the extra step fo creating and array and parsing the array for the final 2 lines that the pipeline generates.

To visualize how the pipeline works, the pipeline takes a second parameter "test_mode" which by default is set to False. If a True value is passed, it generates a grid of images that show the transformation at each of the steps listed above. 

`def lane_detection_pipeline(img, test_mode = False):`



###2. Identify potential shortcomings with your current pipeline

I researched multiple papers published on lane detection. I wish I could have implemented my learnings from those papers. Unfortunately due to the time constraint, my current implementation has the following shortcomings:

1. Curved lane handling - Currently it works welll for straight lanes within view
2. Dynamic determination of region of view - The current implementation has a fixed polygon to clip out the view
3. Determination of vanishing point and then filtering out line segments that compose the lane - Current solution has a fixed vanishing point and a fixed angle for filtering out lanes
4. Lateral position of vehicle within a lane - The solution assumes that the vehicle is in the middle of the given lane. If the vehicle is driving on either edge of the lane, the solution clips some parts of the lane
5. Efficiency - The code has more optimization that can be done. Currently it works well when you run it on your laptop/desktop. But the hardware in vehicles will be more constrained
6. Carrying over data from the previous frame - The current solution treats each frame in the video as an independent picture and computes. If computation done on the previous frame is carried over to the next frame, it can make the solution more robust and efficient.

###3. Suggest possible improvements to your pipeline

All of the points that I have listed in the previous section are areas for improvement. I will continue to improve on the solution (without the time pressure :) )

