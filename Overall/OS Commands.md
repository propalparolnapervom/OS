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

## DRIVES

### Get info

#### blkid
The `blkid` command allows you to display information about available block devices.

```
blkid

blkid device_name

# obtain more detailed information
blkid -po udev device_name
```




### Mount volume
Mount all from `/etc/fstab` file:
```
mount 
```

### Unmount volume









