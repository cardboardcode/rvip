# **RVIP** [Out The Box]
**[Version]:** 1.0.0

# **What Is This?**
This document provides instruction to set-up **RVIP** as a complete Robotic Vision suite.

This is based on the combination of ROS packages and external libraries used for testing purposes.

This **suite** is made of:
1. OpenGR
2. darknet_ros
3. rvip

# **Dependencies**
### This section lists all third-party softwares used by **RVIP**.

**Click** the hyperlinks for download instructions.

1. [Ubuntu 18.04 (Bionic Beaver)](https://ubuntu.com/download/desktop)
    - For the operating system
2. [ROS Morenia Melodic](http://wiki.ros.org/melodic/Installation/Ubuntu)
    - For the ROS base
3. OpenGR
    - For pose estimation. `Refer below for download instructions.`
4. [OpenCV 3.2.0](http://www.codebind.com/linux-tutorials/install-opencv-3-2-ubuntu-18-04-lts-linux/)
    - For reading and displaying images
5. [Point Cloud Library](https://askubuntu.com/questions/1134856/how-to-install-pcl-on-ubuntu-18-04)
    - For processing 3D information
6. darknet_ros
    - For object recognition using only 2D RGB images. `Refer below for download instructions.`

# **Connecting To Camera**
### This section provides instructions on how you can connect **RVIP** to your ROS-supported **Depth Sensor/Camera**.  
`Ensure that you have finished setup before doing this!`

1. **Identify** **topics** and `frame_id` TF frame which your **Depth Sensor/Camera** is publishing the following information on:

> `3D PointCloud`
>
> `Rectified 2D RGB Image`

Eg. In my own use case, I use a Kinect v2 One Depth Sensor. The **topics** and `frame_id` TF frame thus look like so:
> `/kinect2/hd/points` - For 3D PointCloud

> `/kinect2/hd/image_color_rect` - For Rectified 2D RGB Image

> `/kinect2_rgb_optical_frame` - For frame_id TF frame

2 . **Edit** [line 4]() of the `darknet_ros/darknet_ros/config/ros.yaml` file to the **Rectified 2D RGB Image** you identified earlier.
```yaml
    topic: /kinect2/hd/image_color_rect
```

3. **Run** the command below to launch **RVIP**.
> `$ roslaunch rvip run.launch`

# **Setup**

### There are **3** parts to the setup.
1. OpenGR Setup
2. darknet_ros Setup
3. RVIP Setup

# !**`WARNING`**!
> [WIP] The **OpenGR** library used in 0.0.1 alpha version is an outdated version. Please follow the instructions below to download the specific version of OpenGR highlighted.

### **[OpenGR Setup]**

Execute the commands below in terminal to **install** the **OpenGR** library.

 `$ git clone https://github.com/STORM-IRIT/OpenGR.git`

 `$ cd OpenGR`

 `$ git reset --hard 0967cd880950b35786b8fd098837c9eb1fe2aca4`

 `$ mkdir build && cd build`

 `$ cmake -DCMAKE_BUILD_TYPE=Release ..`

 `$ sudo make install`

### **[darknet_ros Setup]**

1. **Go** to the `src` directory of your **catkin** workspace.
> `$ cd path/to/your/<workspace_name>/src`

2. **Download** the git repository into your local workstation.
> `$ git clone --recursive https://github.com/leggedrobotics/darknet_ros.git`

3. **Replace** the `CMakeLists.txt` under `darknet_ros/darknet_ros` with the  
`CMakeLists.txt` from [this link](https://github.com/benjaminabruzzo/darknet_ros/blob/master/darknet_ros/CMakeLists.txt).

This removes reliance of CUDA temporarily to run **darknet_ros** YOLO object detection. The package uses **CPU** only to enhance software portability.

4. **Build** **darknet_ros** ROS package.
> `$ catkin build darknet_ros -DCMAKE_BUILD_TYPE=Release`

5. **Source** your workspace to ensure that you have **darknet_ros** ready to run.
> `$ source path/to/your/<workspace_name>/devel/setup.bash`

### **[RVIP Setup]**

1. **Download** this repository into the `src` directory of your **catkin** workspace.
> `$ cd path/to/your/<workspace_name>/src`
>
> `$ git clone https://github.com/cardboardcode/rvip.git`

2. **Build** **RVIP** ROS package.
> `$ catkin build rvip -DCMAKE_BUILD_TYPE=Release`

3. **Source** your workspace.
> `$ source path/to/your/<workspace_name>/devel/setup.bash`

# **Run**
After following the instructions under **Setup**, run the commands below:

1. Go to the root of your catkin workspace.
> `$ cd path/to/your/\<workspace_name>/``

2. **Source** your workspace..
> `$ source path/to/your/<workspace_name>/devel/setup.bash`

3. **Build** your workspace.
> `$ catkin build -DCMAKE_BUILD_TYPE=Release`

4. **Run** RVIP. Replace **camera_3d_input_topic** with the **3D PointCloud** camera topic you identified earlier.
> `$ roslaunch rvip run.launch camera_3d_input_topic:=/kinect2/hd/points camera_tf_frame:=/kinect2_rgb_optical_frame`


## **[Issues?]**
Having trouble compiling and using **RVIP**? **Check** out the following frequently encountered issues to see if it helps.

### 1. **darknet_ros** is taking **FOREVER** to build.
Please **refer** to [this GitHub issue](https://github.com/leggedrobotics/darknet_ros/issues/129#issuecomment-533609094) for more details on how to solve it.

For further issues, [click here](https://github.com/cardboardcode/rvip/issues) to report as an issue here in this repository.
