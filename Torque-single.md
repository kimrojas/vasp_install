

ibxml2-devel, openssl-devel, and boost-devel packages must be installed.
I also have tcl (8.6 installed) 
### install dependencies 
`sudo apt-get install libboost-all-dev  libssl-dev libxml2-dev`

### Download the torque repository (assume you have already downloaded git)

` git clone https://github.com/adaptivecomputing/torque.git 

### unzip the file

` unzip master.zip `

### enter the folder
` cd torque-master `
### launch autogen
` ./autogen.sh`

### configure (I'm including gpu capability, just remove the option for the gpu if you don't want)
`./configure --with-debug --prefix=/opt/torque --enable-nvidia-gpus --with-nvml-lib=/opt/cuda-8.0/lib64/stubs --with-nvml-include=/opt/cuda-8.0/include` 

Result: 
> Building components: server=yes mom=yes clients=yes
>                     gui=no drmaa=no pam=no
>PBS Machine type    : linux
>Remote copy         : /usr/bin/scp -rpB
>PBS home            : /var/spool/torque
>Default server      : hpcl-G11CD
This installs all components needed

### launch make
`make`

### install
`sudo make install`



