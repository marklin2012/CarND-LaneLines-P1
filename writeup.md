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

![alt text][grayscale]

2. I use using guassian_blur function with kernel_size of 7 to blur gray my grayscale image.
![alt text][blurgray]
3. Then I user canny function with low_threshold 50 and high_threshold 150 to get the edges.
![alt text][edges]
4. Then I keeps a region of interest defined by the polygon with a vecters, and I get a mask edge
![alt text][masked_edges]
5. After the masked_edges image, I use hough_lines function with some parameters to  obtain the Hough lines list from the masked area. And I also modified draw_lines function to get lines.
![alt text][lines]
6. Finally, I combine the lines image and origin image copyed to get the final image, then return the image to complete the pipeline function.
![alt text]![lines_edges]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function.

1. I separate the line to left or right with the slope and is in the right side or left side.then I find and record the minimum and maximum value of x and y in each of the right or left.

```python
    width = img.shape[1]
    height = img.shape[0]
    
    minLX = width
    minRX = width
    
    maxLX = 0
    maxRX = 0
    
    minLY = height
    minRY = height
    
    maxLY = 0
    maxRY = 0
    
    for line in lines:
      for x1,y1,x2,y2 in line:
        slope = (y2-y1)/(x2-x1) 
        middle = width/2
        
        if slope < 0:
          # 排除右边线的干扰
          if (x1 < middle and x2 < middle):
            minLX = min(x1, x2, minLX)
            maxLX = max(x1, x2, maxLX)
            minLY = min(y1, y2, minLY)
            maxLY = max(y1, y2, maxLY)
        else:
          # 排除左边线的干扰
          if (x1 > middle and x2 > middle):
            minRX = min(x1, x2, minRX)
            maxRX = max(x1, x2, maxRX)
            minRY = min(y1, y2, minRY)
            maxRY = max(y1, y2, maxRY)
```

2. I will use the maximum and minimum value to calculate the top point and bottom point of the right or left side. Then I can draw the two line.

```python
  slope_left = (minLY-maxLY)/(maxLX-minLX)
  slope_right = (minRY-maxRY)/(minRX-maxRX)
  
  bl = maxLY-slope_left*minLX
  br = maxRY-slope_right*maxRX
  top_height = int(height/2+80)
  bottomX_left = (height-bl)/slope_left
  topX_left = (top_height-bl)/slope_left
  
  bottomX_right = (height-br)/slope_right
  topX_right = (top_height-br)/slope_right

  # draw left line
  cv2.line(img, (int(bottomX_left), height), (int(topX_left), top_height), color, 12)
  
  # draw right line
  cv2.line(img, (int(bottomX_right), height), (int(topX_right), top_height), color, 12)
```

### 2. Identify potential shortcomings with your current pipeline

There are a lot of shortcomings tof my current pipeline.

One potential shortcoming would be what would happen when noise point appear, I can't handle it well. The line will make the mistake line.

Another shortcoming could be the left or right line can't not be draw in the right places sometimes.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to try more parameters to deal with the noise.

Another potential improvement could be to make the interest region more small enough.
