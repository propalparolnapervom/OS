# REDHAT MAINTENANCE



## REPOSITORIES

### LIST

**See configured custom repos**
```
yum repolist all
```

**See valid subscription repos**
```
supscription-manager repos --list
```

**See enabled valid subscription repos**
```
subscription-manager repos --list-enabled
```

### CONFIGURE

**Configure new repository**

To enable specific Red Hat Subscription Management repositories
```
supscription-manager repos --enable subscription.rhn.redhat.com
```

To enable specific custom repositories
```
yum-config manager --enable <repo>
```





















