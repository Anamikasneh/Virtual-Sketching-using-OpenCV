# Virtual-Drawing
Virtual Drawing with color detection using Python and OpenCV library.

For a full presentation of the working project watch the [video](https://www.youtube.com/watch?v=XUWEyNa0Bcw) in Youtube or click on the image-link below.

[![Virtual-drawing-using-Python-and-OpenCV](http://img.youtube.com/vi/XUWEyNa0Bcw/0.jpg)](https://www.youtube.com/watch?v=XUWEyNa0Bcw "Virtual Drawing using Python and OpenCV | Computer Vision")

## Main Idea
The main idea of this project is to allow user to draw anything "virtually" on screen with the help of a web camera.

### [1] Two project windows
The first window (_Frame_), which is also the main one, shows what the camera "sees" with the addition of shapes (rectangles) in order to define regions of interests from which basic functions are being performed. The second one (_Canvas_) with black background shows only user's drawing. 

### [2] Color settings and color detection
We must define the desired color which we want to be detected. For this project we define as desired color the following one :
```python
lower_color = np.array([35,46,106])
upper_color = np.array([86,168, 255])
```
In order the find the exact values for the above arrays use the trackbars method as in project *[Hand Detection and Finger Counting](https://github.com/kostasthanos/Hand-Detection-and-Finger-Counting)*.

### [3] Maximum contour and center
We find the maximum contour of the selected color. 
```python
# Transform to HSV
hsv = cv2.cvtColor(frame.copy(), cv2.COLOR_BGR2HSV)

# Mask from lower and upper color range
mask = cv2.inRange(hsv, lower_color, upper_color)

# Find contours in mask
cnts = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
cnts = imutils.grab_contours(cnts)

# Find maximum contour
max_cnt = max(cnts, key = cv2.contourArea)
```

Then we find the center of the max contour and check if center is inside any of our regions of interests.
```python
# Center of max contour
M = cv2.moments(max_cnt)
if M["m00"] != 0:
  cX = int(M["m10"]/M["m00"])
  cY = int(M["m01"]/M["m00"])
  cv2.circle(frame, (cX, cY), 4, green, 2)
  center = (cX, cY)
```

### [4] Drawing and save
In order to see our drawing we keep a list of points (x,y)-values from each center.
```python
pts = []
if "(cX, cY) is inside the desired frame regions then" :
  pts.append(center) # Append point to list
```
If we want to save the current drawing we can use **save_draw()** function from file *[save_drawing.py](https://github.com/kostasthanos/Virtual-Drawing-OpenCV/blob/main/save_drawing.py)*. This function saves all drawings from black *Canvas* window in a local file with name **drawings**.
```python
#Name of new drawing : "drawing(number)_day-month-year_hour:minutes.jpg"
new_drawing = 'drawing' + str(number_of_drawings+1) + '_' + date_time + '.jpg'

def save_draw(image):
  cv2.imwrite(dir_name + '/' + new_drawing, image)
```

More details about the code are being exlpained as comments inside the files *[virtual_drawing.py](https://github.com/kostasthanos/Virtual-Drawing-OpenCV/blob/main/virtual_drawing.py)* and *[save_drawing.py](https://github.com/kostasthanos/Virtual-Drawing-OpenCV/blob/main/save_drawing.py)*

For a full presentation of the working project watch the [video](https://www.youtube.com/watch?v=XUWEyNa0Bcw) in Youtube.

# Virtual Sketching using OpenCV

Virtual Sketching using OpenCV is a project that enables users to create digital sketches in real-time using a webcam. By applying computer vision techniques through the OpenCV library, this project converts the live video feed into a simplified black-and-white sketch, simulating the act of drawing with a pencil on paper.

## Features

- Real-time webcam feed conversion to sketch.
- Customizable sketch intensity and contrast settings.
- Instant visual feedback for a responsive experience.
- User-friendly interface with easy-to-use controls.

## How to Run

1. Clone this repository to your local machine:

2. Make sure you have Python and the required dependencies installed. You can install the dependencies using:

3. Open a terminal and navigate to the project folder.

4. Run the `virtual_sketching.py` script:

5. A window will open showing the live webcam feed alongside the corresponding sketch.

6. Use the provided controls to adjust the sketch intensity and contrast in real-time.

## Examples

![Virtual Sketching Example](/examples/virtual_sketching_example.png)

## Contributing

Contributions to this project are welcome! If you encounter any issues or have suggestions for improvements, please open an issue. If you'd like to contribute code, follow these steps:

1. Fork the repository.

2. Create a new branch for your feature/bugfix:

3. Commit your changes:

4. Push to your forked repository:

5. Create a pull request on the original repository.

## License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and distribute the code as per the terms of the license.

## Contact

For questions or inquiries, please contact [Your Name](mailto:youremail@example.com) or visit [Your GitHub Profile](https://github.com/yourusername).

