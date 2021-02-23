# Clearpath Jackal ROS Noetic Bringup
![jackal](https://user-images.githubusercontent.com/70287453/108798093-87bcd680-7552-11eb-943f-d65a9a743770.jpg)
## Introduction
This README contains instructions of bringing up [Clearpath Jackal](https://clearpathrobotics.com/jackal-small-unmanned-ground-vehicle/) on ROS noetic which is newest verison of ROS. Since many packages used for Jackal are not officially released for ROS noetic, this instruction will guide you how to build and set up jackal from scratch. Thanks for Jordan Haskel, our previous MSR student who wrote the [ROS Melodic(previous verison) bringup](https://github.com/robo-jordo/jackal_melodic_bringup) for Jackal and it helps me a lot in the buidling process.
## Fresh Start
I recommended preparing a new SSD instead of wiping out the old SSD, this is helpful for backtracking if you made any mistakes and for debugging.

1. Install Ubuntu 20.04 LTS:
* Prepare a bootable USB with an Ubuntu 20.04 LTS iso image. Follow the official ubuntu instructions [here](https://ubuntu.com/tutorials/create-a-usb-stick-on-ubuntu#1-overview).
* First make sure the jackal is powered off, then insert the USB into the Jackal along with monitor, keyboard and mouse.
* Wait for Jackal to boot up(It should boot automatically, if it doesn't then follow [instructions] here(https://ubuntu.com/tutorials/install-ubuntu-desktop#4-boot-from-usb-flash-drive))
* 




