
PRIMARY REFERENCE: https://ubuntuforums.org/showthread.php?t=1372508
https://jabriffa.wordpress.com/2015/02/11/installing-torquepbs-job-scheduler-on-ubuntu-14-04-lts/
https://github.com/adaptivecomputing/torque
ibxml2-devel, openssl-devel, and boost-devel packages must be installed.
I also have tcl (8.6 installed) 
### install dependencies 
`sudo apt-get install libboost-all-dev  libssl-dev libxml2-dev libpam0g-dev`

### Download the torque repository (assume you have already downloaded git)

` git clone https://github.com/adaptivecomputing/torque.git 

### unzip the file

` unzip master.zip `

### enter the folder
` cd torque-master `
### launch autogen
` ./autogen.sh`

### configure (I'm including gpu capability, just remove the option for the gpu if you don't want)
`./configure --prefix=/opt/torque --with-debug --prefix=/opt/torque --enable-nvidia-gpus --with-nvml-lib=/opt/cuda-8.0/lib64/stubs --with-nvml-include=/opt/cuda-8.0/include --with-pam` 

Result: 
> Building components: server=yes mom=yes clients=yes
>                     gui=no drmaa=no pam=yes
>PBS Machine type    : linux
>Remote copy         : /usr/bin/scp -rpB
>PBS home            : /var/spool/torque
>Default server      : hpcl-G11CD
This installs all components needed

### launch make
`make`

### install
`sudo make install`

!NOT SURE WITH THIS. \
service pbs_mom stop  \
service pbs_server stop \
service pbs_sched  stop \

pbs_server -t create \
killall pbs_server  \

echo head0.local > /etc/torque/server_name \

echo hpcl-G11CD > /var/spool/torque/server_priv/acl_svr/acl_hosts \

echo root@hpcl-G11CD > /var/spool/torque/server_priv/acl_svr/operators  \ 

echo root@hpcl-G11CD > /var/spool/torque/server_priv/acl_svr/managers \

echo "head0.local np=8 gpus=1" > /var/spool/torque/server_priv/nodes \

echo "$pbsserver head0.local" >> /var/spool/torque/mom_priv/config \

echo "logeven=255" >> /var/spool/torque/mom_priv/config   \


END \


### add lib
echo /usr/local/lib >> /etc/ld.so.conf
ldconfig


### setup

./torque.setup root hpcl-G11D



