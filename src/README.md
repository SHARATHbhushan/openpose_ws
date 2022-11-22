## Ros Openpose installation, Ubuntu 18.04

This project is a roadmap to install and run openpose with ros wrapper in ubuntu 18.04

###Prerequisites

Follow the link and install the following Prerequisites

- ROS Melodic : http://wiki.ros.org/melodic/Installation/Ubuntu

- Install librealsense: https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md

- Install cmake-gui for ubuntu 18.04: https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/v1.6.0/doc/prerequisites.md#ubuntu-prerequisites

- use this cmake installation path further

### GPU setup

This project was tested on Lenevo Thinkpad with Nvidia Quadro P2000 GPU and intel i7 8th gen processor.

Follow the steps to setup GPU to work with openpose

- Installed nvidia driver version: 470

can be installed from ubuntu's software and updates application or using cuda toolkit

#### cuda toolkit installation (Cuda 10.0)

- Use runfile installation and also apply the patch
- https://developer.nvidia.com/cuda-10.0-download-archive 
- Post installation steps [mandatory](https://developer.download.nvidia.com/compute/cuda/10.0/Prod/docs/sidebar/CUDA_Installation_Guide_Linux.pdf#%5B%7B%22num%22%3A230%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C108%2C481.949%2Cnull%5D)
- check installation: `nvcc --version`

#### cudnn (7.6.5)

- download cuDNN Library for Linux
- https://developer.nvidia.com/rdp/cudnn-archive#a-collapse765-10
- extract the folder


```
$ cd folder/extracted/contents
$ sudo cp -P include/cudnn.h /usr/include
$ sudo cp -P lib64/libcudnn* /usr/lib/x86_64-linux-gnu/
$ sudo chmod a+r /usr/lib/x86_64-linux-gnu/libcudnn*
```

###Installation

openpose installation 

```
$git clone https://github.com/CMU-Perceptual-Computing-Lab/openpose
$git checkout tags/v1.6.0
$cd openpose
$mkdir build && cd build
$pathtocmake/bin/cmake-gui ..
(select build python, click on configure, unix configuration and generate)
$sed -ie 's/set(AMPERE "80 86")/#&/g'  ../cmake/Cuda.cmake
$sed -ie 's/set(AMPERE "80 86")/#&/g'  ../3rdparty/caffe/cmake/Cuda.cmake
$make -j`nproc`
$sudo make install
```

test installation

`$./build/examples/openpose/openpose.bin --video examples/media/video.avi`

###ROS setup

- clone repositoy (todo)




building environment

```
sudo apt-get install ros-melodic-ddynamic-reconfigure
cd openpose_ws/src
roscd ros_openpose/scripts
chmod +x *.py
cd ../../..
catkin_make
```

- If compilation fails by showing the following error-

`/usr/bin/ld: cannot find -lThreads::Threads`

- In this case, please put the following by editing the              CMakeLists.txt

`find_package(Threads REQUIRED)`

- also you can find more information [here](https://github.com/ravijo/ros_openpose#troubleshooting)


- follow these [steps](https://github.com/ravijo/ros_openpose#configuration) to add model path to launch file

- launch nodes

`roslaunch ros_openpose run.launch`

- if libcaffe [error](https://github.com/CMU-Perceptual-Computing-Lab/openpose/issues/148) persists :`export LD_LIBRARY_PATH=/path/to/folder/contains/libcaffe/:$LD_LIBRARY_PATH`







