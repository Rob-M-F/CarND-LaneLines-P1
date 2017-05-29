# **Finding Lane Lines on the Road** 


### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline is encapsulated within the find_lanes function and is unchanged between image and video processing. This functiona handles the image preprocessing, lane finding and image postprocessing.  

In preprocessing, the image is reduced to grayscale and cropped to the region of interest. The resulting image is then blurred. The result of this process is then passed on.  

Lane finding consists of canny edge detection followed by hough line detection. Within the hough line detection function, a further filtering step occurs before the final lane markings are drawn. Most of the work of the algorithm is done within this filtering function, filter_lines.  

Filter lines starts by calculating the slope and y-intercept of each candidate line, then extending these lines to the image borders. It then performs the same function for the ROI bounding points. Only the slope and image border points are kept for further filtering. Candidate lines are dropped if the slope is between 0.25 and -0.25, in addition, they are dropped if their endpoints and slope match a previously accepted line too closely.  The lines clipped to a point below the typical horizon and separated by slope into left and right values. If either the left or right list is empty, all lines are returned. Otherwise, the highest slope from each side is returned to hough lines. Finally, draw_lines is called, which draws all of the lines it receives from hough lines.  

Postprocessing simply combines the hough_lines results with the original image.  


### 2. Identify potential shortcomings with your current pipeline

Unfortunately, this approach is naive at handling lines within a lane and fails completely on curves.  It does not always finding lane edges and can be erratic between frames of video.  


### 3. Suggest possible improvements to your pipeline

Modeling lanes and lines in a class structure would allow resolution of these issues. Line modeling can resolve issues with dashed lines and curves. While lane modeling can resolve continuity between frames and apply vanishing point geometry.  