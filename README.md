# Works in Ubuntu16.04

## Preface
This repository contains most of my works in Ubuntu16.04, including system settings, ROS, CV etc.
This repository mainly helps the freshman,like me, to decrease the difficulties during their study in ROS.

## Operating System & Environment
Preparing your operating system that you satisfied with, via dual system installation.

### Install Ubuntu16.04
Although I was leaning towards 18.04 at the time of writing this article, due to subsequent work, this article focuses on the dual-system approach to Ubuntu16.04 installation. Of course, if your device allows it, and you have enough memory, you can install it from a virtual machine.

#### System disk & system Image
NEEDINGS
DEVICE     | NOTES
-------- | -----
Hard disk space  | Pre-allocation via disk management (you can also reserve a disk)
USB  | 8G, as the system installation disk
UltraISO  | Software used to make syshttps://img-blog.csdnimg.cn/20210713093436475.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1lvc2hpbm9fSmlu,size_16,color_FFFFFF,t_70tem installation disks

Open Disk Manager and prepare enough hard disk capacity for your system. As shown in figure.
<img src="https://img-blog.csdnimg.cn/20210713093052990.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1lvc2hpbm9fSmlu,size_16,color_FFFFFF,t_70" width="50%">

You can see I've pre-allocated 80GB of space to Ubuntu.

Use the system disk software to pre-install the image into the USB disk (note that the USB disk will be formatted, please backup the data first), as shown in the figure.

<img src="https://img-blog.csdnimg.cn/20210713093436475.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1lvc2hpbm9fSmlu,size_16,color_FFFFFF,t_70" width="50%">

#### System installation configuration
With the above steps in place, you can start installing Ubuntu.
Here, the system installation process can be carried out according to your own choice. The main process can be found in Bilibili. Here is an installation process from one of UPs that I refer to (not myself).[Vedio of Ubuntu installation](https://www.bilibili.com/video/BV1Wb411W7Br?from=search&seid=7993337817834211111/)

### System environment configuration
#### Root mode Settings
Under normal circumstances, some files need to be accessed through the sudo command. After entering root mode, you can have the highest permissions to manage the files.
```S
// Open Terminal
sudo passwd root
// Set the password
su -
// Enter the password,Enter root mode.
```
#### Mirror source update
[This is the Ali mirror source](https://developer.aliyun.com/special/mirrors/notice)（Is down for maintenance）。
```S
// Open Terminal
sudo gedit /etc/apt/sources.list
// Comment the original content and paste the copied source into this file for saving
sudo apt-get update
// Finished
```
You can also update the source through the software Settings, the appropriate source for Ubuntu source code download to improve stability and speed.

#### Solved some computer WiFi driver problems
With the Lenovo Y7000/9000 series, you may not be able to use WiFi after installing the system. To do this, you need to do the following to turn on WiFi.
```S
// Open Terminal,Check the hardware
rfkill list all
// Remove the wireless part
sudo modprobe -r ideapad_laptop
// Create the launch configuration file
sudo gedit /etc/modprobe.d/ideapad.conf
```
Add the following to the blank text and save
>blacklist ideapad_laptop

```S
// reboot
reboot
```
OK, you can use Wifi now.

#### Other software installation
Including the browser, the input method you can refer to the previous video.
[Vedio of Ubuntu installation](https://www.bilibili.com/video/BV1Wb411W7Br?from=search&seid=7993337817834211111/)

## Python environment configuration
This section explains how to deploy your own Python environment in Ubuntu, mainly for Python3 installation updates. Since the Python dependency of ROS is mainly Python 2.5, and with the passage of time, the version support of Python is also continuously improved, I have a problem of incompatibility between Conda and ROS's Python. Those who are interested can study RO2 and Python and Conda related details, here is only the introduction of the basic Python 3 deployment.

First, download the corresponding version of Python 3.7.* (* means all works) from [python.org](https://www.python.org/downloads/source/)

If you want to open the software update interface, you can link back to 3.5 if you want to open it. Therefore, my scheme is not the optimal environment deployment scheme, you can determine the environment you need to install and deploy.

Open the downloaded folder, unzip it and install it in the following steps.
```S
// Open Termical(I used to put download installations in /usr/local/)
./configure --prefix=/usr/local/python3.7.1
// Compile the file. Make test can be used to check whether the file is missing after compiling
make
// install
sudo make install
// Add an environment variable
PATH=$PATH:$HOME/bin:/usr/local/python3.7.1/bin
// Creating Soft Links
rm /usr/bin/python3
ln -s /usr/bin/python3.7 /usr/bin/python3 
```
Finished.You can type python3 to see that the corresponding version has been updated.

## ROS environment configuration
For the installation and use of ROS, please refer to Gu Yue's video.
[vedio of ROS installation](https://www.bilibili.com/video/BV1zt411G7Vn?from=search&seid=16494886590287037760)
```S
// Open the terminal for source updates
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

sudo apt-get update

// Install ROS via source code（if with clash, it will be really easy）
sudo apt-get install ros-kinetic-desktop-full

// init rosdep
sudo rosdep init
```
f the above step report error, in most cases, the git package web page can not be opened, you can proceed to the operation. Clash makes it easier.
```s
sudo gedit /etc/hosts
```
Add the following
>151.101.84.133 raw.githubusercontent.com

Try it if you still have problems
```s
sudo gedit  /etc/ros/rosdep/sources.list.d/20-default.list
```
Add the following
>#os-specific listings first
>yaml https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/osx-homebrew.yaml osx
>
>#generic
yaml https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/base.yaml
yaml https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/python.yaml
yaml https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/ruby.yaml
gbpdistro https://raw.githubusercontent.com/ros/rosdistro/master/releases/fuerte.yaml fuerte
>
>#newer distributions (Groovy, Hydro, ...) must not be listed anymore, they are being fetched from the rosdistro index.yaml instead

After solving the above problems, let's move on.
```s
rosdep update

// Add an environment variable
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc

// Install the required dependencies
sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential

// start ros
roscore
```
If you can successfully turn on the ROS Master, you're done!

## CUDA installation
While writting,many content is added . It should come to this side as the last part. (LOL)
First, you need to make sure that you have a non-air, CUDA requires Nvidia graphics card support, you can find the corresponding version of your graphics card can install CUDA.
Installation of CUDA10 using DEB is shown here. In order to minimize subsequent crashes, pre-search preparation is in place. This method does not require shutting down the graphical interface.
Open the [CUDA official website](https://developer.nvidia.com/cuda-toolkit-archive), you can choose the corresponding version, and choose the DEB package to install according to your computer.
<img src="https://img-blog.csdnimg.cn/20210713152008484.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1lvc2hpbm9fSmlu,size_16,color_FFFFFF,t_70" width="50%">

The official website will give the installation steps, according to the official website steps, with a little operation, you can easily install.
open the terminal, first disable the original Nouveau driver.
```s
// Disabled if no output is returned from the following command
lsmod | grep nouveau

// Disable, if any output
sudo gedit /etc/modprobe.d/blacklist.conf
```
Enter the following
>blacklist nouveau
options nouveau modeset=0

Return to the terminal after updating the core, restart
```s
sudo update-initramfs –u
```
```s
// Verify again that it is disabled
lsmod | grep nouveau

// Start install CUDA
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-ubuntu1604.pin

sudo mv cuda-ubuntu1604.pin /etc/apt/preferences.d/cuda-repository-pin-600

wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda-repo-ubuntu1604-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
// Make sure your Nouveau driver is turned off before doing this
sudo dpkg -i cuda-repo-ubuntu1604-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb

sudo apt-key add /var/cuda-repo-10-2-local-10.2.89-440.33.01/7fa2af80.pub

sudo apt-get update

sudo apt-get -y install cuda

reboot
// Environment configuration
sudo gedit ~/.bashrc
```
Enter at the end and save.
>export PATH=/usr/local/cuda-10.2/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

```
// Verify that the installation is complete
nvcc -V
```
See the return CUDA version complete!
## summary
This is the end of the beginner's tutorial. I hope it can help you and hope it gets a stat.
