# Blob-Detection

# Introduction
In this project, Blob Detection and Contours are used to find the blobs and the outer wrap. A blob is a group of connected pixels with the same properties, such as color, intensity, or texture. Blob detection algorithms are used for various applications, including object recognition, motion detection, and image segmentation.

# Recognise the inner droplets
At first, the image was dilated and eroded but it is found that this particular method makes the outer wrap with the blobs which is not desired, so initial erosion and dilation are not implemented (see Fig1).

<p align="center">
  <img src="https://github.com/Dharmendra04/Blob-Detection/blob/main/blob_detection_images/img1.png">
  <br />
  <em>Fig. 1. Top: Without the morphological operation, Down: With Erosion and Dilation
</em>
</p>

So initially a masked image was created to extract the brown color blobs separately from the image. cv2.inrange method was used and HSV color scale was used as it has a lot of desirable features in handling colors rather than an RGB scale. As shown in Fig 2, the initial hue value for the brown color was roughly guessed between 0 and 40 and then carefully experimented within this range with the help of Fig 3, by observing whether the detected blobs exactly coincide with the droplets. After some experimentation, the final color range of (10,0,0) and (25,255,255) was selected for the in-range function as the lower and upper boundary. When the lower range is slightly reduced it is not detecting some of the brown droplets and as the higher range is slightly increased it started to detect other parts which are not blobs as can be seen in Fig 3.


<p align="center">
  <img src="https://github.com/Dharmendra04/Blob-Detection/blob/main/blob_detection_images/img2.png">
  <br />
  <em>Fig. 2. The range in HSV color Scale used to pick Brown color
</em>
</p>


<p align="center">
  <img src="https://github.com/Dharmendra04/Blob-Detection/blob/main/blob_detection_images/img3.png">
  <br />
  <em>Fig. 3. For the selected HSV range the Detected Blobs are coinciding with the droplets
</em>
</p>

Simple blob detector was used for detecting the inner blobs. All parameters were set to False instead of Convexity as when changing the parameters required blobs are not getting detected. Minimum Convexity is given as 0.6 to detect the first blob(left side) as this blob is not a full circular shape and part of it will disappear in the main video. After that blobs are sorted in the descending order of the Area and the first 7 blobs are selected and drawn on each frame. Dilation of (4,4) and Erosion of (4,4) were chosen carefully after many experiments. These morphological operations can be beneficial in order to fill the small gaps inside the blobs and smooth the edges of the blobs using erosion. Interestingly in the main output video, the kernel size of(10,10) for Dilation gave less noisy output, but the detected blobs are smaller than the inside droplets (see Fig 4). More importantly, an ellipse is uses as the structural element for the detection as most of the inside droplets are not circular in shape. Because the elliptical shape can match the elongated shape and keep the shape unchanged while removing the noises.

<p align="center">
  <img src="https://github.com/Dharmendra04/Blob-Detection/blob/main/blob_detection_images/img4.png">
  <br />
  <em>Fig. 4. Top: Correctly coinciding Blobs, Down: Blobs are smaller than inside droplets
</em>
</p>

# Recognizing outer wrap
Using contours the outer warp is detected and a binary mask is created to detect the black color using HSV scale. The suitable range for the black color is found as lower limit (0,0,0) and upper limit (50,50,50). cv2.RETR EXTERNAL is used to identify only the outermost boundary of the warp, and cv2.CHAIN APPROX SIMPLE is used only to save only the endpoints of the contour so that it can reduce the memory used for computation.

<p align="center">
  <img src="https://github.com/Dharmendra04/Blob-Detection/blob/main/blob_detection_images/img5.png">
  <br />
  <em>Fig. 5. Top: Only the right part of the image was considered when finding contours, Down: Detected contours in outer wraps

</em>
</p>

More importantly, half of the image section is only consid- ered, as in Fig 5 (top), it can be seen there are some lines on the right side of the figure which may also be considered as a contour and create noise in the output video, to avoid half of the image part is extracted and then cv2.findConours operation was applied.

# Count successfully formed droplets
For counting the successfully formed inner blobs, the center of the 4th blob (from the left side) is tracked within a range of area as shown in Fig 6. If any centres of blobs are detected the number of counts is increased by 1 using count+=1. After calculating the detected blobs for all frames, the following equation is used to calculate the total number of blobs detected. Totally 11 droplets were found using this method and it is displayed in the code.

The total number of frames in the video = 675 frames total number of seconds in the video = 22sec

total number of times taken to create each drop = 3sec

Hence, total frames for creating a drop with inside blobs = 675 x3 = 90f rames 22

Hence, if the total detected droplets = 3 (initial droplets) + total droplets found in all frames ∗ 90

<p align="center">
  <img src="https://github.com/Dharmendra04/Blob-Detection/blob/main/blob_detection_images/img6.png">
  <br />
  <em>Fig. 6. Only the right part of the image was considered when finding contours, Down: Detected contours in outer wraps

</em>
</p>

# Centre of Mass Droplets
The center of mass is predicted using finding the center of blobs for the blobs and using cv2.moments for the outer droplets. The final snapshot shows clearly the detected mass centers for blobs(black) and outer wrap(blue) (ref Fig 7)
All the mathematical explanations are given in Appendix A.

<p align="center">
  <img src="https://github.com/Dharmendra04/Blob-Detection/blob/main/blob_detection_images/img7.png">
  <br />
  <em>Fig. 7. A screenshot of the output video showing inside blob center of mass and outer wrap center of mass

</em>
</p>






