
PRIMARY REFERENCE: https://ubuntuforums.org/showthread.php?t=1372508
https://jabriffa.wordpress.com/2015/02/11/installing-torquepbs-job-scheduler-on-ubuntu-14-04-lts/
https://github.com/adaptivecomputing/torque
ibxml2-devel, openssl-devel, and boost-devel packages must be installed.
I also have tcl (8.6 installed) 
### install dependencies 
`sudo apt-get install libboost-all-dev  libssl-dev libxml2-dev libpam0g-dev libtool-bin`

### Download the torque repository (assume you have already downloaded git)

` git clone https://github.com/adaptivecomputing/torque.git 

### unzip the file

` unzip master.zip `

### enter the folder
` cd torque-master `
### launch autogen
` ./autogen.sh`

### configure (I'm including gpu capability, just remove the option for the gpu if you don't want)
`./configure --prefix=/usr/local/torque --with-debug --enable-nvidia-gpus --with-nvml-lib=/opt/cuda-8.0/lib64/stubs --with-nvml-include=/opt/cuda-8.0/include --with-pam ` 

### launch make
`make`

### install
`sudo make install`

### COPY SERVICES
```
cp debian.pbs_mom /etc/init.d/pbs_mom && update-rc.d pbs_mom defaults
cp debian.pbs_sched /etc/init.d/pbs_sched && update-rc.d pbs_sched defaults
cp debian.pbs_server /etc/init.d/pbs_server && update-rc.d pbs_server defaults

./torque.setup root hpcl-G11CD
```

### STOP AND REFRESH PBS_SERVER CREATION
```
/etc/init.d/pbs_mom stop
/etc/init.d/pbs_sched stop
/etc/init.d/pbs_server stop
killall pbs_server            #kills the pbs_server created by torque.setup
pbs_server -t create
killall pbs_server
```

### EDIT THE SERVER NAMES
```

echo hpcl > /etc/torque/server_name
echo hpcl > /var/spool/torque/server_priv/acl_svr/acl_hosts 
echo root@hpcl > /var/spool/torque/server_priv/acl_svr/operators
echo root@hpcl > /var/spool/torque/server_priv/acl_svr/managers
echo hpcl > /var/spool/torque/server_name   
echo "hpcl np=8 gpus=1" > /var/spool/torque/server_priv/nodes
echo hpcl > /var/spool/torque/mom_priv/config


```

### ADD PATH OF BIN FILES
```
PATH=$PATH:/usr/local/torque/bin
PATH=$PATH:/usr/local/torque/sbin
```

### START THE SERVICES
/etc/init.d/pbs_server start
/etc/init.d/pbs_sched start
/etc/init.d/pbs_mom start
pbs


/etc/init.d/pbs_server restart
/etc/init.d/pbs_sched restart
/etc/init.d/pbs_mom restart



service pbs_mom start
service pbs_server start
service pbs_sched  start
service trqauthd start

### TEST:
COMMAND:    `pbsnodes -a`

OUTPUT:

```
hpcl@hpcl-G11CD:~$ pbsnodes -a
head0.local
     state = down
     power_state = Running
     np = 8
     ntype = cluster
     mom_service_port = 15002
     mom_manager_port = 15003
     gpus = 1
```

## END




### **********************************************************************************************************************************


END \


### add lib
echo /usr/local/lib >> /etc/ld.so.conf
ldconfig


### setup

./torque.setup root hpcl-G11D



./configure --with-debug --prefix=/usr/local/torque --enable-nvidia-gpus --with-nvml-lib=/opt/cuda-8.0/lib64/stubs --with-nvml-include=/opt/cuda-8.0/include 2>&1 | tee configure_torque.log

make 2>&1 | tee make.log
****************************************************

checkinstall /home/hpcl/torque/torque-master/torque-package-server-linux-x86_64.sh --install 2>&1 | tee checkinstall_torque_server.log
checkinstall /home/hpcl/torque/torque-master/torque-package-clients-linux-x86_64.sh --install 2>&1 | tee checkinstall_torque_clients.log
checkinstall /home/hpcl/torque/torque-master/torque-package-mom-linux-x86_64.sh --install 2>&1 | tee checkinstall_torque_mom.log

cp debian.pbs_mom /etc/init.d/pbs_mom && update-rc.d pbs_mom defaults
cp debian.pbs_sched /etc/init.d/pbs_sched && update-rc.d pbs_sched defaults
cp debian.pbs_server /etc/init.d/pbs_server && update-rc.d pbs_server defaults
cp debian.trqauthd /etc/init.d/trqauthd && update-rc.d trqauthd defaults

mkdir /etc/torque
echo head0.local >> /etc/torque/server_name
echo hpcl-G11CD > /var/spool/torque/server_priv/acl_svr/acl_hosts 
echo root@hpcl-G11CD > /var/spool/torque/server_priv/acl_svr/operators
echo root@hpcl-G11CD > /var/spool/torque/server_priv/acl_svr/managers

echo head0.local > /var/spool/torque/server_name   
echo "head0.local np=8 gpus=1" > /var/spool/torque/server_priv/nodes

echo "$pbsserver head0.local" > /var/spool/torque/mom_priv/config
echo "logeven=255" >> /var/spool/torque/mom_priv/config


