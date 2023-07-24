# HRL-UGV-Research-Project
This project is developed by Human Robotics Lab at the University of Melbourne based on **[Ubuntu](https://releases.ubuntu.com/)** and **[ROS](http://wiki.ros.org)**. If you have any questions, feel free to contact me: yangmengfeix@student.unimelb.edu.au.

# Introduction:
The framework of this project consists four parts: communication layer, function layer, decision layer, and system (including the SDK for hardware systems, and the system-wide ROS messages).

The communication layer is in charge of intra-robot communication between the on-board computer and the Real Time low-level robot controller. Currently, it supports the ACCR-UTGV platform and will support DJI AI Robot in short future. These are two main UGV platforms we have currently in the lab.

The function layer contains the most of the functions including perception, navigation, and some other small functions.

The decision layer currently can demonstrate a certain level of intelligence by using differnt algorithms. In future, the decision layer will support multi-agent decision-making framework.

Aside from these three layers, a simulator based on the Gazebo is included to provide a virtual platform for testing purpose. It is expected to be replaced by the Unity engine in the next step.

# 1. ROS Installation
This project has been tested on both ROS Melodic (with Ubuntu 18.04) and ROS Noetic (with Ubuntu 20.04). The main development is based on ROS Noetic and Ubuntu 20.04.
ROS installation instructions can be found via ROS website. Download [Melodic](http://wiki.ros.org/melodic/Installation/Ubuntu) or [Noetic](http://wiki.ros.org/noetic/Installation/Ubuntu) based on the Ubuntu version. The *desktop-full* version is recommended for this project.

## *Optional* (Recommanded)
After finishing the installation of ROS, you can use the following code to make your ROS environment working **globally**.
```
echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> ~/.bashrc
```
In this case, in the following steps, you can always **ignore** the following code.
```
source /opt/ros/$ROS_DISTRO/setup.bash
```
This makes your life easier, as you don't need to include this line every time when you use a ROS command in a new terminal.

If you want to **remove** this global setting from your device, simply go to `Home`, and then press `Ctrl` + `H` to show all hidden files, then open the `.bashrc` file and delete the following line:
```
source /opt/ros/$ROS_DISTRO/setup.bash
```    
# 2. Packages Installation
Install necessary dependency packages:
```
sudo apt clean && sudo apt update
sudo apt install -y libgoogle-glog-dev 
source /opt/ros/$ROS_DISTRO/setup.bash
sudo apt-get install -y ros-$ROS_DISTRO-gmapping ros-$ROS_DISTRO-navigation ros-$ROS_DISTRO-tf2-sensor-msgs ros-$ROS_DISTRO-teleop-twist-keyboard ros-$ROS_DISTRO-teb-local-planner ros-$ROS_DISTRO-realsense2-camera ros-$ROS_DISTRO-behaviortree-cpp-v3 ros-$ROS_DISTRO-roslint
sudo apt-get update && sudo apt-get upgrade    
```
# 3. Livox 3D LiDAR SDK
The ros wrapper of Livox 3D LiDAR is included in this project. If the Livox SDK hasn't been installed and the Livox LiDAR is not going to be used in the project, the Livox ros wrapper will be automatically skipped in catkin_make. You can also choose to remove them from your package manually or using the following code, and heading to the [next step](README.md#4-complie).
```
cd HRL-UGV-Research-Project/src/system
rm -r livox_ros_driver-master
```

If the Livox 3D LiDAR is going to be used, the Livox SDK is needed. Please install its [SDK](https://github.com/Livox-SDK/Livox-SDK) using following codes.

## For Ubuntu 18.04/16.04/14.04 LTS (normal PC)
```
cd
sudo apt install cmake
git clone https://github.com/Livox-SDK/Livox-SDK.git
cd Livox-SDK
cd build && cmake ..
make
sudo make install
sudo apt install -y ros-$ROS_DISTRO-pcl-ros ros-$ROS_DISTRO-cmake-modules
cd
```

## ARM-Linux Cross Compile
The procedure of cross compile Livox-SDK in ARM-Linux are shown below.

### Dependencies
Host machine requires install cmake. You can install these packages using apt:
```
sudo apt install cmake
```
### Cross Compile Toolchain
If your ARM board vendor provides a cross compile toolchain, you can skip the following step of installing the toolchain and use the vendor-supplied cross compile toolchain instead. The following commands will install C and C++ cross compiler toolchains for 32bit and 64bit ARM board. You need to install the correct toolchain for your ARM board. For 64bit SoC ARM board, only install 64bit toolchain, and for 32bit SoC ARM board, only install 32bit toolchain.

Install ARM 32 bits cross compile toolchain：
```
sudo apt-get install gcc-arm-linux-gnueabi g++-arm-linux-gnueabi
```

Install ARM 64 bits cross compile toolchain：
```
sudo apt-get install gcc-aarch64-linux-gnu g++-aarch64-linux-gnu
```

### Cross Compile Livox-SDK
For ARM 32 bits toolchain，In the Livox SDK directory，run the following commands to cross compile the project:
```
cd Livox-SDK
cd build && \
cmake .. -DCMAKE_SYSTEM_NAME=Linux -DCMAKE_C_COMPILER=arm-linux-gnueabi-gcc -DCMAKE_CXX_COMPILER=arm-linux-gnueabi-g++
make
sudo make install
sudo apt install -y ros-$ROS_DISTRO-pcl-ros ros-$ROS_DISTRO-cmake-modules
cd
```
For ARM 64 bits toolchain，In the Livox SDK directory，run the following commands to cross compile the project:
```
cd Livox-SDK
cd build && \
cmake .. -DCMAKE_SYSTEM_NAME=Linux -DCMAKE_C_COMPILER=aarch64-linux-gnu-gcc -DCMAKE_CXX_COMPILER=aarch64-linux-gnu-g++
make
sudo make install
sudo apt install -y ros-$ROS_DISTRO-pcl-ros ros-$ROS_DISTRO-cmake-modules
cd
```

# 4. Complie
As long as the package is under some child directory of src, catkin can detect it automatically. The next step is then to build the system.
```
source /opt/ros/$ROS_DISTRO/setup.bash
cd HRL-UGV-Research-Project
catkin_make
``` 
# 5. Test your project
Several different launch files will be provided after finish testing.

# TODO
At this beginning level, we still have a lot of works that need to be done. Here are some short-term goals:

* Communication layer:
  * Update the ACCR-UTGV protocol. :heavy_check_mark:
  * Add support for DJI AI robot. :heavy_check_mark:
  * Add MQTT protocol to allow data communication between agents or a monitor device. :heavy_check_mark:
  
* Function layer:
  * Add computer vision function.
  * Add navigation function. :heavy_check_mark:
  * Add Livox 3D LiDAR support. :heavy_check_mark:
  * Add RPLiDAR support. :heavy_check_mark:
  
* Decision layer:
  * Add Finite State Machine support, and examples.
  * Add Decision Tree support, and examples. :heavy_check_mark:
  * Add multi-agent support in far future.
