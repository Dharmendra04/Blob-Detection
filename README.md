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





