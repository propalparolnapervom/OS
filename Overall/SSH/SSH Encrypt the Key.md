# SSH ENCRYPT THE KEY


## OVERALL

If you already have the key, which is not encrypted yet, you have to encrypt it.


## ENCRYPTION STEPS

```
chmod 700 mytestacc_frankfurt.pem
ssh-keygen -p -f mytestacc_frankfurt.pem

      Enter new passphrase (empty for no passphrase): 
      Enter same passphrase again: 
      Your identification has been saved with the new passphrase.

chmod 400 mytestacc_frankfurt.pem
```



