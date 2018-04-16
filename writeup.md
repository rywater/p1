# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline is primarily six different steps. Most of the parameters came from trial and error based off of initial values recommended during the lessons. Most parameter changes were due to experimentation and "gut-feeling". In any case, first, the image is converted to greyscale. Second, a gaussian_blur is added to soften the image some. After this, Canny edge detection is used to find any sharp edges, which is then narrowed down by an area of interest that seems appropriate for this particular set of images. Lastly, Hough lines are added and drawn which is then weighed and laid over the original image.

The draw_lines function was modified to approximately bucket lines by their slope. I chose to round slope values to use as a key in a dict for each bucket. From there, I added a min/max reference point for that particular slope to see what area was already likely represented. For most of the images, my image processing already extended or connected most lines pretty well. However, issues arrose connecting dashed lanes to the bottom of the image. To fix this, I use the bottom point referenced by the slopes I've tracked and draw a line from there to the bottom of the image contuing the slope. This creates a somewhat continuous line through the area of interest. I chose not to draw a single line because I had some difficulty bucketing the lines and instead chose to leave outliers as artifacts of the process should they appear. That being said, I increased the thickness of the lines to give closely adjacent ones the appearance of being roughly a single line.


### 2. Identify potential shortcomings with your current pipeline

The area of interest is of generous size which sometimes lets stray lines in. This is especially obvious with the optional challenge. I could spend time finding a more optimal area to use.

I don't filter out lines that aren't within a certain slope range. I could probably assume that lanes have roughly a certain slope, and filter appropriately. This would mitigate problems with horizontal lines being highlighted.

I don't draw the whole length of the line across the area of interest, this sometimes gives a bit of a jittery feel to dashed lanes that aren't a strongly displayed.

### 3. Suggest possible improvements to your pipeline

I could narrow the area of interest to help filter out strays a bit better.

I could assume a certain range for slopes that they are likely to appear in, mitigating the appearance of horizontal lines or sharper angles.

I'd also like to dial back on the initial processing. Tweaking parameters for some of the algorithms feels a bit like guesswork to me rather than intentional design. I worry that tweaking things too much in one direction exposes the pipeline to weaknesses that I'm not aware of.

I could retain a "history" of the line position and average against that as well. Right now, my lines will come and go on the dashed side since each frame is treated independently. If instead I assumed that lanes were likely to be in roughly the same place over the course of many frames, I could probably remove much of that jitter and created smoother lines.