# Jetson-Ros-Installation

https://www.stereolabs.com/blog/ros-and-nvidia-jetson-nano/

Installation
Open a new terminal by pressing Ctrl + Alt + t or executing the “Terminal” application using the Ubuntu 18 launch system.

Set up the Jetson Nano to accept software from packages.ros.org:

$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
Add a new apt key:

$ sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
[Note: the ROS GPG key has changed due to a security issue on the ROS build farm server. If you configured your Jetson Nano for ROS following this guide before the 24th June 2019, please follow this guide to replace the old key in the correct way]

Update the Debian packages index:

$ sudo apt update
Install the ROS Desktop package, including support for rqt, rvizand other useful robotics packages:

$ sudo apt install ros-melodic-desktop
Note: “ROS Desktop Full” is a more complete package, however it is not recommended for an embedded platform; 2D/3D simulators will be installed with it and they take too much space on ROM, and are too computationally hungry to be used on the Nano.

Initialize rosdep. rosdep enables you to easily install system dependencies for source code you want to compile and is required to run some core components in ROS:

$ sudo rosdep init 
$ rosdep update
It is recommended to load the ROS environment variables automatically when you execute a new shell session. Update your .bashrc script:

$ echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc 
$ source ~/.bashrc
Now the Jetson Nano is ready to execute ROS packages and become the brain of your autonomous robot.

Configure a catkin workspace
To start running your own ROS packages or install other packages from source (such as the ZED ROS wrapper for example), you must create and configure a catkin workspace.

Install the following dependencies:

$ sudo apt-get install cmake python-catkin-pkg python-empy python-nose python-setuptools libgtest-dev python-rosinstall python-rosinstall-generator python-wstool build-essential git
Create the catkin root and source folders:

$ mkdir -p ~/catkin_ws/src 
$ cd ~/catkin_ws/
Configure the catkin workspace by issuing a first “empty” build command:

$ catkin_make
Finally, update your .bashrc script with the information about the new workspace:

$ echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc 
$ source ~/.bashrc
Your catkin workspace is now ready to compile your ROS packages from source directly onto the Jetson Nano.


Getting Started with ZED stereo camera on Jetson Nano
How can a robot be autonomous without perceiving the world? The ZED and ZED Mini 3D depth cameras are the ideal companions for a Jetson Nano and ROS-powered robot.

To get your ZED running with ROS on Nano, go to the source folder of the catkin workspace that you just created:

$ cd ~/catkin_ws/src
Clone the ZED ROS wrapper Github repository. The ZED wrapper allows you to add real-time depth sensing, stereo visual odometry, and 3D SLAM to your autonomous robot.

$ git clone https://github.com/stereolabs/zed-ros-wrapper.git
Check the dependencies:

$ cd ~/catkin_ws
$ rosdep install --from-paths src --ignore-src -r -y
The rosdep command explores all the packages available in the src folder and verifies that all the declared dependencies are available, automatically installing the missing ones.

Compile the ZED ROS wrapper:

$ catkin_make -DCMAKE_BUILD_TYPE=Release
You can now visualize the video and depth data that your ZED camera captures using this simple command:

$ roslaunch zed_display_rviz display.launch
For ZED Mini:

$ roslaunch zed_display_rviz display_zedm.launch 

For more information on using the ZED ROS wrapper, read our Getting Started guide.

If you need help with setting up ROS on your Jetson Nano or using your ZED stereo camera, don’t hesitate to contact us on the support portal or Github issue system.


