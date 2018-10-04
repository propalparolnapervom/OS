# SSH PROXYCOMMAND


## OVERALL

[Why](https://heipei.github.io/2015/02/26/SSH-Agent-Forwarding-considered-harmful/) ProxyCommand is much better than Agent Forwarding.


## TASK

Make following connection: 

`local_PC -> public_IP (35.158.221.145) -> private_IP (172.31.33.38)`


## CONNECTION: DIRECT

> Note: access from the Bastion's private_IP has to be open for private_IP server

```
ssh -o ProxyCommand='ssh -i ~/overall/keys/aws/mytestacc/mytestacc_frankfurt.pem -W %h:%p ec2-user@35.158.221.145' ec2-user@172.31.33.38
```


## CONNECTION: VIA CONFIG FILE



### EXAMPLE 1

> Note: access from the Bastion's private_IP has to be open for private_IP server

Add/create following to `~/.ssh/config` file
```
Host bastion_host
  IdentityFile ~/overall/keys/aws/mytestacc/mytestacc_frankfurt.pem
 	User ec2-user
 	Hostname 35.158.221.145
 
 Host private_host
  IdentityFile ~/overall/keys/aws/mytestacc/mytestacc_frankfurt.pem
 	User ec2-user
  Hostname 172.31.33.38
 	ProxyCommand ssh bastion_host -W %h:%p 
```

Now you can connect to both servers like this:
```
ssh bastion_host
ssh private_host
```


### EXAMPLE 2

> Note: access from the Bastion's private_IP has to be open for private_IP server

Add/create following to `~/.ssh/config` file
```
Host bastion_host
  IdentityFile ~/overall/keys/aws/mytestacc/mytestacc_frankfurt.pem
 	User ec2-user
 	Hostname 35.158.221.145
 
 Host 172.*
  IdentityFile ~/overall/keys/aws/mytestacc/mytestacc_frankfurt.pem
 	User ec2-user
 	ProxyCommand ssh bastion_host -W %h:%p 
```

Now you can connect to both servers like this:
```
ssh bastion_host
ssh 172.31.33.38
```





































