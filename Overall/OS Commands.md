# OS COMMANDS


## USEFUL LINKS

[20 Commands Line Tools To Monitor Linux Performance](https://www.tecmint.com/command-line-tools-to-monitor-linux-performance/)

[SystemD Boot Process](https://opensource.com/article/17/2/linux-boot-and-startup)


## INPUT/OUTPUT

Replace existing values in the file with new one
```
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```


## ENCRYPTING


Encode your word with `base64`
```
echo -n 'adminda' | base64

  YWRtaW5kYQ==
```

Decode your word, encoded with `base64`
```
echo 'YWRtaW5kYQ==' | base64 --decode

  adminda
```

## FILE SYSTEM

### List

Which FS is in use:
```
lsblk -f

  NAME            FSTYPE      LABEL UUID                                   MOUNTPOINT
  nvme0n1
  └─nvme0n1p1     xfs               ef6ba050-6cdc-416a-9380-c14304d0d206   /
  nvme1n1         LVM2_member       OkcS2c-AdMq-OK5W-5VLB-9CX8-UifB-7ty1Zh
  └─vg_log-lv_log xfs               37a148e9-71c1-49d2-b569-2673c6529fe7   /opt/csmarketslive_log
  nvme2n1         LVM2_member       C3Ju0e-qq9c-gxFE-gIbQ-SvG1-tpVE-yHNuUK
  └─vg_app-lv_app xfs               f09e1250-fea4-4ecd-a3b2-5b2604224ace   /opt/csmarketslive

   # OR
   
df -hT

  Filesystem                Type      Size  Used Avail Use% Mounted on
  /dev/nvme0n1p1            xfs        30G   21G  9.6G  69% /
  devtmpfs                  devtmpfs   16G     0   16G   0% /dev
  tmpfs                     tmpfs      16G  8.0K   16G   1% /dev/shm
  tmpfs                     tmpfs      16G  836K   16G   1% /run
  tmpfs                     tmpfs      16G     0   16G   0% /sys/fs/cgroup
  /dev/mapper/vg_app-lv_app xfs        50G  4.8G   46G  10% /opt/csmarketslive
  /dev/mapper/vg_log-lv_log xfs      1000G  932G   69G  94% /opt/csmarketslive_log
  tmpfs                     tmpfs     3.1G     0  3.1G   0% /run/user/555
  tmpfs                     tmpfs     3.1G     0  3.1G   0% /run/user/1022
  tmpfs                     tmpfs     3.1G     0  3.1G   0% /run/user/0
```

Does specified block device have a FS installed on top of it:
```
# Does not have
file -s /dev/xvdf

   /dev/xvdf: data


# Does have
file -s /dev/xvda1

   /dev/xvda1: SGI XFS filesystem data (blksz 4096, inosz 512, v2 dirs)
```


## BLOCK DEVICES

### Get info

#### lsblk
`lsblk` lists information about all available or the specified
       block devices. 
The lsblk command reads the `sysfs` filesystem and
       `udev` db to gather information. 
If the `udev` db is not available or
       `lsblk` is compiled without udev support, then it tries to read
       `LABELs`, `UUIDs` and filesystem types from the block device. In this
       case `root` permissions are necessary.
       
       
To display block devices. 
```
lsblk

  NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
  nvme0n1     259:2    0   30G  0 disk 
  └─nvme0n1p1 259:3    0   30G  0 part /
  nvme1n1     259:0    0  150G  0 disk /opt/co_log
  nvme2n1     259:1    0   30G  0 disk /opt/co
```
To list partitions with filesystems types
```
lsblk -f

  NAME        FSTYPE LABEL UUID                                 MOUNTPOINT
  nvme0n1                                                       
  └─nvme0n1p1 xfs          f41e390f-835b-4223-a9bb-9b45984ddf8d /
  nvme1n1     xfs          3750b575-04b1-4429-9514-1e624e2fade1 /opt/co_log
  nvme2n1     xfs          4d472bc3-9510-4cd0-a84b-1deab218d8ee /opt/co
```

To display empty block devices as well.  
```
lsblk -a 
```

To print size information in bytes. 
```
lsblk -b
```

To print selected columns of block-devices. 
```
lsblk -o SIZE, NAME, MOUNTPOINT 
```




#### blkid
The `blkid` command allows you to display information about available block devices.
```
blkid

  /dev/nvme0n1p1: UUID="f41e390f-835b-4223-a9bb-9b45984ddf8d" TYPE="xfs"
```

Get info regarding specific device name
```
blkid device_name
```

Get more detailed information
```
blkid -po udev device_name
```



## PARTITIONS

### Get info

To list existing partitions on Linux
```
fdisk -l
```


## LVM (Logical Volume Management)

### General

[Introductino to LVM concepts](https://www.digitalocean.com/community/tutorials/an-introduction-to-lvm-concepts-terminology-and-operations)
[Video: Introduction to LVM](https://youtu.be/dMHFArkANP8?t=161)

### List

Show all PV (`Physical Volumes`)

> **NOTE**: `Physical Volume` is a `physical device`, that already registered by LVM via `pvcreate`
```
pvs

  PV           VG     Fmt  Attr PSize     PFree
  /dev/nvme1n1 vg_log lvm2 a--  <1000.00g    0
  /dev/nvme2n1 vg_app lvm2 a--    <50.00g    0
```

Show all VG (`Volume Group`)
```
vgs

  VG     #PV #LV #SN Attr   VSize     VFree
  vg_app   1   1   0 wz--n-   <50.00g    0
  vg_log   1   1   0 wz--n- <1000.00g    0
  
   #OR
   
vgs -o +lv_size,lv_name

  VG     #PV #LV #SN Attr   VSize     VFree LSize     LV
  vg_app   1   1   0 wz--n-   <50.00g    0    <50.00g lv_app
  vg_log   1   1   0 wz--n- <1000.00g    0  <1000.00g lv_log
  
   #OR
   
vgdisplay <VG_NAME>
```

Show all LV (`Logical Volume`)
```
lvs

  LV     VG     Attr       LSize     Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_app vg_app -wi-ao----   <50.00g
  lv_log vg_log -wi-ao---- <1000.00g
```

Scan the system for `block devices` that LVM can see and manage.
```
lvmdiskscan

  /dev/nvme2n1       [      50.00 GiB] LVM physical volume
  /dev/vg_log/lv_log [   <1000.00 GiB]
  /dev/nvme1n1       [       1.17 TiB] LVM physical volume
  /dev/vg_app/lv_app [     <50.00 GiB]
  /dev/nvme0n1       [      30.00 GiB]
  /dev/nvme0n1p1     [     <30.00 GiB]
  2 disks
  2 partitions
  0 LVM physical volume whole disks
  2 LVM physical volumes
```

### Resize

[Docs: How to resize on AWS EBS volume](https://aws.amazon.com/premiumsupport/knowledge-center/create-lv-on-ebs-volume/)

1. [Modify](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/requesting-ebs-volume-modifications.html#modify-ebs-volume) the size of the existing EBS volume.

2. Resize the PV (`Physical Volume`)
```
# Which PV to resize
pvs
  PV           VG     Fmt  Attr PSize     PFree
  /dev/nvme1n1 vg_log lvm2 a--  <1000.00g    0
  /dev/nvme2n1 vg_app lvm2 a--    <50.00g    0

# Resize it
# (the pvresize without options resizes PV to the size the OS reports for the underlying partition)
pvresize /dev/nvme1n1

# Check the result
pvs

  PV           VG     Fmt  Attr PSize   PFree
  /dev/nvme1n1 vg_log lvm2 a--    1.17t 200.00g
  /dev/nvme2n1 vg_app lvm2 a--  <50.00g      0
```

3. Extend the LV (`Logical Volume`)
```
# Make sure free space is available for LV
# within corresponding VG
vgs

  VG     #PV #LV #SN Attr   VSize   VFree
  vg_app   1   1   0 wz--n- <50.00g      0
  vg_log   1   1   0 wz--n-   1.17t 200.00g

   # OR
   
vgdisplay vg_log


# Which LV to extend
lvs

  LV     VG     Attr       LSize     Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_app vg_app -wi-ao----   <50.00g
  lv_log vg_log -wi-ao---- <1000.00g

# Extend it
# 
# To 100% of free space, available on PV for specified LV
#    lvextend -l +100%FREE /dev/<VG_NAME>/<LV_NAME>
lvextend -l +100%FREE /dev/vg_log/lv_log

  Size of logical volume vg_log/lv_log changed from <1000.00 GiB (255999 extents) to 1.17 TiB (307199 extents).
  Logical volume vg_log/lv_log successfully resized

# Make sure LV was extended
lvs

  LV     VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_app vg_app -wi-ao---- <50.00g
  lv_log vg_log -wi-ao----   1.17t
  
lsblk

  NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
  nvme0n1         259:2    0   30G  0 disk
  └─nvme0n1p1     259:3    0   30G  0 part /
  nvme1n1         259:1    0  1.2T  0 disk
  └─vg_log-lv_log 253:0    0  1.2T  0 lvm  /opt/csmarketslive_log
  ...
```

4. Extend the file system

> **Note**: Depending on your use case, follow the steps for `XFS` or `Ext2, Ext3, and Ext4` file systems.

Which FS is in use:
```
lsblk -f

  NAME            FSTYPE      LABEL UUID                                   MOUNTPOINT
  nvme0n1
  └─nvme0n1p1     xfs               ef6ba050-6cdc-416a-9380-c14304d0d206   /
  nvme1n1         LVM2_member       OkcS2c-AdMq-OK5W-5VLB-9CX8-UifB-7ty1Zh
  └─vg_log-lv_log xfs               37a148e9-71c1-49d2-b569-2673c6529fe7   /opt/csmarketslive_log
  nvme2n1         LVM2_member       C3Ju0e-qq9c-gxFE-gIbQ-SvG1-tpVE-yHNuUK
  └─vg_app-lv_app xfs               f09e1250-fea4-4ecd-a3b2-5b2604224ace   /opt/csmarketslive

   # OR
   
df -hT

  Filesystem                Type      Size  Used Avail Use% Mounted on
  /dev/nvme0n1p1            xfs        30G   21G  9.6G  69% /
  devtmpfs                  devtmpfs   16G     0   16G   0% /dev
  tmpfs                     tmpfs      16G  8.0K   16G   1% /dev/shm
  tmpfs                     tmpfs      16G  836K   16G   1% /run
  tmpfs                     tmpfs      16G     0   16G   0% /sys/fs/cgroup
  /dev/mapper/vg_app-lv_app xfs        50G  4.8G   46G  10% /opt/csmarketslive
  /dev/mapper/vg_log-lv_log xfs      1000G  932G   69G  94% /opt/csmarketslive_log
  tmpfs                     tmpfs     3.1G     0  3.1G   0% /run/user/555
  tmpfs                     tmpfs     3.1G     0  3.1G   0% /run/user/1022
  tmpfs                     tmpfs     3.1G     0  3.1G   0% /run/user/0
```

Check current FS size
```
df -h

  Filesystem                 Size  Used Avail Use% Mounted on
  ...
  /dev/mapper/vg_log-lv_log 1000G  932G   69G  94% /opt/csmarketslive_log
  ...
```

For `Ext2, Ext3, and Ext4` file systems:
```
resize2fs /dev/mapper/vg_log-lv_log
```

For `XFS` file systems:
```
# Install the pckg, if no `xfs_growfs` tool installed
yum install xfsprogs

# With no additional keys, it will grow the FS to the maximum size supported by the device.
#   xfs_growfs </mount/point>
xfs_growfs /dev/mapper/vg_log-lv_log
```

Check the FS size was increased
```
df -h

  Filesystem                 Size  Used Avail Use% Mounted on
  ...
  /dev/mapper/vg_log-lv_log  1.2T  932G  269G  78% /opt/csmarketslive_log
  ...
```



## DRIVES

### List

In order to check that your drive partition was correctly mounted, you can use the “lsblk” and inspect the mountpoint column.

```
lsblk -f
```


### Mount (temp)

To mount drives on Linux
```
mount <device> <dir>

mount /dev/sda1 ~/mountpoint
```


### Mount (permanent)

Mount all from `/etc/fstab` file:
```
mount -a
```

Mount only FS from `/etc/fstab` file with type `nfs`:
```
mount -a  -t nfs4
```

### Unmount

To unmount the `/dev/sdc1` device, for example
```
umount /dev/sdc1
```

Check unmounted device
```
lsblk /dev/sdc1

  OR 
  
findmnt /dev/sdc1
```






