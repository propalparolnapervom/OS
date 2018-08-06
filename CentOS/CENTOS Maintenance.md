# CENTOS MAINTENANCE


## PACKAGES



### INSTALL

**Install necessary package**
```
  #Note: running update is not strictly necessary, but keeping installed packages up to date 
  #is a good sysadmin practice 
  #for security and dependency reasons.
  
sudo yum -y update && sudo yum -y install python2
```


### SEARCH

**Which package has already installed the file**

For examle, which pachage has installed the `cd` file
```
rpm -qf /usr/bin/cd

      bash-4.2.46-20.el7_2.x86_64
```

**Which package we need to install the file**

For example, which package installs the `ansible`
```
yum whatprovides ansible

      Loaded plugins: fastestmirror
      Loading mirror speeds from cached hostfile
       * base: centos.itt-consulting.com
       * extras: centos.itt-consulting.com
       * updates: centos.itt-consulting.com
      ansible-2.4.2.0-2.el7.noarch : SSH-based configuration management, deployment, and task execution system
      Repo        : extras
```

**Search specific package**

With specific word in the package name
```
yum search ansible
```

With specific word not only in the package name, but in its description too
```
yum search jall ansible
```


### LIST

**View installed packages**

Via RPM (RPM Package Manager)
```
  #-q meaning query 
  #-a enables listing of all installed packages
      
      
  #All
 
rpm -qa


  #Specific
  
rpm -qa python*
```

Via YUM (Yellowdog Updater, Modified)
```
  #All
  
yum list installed


  #Specific
  
yum list installed python*
```

Via YUM-Utils
```
repoquery -a --installed
```

**See package description**

```
yum info docker
```

### DELETE

Uninstall the package, leave its configuration files 
```
yum remove package_name
```

Uninstall the package together with all its metadata
```
yum erase package_name
```



























