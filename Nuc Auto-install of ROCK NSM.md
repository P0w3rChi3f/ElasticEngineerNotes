# NUC Auto Install of ROCK NSM

Start by following the steps outlined in the [ROCKNSM.io](https://docs.rocknsm.io/install/install/) documentation.

On first login the wireless nic was not working.  After running `nmcli` my wireless adapter had an error stating `plugin missing`.

I connected ethernet port `enp3s0` to my switch.  Then I had to run `nmtui` -> `edit a connection` -> `enp3s0` -> `edit`  
___  

## enp3s0 configuration  

Profile Name: `enp3s0`  
Device: `enp3s0` (***Insert Mac Here***)  
Ethernet: (***Leave Default***)  
IPv4 Configuration: `Automatic`  
IPv6 Configuration: `Automatic`  
Select `Automatically Connect` and `Avaiable to all users`  
Select `OK`  
Select `Back`  
Scroll to `Quit`  
And select `OK`
___  
