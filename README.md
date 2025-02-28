# r550-ros-bot-humble

![r550 Simulation Success](https://github.com/910514/r550-ros-bot-humble/blob/main/images/r550_gazebo_humble.png)  

This repository contains a ROS 2 Humble adaptation of the r550 educational robot (麦克纳姆轮版) originally designed for ROS 1. It provides simulation support in Gazebo, including URDF and SDF models, launch files, and meshes. The project simplifies the original Mecanum wheel robot emulation by using a differential drive plugin due to the unavailability of a ROS 2-compatible Mecanum drive plugin at the time of development.

---

## Overview

The r550-ros-bot-humble project adapts the r550 robot—a four-wheeled Mecanum platform with swing arm suspension—for ROS 2 Humble. The official release targets ROS 1, but this repo modifies the code and structure to run seamlessly on ROS 2, leveraging a Jetson Nano 4GB as the ROS master, a 镭神N10P lidar, and a C70大FOV RGB camera. Key parameters and trade-offs are detailed below.

---

## Folder Structure

```
wheeltec_description/
├── CMakeLists.txt               # Build configuration for ROS 2
├── launch/
│   └── wheeltec_gazebo.launch.py # Launch file for Gazebo simulation
├── meshes/
│   └── mini_4wd/                # STL meshes for robot components
│       ├── base_link.STL
│       ├── camera_link.STL
│       ├── left_front_wheel_link.STL
│       ├── left_rear_wheel_link.STL
│       ├── right_front_wheel_link.STL
│       ├── right_rear_wheel_link.STL
│       └── rplidar_link.STL
├── models/
│   └── wheeltec/
│       └── wheeltec.sdf         # SDF model for Gazebo simulation
├── package.xml                  # ROS 2 package manifest
├── urdf/
│   └── wheeltec.urdf            # URDF model for robot description
└── worlds/
    └── wheeltec.world           # Gazebo world file
```

---

## Robot Parameters

The r550 robot specifications are sourced from the official market listing on Taobao ([link](https://e.tb.cn/h.TulZ9W5W6BLkY7n?tk=D6cVekYc2Eq)):

- **Structure**: Four-wheel Mecanum with swing arm suspension, 75mm metal Mecanum wheels
- **Servos**: None
- **Dimensions**: 266 x 230 x 202 mm
- **Weight**: 2.9 kg
- **Payload Capacity**: 6 kg
- **Max Speed**: 1.4 m/s
- **Endurance**: 5.5 hours (at 0.45 m/s)
- **Motor**: MG513 metal gear reduction motor
- **Encoder**: 500-line AB-phase high-precision GMR encoder (Chinese patent: 202122109210.2)
- **Controller**: STM32F407VET6
- **ROS Master**: Jetson Nano 4GB
- **Lidar**: 镭神N10P
  - Range: 25 m
  - Scan Frequency: 6–12 Hz (adjustable)
  - Sampling Frequency: 5400 Hz
  - Output: Angle, distance, light intensity
  - Angular Resolution: 0.4°–0.8° (adjustable)
  - Ambient Light Resistance: 60K Lux (outdoor capable)
  - Motor: Brushless
  - Principle: Time-of-Flight (TOF)
- **Camera**: C70大FOV RGB
  - Resolution: 720P (1280x720)
  - Drive: UVC, plug-and-play
  - Field of View: H 64.5°, V 50°
  - Interface: USB 2.0
  - Frame Rate: 25 fps
  - Dimensions: 47 x 38 x 28.5 mm
  - Sensor: CMOS
  - Weight: 50 g

---

## Trade-offs

To simplify emulation in ROS 2 Humble:
- **Differential Drive Emulation**: The original r550 uses Mecanum wheels for omnidirectional movement, but due to the absence of a readily available `libgazebo_ros_mecanum_drive.so` plugin for ROS 2, this repo falls back to `libgazebo_ros_diff_drive.so`. This limits simulation to forward/backward and turning motion, omitting lateral strafing capabilities. Future updates may integrate a Mecanum plugin if sourced.
- **Simplified Suspension**: The swing arm suspension is not modeled in the SDF/URDF, treating wheels as rigidly attached to the base for simulation simplicity.

---

## Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/<your-username>/r550-ros-bot-humble.git
   cd r550-ros-bot-humble
   ```

2. **Build with ROS 2 Humble**:
   ```bash
   source /opt/ros/humble/setup.bash
   colcon build
   ```

3. **Source the Workspace**:
   ```bash
   source install/setup.bash
   ```

4. **Launch Simulation**:
   ```bash
   ros2 launch wheeltec_description wheeltec_gazebo.launch.py
   ```

---

## Usage

- **Topics**:
  - `/wheeltec/cmd_vel`: Publish `geometry_msgs/msg/Twist` for control.
  - `/wheeltec/odom`: Odometry data.
  - `/c70/image_raw`: Camera RGB feed.
  - `/c70/camera_info`: Camera metadata.
  - `/rplidar/scan`: Lidar scan data.

- **Test Movement**:
  ```bash
  ros2 topic pub /wheeltec/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.1}, angular: {z: 0.0}}"
  ```

---

## References

- **Official Release**: The original ROS 1 implementation is available from Wheeltec’s official source: [https://pan.baidu.com/e/1ohSwtrPQlGXbYJYAj2ssww](https://pan.baidu.com/e/1ohSwtrPQlGXbYJYAj2ssww). This repo builds upon that foundation, adapting it for ROS 2 Humble.
- **Market Listing**: Parameters and details sourced from Taobao: [link](https://e.tb.cn/h.TulZ9W5W6BLkY7n?tk=D6cVekYc2Eq).