# Clearpath Jackal ROS Noetic Bringup
![jackal](https://user-images.githubusercontent.com/70287453/108798093-87bcd680-7552-11eb-943f-d65a9a743770.jpg)
## Introduction
This README contains instructions of bringing up [Clearpath Jackal](https://clearpathrobotics.com/jackal-small-unmanned-ground-vehicle/) on ROS noetic which is the newest verison of ROS. Since many packages used for Jackal are not officially released for ROS noetic, this instruction will guide you how to build and set up jackal from scratch. Thanks to Jordan Haskel, our previous MSR student who wrote the [ROS Melodic(previous verison) bringup](https://github.com/robo-jordo/jackal_melodic_bringup) for Jackal and it helps me a lot in the building process.
## Fresh Start
I recommended preparing a new SSD instead of wiping out the old SSD, this is helpful for backtracking if you made any mistakes and for debugging.
#### 1. Installing Ubuntu 20.04 LTS:
* Prepare a bootable USB with an Ubuntu 20.04 LTS iso image. Follow the official ubuntu instructions [here](https://ubuntu.com/tutorials/create-a-usb-stick-on-ubuntu#1-overview).
* First make sure the jackal is powered off, then insert the USB into the Jackal along with monitor, keyboard and mouse.
* Wait for Jackal to boot up. It should boot automatically, if it doesn't then follow instructions [here](https://ubuntu.com/tutorials/install-ubuntu-desktop#4-boot-from-usb-flash-drive).
* Once complete, remove the USB and restart the Jackal and you should see the ubuntu login screen.
#### 2. Setting up the wireless network
This step is not strictly necessary but it is very convenient if Jackal directly connected to Wifi.

I used router in Northwestern MSR Lab and it is already set up, below are steps for setting up a new router from previous MSR student.

* Connect the Jackal to your WiFi network.
* Once connected open a Terminal on the Jackal.
* Use $ ifconfig to find out the mac address of the Jackal. This will be listed under your wireless network interface (something like wlp2s0). The mac address will be six sets of hexadecimal numbers separated by colons.
* Log into the setting of your router. These next steps will vary depending on router. But I will provide the instructions as for a tp-link (TL-WR940N) router.
* Navigate to the IP & MAC binding tab on your WiFi router
* Use the MAC address found with ifconfig to bind a desired IP adress to your Jackal (the same can be done with your computer)
* #### Note: use an IP address starting with 192.168.0.XXX so as not to overlap with the IP address of the Velodyne.
* Restarting the router might be necessary for these changes to work.
* Check that this has worked by connecting to the WiFi and running ifconfig and checking that the IP address of your wireless network interface is what you set it as.
* To allow for easy hostname resolution, you will want to add these new static IP addresses to the top of your /etc/hosts file.

      <STATIC_IP>    <HOSTNAME>

For example, on jackal, your host file should looks like this:

    127.0.0.1       localhost
    127.0.1.1       jackal-desktop
    192.168.0.101   dinvincible98

host file on your remote pc:

    127.0.0.1       localhost
    127.0.1.1       dinvincible98
    192.168.0.100   jackal-desktop

* Restart your network manager or your whole pc for changes to take affect.
* Connect to the router and try to ping jackal or SSHing to check if the setup works.
* #### Note: You need to install openssh_server(run sudo apt-get install openssh_server) to let ssh work.
#### 3. Installing ROS Noetic
* Install ROS Noetic on both of your remoet pc and Jackal. Follow the insturctions [here](http://wiki.ros.org/noetic/Installation/Ubuntu).
* I recommended to install Desktop-Full Install as shown on the instructions.
#### 4. Building packages and dependencies on Jackal
Since many packages for Jackal written by Clearpath aren't released for ROS noetic so they need to be built from source.

Below are packages need to be built from source:
* [jackal](https://github.com/jackal/jackal)
* [jackal_robot](https://github.com/jackal/jackal_robot)
* [interactive_marker_twist_sevrer](https://github.com/ros-visualization/interactive_marker_twist_server)
* [LMS1xx](https://github.com/clearpathrobotics/lms1xx)
* [ndt_omp](https://github.com/koide3/ndt_omp)
* [rosserial](https://github.com/ros-drivers/rosserial)
* [openslam_gmapping](https://github.com/ros-perception/openslam_gmapping)
* [slam_gmapping](https://github.com/ros-perception/slam_gmapping)

Below are packages released for ROS Noetic:

Use sudo apt-get install
* slam_toolbox
* nmea_msgs

In order to build those pkgs from source:
* Create a catkin workspace on Jackal
            
      $ mkdir -p ~/jackal_ws/src
      $ cd jackal_ws
      $ catkin_make

* git clone above pkgs into /src directory of the workspace
* Install all required dependencies

      $ cd jackal_ws
      $ rosdep install --from-paths src --ignore-src -r -y

* In jackal_ws, run catkin_make to build pkgs.

      $ catkin_make

#### 5. Building Jackal packages on remote pc
Firstly, install all preivous packages on your computer first, then build below packages from source:
* [jackal_desktop](https://github.com/jackal/jackal_desktop)
* [jackal_simulator](https://github.com/jackal/jackal_simulator)
* [hector_gazebo](https://github.com/tu-darmstadt-ros-pkg/hector_gazebo)
* [hector_models](https://github.com/tu-darmstadt-ros-pkg/hector_models)

Build those pkgs follow the previous instructions, then you can follow the official Jackal [tutorial](https://www.clearpathrobotics.com/assets/guides/kinetic/jackal/simulation.html) and play around.
#### 6. Setting up ROS between Jackal and remote PC
To let your Jackal and PC share a ROS master, you need to set up ROS_MASTER_URI and ROS_HOSTNAME environment on both Jackal and remote pc.

Add this file to Jackal:
* [setup_jackal.bash](https://github.com/dinvincible98/Jackal_ROS_Noetic_Bringup/blob/main/setup_jackal.bash)
* #### Note: I sourced my workspace in the bash file for convenience, you should change the last line to your own workspace.

Add this file to remote pc:
* [setup_laptop.bash](https://github.com/dinvincible98/Jackal_ROS_Noetic_Bringup/blob/main/setup_laptop.bash)
* #### Note: I sourced my workspace in the bash file for convenience, you should change the last line to your own workspace.
* #### Important note: Remeber to source those files whenever you open a new terminal(source setup_jackal.bash if on jackal, source setup_laptop.bash if on remote pc). Or you can add those two files to each's ~/.bashrc if you think it is more conveninent(However if you are not only using Jackal, this way is not recommended).
#### 7. Final setup for Jackal specifics
Those are key steps to get Jackal up and running by setting up all undocumented intricates implemented by Clearpath on a Jacakl image.

* #### [udev rules](https://github.com/dinvincible98/Jackal_ROS_Noetic_Bringup/blob/main/51-ros-indigo-jackal-firmware.rules)

Add this file to Jackal. Copying the rules into /etc/udev/rules.d foler on Jackal. Restart the Jackal to let it take affect.
* #### Note: If this file is not present or not working, the Jackal will not find the motor control board in the /dev folder.
* #### Velodyne 16 setup

In order to get the VLP-16 lidar work with Jackal, Jackal need to set up to interface with the lidar over  the network and some packages are required. The default address for VLP-16 in MSR lab is 192.168.1.201.        (It is already set up, so you can ping it to make sure it works)

Required packages:
Use sudo apt-get install  
* ros-noetic-velodyne-pointcloud (This package contains nodes to read the poincloud data from the velodyne and publish it to the relevant ROS topics.)
* ros-noetic-pointcloud-to-laserscan (This package contains a node to convert 3D point cloud data to a single layer laserscan, which is useful for gmapping and slam_toolbox.)

To publish pointcloud data to a topic, VLP16_point.launch can be used. See [velodyne.launch](https://github.com/dinvincible98/Jackal_ROS_Noetic_Bringup/blob/main/launch/velodyne.launch) file in the repo.

* #### PS3 remote setup
I have not done set up PS3 controller since there are some intractable porblems due to verison problems.  I skipped this part because it is not necessary for my project. However, it is better to set it up for safety concern.

* #### Setup boot procedure
Based on Jordan's [instruction](https://github.com/robo-jordo/jackal_melodic_bringup), there are two sevices and two executable file for Jackal so jackal will automatically be prepared to run when powered on. However, I encountered some system errors due to version when I tried to add those files and they cannot execute. Basically, those two executable files source the jackal_ws, bring up the jackal_move_base and velodyne lidar and run the PS3 controller. Since I have not fix the system error, I did the following:

On both jackal and remote pc:
     
      $ nano ~/.bashrc
Add this line:
      
      $ source ~jackal_ws/devel/setup.bash

On Jackal:
      Include base.launch in your launch file as shown in [slam_toolbox_jackal.launch](https://github.com/dinvincible98/Jackal_ROS_Noetic_Bringup/blob/main/launch/slam_toolbox_jackal.launch)


## Starting the Jackal(Northwestern MSR Lab)
* Turn on the jackal then remember to press the small red button.
* Connect remote pc to MSR jackal router
* SSH into Jackal (ssh jackal@jackal-desktop)
* Make sure you clone this repo to both Jackal and your remote pc
* In every new terminal on the jackal: source setup_jackal.bash
* In every new terminal on the remote pc: source setup_laptop.bash
* On Jackal terminal:
      
      $ roslaunch jackal_noetic_bringup velodyne.launch(Bring up VLP-16)
      $ roslaunch jackal_noetic_bringup slam_toolbox_jackal.launch(Bring up move_base, jackal_base and slam_toolbox)

* On Remote pc:
      
      $ roslaunch jackal_noetic_bringup slam_toolbox_pc.launch(Rviz)


## Demo 
### SLAM

![jackal_slam](https://user-images.githubusercontent.com/70287453/108808584-8d271a80-756c-11eb-939b-a7d0e586fe40.gif)





