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

## Update Rock NSM

* Cache your repositories  

```bash
sudo yum makecache fast
```

* Update the system  

```bash
sudo yum update -y
```

* Install missing NetworkManager-wifi

```bash
sudo yum install NetworkManager-wifi -y
sudo reboot
```

___

## Optional Driver install

___

### From remote computer

I had to install an optional driver for my wireless nic.  I have an Intel 3165 nic and the drivers can be downloaded from [here](https://www.intel.com/content/www/us/en/support/articles/000005511/network-and-i-o/wireless-networking.html)  

Once downloaded I copied them to my NUC  

* Command assumes there is a directory called `/tmp/wirelessdirvers` already creaated on the NUC

```ssh
scp /path/to/my/dowloaded/drivers/tar/file NUCuser@ip.address.to.NUC:/tmp/wirelessdirvers
```  

___

### On the NUC

Navigate to where you put your tarfile.  ***In my case it is located at /tmp/wirelessdrivers***.  Then untar the file.

```bash
cd /tmp/wirelessdirvers  
tar -xvf iwlwifi-7265-ucode-25.30.14.0.tgz  
```

cd into the new unzipped directory

```bash
cd iwlwifi-7265-ucode-25.30.14.0
```

Follow the instructions in the `README` file.

I just copied the iwlwifi-7265-ucode-14 and iwlwifi-7265D-ucode-14 to /lib/firmware  

```bash  
sudo cp /tmp/wirelessdriver/iwlwifi-7265-ucode-25.30.14.0/iwlwifi-7265D-14.ucode /lib/firmware/

sudo cp /tmp/wirelessdriver/iwlwifi-7265D-ucode-25.30.14.0/iwlwifi-7265D-14.ucode /lib/firmware/
```

I finally got it to work by running `sudo yum install iwl7265-firmware.noarch`
___

## Configure Wireless for management

Go to network configuration  

```bash
sudo nmtui  
```

Select your *WiFi Connection* then click `Activate`  
Add *Your Wifi* security key then click `OK`  

* if it fails just try again  

Then click `Back`  
Then select `Quit`  
Then click `OK`  
___
We can verify if everthing is working by seeing if our wireless card is listed and pulling an IP address

```bash  
ip a
```

If wireless is working make note of its address.  Now you can disconnect the LAN connection and ssh into the wireless connection.  If everything is good to go you can move onto installing all the apps.  

___  

## Inital Rock install

Follow the [Rock install guide](https://docs.rocknsm.io/configure/setup-tui/)  

```rock
sudo rock setup  
sudo rock tui  
```

### Minor changes I made  

Ran into a problem where ElasticSearch would time out on start.  I followed the instructions [here](https://unix.stackexchange.com/questions/227017/how-to-change-systemd-service-timeout-value) to change the TimeoutStartSec to 900.  

```bash
/usr/lib/systemd/system/elasticsearch.service
TimeoutStartSec=900
```

Changed Xms3g to Xms1g and Xmx3g to Xmx1g.  Added -XX:-AssumeMP to line 38  

```bash
vi /etc/elasticsearch/jvm.options
-Xms1g
-Xmx1g  
```

If the install fails at any point it can be restared.  

```bash
rock deploy --limit @/usr/share/rock/playbooks/deploy-rock.retry  
```  
