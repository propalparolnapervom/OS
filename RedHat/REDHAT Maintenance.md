# REDHAT MAINTENANCE



## REPOSITORIES

**See configured repositories**
```
yum repolist all
```

**See valid subscription repos**
```
supscription-manager repos --list
```

**Configure new repository**

To enable specific Red Hat Subscription Management repositories
```
supscription-manager repos --enable subscription.rhn.redhat.com
```

To enable specific custom repositories
```
yum-config manager --enable <repo>
```





















