Super_easy_way_of_WSL2_SSL_Server.md
---
Written on 8/23/2020

Super easy and simple way to access WSL2 SSH server from an external machine.
---

## Summary
I found Scott Hanselman's blog article. it gave me good advice to make settings of WSL2 SSH server host. I brushed up and added some trick to the contents.
After that, It becomes super easy way to access WSL2 SSH server from an external machine.

There are three main solutions.
- Added the way to make SSH Key.
- No need to provide any scripts, and don't mind internal IP address.
- You can redirect any WSL2 service with the same way.

## Details

### Edit /etc/ssh/sshd_config
```sh
...STUFF ABOVE THIS...
Port 2222
#AddressFamily any
ListenAddress 0.0.0.0
#ListenAddress ::

...Option Stuff
PasswordAuthentication yes
```

### Generate SSH Key

```sh
$ sudo ssh-keygen -t ecdsa -N '' -f /etc/ssh/ssh_host_ecdsa_key
$ sudo ssh-keygen -t ed25519 -N '' -f /etc/ssh/ssh_host_ed25519_key
$ sudo ssh-keygen -t rsa -N '' -f /etc/ssh/ssh_host_rsa_key
```

### Redirect port into WSL2

```sh
> netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=22 connectaddress=127.0.0.1 connectport=2222
```

You can run this from elevated command script file.


### Open the firewall
On Windows 10 firewall settiings, you have to open tcp port 22 and 2222.

### Start SSH Server

```sh
$ sudo service ssh start
```

### How to access from an external machine

```sh
$ ssh your.Windows10.machine.address
```


## Reference

[How to SSH into WSL2 on Windows 10 from an external machine 
(DO NOT DO THE INSTRUCTIONS IN THIS POST)](https://www.hanselman.com/blog/CommentView.aspx)

https://www.hanselman.com/blog/CommentView.aspx


[THE EASY WAY how to SSH into Bash and WSL2 on Windows 10 from an external machine](https://www.hanselman.com/blog/THEEASYWAYHowToSSHIntoBashAndWSL2OnWindows10FromAnExternalMachine.aspx)

https://www.hanselman.com/blog/THEEASYWAYHowToSSHIntoBashAndWSL2OnWindows10FromAnExternalMachine.aspx

