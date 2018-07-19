# README

This project is runs ROVIO **without ROS** on a **laptop** camera and IMU sensors plugged in from the USB port.

This project is based on the ROVIO_NOROS. I modified it so that it runs with a laptop and its sensors.
https://github.com/pangfumin/Rovio_NoRos

ROVIO_NOROS is based on ROVIO. ROVIO_NOROS removed ROS from ROVIO.
https://github.com/ethz-asl/rovio

#Installation
The project was built on Ubuntu 14.04, as well as this installation menual.

This installation menu starts from installing Ubuntu 14.04. I hope I have included every dependency needed.

##Install Dependencies

Usually, you run update first
```
sudo apt-get update
```

And it is suggested to install build-essential
```
sudo apt-get install build-essential
```
And then, others.
```
sudo apt-get install cmake
sudo apt-get install libopencv-dev
sudo apt-get install libeigen3-dev
sudo apt-get install libboost-all-dev
sudo apt-get install freeglut3-dev
sudo apt-get install libglew-dev
sudo apt-get install libyaml-cpp-dev
sudo apt-get install libxmu-dev libxi-dev
```

##Install HIDAPI

You can go to the official website for HIDAPI, as following:
https://github.com/signal11/hidapi
Or you can just run the following commands to install it.
```
sudo apt-get install dh-autoreconf
sudo apt-get install git
sudo apt-get install libudev-dev
sudo apt-get install libusb-1.0-0-dev
git clone git://github.com/signal11/hidapi
./bootstrap
./configure
make
sudo make install     <----- as root, or using sudopi.git
```

##Install Google Test

Google test is a framework for writing C++ unit tests.

Use the following command to install Google Test.
```
sudo apt-get install libgtest-dev
```

Be notice that this command only copy the source code to /usr/src/gtest. You have to build and install it yourself.

```
sudo cd /usr/src/gtest
sudo mkdir build
sudo cd build
sudo cmake ..
sudo make 
sudo cp *.a /usr/lib
```

##Build & Run

```
#!command
mkdir build
cd build
cmake ..
make
```

Run ,go to the project dir.
```
#!command
./run.sh
```

Or, you can just run **all.sh** to build & run or to rebuild & rerun.

# 处理图像和IMU

    图像和IMU的处理沿用了PangFumin的思路。只是把输入换成了笔记本摄像头和MPU6000传感器。
    摄像头数据的读取放在了主线程中，IMU数据的读取放在了单独的线程中。
    主线程和IMU线程通过一个Queue共享数据。
# 测试
    反复实验的经验如下：一开始的时候很关键。最好开始画面里有很多的feature。
    这里有很多办法，我会在黑板上画很多的feature，或者直接把Calibration Targets放在镜头前。
    如果开机没有飘（driftaway），就开始缓慢的小幅移动，让ROVIO自己去调整CameraExtrinsics。
    接着，就可以在房间里走一圈，再回到原点。

    我一开始以为如果有人走动，ROVIO会不准。但是实测结果发现影响有限。
    ROVIO有很多特征点，如果一两个特征点位置变动，ROVIO会抛弃他们。这里，ROVIO相当于在算法层面，解决了移动物体的侦测。
# ROVIO与VR眼镜

    一个是微软的Kinect游戏机，用了RGBD摄像头。
    而HTCvive和Oculus，则使用空间的两点和手中的摄像机定位。
    ROVIO没有深度信息，这也许就是为什么它容易driftaway。

    如果要将ROVIO用于产品上，可能还需要一个特定的房间，在这个房间里，有很多的特征点。
    如果房间里是四面白墙，恐怕ROVIO就无法运算了。

# 有限空间的定位

    但是如果环境是可以控制的，也就不需要ROVIO了。
    可以在房间里的放满国际象棋盘，在每个格子里标上数字，
    这样只需要根据摄像头视野中的四个角上的feature(数字)，就能确定位置了。
    这里不需要什么算法，因为这样的排列组合是有限的。
    只需要一个数据库就可以了。在POC阶段，可能会用数字。当然到了产品阶段会换成别的东西。




测量的时候，需要用到精确的深度信息。测量完了，就不需要深度信息了。因为已经建立起了图像和深度信息的对应关系了。

