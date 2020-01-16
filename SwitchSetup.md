# Switches

`ll /dev` to look at devices

`screen` command in Linux provides the ability to launch and use multiple shell sessions from a single ssh session. 

## Switch Setup
`sudo vi /echssh/ssh_config`
    uncomment the lines that start with MAC's and Ciphers
    hostkeyAlgorithms ssh-dss,ssh-rsa
    kexAlgorithms +diffie-hellman-group1-sha1
```
ssh perched@10.0.0.1
perched1234!@#$
```

## Setup DHCP
```
ip dhcp excluded-address 10.0.x.1 10.0.x.1
ip dhcp pool <student Group>
network 10.0.x.0 255.255.255.252
default-router 10.0.x.1
dns-server 192.168.2.1
exit
```

## Setup VLans
```
vlan <SG#>
name <Student Group>
state active
no shut
exit
```

## Add interfaces
```
Interface Gig 1/0/x
switchport
switcchport access valn SG#
no shut
interface valna SG#
ip address 10.0.x.1 255.255.255.252
no shut
exit
```
## Enable Static Route
```
ip routing
ip rout 172.16.x.0 255.255.255.0 10.0.x.2
```

