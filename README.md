# Clearpath Jackal ROS Noetic Bringup
![jackal](https://user-images.githubusercontent.com/70287453/108798093-87bcd680-7552-11eb-943f-d65a9a743770.jpg)
## Introduction
This README contains instructions of bringing up [Clearpath Jackal](https://clearpathrobotics.com/jackal-small-unmanned-ground-vehicle/) on ROS noetic which is newest verison of ROS. Since many packages used for Jackal are not officially released for ROS noetic, this instruction will guide you how to build and set up jackal from scratch. Thanks for Jordan Haskel, our previous MSR student who wrote the [ROS Melodic(previous verison) bringup](https://github.com/robo-jordo/jackal_melodic_bringup) for Jackal and it helps me a lot in the buidling process.
## Fresh Start
I recommended preparing a new SSD instead of wiping out the old SSD, this is helpful for backtracking if you made any mistakes and for debugging.
#### 1. Installing Ubuntu 20.04 LTS:
* Prepare a bootable USB with an Ubuntu 20.04 LTS iso image. Follow the official ubuntu instructions [here](https://ubuntu.com/tutorials/create-a-usb-stick-on-ubuntu#1-overview).
* First make sure the jackal is powered off, then insert the USB into the Jackal along with monitor, keyboard and mouse.
* Wait for Jackal to boot up(It should boot automatically, if it doesn't then follow instructions [here](https://ubuntu.com/tutorials/install-ubuntu-desktop#4-boot-from-usb-flash-drive).
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

#### 3. Installing ROS Noetic
* Install ROS Noetic on both of your remoet pc and Jackal. Follow the insturctions [here](http://wiki.ros.org/noetic/Installation/Ubuntu).
* I recommended to install Desktop-Full Install as shown on the instructions.

#### 4. 




    


    
             
 




