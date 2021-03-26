assuming `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub` are user's private and public keys, and only ssh client can be used. 

# Question 1

we can use ssh option `-J` to proxy connection to server through bastion host.

one liner to connect to server1:

`ssh -J ubuntu@212.186.105.45 ubuntu@192.168.0.1`

# Question 2

one solution would be to define ssh "Host" to simplify the command to connect to each servers. this can be done in ssh client config file on local mahcine. 

`vim ~/.ssh/config`

```
#####################
### BASTION HOSTS ###
#####################

Host bastion1
    HostName 212.186.105.45
    User ubuntu
    IdentityFile ~/.ssh/id_rsa

Host bastion2
    HostName 212.186.105.48
    User ubuntu
    IdentityFile ~/.ssh/id_rsa

Host bastion3
    HostName 212.186.105.49
    User ubuntu
    IdentityFile ~/.ssh/id_rsa

####################
### SERVER HOSTS ###
####################

Host server1
    HostName 192.168.0.1
    User ubuntu
    IdentityFile ~/.ssh/id_rsa
    ProxyJump bastion1

Host server2
    HostName 192.168.0.2
    User ubuntu
    IdentityFile ~/.ssh/id_rsa
    ProxyJump bastion1

Host server3
    HostName 192.168.0.3
    User ubuntu
    IdentityFile ~/.ssh/id_rsa
    ProxyJump bastion2

Host server4
    HostName 192.168.0.4
    User ubuntu
    IdentityFile ~/.ssh/id_rsa
    ProxyJump bastion3
```

with this configuration in place on local machine, one can ssh to specific servers directly using following command:

`ssh server3`

will forward ssh connection through bastion2 before connecting to server3.
