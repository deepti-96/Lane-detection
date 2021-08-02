# LANE DETECTION AND LANE DEPARTURE MONITORING

## Project Description & Objective
This project was designed to demonstrate how a lane detecting system on a car with a front facing camera works.<br />

This technology is becoming increasingly common in cars, and it is an important component of the advanced driver assistance systems (ADAS) used in autonomous and semi-autonomous vehicles. This feature oversees identifying lanes, measuring curve radius (curve tightness), and keeping track of the offset from the center. With this information, the system enhances safety by ensuring that the car remains centered within the lane lines and provides comfort if it is programmed to operate the steering wheel to negotiate moderate turns on highways without human input. This is a reduced version of what is used in production cars, and it performs best under ideal conditions (clear lane lines, stable light conditions). A dash cam clip is supplied in this repository for the script to operate with.<br />

## Getting started
In terms of how to operate this project, it takes a straightforward approach.<br />
Only two files are required to get it up and running.<br />
• laneDetection is the sole file that can analyze images and detect lanes.<br />
• drive is the video file on which image processing is done.<br />
Ensure that both files (laneDetection.py and drive.mp4) are in the same folder.<br />

## Necessary requirements
It is necessary to have the following applications installed, along with their corresponding versions, to complete the project successfully:<br />
• Python 3.6 or above is required.<br />
• OpenCV version 3 or above (Will be used by the script and can be downloaded using pip)<br />
• NumPy 1.14 or higher is required (Will be used by the script and can be downloaded using pip)<br />
• Scipy version 1.1 or above (Will be used by the script and can be downloaded using pip)<br />

## Environment and Dependencies
It should be mentioned that this project was created on a Microsoft Windows 10 (64 Bit) computer, but it should function on other systems with minimal adjustments. Follow the steps in the preceding section (Necessary requirements) for Python packages to ensure that you have the right versions of dependencies.<br />

## How Does the Code Function?
For the script laneDetection.py, a video file containing dashcam footage of a car traveling along the highway is given. The Python software uses a modular method to do lane detection and includes numerous functionalities.<br />

## Image Processing

### readVideo ()
First up is the readVideo() function to access the video file drive.mp4 which is in the same directory.<br />

### processImage ()
By performing some processing techniques, this function separates white lane lines, preparing them for further optimization by the upcoming functions. By applying HLS color filtering, it filters out the whites in the frame and converts it into grayscale, it applies thresholding to eliminate duplicate detections other than lanes, gets blurred, and then edges are extracted with the cv2.Canny() function.<br />

### perspectiveWarp ()
After we get the image we desire, we apply a perspective warp. Four points are put on the frame in such a way that they only surround the area where lanes are present (as shown), and the data is then mapped into another matrix to produce a bird’s eye view of the lanes. This will allow us to work with a far more precise picture and aid with lane curvature detection. It should be noted that if a different video is utilized, this process may alter. With this footage in mind, the preset four points are computed. If another video with a little different oriented camera is added, it should be returned. <br />

![image](https://user-images.githubusercontent.com/72935128/127897521-b174d4b5-8aaf-4af9-93fe-7fd2ed065f61.png)<br />

## Lane Detection, Curve Fitting & Calculations

### plotHistogram ()
Plotting a histogram for the bottom half of the image is critical for determining the exact location of the left and right lanes. When looking at the histogram, there are two separate peaks where all the white pixels may be seen. This is a great way to tell where the left and right lanes start. Because the histogram x coordinate corresponds to the x coordinate of our analyzed frame, we now have x coordinates with which to begin looking for lanes.<br />

![image](https://user-images.githubusercontent.com/72935128/127897626-5254496b-f487-42dc-9acb-bbb98f0067c9.png)<br />

### slide_window_search ()
To recognize lanes and their curvature, a sliding window method is utilized. It takes the data from the preceding histogram function and creates a box with a lane in the middle. Then it builds another box on top of it, based on the white pixel locations from the preceding box, all the way to the top of the frame. We now have the data we need to perform some computations. The curve is then fitted in pixel space using a second-degree polynomial fit.<br />

### general_search ()
Following the slide window search () function, the general search () method may now fill in an area around the identified lanes, and then use the second degree polyfit to create a yellow line that overlaps the lanes rather precisely. This line will be used to calculate the radius of curvature, which is necessary for calculating steering angles.<br />

### measure_lane_curvature ()
The np.polyfit() method is used again with the information supplied by the previous two functions, but the values are multiplied by the xm_per_pix and ym_per_pix variables to transform them from pixel to meter space. xm_per_pix is set to 3.7 / 720, which equates to a lane width of 3.7 meters and left and right lane base x coordinates determined from the histogram, which translates to a lane width of roughly 720 pixels. Because the frame height is 720, ym_per_pix is also set to 30 / 720.<br />

![image](https://user-images.githubusercontent.com/72935128/127897911-33de0ba1-8e1c-43b2-b85a-7bc249358380.png)<br />


## Visualization and Main Function

### draw_lane_lines ()
Following that, several approaches are used to visualize the identified lanes as well as other data that will be presented in the final image. This program uses identified lanes to fill the space within them with a green hue. It also depicts the lane's center by saving the mean of the left_fitx and right_fitx lists in the pts mean variable, which is subsequently represented by a yellowish tint. This variable is also used to determine whether the car is displaced to one side or centered in the lane.<br />

### offCenter ()
The offset value is calculated and shown in meter space using the pts mean variable in the offCenter () method.<br />

### addText ()
Finally, the procedure and information shown would be completed by adding text to the finished image.<br />

### main ()
The main function is where all these functions are called in the proper order and where the video loop is contained.<br />

![image](https://user-images.githubusercontent.com/72935128/127897950-ad3a5ad9-f63c-40f2-ac5b-804cc87fc7e3.png)<br />
 
## Important Points to Remember
It should be mentioned that this project was built for demonstration reasons only, and its success is dependent on the footage given. The image processing portion is kept relatively basic, and it may fail to detect lanes in low or unsteady light. Lane lines must be readily visible, and tighter bends might make fitting curves and visualization more difficult. The fundamental aim behind the creation of this project was for self-improvement and educational objectives.<br />

## Additional Remarks
When comparing this project to one of the production-ready versions, things are a lot more complicated, because error rates should be extremely low, and the system should be able to react to a variety of scenarios. For instance, unstable light circumstances with a large shift in light density, or weather conditions that affect road surface visibility, etc. In this project, several variables are hardcoded, and any modification might cause the setup to fail. As a result, techniques like machine learning should be employed to make this system more adaptable and less likely to fail in real-world scenarios.<br />

## References
The following presentation was found to be useful during the creation of this project, and this project takes a similar approach to what is described in the presentation.<br />
Presentation: https://www.youtube.com/watch?v=VyLihutdsPk&t=1295s

