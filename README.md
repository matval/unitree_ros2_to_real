# Introduction
This repository can send control command to real robot from ROS2 and republish camera videos as rostopics.

## Packages:

Basic message function: `unitree_legged_msgs`

The interface between ROS and real robot: `unitree_legged_real`

# Dependencies
* Install Boost:
```
sudo apt-get install libboost-all-dev
```
* [unitree_legged_sdk](https://github.com/unitreerobotics): v3.5.1
* [Lightweight Communications and Marshalling (LCM)](https://github.com/lcm-proj/lcm/releases): v1.5.0

# Configuration
First, creat a directory.
```
mkdir -p ~/ros2_ws/src
```
Then download this package into this `~/ros_ws/src` folder. 

After you download this package into this folder, your folder should be like this
```
~/ros2_ws/src/unitree_quadruped_go1
```

Install dependencies with rosdep
```
rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-5.3.1 urdfdom_headers"
```

# Build
```
colcon build
```

# Setup the net connection
First, please connect the network cable between your PC and robot. Then run `ifconfig` in a terminal, you will find your port name. For example, `enx000ec6612921`.

Then, open the `ipconfig.sh` file under the folder `unitree_legged_real`, modify the port name to your own. And run the following commands:
```
sudo chmod +x ipconfig.sh
sudo ./ipconfig.sh
```
If you run the `ifconfig` again, you will find that port has `inet` and `netmask` now.
In order to set your port automatically, you can modify `interfaces`:
```
sudo gedit /etc/network/interfaces
```
And add the following 4 lines at the end:
```
auto enx000ec6612921
iface enx000ec6612921 inet static
address 192.168.123.162
netmask 255.255.255.0
```
Where the port name have to be changed to your own.

# Run the package
Before you do high level or low level control, you should run the `ros2_udp` node, which is a bridge that connects users and robot
```
ros2 run unitree_legged_real ros2_udp highlevel
```

or

```
ros2 run unitree_legged_real ros2_udp lowlevel
```

it depends which control mode(low level or high level) you want to use.

In the high level mode, you can run the node `ros2_walk_example`
```
ros2 run unitree_legged_real ros2_walk_example
```

In the low level mode, you can run the node `ros2_position_example`
```
ros2 run unitree_legged_real ros2_position_example
```

And before you do the low-level control, please press L2+A to sit the robot down and then press L1+L2+start to make the robot into
mode in which you can do joint-level control, finally make sure you hang the robot up before you run low-level control.

