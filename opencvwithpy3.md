## Hacking Opencv 3.2 with python 3.5

OpenCV or open computer vision is not only a great library to learn about the basic of vision in general but it is also one of the most widely used libraries at the production level. In order to get it working on your machine (Linux assumed) you need to follow a set process which I have generalised into a standard bash script.

Hoever it is usually built with python 2.x support. With libraries like [darkflow](https://github.com/thtrieu/darkflow) requiring python 3 support with opencv you have the simplest option as:

```
sudo pip3 install opencv-python
```
The problem with this install is on Linux it is not built with FFMPEG, which mean you cannot use commands like cv2.imshow() or cv2.VideoCapture() which basically is pointless if you want to work with cameras.

However currently there is not much support for the same and almost every tutorial requires you to install a virtual environment which may or may not be the thing you require.

SO LETS HACK!

# Building without Virtual Environment

1. Install the dependencies
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential cmake pkg-config
sudo apt-get install libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libgtk-3-dev
sudo apt-get install libatlas-base-dev gfortran
```
2. Fetch the latest version of opencv (3.2) and unzip it

```
wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.2.0.zip
unzip opencv.zip
wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.2.0.zip
unzip opencv_contrib.zip
```

3. Install the python3 dependencies

```
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
sudo apt-get install python3.5
pip install numpy
```

4. Make a build subfolder inside your opencv-3.2.0 folder

```
cd ~/opencv-3.2.0/
mkdir build && cd build
```

5. Cmake using the following command

```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D PYTHON3_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.5m.so \
-D PYTHON3_EXECUTABLE=/usr/bin/python3.5m \
-D PYTHON3_INCLUDE_DIR=/usr/include/python3.5m/ \
-D PYTHON_INCLUDE_DIR=/usr/include/python3.5m/ \
-D PYTHON3_INCLUDE_DIRS=/usr/include/python3.5m/ \
-D PYTHON_INCLUDE_DIRS=/usr/include/python3.5m/ \
-D BUILD_opencv_python3=ON \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.2.0/modules \
-D INSTALL_C_EXAMPLES=OFF \
-D BUILD_EXAMPLES=ON \
-D BUILD_EXAMPLES=ON ..
```

# ENJOY!
