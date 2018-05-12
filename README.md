# arm_force_control_pack
## Introduction
This repository is initially constructed in the process of MSc thesis in University of Tartu. 
Main purpose of this repository is to combine all needed ROS packages for hardware agnostic compliant control of collaborative industrial manipulator. There is no robot-specific packages/libraries or configurations included.

At the time of writing this, the package is tested successfully on two different robot manipulators:

* Franka Emika Panda
* Universal Robot UR5

## Requirements for manipulator

Since no software can be fully hardware-agnostic, a list of minimal requirements for the hardware should be defined.
In order for the software bundle to work, the prerequisites for the robot and its controller are following:

1. The robot should be a serial manipulator
2. Driver or controller of the robot should publish FT data (geometry_msgs/Wrench)
3. The robots' driver or controller should subscribe to geometry_msgs/Twist or trajectory_msgs/JointTrajectory in order to move its end-effector (EEF) in Cartesian space by receiving velocities for EEF or joint-based velocities respectively.
4. The robot should have valid MoveIt! configuration and URDF model, because functionalities from move_group node are used.

## Functional description

* contact_control - Holds main compliant control functionality. In a nutshell, it subscribes to FT data (geometry_msgs/Wrench) applied on EEF and publishes EEF velocity commands in Cartesian space (geometry_msgs/Twist) according to provided control laws.
* jog_arm - This package is middleware between robots controller (or driver) and contact_control published velocity commands. Since all manipulators' controllers do not provide Cartesian space velocity control functionality, jog_arm provides conversion from geometry_msgs/Twist to trajectory_msgs/JointTrajectory message, which provides joint-level velocities.
* netft_utils - This is a dependency for contact_control package. It provides utils for FT data processing, such as filtering and assigning movements constraints.
* keyboard_publisher - Provides functionality to easily publish geometry_msgs/Twist messages using keyboard. Very useful for testing jogging functionality and moving robot manually.
* majorana - Includes set of generic force and impedance control messages and services. The package is an interface between contact_control and ROS ecosystem.
* manipulator_control_gui - Graphical user interface, that includes elements for setting control laws on every direction with required parameters. 

## Set-up

### Main compliant control package set-up

Yous should already have ROS installed and catkin workspace created. 

First step is to recursively clone this repository into you workspace's src folder:

1. `~/../my_workspace/src$ git clone https://github.com/ut-ims-robotics/manipulator_force_control_pack.git --recursive`

Next, install all dependencies:

2. `~/../my_workspace$ rosdep install --from-paths --ignore-src .`

Compile:

3. `~/../my_workspace$ catkin_make`

If no dependency installation errors or compiler errors occur at this point, we are ready to set up robot-specific package(s) and configurations.

### Robot-specific set-up
At the time of writing, the package is tested on two robots. Next, the setup and running the package with these robots are described.
Since the software package is claimed as hardware-agnostic, treat following set-up guides as examples of how to get this compliant control package to work on a manipulator.

### UR5 set-up and running

TODO

### Franka Emika Panda set-up and running

TODO