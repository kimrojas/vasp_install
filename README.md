# VASP Installation

## Overview

In this project, I will be installing vasp and its gpu port. The main compiler for this would be an Intel compiler in combination with the NVIDIA CUDA Compiler for the gpu port. This project would serve as a workstation for our team and would also serve as a guide for those in the same confusing path as me. 

The following main softwares will be discussed in the following sections:

1. Intel Compiler Installation
2. NVIDIA driver and Cuda Installation
3. VASP and its GPU port building 

## Computer specification

I know that this guide would surely not be universally applicable in the wide range of hardwares. The computer that I am installing this in is characterized by the following

a. Processor - Intel LGA1151 I7-6700 \
b. Random-Access Memory (RAM) - 32 Gb \
c. Graphics Card - NVIDIA GTX 1070 (Pascal GPU) \
d. Storage - 128 Gb SSD + 1 Tb HDD

## OS Installation

I have chosen to use UBUNTU 16.04.5 LTS (Dekstop version). \
It can be downloaded here (http://releases.ubuntu.com/16.04/). 

Prepare the Boot drive ( you can follow the guide: https://www.howtogeek.com/howto/linux/create-a-bootable-ubuntu-usb-flash-drive-the-easy-way/)

Just proceed with the installation, I would suggest the following manual partition:

a.  sda1 – linux-swap – 33gb \
b. sda2 – ext4 – 30gb (MOUNT:/var) \
c.  sda3 – ext4 – 10gb (MOUNT:/boot) \
d. sda3 – ext4 – rest of sda disk (MOUNT:/opt) \
e.  sdb1 – ext4 – 200gb (MOUNT:/) \
f. sdb2 – ext4 – rest of sdb disk (MOUNT:/home) \
*Note: sda = SSD, sdb = HDD






OS Installation 
asd
asdasdsad
