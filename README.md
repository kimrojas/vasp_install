# VASP Installation

## Overview

In this project, I will be installing vasp and its gpu port. The main compiler for this would be an Intel compiler and mpi in combination with the NVIDIA CUDA Compiler for the gpu port. This project would serve as a workstation for our team and would also serve as a guide for those in the same confusing path as me. 

The following main softwares will be discussed in the following sections:

a. Intel Compiler Installation \
b. NVIDIA driver and Cuda Installation \
c. VASP and its GPU port building 

For this section, I uploaded the installers that I used at the time of my installation. All except for the VASP source code as it is sensitive. VASP source code comes witht the license, please refer to [VASP](https://www.vasp.at/index.php/faqs/71-how-can-i-purchase-a-vasp-license)

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
| sda1        | ext4       | rest  | /           |
| sdb1        | ext4       | rest  | /home       |
| sdb2        | Linux-swap | 33gb  |             |

*Note: sda = SSD, sdb = HDD, rest = rest of the diskspace
*Note: Set pcie_aspm=off or disable the similar parameter in bios
## Preparing dependencies and distribution update

### Update the kernel (Due to the need of initramfs command)

#### 1. Download the kernel update [here](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/i915). 
This [guide](https://askubuntu.com/questions/832524/updated-kernel-to-4-8-now-missing-firmware-warnings/832528#832528) would be useful as reference. For this project, I only needed **kbl_guc_ver9_14.bin** and **bxt_guc_ver8_7.bin**
#### 2. Go to the folder containing the kernel update
#### 3. Copy the files to the kernel folder
```shell
% sudo cp kbl_guc_ver9_14.bin /lib/firmware/i915
% sudo cp bxt_guc_ver8_7.bin /lib/firmware/i915
```

### Preparations proper

#### Updates and dependencies

```shell
% sudo apt-get update 
% sudo apt-get dist-upgrade
% sudo apt-get -y install build-essential emacs dkms synaptic g++ g++-multilib gfortran
% sudo apt-get update
```
## NVIDIA Driver and Cuda Installation

### Prepare or copy the cuda-deps file

1. Create file 
```shell
% gedit cuda-deps
```

2.	Paste the following: 

 > ca-certificates-java default-jre default-jre-headless fonts-dejavu-extra freeglut3 freeglut3-dev java-common libatk-wrapper-java libatk-wrapper-java-jni  libdrm-dev libgl1-mesa-dev libglu1-mesa-dev libgnomevfs2-0 libgnomevfs2-common libice-dev libpthread-stubs0-dev libsctp1 libsm-dev libx11-dev libx11-doc libx11-xcb-dev libxau-dev libxcb-dri2-0-dev libxcb-dri3-dev libxcb-glx0-dev libxcb-present-dev libxcb-randr0-dev libxcb-render0-dev libxcb-shape0-dev libxcb-sync-dev libxcb-xfixes0-dev libxcb1-dev libxdamage-dev libxdmcp-dev libxext-dev libxfixes-dev libxi-dev libxmu-dev libxmu-headers libxshmfence-dev libxt-dev libxxf86vm-dev lksctp-tools mesa-common-dev  x11proto-core-dev x11proto-damage-dev  x11proto-dri2-dev x11proto-fixes-dev x11proto-gl-dev x11proto-input-dev x11proto-kb-dev x11proto-xext-dev x11proto-xf86vidmode-dev xorg-sgml-doctools xtrans-dev libgles2-mesa-dev
 
 3. Install the dependencies
 
 ```shell 
 % cat cuda-deps | xargs sudo apt-get -y install
 ```
 
 ### Driver Installation
 
 #### 1. Reboot 
```shell
% sudo reboot now
```
#### 2. On login screen access TTY1 by presing *ctrl + alt + F1*

#### 3. stop lightdm
```shell
% sudo service lightdm stop
```

#### 4. Install the driver
```shell
% sudo apt-get --purge remove nvidia-*
% sudo add-apt-repository ppa:graphics-drivers/ppa
% sudo apt-get update
% sudo apt-get install nvidia-375 
% sudo reboot now
 ```
 
 #### 5. Check driver version
 ```shell
 % nvidia-smi
 % cat /proc/driver/nvidia/version
 ```
#### 6. Output should be similar to this:
>

**CHECK PLS CHECK**

### Cuda Installation

#### 1. Download [Cuda 8.0](https://developer.nvidia.com/cuda-80-ga2-download-archive)

Download by:

![img1]

![img2]

[img1]: https://github.com/kimrojas/vasp_install/blob/master/readmereference/img1.PNG
[img2]: https://github.com/kimrojas/vasp_install/blob/master/readmereference/img2.PNG

#### 2. Reboot
```shell
% sudo reboot now
```

#### 3. on Login screen, access TTY1 by pressing 'ctrl + alt + f1'

#### 4. stop lightdm
```shell
% sudo service lightdm stop
```

#### 5. Navidate to the folder containing the cuda8.0 run file (cuda_8.0.27_linux.run)

#### 6. Install
```shell
% chmod 755 cuda_*
% ./cuda_8.0.27_linux.run --silent --toolkit --samples -- samplespath=/usr/local/cuda-8.0/samples --override
% ./cuda_8.0.27_linux.run --silent --toolkit --toolkitpath=/opt/cuda-8.0/ --samples -- samplespath=/opt/cuda-8.0/samples --override (optional)
```

#### 7. Designate the CUDA PATH 
```shell
% gedit ~/.bashrc
```
**Add the following lines to the bottom of the file**
> export PATH=$PATH:/usr/local/cuda-8.0/bin \
> export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-8.0/lib64

**Source the path afterwards**
```shell
% source ~/.bashrc
```
#### 8. Verify the cuda installation
```shell
nvcc -V
```

## Intel Package

#### 1. Download the package [here](https://software.intel.com/en-us/parallel-studio-xe)

Note: For students and faculty that requires the compiler, the license is **FREE**. Just register as a student/faculty and you will be given a license. 

#### 2. Extract the package

```shell
% tar zxvf parallel*
```

#### 3. Install the package

```shell
% cd parallel*
% sudo ./install.sh
```

Just keep on accepting everything and enter your Intel license to activate the compiler. 

#### 4. Designate the INTEL PATH

```shell
% gedit ~/.bashrc
```

add the following lines at the bottom of the file

For the Intel Compiler ( Fortran & C++ ) environmental setting \

>  export PATH=$PATH:/opt/intel/vtune_amplifier_xe \
>  export PATH=$PATH:/opt/intel/inspector_xe \
>  source /opt/intel/bin/compilervars.sh intel64 \

for the mpiifort (intel mpi) 

> source /opt/intel/impi/5.1.3.258/bin64/mpivars.sh 


#### 5. Source

```shell
% source ~/.bashrc
```

## VASP Installation

#### 1. Prepare installation directory 

Have the following file inside a folder. For my case, I have 'VASP' folder.
> vasp.5.4.4.tar.gz \
> patch.5.4.4.16052018.gz

#### 2. Extract the package and combine
```shell
% tar zxvf vasp.5.4.4.tar.gz
% gunzip patch.5.4.4.16052018.gz
% cp patch* vasp.5.4.4
% cd vasp.5.4.4
% patch -p0 < patch.5.4.4.16052018.gz
```

#### 3. Install


```shell
% cd vasp.5.4.4
% cp /arch/makefile.include.linux_intel makefile.include
```

**Change a portion of your makefile.include depending on your system**

In my system, I am using the GTX 1070 which has a compute capability of 6.1 based on [ref.](https://developer.nvidia.com/cuda-gpus) and thus my edit on the makefile.include is:

```shell
% gedit makefile.include
```

Change the GENCODE_ARCH tag to:
> GENCODE_ARCH := -gencode=arch=compute_61,code=\"sm_61,compute_61\"

#### 4. Building the executables

##### 4.1. CPU-based VASP

```shell
% make all
```
This will build the std, ncl and gam version of VASP

##### 4.2. GPU-ported VASP

```shell 
% make gpu gpu_ncl
```

This will build the gpu-ported std and ncl version of VASP.

#### 4.3. Verify the building process
The build details will be in `/build` while the executable is in `/bin`

```shell
% ls -l /build
% ls -l /bin
```

## Global PATHs

access as root 
```shell 
% gedit /etc/environment
% gedit /profile.d/cuda.sh
% gedit /profile.d/intel.sh
```


