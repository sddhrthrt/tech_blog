---
layout: post
title: "Step 1 of HCI-MP: Installing OpenGL + OpenCV"
date: 2013-02-13 02:14
comments: true
categories: technology howto 
tags:
 - notes
 - opengl
 - opencv
 - hci
 - install
---

I need to install OpenCV and OpenGL to work together for my project. So here are the 
notes I took while installing OpenGL and OpenCV:

``` bash Installing OpenGL http://singhgurjot.wordpress.com/2012/05/17/how-to-install-openglglut-libraries-in-ubuntu-12-04/ From here:
sudo apt-get install freeglut3 freeglut3-dev
sudo apt-get install binutils-gold 
```
<!-- more -->

Installing OpenCV, on the other hand, is a tedious task. Here is the procedure
in a nutshell: 

``` bash Installing OpenCV http://www.samontab.com/web/2012/06/installing-opencv-2-4-1-ubuntu-12-04-lts/ From excellent guide here:
sudo apt-get update
sudo apt-get upgrade

sudo apt-get install build-essential libgtk2.0-dev libjpeg-dev libtiff4-dev libjasper-dev libopenexr-dev cmake python-dev python-numpy python-tk libtbb-dev libeigen2-dev yasm libfaac-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev libqt4-dev libqt4-opengl-dev sphinx-common texlive-latex-extra libv4l-dev libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev

cd ~
wget http://downloads.sourceforge.net/project/opencvlibrary/opencv-unix/2.4.1/OpenCV-2.4.1.tar.bz2
tar -xvf OpenCV-2.4.1.tar.bz2
cd OpenCV-2.4.1

mkdir build
cd build
cmake -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_QT=ON -D WITH_OPENGL=ON ..

```

Now we gotta check in the generated output if Python, OpenGL supports are ticked. Qt
can also be checked for, but I didn't want it, so I ignored that part.

If OpenGL is not ticked, that's because you need to install GTK+ Extensions for OpenGL, which you can do by:

``` bash GTK+ Extensions for OpenGL http://stackoverflow.com/questions/11035500/trying-to-build-opencv-2-4-1-with-opengl-support SO Question here:

sudo apt-get install libgtkglext1 libgtkglext1-dev

```

Once we are done, we are all set to install OpenCV:


``` bash Installing OpenCV http://www.samontab.com/web/2012/06/installing-opencv-2-4-1-ubuntu-12-04-lts/ From excellent guide here:

make
sudo make install


```
You may need to log-out and log-in again. Now you have installed OpenCV and OpenGL.
