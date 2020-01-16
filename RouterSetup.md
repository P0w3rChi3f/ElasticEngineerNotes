# Router Build

Need pfsense box (black box - mine is labelled SG09, 4 lan, 1 com, 2 usb, 1 vga), keyboard, thumbdrive, no mouse required.

## Hardware Setup

1. Boot pfsence from thumbdrive  
a. Plug in VGA monitor before bootup.
1. Press `ESC` to enter BIOS Setup
1. Press `F11` to enter boot menu

## Initial Install

1. Press `A` to Accept
1. Press `I` to Install
1. Press `OK` for default US keymap
1. Select `Auto` for guided disk setup  
    a. Install runs... select default options
1. Reboot
## Configure PFSence

Lan4 | Lan 3| Lan 2 | Lan 1 | Com
---- | ---- | ----- | ----- | --- 
em3  | em2  | em1   | em0   | local

1. Press `1` to assign interface
1. Press `n` to skip vlan setup
1. Set `em0` as wan
1. Set `em1` as lan
1. Press `y` to proceed  
    a. Nothing to finish

## Set Interface IPs

1. Press `1` to configure wan IP
1. Press `y` for DHCP
1. Press `n` for DHCPv6
1. Leave Blank for none
1. press `y` for http webconfig protocol
1. Press `2` to configure lan IP
1. Set IP to 172.16.90.1 /24
1. Select `enter` for none
1. select `enter` to disable DHCPv6
1. Select `y` to enable DHCP
1. Set start range to 172.16.90.100
1. Set end range to 172.16.90.254
1. Select `y` for http webconfig protocol

## Setup Lab Machine  
1. Set lab machine to a static ip of 172.16.90.10  
    a. I was able to get this to work since DHCP was enabled)  
1. Plug into the LAn Interface  
    a. LAN2of front face
1. Browse to 172.16.90.1
1. Login with `admin:pfsence`

## Configure PFSence through GUI

### Wizard > PFSence Setup > Genteral Information

1. Set hostname `SG90`
1. Set pfsence localdomain `local.lan`
1. Set primary DNS to `192.168.2.1`
1. Uncheck `Block Private Networks`
1. Set LAN IP address to `172.16.90.1`
1. Set subnet `24`
1. Set admin password `Password01`
1. Log back in
1. Dashboard overview
1. In the top right press [`+`]
1. Add `Service Status` and `Traffic Graphs`

### Firewall > Rules  
Secure out the box! default deny behaviours  

#### Firewall > Rules > WAN > Add  
Set the following

Pass | Proto | Source | Dest
--- | --- | --- | ---
 Null | All | Any | Any

#### Firewall > NAT > Outbound 
Select the `disable` radio button

### **Configure Remaining Interfaces**
#### Assign
1. Go to `Interface > Assignments
1. Add em2 (OPT1)
1. Add em3 (OPT2)
1. Click `Save`

#### Enable
1. Click interface name in list  
    a. e.g. `OPT1`
1. `Enable` interface
1. Ipv4 config type - `none`
1. Ipv6 config type - `none`
1. Click `Save`

#### Check for double-gateway issue
1. Go to `System > routing`  
    a. make sure there is not more than one gateway entry