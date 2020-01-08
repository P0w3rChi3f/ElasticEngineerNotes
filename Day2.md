Switches 
    ll /dev to look at devices
    screen

static routes
config t
ip route subnet dest


Chow port #9 
Honeycutt/Jackson port #33

Setup switch
sudo vi /echssh/ssh_config
    uncomment the lines that start with MAC's and Ciphers
    hostkeyAlgorithms ssh-dss,ssh-rsa
    kexAlgorithms +diffie-hellman-group1-sha1
ssh perched@10.0.0.1
perched1234!@#$

Setup DHCP
    ip dhcp excluded-address 10.0.x.1 10.0.x.1
    ip dhcp pool <student Group>
    network 10.0.x.0 255.255.255.252
    default-router 10.0.x.1
    dns-server 192.168.2.1
    exit

Setup VLans
    vlan <SG#>
    name <Student Group>
    state active
    no shut
    exit

Add interfaces
    Interface Gig 1/0/x
    switchport
    switcchport access valn SG#
    no shut
    interface valna SG#
    ip address 10.0.x.1 255.255.255.252
    no shut
    exit

Enable Static Route
    ip routing
    ip rout 172.16.x.0 255.255.255.0 10.0.x.2

Build router - need pfsense box (black box - mine is labelled SG01, 4 lan, 1 com, 2 usb, 1 vga), keyboard, thumbdrive, no mouse required
	Boot from pfsense thumbdrive - plug in vga monitor before bootup
Hardware setup
	[Esc - enter BIOS setup]
	F11 - boot menu					//I picked the first UEFI USB boot option (2 listed, plus general USB)
Initial install
	A - accept
	I - install (OK) - default US keymap
	Auto - Guided Disk Setup
	...install runs…					//select default options
	reboot

Config PFSence

| Lan4 | Lan 3| Lan 2 | Lan 1 | Com |
| --- | --- | --- | --- | --- | 
| en3 | en2 | en1 | en0 | local| 

1- assign interface
n - skip vlan setup
en0 - wan
en1 - lan interface
(nothing to finish) - y - proceed

2) Set interface IPs
    1 - Wan Interface
    y - dhcp
    n - dhcpv6
    blank for "none" y - http webconfig protocol
    2 - LAN int
    172.168.xx.1 /24
    <enter> for none
    <enter> for none disable IPv6
    y - enable DHCP
    172.16.x0.100 Range start
    172.16.x0.254 reange end
    y - http webconfig protocol

set your lab machine to static ip 172.16.xx.10
plug int the LAN interface (Lan2 on front face) and browse to 172.16.xx.1
complete first login to web GUI

admin:pfsence

Wizard > Pfsence Setup > Genteral inormation
set hostname SG## - pfsence localdomain - local.lan
primary dns
uncheck block private networks
LAN IP address - 172.16.x.1
subnet - 24
set (and document new admin password) - Password01

log back in
dashboard overview
top righ + and add:
service status
traffic graphs

firewall> Rules
secure out the box! default deny behairo
firewall > rules > wan > add
pass, proto=all, source=any, dest=any

now disable NAT firewall > nat > outbound: disable radio button

Configrue Remainin interfaces 
    assign
        interface > assignments
        add em2 (OPT1)
        add em3 (OPT2)
        save
    enable
        Click interface name in list, e.g. OPT1
		Enable interface
		Ipv4 config type - none
		Ipv6 config type - none
		Save
	Check for double-gateway issue
		System > routing:
			Make sure there is not more than one gateway entry

CentOS Networking 
ip addr
review information displated
ip -4 addr -only show ipv4
UP,Lower_UP, satae up

ip cheathsheet 
    ip addr
            ip a show dev  enp0s3
            ip a add x.x.x.x/24 dev xxx
            ip a del x.x.x.x/24 dev xxx

    ip link 
        ip link show dev enp0s3
        ip -s link
        ip link set dev xxx up
        ip link set dev xxx down
        ip link set enpxxxx promisc on

    ip route
    
    ip neigh
        ip neigh show dev enp0s3

Network Manager
    systemctl status NetworkManager
    nmcli devicese
    nmtui 
    sudo nmtui 

Network scripts
    ll /etc/sysconfig/network-scritps
        BOOTPROTO="none" - static
        IPADDRESS=x.x.x.x
        PPREFIX=24
        GATEWAY=x.x.x.x
        DNS1=192.168.2.1
        DNS2=172.16.2.1
    systemctl restart network

Disable IPv6
    sysctl.conf
    edit the config file to disable IPv6 on all interfaces
        ```
        sudo vi /etc/sysctl.conf
        net.ipv6.conf.all.disable_ipv6 = 1
        net.ipv6.conf.default.disable_ipv6 = 1
        net.ipv6.conf.lo.disable_ipv6 = 1
        ```
    Load change from /etc/sysctl.conf file `sudo sysctl -p`

    host file
    edit the host file to remove the ipv6 entry ::1
    `sudo vi /etc/host'

Firewalld - basic Usage
    Open / allow traffic
    `sudo firewall-cmd --zone=public --add-port ####/tcp` -- permanent
    `sudo firewall-cmd --reload`
    `sudo firewall-cmd --list-ports`
    
    Close / deny traffic 
    sudo firewall-cmd --zone=public --remove-port=####/tcp --permanent
    sudo firewall-cmd --reload

    Commit all running firewall rules into the startup rules
    sudo firewall-cmd --runtime-to-permanent

    list all firewall zones
    sudo firewall-cmd --list-all-zones

Monitoring Activity

`ss -lnt` (shows all lisening ports)

Working With Repositories (local & Remote)

On Sensor
yum list
yum repo list /etc/yum.repos.d

Yum commands
    `yum --help`
    `yum install`
    `yum update`
    `yum search`
    `yum provides` (searches through packages for a particular application and returns the results) 
        `yum provides reposync`
    `yum makecache fast` (first repository is cached/ fast = fastest speed)
    `yum clean all` (deletes all local repo databases)
    `yum history`
    `yum history undo #` (undo a commnd in history)

cd /etc/yum.repos.d

[ ] -- Repo ID
name= (discriptive name, can be duplicate)
baseurl= (location of repo) (http:\\repo\localdirectory) local location /var/www/html/repos
enable= (not required, 1 = enabled)
gpgcheck= (create or download the keys, 0=off, 1=check)


yum.conf (Changes are made globaly)
/etc/yum.conf

yum-config-manager --disable <Repository Name>
yum-config-manager --enable <Repository Name>
 