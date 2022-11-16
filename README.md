# Moveit1_Tutorials
A Tutorial for moveit install on ROS noetic. 
In this tutorial some unconventional ROS tools (like 'catkin build') shall be used. If you're not experienced with ROS system, I'd suggest reinstalling your whole operating system to start with a clean slate. The required OS is Ubuntu 20.04 LTS.
First of all, add an entry to your sources list.

'''
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
'''

Set up your keys

'''
sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
'''

Update your system

'''
sudo apt update
sudo apt upgrade
'''

Install ROS

'''
sudo apt install ros-noetic-desktop-full
'''

To verify everuthing is correct run a search for available ROS packages

'''
apt search ros-noetic
'''

Set up your environment by sourcing the following bash file

'''
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
'''

Install all dependencies required for building packages

'''
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
'''

Initialize rosdep

'''
sudo apt install python3-rosdep
sudo rosdep init
rosdep update
'''

Now its time to create a catkin workspace. Execute all of the following commands. If the last command returns error 'catkin: command not found' refer to 'Troubleshooting[1]' section at the end of this tutorial.

'''
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin build
'''


