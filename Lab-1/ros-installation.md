# ROS2 Humble and Gazebo 11 Installation Guide

This guide provides step-by-step instructions for installing ROS2 Humble and Gazebo 11 on Windows using WSL (Ubuntu 22.04).

## Prerequisites

### Install Ubuntu 22.04 on WSL

1. Open the Microsoft Store
2. Search for and install [Ubuntu 22.04.5 LTS](https://www.microsoft.com/store/productId/9PN20MSR04DW)
3. Launch Ubuntu from the Start menu or WSL terminal

## Installation Steps

### 1. Install Gazebo

```bash
sudo apt update
sudo apt upgrade
sudo apt install gazebo
```

### 2. Install ROS2 Humble

Reference: [ROS 2 Documentation: Humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)

**Note:** It's recommended to copy commands from the official website. Commands are provided here for convenience.

#### Step 1: Set Locale

```bash
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```

#### Step 2: Setup Sources

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

#### Step 3: Add ROS 2 GPG Key

```bash
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

#### Step 4: Update Package Index

```bash
sudo apt update
sudo apt upgrade
```

#### Step 5: Install ROS2 Desktop

```bash
sudo apt install ros-humble-desktop
```

#### Step 6: Install Development Tools

```bash
sudo apt install ros-dev-tools
```

#### Step 7: Setup Environment (Auto-source on Terminal Launch)

This step adds the ROS source file to your bash configuration, so it runs automatically whenever you open a terminal.

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

#### Step 8: Test ROS2 Installation

Open two separate terminal windows and run:

**Terminal 1:**
```bash
ros2 run demo_nodes_cpp talker
```

**Terminal 2:**
```bash
ros2 run demo_nodes_py listener
```

You should see the talker publishing messages and the listener receiving them.

## Connecting ROS2 to Gazebo 11

Reference: [Gazebo ROS Packages Tutorial](http://gazebosim.org/tutorials?tut=ros2_installing&cat=connect_ros)

**Note:** Replace `foxy` with `humble` when following the tutorial.

### Install Gazebo ROS Packages

```bash
sudo apt install ros-humble-gazebo-ros-pkgs
sudo apt install ros-humble-ros-core ros-humble-geometry2
```

### Test the Integration

#### 1. Launch Gazebo with Demo World

```bash
gazebo --verbose /opt/ros/humble/share/gazebo_plugins/worlds/gazebo_ros_diff_drive_demo.world
```

#### 2. (Optional) View World File

Install `gedit` for easier file editing in WSL:

```bash
sudo apt install gedit
gedit /opt/ros/humble/share/gazebo_plugins/worlds/gazebo_ros_diff_drive_demo.world
```

#### 3. Test Robot Control

Open a third terminal and publish a velocity command:

```bash
ros2 topic pub /demo/cmd_demo geometry_msgs/Twist '{linear: {x: 1.0}}' -1
```

The robot in Gazebo should start moving forward.

## Troubleshooting

- If you encounter permission issues, ensure you're running commands with appropriate privileges
- For display issues in WSL, you may need to install an X server (like VcXsrv or WSLg)
- Make sure all terminals have sourced the ROS2 setup file (or restart terminals after Step 7)

## Additional Resources

- [ROS2 Humble Documentation](https://docs.ros.org/en/humble/)
- [Gazebo Tutorials](http://gazebosim.org/tutorials)
- [ROS2 + Gazebo Integration](http://gazebosim.org/tutorials?cat=connect_ros)
