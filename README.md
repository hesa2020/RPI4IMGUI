# RPI4IMGUI
Working implementation for running imgui on a RasperryPI 4 Model 4.

It can be ran without a desktop environment and my touchscreen still works in this setup!

I will eventually make a guide to explain how to install and run this project.

# But for now here are the notes ive taken:

# update pi kernel to latest
sudo rpi-update
# edit device configs:
## dtoverlay=vc4-kms-v3d
## max_framebuffers=2
## hdmi_force_hotplug=1
## hdmi_group=2
## hdmi_mode=81
sudo nano /boot/config.txt

# reboot
sudo reboot

# dependencies
sudo apt-get update
sudo apt-get install libegl1-mesa-dev libdrm-dev libgbm-dev libgles2-mesa-dev libglfw3-dev libgles2-mesa-dev libudev-dev libsdl2-dev
# this line didnt fix one of my issue but I ran it at some point: 
sudo apt-get install libraspberrypi-dev raspberrypi-kernel-headers
# fix issue with cmake not finding lib bcm_host
sudo cp libbcm_host.so /usr/lib/libbcm_host.so

# install everything Debian uses to build SDL
sudo apt build-dep libsdl2
# Grab the latest stable SDL source tarball 
wget https://www.libsdl.org/release/SDL2-2.0.10.tar.gz
# extract it
tar -xf SDL2-2.0.10.tar.gz
cd SDL2-2.0.10/
# configure
./configure --enable-video-kmsdrm
# Build & install SDL (Make sure it uses libudev if you want inputs working)
make -j4 && sudo make install
