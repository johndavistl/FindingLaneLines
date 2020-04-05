
# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

#### Rubric specification
    1. The output must be an annotated version of the input video
    2. Annotation of left and right lane lines
    3. Visually full extend of lane lines are covered
    

[//]: # (Image References)

[image3]: ./test_images/solidWhiteCurveout.jpg
---

### Reflection

I really enjoyed the project. The classes were upto providing a clear understanding of the problem. I also got useful help from **Knowledge Section**; an answer by mentor ```Mr. Mate B ```helped in compeleting the project.

### 1. Pipeline

My pipline can be divided into 2 sections and 6 steps.<br><br>
In first section,<br>
   * we take the image (frames in a video) and change it to a gray scale image. ```cv2.cvtColor()```<br>
   * The gray scale image is again smoothened with Gaussian Blurring.```cv2.GaussianBlur()```<br>
   * That image is given to Canny Edge Detection.```cv2.Canny()```<br>
   Now we have to work on finding the lane lines.<br>
   Upto this was easy and from the lectures.<br><br>
   Image can be found at [Canny Output](./test_images/out1.jpg)<br>
   <br>

In second section, <br>
   * The image with edges is processed to take only the portion with the lane lines and quadrilateral masking function is used to do that.<br>```cv2.bitwise_and()``` function is used where 'image' and 'mask' is given as input and mask is the portion selected from image according to quadrilateral<br>
    Region selected from the Canny output can be found at [Masked Ouput](./test_images/out2.jpg)<br>
   * The masked region is the given to Hough Transform function to find lines corresponding to points (points in the line is given as output) ```cv2.HoughLinesP()```<br>
   * The points from line is given to ```draw_lines()``` function to draw line on lane lines in image<br>
    
 The output from 'draw_lines()' function is combined with original image to get the lane lines annotated. <br>
 The ```cv2.addWeighted()``` is used to annotate the original image.
    ![alt text][image3]
    
  ### draw_lines()
  
  This function is created by some help from a blog describing use of ```np.polyfit()``` in Linear Regression<br>
  Input : points returned by Hough function . The preprocessing steps involved:
    * The points from Hough Transformation is selected according to **slope** 
         (slope_threshold is used to discard lines).
    * The points (belongs to line) are then classified to **left lane line points** and **right lane line points**
    * **Linear Regression** is used to find parameters of line fitting the lane lines ```np.ployfit()```
    * The parameters then found is used to give lower and upper end points of lane lines
    * The lines are drawn using ```cv2.line()```<br>
    The left and right lane lines are drawn seperately<br>
    <br>
    Vectors are used to create the line for lane lines but their performance was poorer in videos.<br>

### 2. Identify potential shortcomings with your current pipeline
    
    1. The lines shivers in the video.
    2. Curves are not possible to annotate. 
    3. Video with a different size and conditions fails in this pipeline (Challenge video)
    

### 3. Suggest possible improvements to your pipeline

    1. Automate pipeline according to changing image properties. (inferring masking area - vertices) 
    2. Improve the 'draw_lines()' to remove shaking in subsequent video frames, by averaging changes to coordinates in these frames.
       
    
