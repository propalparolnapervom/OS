# SSH

## SSH PROTOCOL

[Official Docs](https://www.ssh.com/ssh/protocol/)

The **SSH protocol** (also referred to as Secure Shell) is a method for secure remote login from one computer to another. 
It provides several alternative options for strong authentication, and it protects the communications security and integrity with strong encryption. 
It is a secure alternative to the non-protected login protocols (such as telnet, rlogin) and insecure file transfer methods (such as FTP).

---------------------------

There are several options that can be used for user authentication. The most common ones are *passwords* and *public key* authentication.

The public key authentication method is primarily used for automation and sometimes by system administrators for single sign-on. 

The idea is to have a cryptographic key pair - public key and private key - and configure the public key on a server to authorize access and grant anyone who has a copy of the private key access to the server. 

The keys used for authentication are called **SSH keys**.

Public key authentication is also used with smartcards, such as the CAC and PIV cards used by US government.


---------------------------

Once a connection has been established between the SSH client and server, the data that is transmitted is encrypted according to the parameters negotiated in the setup. During the negotiation the client and server agree on the symmetric encryption algorithm to be used and generate the encryption key that will be used. The traffic between the communicating parties is protected with industry standard strong encryption algorithms (such as AES (Advanced Encryption Standard)), and the SSH protocol also includes a mechanism that ensures the integrity of the transmitted data by using standard hash algoritms (such as SHA-2 (Standard Hashing Algorithm)).


## PUBLIC KEY AUTHENTICATION

[Official Docs](https://www.ssh.com/ssh/public-key-authentication)

The motivation for using public key authentication over simple passwords is:

  - security: it provides cryptographic strength that even extremely long passwords can not offer. 
    
  - usability: it allows users to implement single sign-on across the SSH servers they connect to. Public key authentication also allows automated, passwordless login that is a key enabler for the countless secure automation processes that execute within enterprise networks globally.


### ASYMMETRIC CRYPTOGRAPHY - ALGORITHMS

As with any encryption scheme, public key authentication is based on an algorithm.

There are several well-researched, secure, and trustworthy algorithms out there - the most common being the likes of RSA and DSA.

Unlike the commonly known (symmetric or secret-key) encryption algorithms the public key encryption algorithms work with two separate keys. These two keys form a pair that is specific to each user


### KEY PAIR - PUBLIC AND PRIVATE

In the SSH public key authentication use case, it is rather typical that the users create (i.e. provision) the key pair for themselves. 

Each SSH key pair includes two keys:

  - A [public key](https://www.ssh.com/cryptography/public-key) that is copied to the SSH server(s). Anyone with a copy of the public key can encrypt data which can then only be read by the person who holds the corresponding private key. Once an SSH server receives a public key from a user and considers the key trustworthy, the server marks the key as authorized in its authorized_keys file. Such keys are called **authorized keys**.

  - A [private key](https://www.ssh.com/cryptography/private-key) that remains (only) with the user. The possession of this key is proof of the user's identity. Only a user in possession of a private key that corresponds to the public key at the server will be able to authenticate successfully. The private keys need to be stored and handled carefully, and no copies of the private key should be distributed. The private keys used for user authentication are called **identity keys**.


### SETTING UP PUBLIC KEY AUTHENTICATION FOR SSH

The following simple steps are required to set up public key authentication (for SSH):

- Key pair is created (typically by the user). This is typically done with [ssh-keygen](https://www.ssh.com/ssh/keygen/).

- Private key stays with the user (and only there), while the public key is sent to the server. Typically with the [ssh-copy-id](https://www.ssh.com/ssh/copy-id) utility.

- Server stores the public key (and marks it as authorized).

- Server will now allow access to anyone who can prove they have the corresponding private key


### HANDLING OF THE PRIVATE KEY

It is extremely important that the privacy of the private key is guarded carefully. For most user-driven use cases this is accomplished by encrypting the private key with a [passphrase](https://www.ssh.com/ssh/passphrase).

> In practice, however, most SSH keys are without a passphrase. 

> There is no human to type in something for keys used for automation. 

> The passphrase would have to be hard-coded in a script or stored in some kind of vault, where it can be retrieved by a script. 

> An attacker with sufficient privileges can easily fool such a system. Thus, there would be relatively little extra protection for automation.

When a private key is needed the user is asked to supply the passphrase so that the private key can be decrypted. The handling of passphrases can be automated with an [SSH agent](https://www.ssh.com/ssh/agent).

In most automated use cases (scripts, applications, etc) the private keys are not protected and careful planning and key management practises need to be excercised to remain secure and compliant with regulatory mandates.


## CREATE A KEY PAIR

### SSH-KEYGEN - GENERATE A NEW SSH KEY

[Official Docs](https://www.ssh.com/ssh/keygen/)

**ssh-keygen** is a tool for creating new authentication key pairs for SSH. Such key pairs are used for automating logins, single sign-on, and for authenticating hosts.


Generate a key pair
```
      #Optionally, choose algorithm to use or leave it default:
      #   ssh-keygen -t rsa -b 4096
      #   ssh-keygen -t dsa
      #   ssh-keygen -t ecdsa -b 521
      #   ssh-keygen -t ed25519

ssh-keygen

      Generating public/private rsa key pair.
      Enter file in which to save the key (/home/vagrant/.ssh/id_rsa):
      Enter passphrase (empty for no passphrase):
      Enter same passphrase again:
      Your identification has been saved in /home/vagrant/.ssh/id_rsa.
      Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub.
      The key fingerprint is:
      SHA256:2p6k1fQavFsY+4Uckhe9rSu0DgBoFK61RGBzrhFmzX8 vagrant@control-host
      The key's randomart image is:
      +---[RSA 2048]----+
      |  B+=.           |
      | + Oo.       .   |
      |  . B..     . .  |
      |   B ...E  . . o |
      |  o .  .S = o . .|
      |       o = O.o . |
      |      . + B.=.o  |
      |       = . Bo. . |
      |      . o +oo..  |
      +----[SHA256]-----+
```

### COPYING THE PUBLIC KEY TO THE SERVER

Copy *public* (**!!!**) key to **each** *location* and *user* you'd like to connect eventually
```
      #File ~remote_user/.ssh/authorized_keys will be updated on the remote side by default
      
ssh-copy-id -i /home/vagrant/.ssh/id_rsa.pub vagrant@remote-host
```


## SSHD_CONFIG - SSH SERVER CONFIGURATION

[Official Docs](https://www.ssh.com/ssh/sshd_config/)

The OpenSSH server reads a configuration file when it is started. 

Usually this file is `/etc/ssh/sshd_config`, but the location can be changed using the -f command line option when starting `sshd`. 

Some organizations run multiple SSH servers at different port numbers, specifying a different configuration file for each server using this option.



## SSH-AGENT - SSO USING SSH

[Official Docs](https://www.ssh.com/ssh/agent)

The **ssh-agent** is a helper program that keeps track of user's *identity keys* and their *passphrases*. 

The agent can then use the keys to log into other servers without having the user type in a password or passphrase again. 

> This implements a form of single sign-on (SSO).

The SSH agent is used for SSH public key authentication. It uses SSH keys for authentication. Users can create SSH keys using the ssh-keygen command and install them on servers using the ssh-copy-id command.


### STARTING SSH-AGENT

See if ssh-agent is running
```
ps -ef | grep ssh-agent
```

If ssh-agent is not automatically started at login, it can be started manually with the command
```
eval `ssh-agent`
```

Make sure key-based logins to servers are allowed
```
sudo cat /etc/ssh/sshd_config | grep PubkeyAuthentication

  #PubkeyAuthentication yes
```


### ADDING SSH KEYS TO THE AGENT

List private keys currently accessible to the agent
```
ssh-add -l
```

Add private key to ssh-agent
```
ssh-add ~/.ssh/id_rsa
```






