# CENTOS MAINTENANCE: REPOSITORIES



## REPOSITORIES

[Official Yum Docs](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/sec-managing_yum_repositories)


### OVERALL

**YUM** is official Red Hat/CentOS package manager

**YUM Repositories** are warehouses of Linux software (RPM package files). 

**RPM package** file is a *Red Hat Package Manager* file and enables quick and easy software installation on Red Hat/CentOS Linux. 

YUM Repositories hold a number of RPM package files and enable download and installation of new software on our server. 

YUM Repositories can hold RPM package files locally (local disk) or remotely (FTP, HTTP or HTTPS). 

YUM Configuration files hold the information required to successfully find and install software (RPM packages files) on our VPS.

---------------------------

Most common and largest CentOS YUM Repositories:

  [CentOS Official Repository Mirrors](http://www.centos.org/modules/tinycontent/index.php?id=30)
  
  [EPEL Repository Mirrors](http://mirrors.fedoraproject.org/publiclist/EPEL/)
  
  [RPMforge Repository](http://wiki.centos.org/AdditionalResources/Repositories/RPMForge)
  
  [ElRepo Repository](http://elrepo.org/tiki/tiki-index.php)
  
  




### LIST AVAILABLE REPO

List all available repository IDs (already added, enabled/disabled)
```
yum repolist all
```


### YUM REPO CONFIG FILE

We can install new software on Red Hat/CentOS Linux with `yum install packagename` command from console.

Running this command first checks for existing **YUM Repository configuration files** in `/etc/yum.repos.d/` dir. 

It reads each *YUM Repository configuration file* to get the info required to download and install new software, resolves software dependencies and installs the required RPM package files.


YUM Repository configuration files must:

  - be located in /etc/yum.repos.d/ directory
  
  - have .repo extension, to be recognized by YUM

Available YUM Repository configuration file options are:

  **Repository ID** - One word unique repository ID (example: [examplerepo])
  
  **Name** - Human readable name of the repository (example: name=Example Repository)
  
  **Baseurl** - URL to the repodata directory. You can use file://path if repository is located locally or ftp://link, http://link, https://link if repository is located remotely - HTTP Authentication available http://user:password@www.repo1.com/repo1 (example: baseurl=http://mirror.cisp.com/CentOS/6/os/i386/)
  
  **Enabled** - Enable repository when performing updates and installs (example: enabled=1)
  
  **Gpgcheck** - Enable/disable GPG signature checking (example: gpgcheck=1)
  
  **Gpgkey** - URL to the GPG key (example: gpgkey=http://mirror.cisp.com/CentOS/6/os/i386/RPM-GPG-KEY-CentOS-6)
  
  **Exclude** - List of the packages to exclude (example: exclude=httpd,mod_ssl)
  
  **Includepkgs** - List of the packages to include (example: include=kernel)

Required YUM Repository configuration file options are:

  Repository ID

  Name
  
  Baseurl
  
  Enabled
  



### ADDING A YUM REPO

Yum repositories commonly provide their own `.repo` file. 

To add such a repo to your system and enable it, run the following
```
sudo yum-config-manager --add-repo http://www.example.com/example.repo
```



### ENABLE/DISABLE REPO








## PACKAGES



### INSTALL

**Install necessary package**

Running this command first checks for existing *YUM Repository configuration files* in `/etc/yum.repos.d/` dir. 

It reads each *YUM Repository configuration file* to get the info required to download and install new software, resolves software dependencies and installs the required RPM package files.
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
yum search all ansible
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



























