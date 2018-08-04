# REDHAT INSTALL ON VIRTUALBOX

[Official Installation Tutorial](https://developers.redhat.com/products/rhel/hello-world/#fndtn-virtualbox)



## 1. PREPARATION

1. Download [RHEL Server DVD](https://developers.redhat.com/products/rhel/download/) `.iso` file.

2. [Download](https://www.virtualbox.org/wiki/Downloads) and install VirtualBox.






## 2. CONFIGURE VM TO RUN RHEL

**1. Launch VirtualBox**

**2. Define VM storage**

Define where VirtualBox stores the files that are used as virtual hard disks for the VMs you create.

You will need at least 20 GB of available space. 

*File* drop-down menu -> *Preferences...* -> *General* -> 

```
Default Machine Folder:   D:\VM machines\my_rhel_disk_space
```

-> *OK* button


**3. Create a new VM**

*New* button -> 

```
Name:     my_rhel_7.4
Type:     Linux`
Version:  Red Hat (64-bit)
```

-> *Next* button -> 

```
Memory Size (RAM): 2048M    <== Recommended minimum is 2048 MB, 4096 MB is suggested
                                Can be changed later.
```

-> *Next* button -> *Create a virtual hard disk now* radiobutton -> *Create* button -> *VDI (VirtualBox Disk Image)* radiobutton -> *Next* button -> *Dynamically allocated* radiobutton -> *Next* button -> 

```
File location:  my_rhel_7.4_file
File size:      20,00 GB            <== at least 20GB is recommended (it will be allocated as needed)
```

-> *Create* button


Highlight just created VM -> *Settings* button -> *General* -> *Advanced* tab -> 

```
Shared Clipboard: Bidirectional
Drag'n'Drop:      Bidirectional
```

-> *System* -> *Processor* tab -> Set necessary configuration for CPU -> *Network* -> *Adapter 1* tab -> 

```
#"NAT"
This is the easiest to manage and will be fine for many uses. Using NAT, the VM will be able to access resources on your network or the Internet. However services, such as a web server, running inside the VM won’t be directly accessible from outside of the VM.

#"Bridged Adapter"
In this configuration, the VM gets its own IP address, usually using your network’s DHCP server. The VM appears on the network the same way a physical computer would with its own hardware MAC address. The host’s network adapter is shared by a device driver that is installed by VirtualBox. The VM’s virtual network adapter can only be bridged to one physical network adapter at a time. If your system has more than one network adapter you need to choose which one to attach to. If your system switches between wired and wireless connections, you will need to switch bridged adapters for the VM.

Attached to: Bridged Adapter    <== "Bridged Adapter" to attach the VM directly to the physical network
```

-> *Storage* -> Highlight *Empty* under *Controller: IDE* -> On the right click the CD icon -> *Choose Virtual Optical Disk File* -> Choose previously downloaded RHEL `.iso` DVD file -> *OK* button





## 3. BOOT THE VM USING THE ISO FILE AND INSTALL RHEL

Boot the VM using the `.iso` file you downloaded during preparation step as a virtual DVD.

This is a brief overview. Full steps are in [RHEL Installation Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/index.html)


Highlight just configured VM -> *Start* button -> 

```
Install Red Hat Enterprise Linux 7.4
```

-> *Enter* button -> Select your preferred language and keyboard layout to use during installation -> 

*Software selection* -> Choose necessary one (with GUI, for example) -> *Done* button -> 

*Installation destination* -> *sda* is checked (this is a disk we created previously) -> *Done* button -> 

*Network & host name* -> on/off necessary one -> Define desired *Host Name* -> *Done* button -> 

*KDUMP* -> *Enable kdump* checkbox is not checked -> *Done* button -> 

*Begin installation* button -> 

Set root password -> Create at least 1 user -> 

*Reboot* button


## 4. POST-INSTALLATION STEPS




































