# VASP Installation

## Overview

In this project, I will be installing vasp and its gpu port. The main compiler for this would be an Intel compiler in combination with the NVIDIA CUDA Compiler for the gpu port. This project would serve as a workstation for our team and would also serve as a guide for those in the same confusing path as me. 

The following main softwares will be discussed in the following sections:

a. Intel Compiler Installation \
b. NVIDIA driver and Cuda Installation \
c. VASP and its GPU port building 

## Computer specification

I know that this guide would surely not be universally applicable in the wide range of hardwares. The computer that I am installing this in is characterized by the following

a. Processor - Intel LGA1151 I7-6700 \
b. Random-Access Memory (RAM) - 32 Gb \
c. Graphics Card - NVIDIA GTX 1070 (Pascal GPU) \
d. Storage - 128 Gb SSD + 1 Tb HDD

## OS Installation

I have chosen to use UBUNTU 16.04.5 LTS (Dekstop version). \
It can be downloaded here (http://releases.ubuntu.com/16.04/). 

Prepare the Boot drive (you can follow this [guide](https://www.howtogeek.com/howto/linux/create-a-bootable-ubuntu-usb-flash-drive-the-easy-way/))

Just proceed with the installation, I would suggest the following manual partition:

| Disk name   | Type       | Size  | Mount point |
|-------------|------------|-------|-------------|
| sda1        | Linux-swap | 33gb  |             |
| sda2        | ext4       | 30gb  | /var        |
| sda3        | ext4       | 10gb  | /boot       |
| sda4        | ext4       | rest  | /opt        |
| sdb1        | ext4       | 200gb | /           |
| sdb2        | ext4       | rest  | /home       |

*Note: sda = SSD, sdb = HDD, rest = rest of the diskspace

## Preparing dependencies and distribution update

### Update the kernel (Due to the need of initramfs command)

1. Download the kernel update [here](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/i915). This [guide](https://askubuntu.com/questions/832524/updated-kernel-to-4-8-now-missing-firmware-warnings/832528#832528) would be useful as reference. For this project, I only needed **kbl_guc_ver9_14.bin** and **bxt_guc_ver8_7.bin**
2. Go to the folder containing the kernel update
3. copy the files to the kernel folder
```shell
% sudo cp kbl_guc_ver9_14.bin /lib/firmware/i915
% sudo cp bxt_guc_ver8_7.bin /lib/firmware/i915
```

A shortcut would be to run the script:

```shell
% sudo ./prepare.sh |& tee prepare_log.log
```

If this is your first time running it and not really sure if it would run then I would suggest not to procede with the running of the script. Instead follow the step-by-step procedure which is useful particularly in debugging or google-ing the problem. 

```shell
% sudo apt-get update 
% sudo apt-get dist-upgrade
% sudo apt-get install build





OS Installation 
asd
asdasdsad
