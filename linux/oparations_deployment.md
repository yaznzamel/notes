
# Operations deployment




## Schedule tasks
### Managing tasks using cronjobs

Topics to master : 
* List crontab for a user
* create a cronjob to run script periodically
* create a cronjob to run for another user
* manage anacron jobs
* manage "at" jobs

```
# execute all anacron jobs which failed
sudo anacron -n -f

# check current scheduled jobs for at utility
atq

# schedule a command to run only once using at
1. at 15:30 August 20 2054
2. /usr/bin/touch atscheduler
3. CTRL + D

# remove at job by id
atrm <id>

# create anacron job to run every 10 days , 5 min timeout and name db_cleanup
sudo vim /etc/anacrontab
10 5 db_cleanup /usr/bin/touch /root/anacron_created_this


# create a crontab to run multiple hours at specific dat
0 6,23 * * 0 sudo systemctl restart nginx --> this will run on 6AM , 11PM


```



## Create and manage virtual machine 


#### Managing vms using virsh

Install the package manager

```
apt install virt-manager
```


Sample xml file
```
<domain type="qemu">
  <name>TestMachine</name>
  <memory unit="GiB">1</memory>
  <vcpu>1</vcpu>
  <os>
    <type arch="x86_64">hvm</type>
  </os>
</domain>

```

```
# List virtual machine 
virsh list --all

# Create a virtual machine 
virsh define testmachine.xml

# Create a virsh machine
virsh start testmachine.xml

# Shutdown a virtual machine 
virsh shutdown Name


# Describe a virtual machine 
virsh dominfo NAME

# Remove a virtual machine 
virsh undefine NAME

# Remove a virtual machine with all its storage devices
virsh undefine NAME --remove-all-storage


# Autostart a vm when host restarts
virsh autostart NAME


```


#### Setting up vms with bootable os

The previous section we create only a bare metal vm , now we need to create a virtual machine with an operating system. 

Install a minimal cloud image like the ones cloud providers use

```
# install it
wget https://cloud-images.ubuntu.com/minimal/releases/jammy/release-20250424/ubuntu-22.04-minimal-cloudimg-amd64.img

# check the image was not corrupted while downloading 

# get the checksum file
wget https://cloud-images.ubuntu.com/minimal/releases/jammy/release-20250424/SHA256SUMS

# Run the sha256 command 
sha256sum -c SHA256SUMS 2>&1 | grep OK

```


```
# Checking the size of the disk image to be created once we create the vm 
qemu-img info ubuntu-22.04-minimal-cloudimg-amd64.img

# Resize the disk image
qemu-img resize ubuntu-22.04-minimal-cloudimg-amd64.img 10G


# Create a bootable vm 
virt-install --osinfo ubuntu24.04 --name ubuntu1 --memory 1024 --vcpu 1 --import --disk /path/to/ubuntu-22.04-minimal-cloudimg-amd64.img --graphics none --cloud-init root-password-generate=on

# Check the running machine
virsh list --all


```

```
virt-install \
  --osinfo ubuntu24.04 \
  --name ubuntu1 \
  --memory 1024 \
  --vcpus 1 \
  --import \
  --disk ubuntu-22.04-minimal-cloudimg-amd64.img \
  --graphics none \
  --cloud-init user-data=user-data.yaml

```


## Manage services and processes

### Manage services

Topics to master :

* list running processes
* identify user vs kernel processes
* identify a process by it's id , user
* get process id from it's name
* reterieve process hierarchy
* update process niceness
* check process logs
* monitor resources usage of a process


```
# List running processes
ps aux

# list live running processes
top

# identify user vs kernel processes
kernel process has a brackets : [kworker/13:0]
user process does not have : /usr/bin/zsh -i

# identify process details by an id or user

by id : ps <PID>
by user : ps -U jeremy

# find process id from it's name
pgrep ssh

# list processes hierarchy , tree like structure
ps fax

# list open files by processes
lsof -i <PID>

# update the niceness value of a process
nice -n 10 <PID>

renice -n -10 <PID>

# Check a process logs
journactl -u ssh.service

# check all errors across services with priority flag
journalctl -p err


# check the priority logs with the regex
sudo journalctl -p info -g '^c' > .priority/boot.log

# check the cpu , memory usage of a process
ps u 1 > /home/bob/resources.txt

```

## Manage and compile packages


### Manage packages

  APT vs DPKG
* dpkg is the low-level Debian package manager. It installs .deb files directly but doesn’t resolve dependencies.

* apt is a higher-level tool that uses dpkg under the hood and handles dependency resolution, downloads, and repo management.
```
# list current installed packages
apt list

# search for the package of certain installed binary
  dpkg --search /bin/ls

# List the binaries which belong to certain package
  dpkg --listfiles coreutils | greop "^/bin"

# update local repo with remote one 
apt update

# update currnet packages 
apt upgrade

# check package contents
apt show <package-name>

# remove package
apt remove package

# remove a package with it's dependencies
apt autoremove nginx


# check the repository we use for installing packages
ls /etc/apt/sources.list.d/

```

### Compile softwares

Softwares have different dependencies and ways to configure them , however we compile and configure using MAKE utility

```
# Always make sure to inspect the README file before running make commands


# Generate the configuration script
  ./autogen
# Compile with default configuration
  ./configure && make

# Move the compiled code to be global path
  make install

```

