# Moveit1_Tutorials
A Tutorial for moveit install on ROS noetic. 
In this tutorial some unconventional ROS tools (like 'catkin build') shall be used. If you're not experienced with ROS system, I'd suggest reinstalling your whole operating system to start with a clean slate. The required OS is Ubuntu 20.04 LTS.
### 1. ROS Noetic setup
First of all, add an entry to your sources list.

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

Set up your keys

```
sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

Update your system

```
sudo apt update
sudo apt upgrade
```

Install ROS

```
sudo apt install ros-noetic-desktop-full
```

To verify everything is correct run a search for available ROS packages. Its output shouldn't be empty.

```
apt search ros-noetic
```

Set up your environment by sourcing the following bash file

```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Install all dependencies required for building packages

```
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
sudo apt install ros-noetic-catkin python3-catkin-tools python3-osrf-pycommon
```

Initialize rosdep

```
sudo apt install python3-rosdep
sudo rosdep init
rosdep update
```

Now its time to create a catkin workspace. Execute all of the following commands. If the last command returns error 'catkin: command not found' refer to 'Troubleshooting[1]' section at the end of this tutorial.

```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin build
```

To finish initialization of your workspace add a source command to bashrc file 

```
echo 'source ~/ws_moveit/devel/setup.bash' >> ~/.bashrc
source ~/.bashrc
```

**IMPORTANT. from now on every time you build your workspace ONLY 'catkin build' may be used. It REPLACES conventional 'catkin_make' and 'catkin_make_isolated'.**

### 2. Moveit1 installation

Now lets get down to Moveit installation. To verify you have everything is ready, run all theese commands. 

```
rosdep update
sudo apt update
sudo apt dist-upgrade
sudo apt install ros-noetic-catkin python3-catkin-tools python3-osrf-pycommon
sudo apt install python3-wstool
```

Navigate to your catkin workspace and clone moveit from git

```
cd ~/catkin_ws/src

wstool init .
wstool merge -t . https://raw.githubusercontent.com/ros-planning/moveit/master/moveit.rosinstall
wstool remove  moveit_tutorials  # this is cloned in the next section
wstool update -t .
```

Delete panda_moveit_config folder manually. Than clone its newest version from git.

```
cd ~/catkin_ws/src
git clone https://github.com/ros-planning/moveit_tutorials.git -b master
git clone https://github.com/ros-planning/panda_moveit_config.git -b noetic-devel
```

Resolve all dependencies with a specialized ROS tool. Just in case do it twice, does not always work the first time.

```
cd ~/catkin_ws/src
rosdep install -y --from-paths . --ignore-src --rosdistro noetic

cd ~/catkin_ws
rosdep install -y --from-paths . --ignore-src --rosdistro noetic
```

Finally, build all packages. Build will take some time. If for some reason build fails with an error 'Could not find a package configuration file provided by ...', refer to 'Troubleshooting[2]' section.

```
cd ~/catkin_ws
catkin config --extend /opt/ros/${ROS_DISTRO} --cmake-args -DCMAKE_BUILD_TYPE=Release
catkin build
```

That's it, your Moveit showuld work now! Try it with

```
roslaunch panda_moveit_config demo.launch rviz_tutorial:=true
```

For further instrunctions refer to Moveit official documentation: https://ros-planning.github.io/moveit_tutorials/
Please report any incensistencies in this tutorual to me via email.

### 3. Troubleshooting
1. Command 'catkin' is not found. Install it with

```
sudo apt install ros-noetic-catkin python3-catkin-tools python3-osrf-pycommon
```

2. Some packages required by 'catkin build' are not installed. The simplest solution is just to install the missing package  with 

```
apt get ros-noetic-package-name
```
where package-name is a name of the package with whitespaces and underscores replaced by dashes. If that does not work thry to download the missing package from source like descried here (https://github.com/tuw-robotics/tuw_marker_detection/issues/4)

3. On WSL2 there's a bug with manipulator not showing in RVIZ. Collisons are rendering correctly though. It is currently veing investigated.


