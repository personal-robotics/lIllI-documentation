# ROS 2 Installation in Ubuntu

from: https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html

First ensure that the Ubuntu Universe repository is enabled.

```
sudo apt install software-properties-common
sudo add-apt-repository universe
# ensure package lists are up-to-date before continuing
sudo apt update
```

Now add the ROS 2 GPG key with apt.

```
sudo apt install curl
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

Then add the repository to your sources list.

```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

sudo apt update
```

Install basic ROS2 packages:
```
# minimal install: Communication libraries, message packages, command line tools. No GUI tools.
sudo apt install ros-humble-ros-base

# install rosdep, vcs, colcon
# for building ROS packages
sudo apt-get install python3-rosdep2 python3-vcstool python3-colcon-common-extensions

# ROS 2 control related packages
sudo apt install ros-humble-{ros2-control,ros2-controllers}

# for creating robot descriptions
sudo apt install ros-humble-xacro
```

Automatically enable ROS 2 underlay in current and new terminals:

```
source /opt/ros/humble/setup.bash

cat >> ~/.bashrc <<-EOF

source /opt/ros/humble/setup.bash

EOF
```

## Recommended Optional ROS Packages

### Desktop Installation

```
# ROS, demos, tutorials
sudo apt install ros-humble-desktop
# for ROS topic visualizations
sudo apt-get install rviz
```

### Gazebo Simulator

For installation of gazebo for ROS 2 (from https://gazebosim.org/docs/latest/ros_installation)
```
sudo apt install ros-humble-ros-gz
# ROS 2 control related packages
sudo apt install ros-humble-gazebo-ros2-control
# NOTE package ros-humble-gazebo-ros-pkgs is for ROS 1?
```

