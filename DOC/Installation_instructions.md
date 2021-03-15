# Installing Jinx

These are some steps required to get the Jinx PC up and running from a fresh Ubuntu installation. These were tested with ROS Noetic and Ubuntu 20.04. 

# Install ros noetic (tested :+1:) 

Install ros and setup the work space :
https://github.com/wsnewman/learning_ros_setup_scripts/blob/master/install_ros_and_tools_noetic.sh

Install external packages:
https://github.com/wsnewman/learning_ros_external_packages

sudo apt-get install ros-noetic-serial ( does not exist for noetic yet, build it from source https://github.com/wjwwood/serial, you might have to install doxygen from apt)


```sudo apt-get install libusb-1.0-0 libusb-1.0-0-dev automake ```

# Installing Phidget library

Download Linux phidgets drivers from here : 
http://www.phidgets.com/docs/OS_-_Linux

Go into extracted directory 

```
./configure
make 
sudo make install
```

Move udev rules to /etc/udev/rules.d:

``sudo cp plat/linux/udev/99-libphidget22.rules /etc/udev/rules.d``

Add user to dialout group:
```
sudo usermod -a -G dialout $USER
reboot 
```

## Phidgets ros packages
```
cd ros_ws/src
git clone -b noetic https://github.com/ros-drivers/phidgets_drivers.git
git clone https://github.com/ipa320/cob_extern

roscd & catkin_make
```
Copy udev rule to etc/udev/rules.d/:
```
roscd phidgets_api
sudo cp debian/udev /etc/udev/rules.d/99-phidgets.rules
sudo udevadm control --reload-rules
sudo udevadm trigger

```
Resources:
- [phidgets_drivers](https://github.com/ccny-ros-pkg/phidgets_drivers)
- [cob_extern](https://github.com/ipa320/cob_extern)


# Installing Sick driver: (tested :+1:) 

```
roscd
git clone https://github.com/ros-drivers/sicktoolbox.git src/sicktoolbox
git clone https://github.com/ros-drivers/sicktoolbox_wrapper.git src/sicktoolbox_wrapper
rosdep install sicktoolbox rviz
rosmake sicktoolbox rviz
```

Refer to this tutorial: [how-to-install-sicktoolbox-in-kinetic](https://answers.ros.org/question/297938/how-to-install-sicktoolbox-in-kinetic/ )

# Installing baxter: (tested :+1:) 
From : https://sdk.rethinkrobotics.com/wiki/Workstation_Setup

```
sudo apt update
sudo apt-get install git-core python3-argparse python3-wstool python3-vcstools python3-rosdep ros-noetic-control-msgs ros-noetic-joystick-drivers

cd ~/ros_ws/src
wstool init .
wstool merge https://raw.githubusercontent.com/RethinkRobotics/baxter/master/baxter_sdk.rosinstall
wstool update
roscd
catkin make
```
 (I removed cfg/HeadActionServer.cfg from Cmakelist of baxter_interface folder duo to compiling error)

```
roscd
wget https://github.com/RethinkRobotics/baxter/raw/master/baxter.sh
chmod u+x baxter.sh
```

#edit baxter.sh: change baxter name and ros_ip addresss:

```
gedit baxter.sh &
```

Change the following lines in baxter .sh:
```
baxter_hostname="baxter01" #or baxter IP which is: "129.22.143.164"
your_ip="jinx"  #or ip address from ifconfig 
ros_version="noetic"
```
then save and exit

to activate baxter master:
```
roscd
./baxter.sh
```

or you can create an alias in you .bashrc like this:
```
echo "alias baxter_master=~/ros_ws/baxter.sh" >> ~/.bashrc
```
## Update: baxter incompatable with python 3
Baxter packages are still incompatable with python 3. I had to replace my baxter_tools and baxter_interface packages with modified packages that support python 3:

- https://github.com/CentraleNantesRobotics/baxter_interface
- https://github.com/CentraleNantesRobotics/baxter_tools.git


# Install Roboteq driver (motor controller):

```
roscd
git clone https://github.com/Roboteq-Inc/ROS-Driver.git src/roboteq/ROS-Driver
source /tmp/usr/local/setup.sh
catkin_make
```
reference: https://github.com/Roboteq-Inc/ROS-Driver/tree/update_serial/ROS-Driver-Update

# Kinect driver installation (tested :+1:) 
```
sudo apt install ros-noetic-openni2-camera ros-noetic-openni2-launch ros-noetic-openni2-camera-dbgsym 
cd ~/ros_ws/src
git clone https://github.com/ros-drivers/openni_camera.git
cd ~/ros_ws
catkin make
```
For launching kinect and observing the pointcloud, refer to openni_launch wki page:
https://wiki.ros.org/openni_launch

# STDR_Simulator (tested :+1:) 
First install qt4 and make it default
```
sudo add-apt-repository ppa:rock-core/qt4
sudo apt-get update
sudo apt-get install libqtcore4 qt4-qmake libqt4-dev
export QT_SELECT=4
```
make sure it is the active environment by running
```
qtchooser -print-env
```
then install the map_server
```
sudo apt install ros-noetic-map-server
```
Then clone and compile the stdr_simulator package
```
cd ~/ros_ws/src
git clone https://github.com/stdr-simulator-ros-pkg/stdr_simulator.git
cd ..
rosdep install --from-paths src --ignore-src --rosdistro noetic
catkin_make -DQT_QMAKE_EXECUTABLE=/usr/bin/qmake-qt4
```
