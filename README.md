
[//]: # (Image References)
[image_0]: ./misc/mainImage.gif
# Map a World with Slam and a custom Robot
![alt text][image_0] 

## Abstract
 This project is about implementing SLAM(Simultaneous Localization and mapping) with RTAB-MAP(Real-Time Appereance-Base Mapping). Two 2D occupancy grid and a 3D octomap is created from a simulated environment and then map with a custom robot(fouliexBot).
 
 ## Introduction
 In Localization a robot is provided to map its environment. The robot has access to tis movement and sensor data and use them to estimate it pose. However, if the map of the environment doesn't exist. There are many application where there isn't a known map because the area is unexplored or because the surrounding change ofthen therefore the map is not up to date. In such case the robot will have to construct  a map and this leads  to robotic mapping.
 
 Mapping assumes that the robot knows its pose and as usual has access to its movement and sensor data. The robot must produce a map of the environment using the known trajectory and mearurement data.However,even such case can be quite uncommon in the real world. Most of tioime the robot would have neither a map nor know its pose and this is where SLAM comes in.
 
With SLAM, fouliexBot does really good job with just it  own movement and sensory data to build a map of its environment while simultaneously localizng itself relative to map.

## Background
SLAM has 2 best approach which are Grid-based FastSLAM and GraphSLAM.

## Grid-based FastSLAM
The fastSLAM alogirhm uses a custom particle filter approach to solve the full SLAM problem with known correspondences.
Using particles, fastSLAM estimates a posterior over the robot path along with the map. Each of these particles hold the robot trajectory which will give an advantage to SLAM to solve the problem of mapping with known poses. In addition to the robot trajectory, each particle holds a map and each feature of the map is represented by a local Gaussian. With this algorithm, the problem is now divided into separate independent problem. Each of  which aims to solve the problem of estimating features of the map. To solve these independent mini problems FastSlam will use the low dimensional extended Kalman filter. While math features are treathed independently, dependency only exisit between robot pose uncertainty. This custom approach of representing posterior with particle filter and Gaussian is known by the Rao-Blackwellized Particle Filter One.

With the MCL FastSLAM estimates the robot trajectory. With the Low-Dimensional EKF, FastSLAM estimates features of the map.

## GraphSLAM

GraphSlam is a slam algorithm that solves the full slam problem. This means that the algorithm recovers the entire path and map, instead of just the most recent pose and map. This difference allows it to consider dependencies between current and previous poses. 

One example of our GraphSLAM would be applicable is an underground mining. Larges machine called bores,spent every day cutting away at the rockface. The environment changes rapidly and it's important to keep an accurate map of the workspace. One way to map this space would be to drive a vehicle with a LIDAR around the environment and collects data about the surroundings. Then, after the fact, the data can be analyzed to create an accurate map of the environment. GraphSLAM have an improved accuracy over FastSLAM.

FastSlam  uses particesl to instimate the robot's most likely pose. However at any point in time, it's possible that there isn't a particle in the most likely location. In fact, the chances are slim to none especially, in large environments. Since GraphSLAM solves the full slam problem, this means that it can work with all of the data at once to find the optimal solution. FastSLam uses  tidbits of information  with finite number of particles, therefore there's room for error.

## RTAB-MAP
RTAB-Map (Real-Time Appearance-Based Mapping) is a RGB-D Graph-Based SLAM approach based on an incremental apperance-based loop closure detector. For this project, RTAB-MAP is used and it is compsoed of Front-end and Back-end


 

## Project Setup
Download the repo.catkins_w is the name of the active ROS workspace for the project.
```shell
$ cd ~/catkin_ws/src
git clone https://github.com/fouliex/MapAWorldWithSlamAndACustomRobot.git
```
Build the project:
```shell
$ cd ~/catkin_ws
$ catkin_make
```
Source the terminal
$ source ~/catkin_ws/devel/setup.bash

Run the project
```shell
roslaunch fouliex_bot fouliex_world.launch
roslaunch fouliex_bot mapping.launch
roslaunch fouliex_bot rviz.launch
roslaunch fouliex_bot teleop.launch
```


## Dependencies
This project works with Ubuntu 16.04
### 1. ROS Installation

```shell
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' && sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116 && sudo apt-get update && sudo apt-get install ros-kinetic-desktop-full && sudo rosdep init && rosdep update && echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc && source ~/.bashrc
```
### 2. ROS Kinetic Dependencies
```shell
sudo apt-get install ros-kinetic-rtabmap ros-kinetic-rtabmap-ros && sudo apt-get remove ros-kinetic-rtabmap ros-kinetic-rtabmap-ros
```

### 3. RTAO-Map Installation
```shell
cd ~ && git clone https://github.com/introlab/rtabmap.git rtabmap && cd rtabmap/build && cmake .. && make && sudo make install
```
### 4. Add Model collision adjustments
Create the .gazebo folder and  and then add model collision adjustments
```shell
curl -L https://s3-us-west-1.amazonaws.com/udacity-robotics/Term+2+Resources/P3+Resources/models.tar.gz | tar zx -C ~/.gazebo/f
```




