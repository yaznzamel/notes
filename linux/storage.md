### This section for the most common storage commands that every devops engineer must understand and memorize since it provides an entrance to devices adminstration.




### Physical disks

```
# list physical device
lsblk

# parition a physical device to a smaller one
cfdisk /dev/sdba

# list partion UUID and fs type on it
lsblk -f
```


### Manage swaps

```
# list swap devices
swapon --show

# make partition a swap
mkswap /dev/sda1

# turn on the swap
swapon /dev/vdb2

# turn off the swap
swapoff /dev/vdb2
```


### Manage file systems

```
# list file systems
lsblk -f / df -h

# create a file system
mkfs.ext4 /dev/sda

# create a filesystem with inode size
mkfs.ext4 -N 2048 /dev/vdb

# create file system with a label
mkfs.ext4 -L "llm-fs" /dev/sdb

# delete a filesystem
wipefs --all /dev/vdc

# mount file system
mount /dev/sdb2 /mnt/llm-models

# unmount file system
umount /mnt/llm-models

# Mount file system on boot (Avoid ephemeral mounts)
sudo vim /etc/fstab
/dev/vdb1 /llm-models ext4 defaults 0 2
systemctl daemon-reload

```

##### Manage file system mounts

```
man mount

# List mount points
findmnt

# List custom file system mounts
findmnt -t xfs,ext4

# restrict mount actions
mount -o rw /dev/sda1 /mnt

# remount it with the permissions if already mounted
mount -o remount,rw /dev/sda1 /mnt

```


### NFS (access it remotely as it is on your machine)

```
# List exported directories for sharing
cat /etc/exports

# Export one of your directories
/home 10.0.0.0/24(ro,sync,no_subtree_check)

# re-export the nfs
exportfs -r
# Mount nfs locally/remotely
mount 127.0.0.1:/home /mnt

# Configure automatice mount point 
127.0.0.1:/home /mnt nfs defaults 0 0


    

```


## LVM

```
# install lvm
    apt install lvm2 -y
# List lvm data
sudo lvmdiskscan

# List physical volumes
pvs

# Create physical volumes our of disk/partition
sudo pvcreate /dev/sda

# List available volume groups
sudo vgs

# Create volume group
sudo vgcreate vg_llms /dev/sda /dev/sdb

# Extend a volume
sudo vgextend vg_llms /dev/sdc

# remove a pv from a vg
   vgreduce voluem1 /dev/vdc


# create a logical volume out of volume group
    lvcreate --size 0.5G --name smalladata <VG_NAME>

# remove a logical volume 
    lvremove <VG_NAME> <LV_NAME>

# reduce a size of a logical volume 
    lvreSIZE --size 752M volume1/smalldata

# create a filesystem on a logical volume
    mkfs.xfs /dev/volume1/smalldata



```