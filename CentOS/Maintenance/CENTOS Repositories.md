# CENTOS MAINTENANCE: REPOSITORIES


[Official Yum Docs](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/sec-managing_yum_repositories)


## OVERALL

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
  
  



## YUM REPO CONFIG FILE

We can install new software on Red Hat/CentOS Linux with `yum install packagename` command from console.

Running this command first checks for existing **YUM Repository configuration files** in `/etc/yum.repos.d/` dir. 

It reads each *YUM Repository configuration file* to get the info required to download and install new software, resolves software dependencies and installs the required RPM package files.


YUM Repository configuration files must:

  - be located in `/etc/yum.repos.d/` directory
  
  - have `.repo` extension, to be recognized by YUM


Available YUM Repository configuration file options are:

  - **Repository ID** - One word unique repository ID (example: [examplerepo])
  
  - **Name** - Human readable name of the repository (example: name=Example Repository)
  
  - **Baseurl** - URL to the repodata directory. You can use file://path if repository is located locally or ftp://link, http://link, https://link if repository is located remotely - HTTP Authentication available http://user:password@www.repo1.com/repo1 (example: baseurl=http://mirror.cisp.com/CentOS/6/os/i386/)
  
  - **Enabled** - Enable repository when performing updates and installs (example: enabled=1)
  
  - **Gpgcheck** - Enable/disable GPG signature checking (example: gpgcheck=1)
  
  - **Gpgkey** - URL to the GPG key (example: gpgkey=http://mirror.cisp.com/CentOS/6/os/i386/RPM-GPG-KEY-CentOS-6)
  
  - **Exclude** - List of the packages to exclude (example: exclude=httpd,mod_ssl)
  
  - **Includepkgs** - List of the packages to include (example: include=kernel)


Required YUM Repository configuration file options are:

  - **Repository ID**

  - **Name**
  
  - **Baseurl**
  
  - **Enabled**
  

### EXAMPLE OF YUM CONF FILE CREATION

**Step 1: Create YUM Repository configuration file**

Create a new YUM Repository configuration file with `.repo` extension in `/etc/yum.repos.d/`. 
```
export NEW_REPO_FILE=/etc/yum.repos.d/xburser.repo
sudo touch $NEW_REPO_FILE
sudo chmod 644 $NEW_REPO_FILE
sudo ls -la $NEW_REPO_FILE
sudo vi $NEW_REPO_FILE
```


**Step 2: Insert YUM Repository options**

Insert the desired YUM Repository options to the newly created YUM Repository configuration file and save changes.
```
[examplerepo]
name=Example Repository
baseurl=http://mirror.cisp.com/CentOS/6/os/i386/
enabled=1
gpgcheck=1
gpgkey=http://mirror.cisp.com/CentOS/6/os/i386/RPM-GPG-KEY-CentOS-6
```


## CUSTOM YUM REPO

**Step 1: Install "createrepo"**

Install additional software `createrepo` (needed to create Custom YUM Repository)
```
sudo yum install createrepo
```

**Step 2: Create Repository directory**

Create a new dir that will be the location of our Custom YUM Repository and will hold the desired RPM package files. 
```
mkdir /repository1
```

**Step 3: Put RPM files to Repository directory**

Download necessary RPM package files directly to our server with `wget` command from console
```
wget http://mirror.lihnidos.org/CentOS/6/os/i386/Packages/NetworkManager-0.8.1-43.el6.i686.rpm
```

OR

Copy or Move these files to the newly created dire
```
cp /path/to/rpm /repository1
mv /path/to/rpm /repository1
```

**Step 4: Run "createrepo"**

`createrepo` command reads through Custom YUM Repository dir from "Step 2" and creates a new dir called *repodata* in it. 

Repodata directory holds the metadata information for the newly created repo. 

Every time we add additional RPM package files to our Custom YUM Repository, we need to re-create Repository metadata with "createrepo" command. We can create new repo metadata by running the following command
```
createrepo /repository1
```

**Step 5: Create YUM Repository Configuration file**

To start using the newly created Custom YUM Repository, we must create the corresponding YUM Repository Configuration file with `.repo` extension, which must be placed to `/etc/yum.repos.d/` dir. 


Example Custom YUM Repository Configuration file `/etc/yum.repos.d/custom.repo`

```
[customrepo]
name=Custom Repository
baseurl=file:///repository1/
enabled=1
gpgcheck=0
```



## LIST AVAILABLE REPO

List all available repository IDs (already added, enabled/disabled)
```
yum repolist all
```



## ADDING A YUM REPO

Yum repositories commonly provide their own `.repo` file. 

To add such a repo to your system and enable it, run the following
```
sudo yum-config-manager --add-repo http://www.example.com/example.repo
```



## ENABLE/DISABLE REPO



































