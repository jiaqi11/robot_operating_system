# CMPUT412 FALL 2019 - Competition 5 Report

## ***Overview***

This repo is the competition 5 implementations of group 3, and it is built upon
[https://github.com/HumphreyLu6/CMPUT-412-C4]
[https://github.com/HumphreyLu6/CMPUT-412-C3]
[https://github.com/TianqiCS/CMPUT412-C2]
[https://github.com/TianqiCS/CMPUT412-C1]

A competition vedio has been recored, check out in this link: https://www.youtube.com/watch?v=n_t9dkZ2Tzk.

## ***Competiiton Objectives***

Using a Turtlebot to do multiple tasks consists of following track, detecting contours, docking with AMCL and box pushing. Scores are given based on the completeness of tasks, performance of each tasks in a total of 10 minutes run. One thing special in competition 5 is that robots can choose to do partial tasks and accumulate points in one run until time is used up. 

## ***Tasks***

In general, the robot needs to follow a track and do different specific tasks at various locations. The track is white lines on the ground, full read lines and half red lines indicates the robot needs to stop, and half red lines also indicate the robot has specific tasks in the locations. Tasks for each location are detailed as following:

<p align = "center">
    <img src="https://github.com/HumphreyLu6/CMPUT-412-C3/blob/master/images%20and%20video/course.png" width="80%" height="80%">
    <br>
    <em>Fig. 1: Course</em>
</p>

- ***Location 1*** is the first location maked by a short red line. Certain number (1~3) of objects are placed 90 degrees counterclockwise from the line. The robot needs to count the number of objects without leaving the track.
<p align = "center">
    <img src="https://github.com/HumphreyLu6/CMPUT-412-C3/blob/master/images%20and%20video/location1.png" width="40%" height="40%">
    <br>
    <em>Fig. 2: Location 1</em>
</p>

- ***Location 2*** is the second location, which located in the end of an additional track which branched off at a short red line. A white board with certain number(1~3) of shapes is placed at location 2, one of the shape is in green and others are in red. The robot needs to count the number of shapes and detect what shape the the green shape is.

<p align = "center">
    <img src="https://github.com/HumphreyLu6/CMPUT-412-C3/blob/master/images%20and%20video/location2.png" width="40%" height="40%"> 
    <br>
    <em>Fig. 3: Location 2</em>
</p>

- ***Location 4*** is the third location led by a off ramp track. 8 parking stalls are located on the floor which are represented by red squares. A box with AR tags are placed in one of the 2, 3, 4 stalls, and a different AR tag that indicates goal stall is placed in one of remaining stalls(against the wall), the red shapes are placed at spot 6, 7 and 8. The robot need to push the box into the AR tag marked stall and park inside of the stall which has the same shape as the green shape from location 2. After finishing parking at three spots, the robot needs to find the on ramp location and keep following the track.

<p align = "center">
<img src="https://github.com/HumphreyLu6/CMPUT-412-C3/blob/master/images%20and%20video/location4.png" width="40%" height="40%">
    <br>
    <em>Fig. 4: Location 4</em>
</p>

<p align = "center">
    <img src="https://github.com/HumphreyLu6/CMPUT-412-C4/blob/master/images%20and%20video/Box.png" width="40%" height="40%">
    <br>
    <em>Fig. 5: Location 4 Box</em>
</p>

- ***Location 3*** is the last location which is marked by three half red lines, each red line indicated there is a shape located counterclockwise from the track, the robot needs to find the one with same shape as the green shape at location 2.

<p align = "center">
    <img src="https://github.com/HumphreyLu6/CMPUT-412-C3/blob/master/images%20and%20video/location3.png" width="40%" height="40%">
    <br>
    <em>Fig. 6: Location 3</em>
</p>

## _**Pre-requisites**_
-   In order to execute this competition, a few hardwares used are listed below:
    - A laptop
    - Kobuki Turtlebot 2
    - Asus Xtion Pro RGB-D Camera
    - USB webcam
    - Logitech Controller
    - Two foam bumpers
    
    All these hardwares need to be assembled like the robot in the _**Fig. 7**_ to make sure everything runs correctly. Notice that the USB webcam is in between the two foam bumpers.
    
<p align = "center">
    <img src="https://github.com/HumphreyLu6/CMPUT-412-C4/blob/master/images%20and%20video/Robot.jpg" width="40%" height="40%">
    <br>
    <em>Fig. 7: Robot</em>
</p>

-   The project is built with python2.7 on Ubuntu 16.04. Dependencies include ROS kinetic package, smach state machine, and other drivers for the turtle bot sensor. If these are not installed please refer to the official installation page on ROS wiki or official python installation websites.

    -   Kobuki  [http://wiki.ros.org/kobuki/Tutorials/Installation/kinetic](http://wiki.ros.org/kobuki/Tutorials/Installation/kinetic)

    -   Ros-Kinetic  [http://wiki.ros.org/kinetic/Installationu](http://wiki.ros.org/kinetic/Installationu)

    -   Python2  [https://www.python.org/downloads/](https://www.python.org/downloads/)

    -   Smach  [http://wiki.ros.org/smach](http://wiki.ros.org/smach)
    
    -   OpenCV [https://opencv.org](https://opencv.org)

Create or navigate the existing catkin workspace and clone our repository.


## _**Execution**_

-   Once you have the package in your workspace, change the package name to c5

    ```
    cd (your path)/catkin_ws
    catkin_make
    source devel/setup.bash

    ```
    now you can launch the program using

    ```
    roslaunch c5 c5.launch
    ```

-   The launch file c5.launch, the file will launch basic driver for the kuboki robot which is essential for the competition ( minimal.launch and 3dsensor.launch). Next, the file will bring up the basic node for this competition like main file and a usb camera. Finally, there are different sections for in the launch file like example.yaml to give the uvc camera  a basic understanding of view.

-  A map file of the lab is added to the file folder which is used for location 4. In the c4.launch file, the ar_track_alvar is used to regonize the AR tag. We comment out the view_nevigation package to increase the performance of the robot at runtime.

- A few ymal files contains amcl parameter settings are in param folder will be used when amcl.launch is launched.

## ***Strategies***:
- Game Strategy: 
    - Before we made any decision, we did time estimations for every task in our case. Location 1 takes around 14s, location 2 takes 34s, location 4 box pushing takes 1 mintunes with high variances, location 4 shape detection takes 1min 10s, location 3 takes 38s.
    - After a few calculations on the points we can get in 10 mintues in different strategy, we found that implementing box pushing only will score the highest. Following are some calculation results example:
        - Implement all tasks: 200 maximum points/loop, 2-2.5 loops maximum in 10 mintues, so ideally 400 - 500 points in total.
        - Implement location 2 and location 4 tasks only: 174 maximum points/loop, 2.5-3 loops maximum in 10 mintunes, so ideally 410-520 points in total.
        - Implement location 4 box pushing task only: 118 points/loop, 6-6.5 loops maximum in 10 mintues, so ideally 708-767 points in total. 
        
- Track followling:
    - To minimize the time spent on line following, we improved the speed from 0.5 to 0.7 and adjusted the PID controller as well.
    - We put a usb camera at the lower front of the turtle_bot to follow the white line on the ground and the asus camera is used to detect shape of the target. The lower position of the camera improves precision with less exception tolerance as a trade-off.
    - In the function usb_callback, we use the usb camera to detect whether we have a long red line to  short red line. The method is that if it is a long red line there won't be any white in the middle of the track. We think its quicker and easier to identify the difference between two lines.
    - To ensure the target objects are included into the camera, the robot back up a little bit to fit the camera view into the right position.

- Box Pushing:
    - Primarily, we wanted to push the box directly towards the goal without break, and then fine-tune the position of the box by detecting if there are significant position differences between the box center and goal.
    - After a few experiments, we found that when the initial position of the box center is far away from goal (2, 3 squares in between), the robot easily lost the box during pushing the box, and sometimes the box is pushed against the wall. The errors of AMCL navigation is magnified by pushing the box. So we decided to push a short distance at one time.
    - The basic steps are 1. Using AMCL navigation to approach the box,  2. pushing forward, 3.backing up, 4. Going back to 1, repeat until the goal is reached.
    - We plan to not fine tune our box position after pushed it forward for saving time, since the deducations on line tounching is quite gentle. 

- Bumper:
    - To minimize the effect to the line following, we cut a long bumper into half and stuff them beside the webcam, this makes sure the webcam funtions as before.
    
    <p align = "center">
        <img src="https://github.com/HumphreyLu6/CMPUT-412-C4/blob/master/images%20and%20video/Bumper.jpg" width="40%" height="40%">
    <br>
    <em>Fig. 8: Robot Bumper</em>
    </p>

- Shape detection (Not implemented in Competition 5):
    - Used cv2.pyrMeanShiftFiltering to blur image when detect contours' shapes, but this caused nonnegligible lag.
    - To ensure shape detect result is correct, we detect twice with a few seconds gap to check if results are the same.

- AMCL and GMapping:
    - We remapped the whole area, but in this time our map is parallel to the room. It is very easy to understand the geometry relationship in this way. After a fairly accurate map is established, we use photoshop to clear some noise of the map. we set the way points based on the map. By testing out each waypoint one by one, we want to make sure the run time error genreate by the odem has the minimum effect on the final parking spot.
    - Since the usb camera is stilling running during parking into these red squares, it is likely that the robot takes the parking red square as the functional red lines. New global varibies have set to avoid these conflicts.
searching strategy:
    - We used exhaustive search for the parking spot to make sure the robot complete the task and fit into all the squares.
    - Set initial pose when the robot is off ramp instead of the start point of the game to help localization and precision.
    - The docking process is based on waypoints. We test the waypoints one by one to ensure the the robot will dock on point.
    - The robot will skip the rest of waypoints if all task at location 4 have been completed.
    - Compared with C4, we rewrote the parameters as files passed into the launch file, which greatly improved navigation.
- Project management:
    - The code file for work4 is seperate from the main code for further improvement on the coding style.
    - Heavliy used simple task functions like rotation and signal (led and sound) have been seperated from the original file to increase simplicity and reusability.
    - Based on the experience we collected from demo4 and demo5, we carefully develop the map using view_nevigation package.
    - To improve the runtime performance, we choose to not launch rviz, this can be enabled through commenting out lines in launch file.

## _**States**_

<p align = "center">
    <img src="https://github.com/HumphreyLu6/CMPUT412-C5/blob/master/images%20and%20video/smach.png" width="50%" height="50%" alt>
    <br>
    <em>Fig. 9: Root State Machine</em>
</p>

-    Our basic strategy includes using pid controller to follow lines, using opencv contour shape detection to detect shapes, using amcl to do localization, using move_base to reach goal point in the location 4.
-    Here are the process details:
-    Firstly, the robot will start with "Wait" state, once the user send unmarked dock point number and start signal, the robot will start follow the white line.
-    As the robot is running, it will find out whether there is a red long line(which means stop) or a red short line(which means detecting the image) and decide if it needs to switch states.
-    For different working tasks, the difference is based on the global variable of "current_work"
-    The state machine will have some kind of work flow like this:
        - 1. "Following" state will keep the robot following the white line
        - 2. If the robot hit a long red line it will enter the "PassThrough" state to perform a stop at the long red line
        - 3. If the robot hit a short red line it will enter the "TaskControl" state to determine how many 90 degrees it should trun and then it goes into 'Rotate' state which controls the robot's rotation based on the yaw value.
        - 4. In the 'Rotate' state, the robot will determine what kind of work it will do based on current value.
        - 5. For the task to count number of white tubes, the robot will detect how many red contours are in the front and indicate the number by Led lights and sound.
        - 6. For the task at location 2, the robot will detect how many red/green contours are in the front and indicate the total number by Led lights and sound, the robot will remember what shape the green contour is in location2.
        - 7. When the robot goes to 'off-ramp' spot, it will firstly try to identify the location of the AR tag and the box. Then it will calculate the relative distance base on the data it gets. If the AR tag or the box is too far away to be found, it will drive to position 6 and 7 and try to get the AR tags.
        - 8. After the tags have been successfully found, the robot will go to the other side of the box than goal and try to push the box from sideways.
        - 9. For each square the robot push, it will stop and step back to make sure the box is in the right rotation and it is pushing the box in the right way.
        - 10. After the box has been push to the right parking spot which is right in front of the AR tag, the robot will find a way to postition 6,7 and 8 to find out if the shape it saw in location 2 is among the three tags.
        - 11. After the robot finishs all box pushing tasks it will go to the 'on-ramp' point and continue the 'lcoation 3' task, which is find the matching shape at location 2.
        - 12. The robot will go through all the shapes when selecting the shapes. If it found the right one it will make a turn on a light and make a sound.
        - 13. The run is ended when the robot is back to the starting line

## ***Sources***
- https://github.com/jackykc/comp5
- https://github.com/cmput412
- https://github.com/bofrim/CMPUT_412
- https://github.com/nwoeanhinnogaehr/412-W19-G5-public
- https://github.com/stwklu/CMPUT_412_code/
- https://www.pyimagesearch.com/2016/02/08/opencv-shape-detection/
- https://github.com/TianqiCS/CMPUT-412-C2
- https://github.com/HumphreyLu6/CMPUT-412-C4
- https://github.com/HumphreyLu6/CMPUT-412-C3
- https://github.com/HumphreyLu6/CMPUT412_demo5_p2
- https://www.cnblogs.com/kuangxionghui/p/8335853.html
- https://eclass.srv.ualberta.ca/pluginfile.php/5209083/mod_page/content/60/Competition%202_F19%20Line%20Following%20and%20object%20counting.pdf
- https://eclass.srv.ualberta.ca/pluginfile.php/5209083/mod_page/content/60/Comp3_F19GMapping%20and%20AMCL.pdf
