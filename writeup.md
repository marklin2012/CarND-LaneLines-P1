# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[grayscale]: ./source/gray.jpg "grayscale"
[blurgray]: ./source/blur_gray.jpg "blurgray"
[edges]: ./source/edges.jpg "edges"
[masked_edges]: ./source/masked_edges.jpg "masked_edges"
[lines]: ./source/lines.jpg "lines"
[lines_edges]: ./source/line_edges.jpg "lines_edges"
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. 

1. I converted the images to grayscale.
![grayscale]
2. I use using guassian_blur function with kernel_size of 7 to blur gray my grayscale image.
![blurgray
3. Then I user canny function with low_threshold 50 and high_threshold 150 to get the edges.
![edges]
4. Then I keeps a region of interest defined by the polygon with a vecters, and I get a mask edge
![masked_edges]
5. After the masked_edges image, I use hough_lines function with some parameters to  obtain the Hough lines list from the masked area. And I also modified draw_lines function to get lines.
![lines]
6. Finally, I combine the lines image and origin image copyed to get the final image, then return the image to complete the pipeline function.
![lines_edges]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
