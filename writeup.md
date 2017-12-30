# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: test_images_output_v2/solidWhiteRight.jpg "Static"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

For my first attempt at detecting the road lines I decided to go with the basic pipeline described in the lesson:  
- First turn the imae to grayscale.  
- Then apply a small blur to the image.  
- After that, apply the canny edge detection algorythm.  
- Keep only the detected edges within the region of interest.  
- Finally, apply the hough lines algorythm to find the interesting lines.

This was working for the first two videos, and worked right for the challenge except for the part when the road turns lighter, so I thought about what I could do to improve the performance no matter the color of the road.
I decided then to change the **grayscale** step for a color filtering, so I put all the pixels of my image to black except those where the color was white(ish) or yellow(ish) so I effectively removed the road from the equation, now all the algorithm was able to see are the road lines. 

Finally, I decided to use the EMA of the detected lines in order to avoid noise and make the predictions much more smooth.

---

My approach for modificating the `draw_lines()` function is the following:

There doesn't seem to be a unique way of averaging segments, we don't actually want a real average because we want the resulting lines to begin at the bottom of the image.  
First of all I started by filtering out all the lines whose slope was too flat to be part of the road lines.  
After that, I followed the exercice's suggestion and divided the lines in right/left depending on if their slope was positive or negative.
Once splitted in left/right segments I decided to find the intersection of every segment with the horizontal line at the bottom of the image, and average those values to decide where the line should start, then I averaged the slope of all the lines so I ended up with an average starting point and an average slope.  
With that I could draw the lines (always of the same length).

![alt text][image1]
#### Videos:  
[solidWhiteRight](test_videos_output_v2/solidWhiteRight.mp4)  
[solidYellowLeft](test_videos_output_v2/solidYellowLeft.mp4)  
[challenge](test_videos_output_v2/challenge.mp4)  


### 2. Identify potential shortcomings with your current pipeline


Fot this exercice the pipeline works good enoughl but it would be useless in a real scenario because, first of all we're using a hardcoded region of interest, if the camera moved by 1 inch the data wouldn't be useful anymore, second of all we're working with specific light conditions, if those were to change the algorythims wouldn't work anymore, nor they will be able to detect the speficic (also hardcoded) yellow and white colors.
If there was some kind of shadow or obstacle in the road the pipeline will not work anymore, and if we were changing lanes it would go crazy.

### 3. Suggest possible improvements to your pipeline

I think a good way to go would be to, instead of looking for specific features like slope, color or a region of interest, train our model to identify aything that looks like a road line, under any kind of light conditions, or no matter the angle of the camera or if there's an obstacle.
