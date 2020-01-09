# Working With Repositories (local & Remote)

## On Sensor
`yum list`  
`yum repo list /etc/yum.repos.d`

#### Yum commands  
```
yum --help
yum install
yum update
yum search  
yum provides - Searches through packages for a particular application and returns the results  
yum provides 
reposync  
yum makecache fast - First repository is cached/ fast = fastest speed  
yum clean all - Deletes all local repo databases  
yum history
yum history undo # - undo a commnd in history
```

#### Create a repo file

Per Repo Config
```
cd /etc/yum.repos.d

[ ] -- Repo ID
name= (discriptive name, can be duplicate)
baseurl= (location of repo) (http:\\repo\localdirectory) local location /var/www/html/repos
enable= (not required, 1 = enabled)
gpgcheck= (create or download the keys, 0=off, 1=check)
```

Global Repo Config  
```
/etc/yum.conf
[ ]
name=
baseurl=
location=
enable=
gpgcheck=
```

Enable or Disable a repo
```
yum-config-manager --disable <Repository Name>
yum-config-manager --enable <Repository Name>
```

#### Create a yum repo
Modify a local repo to point somewhere else
```
sudo vi /etc/yum.repos.d/local-yum.repo
:%s/repo/192.168.2.86/g
sudo yum clean
```
Install yum utils (rsync,) -- (Done on local machine)  
```
yum install yum-utils
cd /srv/ --(like www and ok with selinux)
mkdir repos && cd repos
reposync -l --repoid=repository_name --download_path=/repo/
reposync -l reopid=repository_name --download_path=/repo/ --newest-only --downloadcomps --download-metadata
```

#### Yum downloader

`yumdownloader suricata` -  Downloads just a package repo  
`repotrack -a ARCH -p /path/to/save/rmms packages` - Used to download every single rpm available in a repo because of the time or space on disk but you do need all the dependancies for the rpm..  you can use a tool called `repotrack`  
`repotrack -a x86_64 -p /var/www/repos/packages suricata zeek`

#### Createrepo
This tool will create the needed databases so that yum knows how to handle them.  
`yum install craterepo`

Create the databses file to make the local repo usable in the simplest form
`createrepo /repo/<repository_name>`

If we want to include the groups/comps we need to pass it the flag and name of the comps file.  It is almost always called `comps.xml` sometimes it can be called something else through. Thing of note is tha you are not provideing a full path to the file but rather just the name of the file.  

/////After Noon notes from Chow////////
#### yum repolist results
`yum repolist`  
	local-WANdisco-git  
	local-base  
	local-elasticsearch-7.x  
	local-epel  
	local-extras  
	local-rocknsm-2.5  
	local-updates  
	local-virtualbox  
	local-zerotier  
	localstuff  
#### Repo Sync
```
reposync -l --repoid=repository_name --download_path=/reo/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=local-base --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=local-elasticsearch-7.x --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=local-WANdisco-git --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=local-epel --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=local-extras --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=local-rocknsm-2.5 --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=local-updates --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=local-virtualbox --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=local-zerotier --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
sudo reposync -l --repoid=localstuff --download_path=/srv/repos/ --newest-only --downloadcomps --download-metadata  
```

//warning --> running low on disk space on student laptop	//instructor is working on a fix, possibly use /home directory instead, but SELinux
df -h									//or just resize the partitions??? - [Git Hub]  (https://gist.github.com/fernandoaleman/b607fe05c521fa00778e5b9c1c7678ed)  

	Filesystem              Size  Used  Avail Use% Mounted on
	/dev/mapper/centos-root   50G   50G  153M 100% /
	devtmpfs                 3.9G     0  3.9G   0% /dev
	tmpfs                    3.9G   16M  3.9G   1% /dev/shm
	tmpfs                    3.9G   11M  3.9G   1% /run
	tmpfs                    3.9G     0  3.9G   0% /sys/fs/cgroup
	/dev/sda1               1014M  170M  845M  17% /boot
	/dev/mapper/centos-home  180G 1015M  179G   1% /home
	tmpfs                    786M  4.0K  786M   1% /run/user/42
	tmpfs                    786M   52K  786M   1% /run/user/1000  

Solution - bind mount to use disk space on /home  
```
sudo -s
mkdir /home/srv
mv  /srv/* /home/srv/
ls /home/srv						//should have repos directory
mount -o bind /home/srv/ /srv
ls /srv							//should still have repos directory
```
```
yum install createrepo	 
yum install nginx  
cd /srv/repos  
ls			// view the various local repos  
/srv/repos/local-base/comps.xml 
```
Note:relative path to comps.xml; creates the sql database; if no comps.xml provided, then remove “-g comps.xml” do this for all the repos (as shown below in small font)
```
createrepo -g comps.xml local-base/  
ls /srv/repos/local-base/repodata			//should see various databases (e.g. sqlite.bz2, xml.gz, etc.)  
cd /srv/repos  
ls local-base  
sudo createrepo -g comps.xml local-base/  
ls local-elasticsearch-7.x  
ls local-elasticsearch-7.x/7.5.1/  
sudo createrepo local-elasticsearch-7.x  
ls local-epel  
sudo createrepo -g comps.xml local-epel  
ls local-extras  
ls local-extras/Packages  
sudo createrepo local-extras  
ls local-rocknsm-2.5  
sudo createrepo local-rocknsm-2.5  
ls localstuff  
sudo createrepo localstuff  
ls local-updates  
ls local-updates/Packages  
sudo createrepo local-updates  
ls local-virtualbox  
sudo createrepo local-virtualbox  
ls local-WANdisco-git  
sudo createrepo local-WANdisco-git  
ls local-zerotier  
sudo createrepo local-zerotier  
```
Become root, still allows logging (sudo -i OR sudo -s ⇒ one keeps shell variables); do not use [sudo - su root] (no logging)

`sudo -s`						
`sudo vi  /etc/nginx/conf.d/repo.conf`			//create conf file; spacing is for readability

```
	server {
	  listen 8008;					//which port to bind to; default for satellite repos
	
	  location / {
	    root /srv/repos;				//root directory starts here
	    autoindex on;				//use internal indexer from nginx
	    index index.html index.htm;		//multiple index files that can be served up
	  }
	error_page 500 502 503 504 /50.x.html;
	location = /50x.html {
	  root /usr/share/nginx/html;
	  }
	}  
```

systemctl restart nginx					//restart nginx
ss -lnt								//verify port 8008 is listening - can access locally using curl
curl localhost:8080						//should see html listing of repos
sudo firewall-cmd --add-port=8008/tcp --permanent		//allow access from other machines
sudo firewall-cmd --reload
curl 192.168.2.86:8008					//test from remote sensor (e.g. sensor) - failed to connect - denied from SELinux
								//not labelled as web file (currently either home or system file)
cd /srv/
ls -Z								//repos is unconfined object - tagged as var item, should be http context - need change context
chcon -R -u system_u -t httpd_sys_content_t repos		//change context recursively
ls -Z								//verify change in context
systemctl restart nginx
ss -lnt
curl 192.168.2.94:8008					//test from remote sensor (e.g. sensor) - now works
//2 steps for troubleshooting, not production environment, e.g. test SELK instead of FSF
	systemctl stop firewalld				//See if firewall issue
	setenforce 0						//With firewall still off, see if SELinux has context
	setenforce 1						//turn back on
	systemctl start firewalld				//turn back on
	getenforce						//verify that back in enforcing mode (1), not permissive mode (0)
Logon to pfsense: http://172.16.40.1/				//note: does not automatically turn on if loses power
	Services → DNS resolver → add Host Overrides
		Host: sg40					
		Domain: local.lan			//make up something unique; remember what it is; local.lan not routed to internet
		IP: 192.168.2.86				//repo laptop ip
		Description: Repos on laptop
		Save
		Apply changes				//verify entry in host overrides
		DNS Query Forwarding			//check the box if you want to be able to “ping repo” and get response from upstream DNS 
Verify in sensor - dns1 and dns2 only setup in sensor
	ssh cw2chow@172.162.100				//ssh into sensor
	ping sg40						//ping does not work, need to change order of DNS
	/etc/sysconfig/network-scripts/ifcfg-enp2s0	
DNS1=172.16.40.1
DNS2=192.168.2.1
systemctl restart network
ping sg40.local.lan					//for some reason, need to ping FQDN - issue on only some student configs...
sudo vi /etc/hosts						//What if no dedicated hardware for DNS on sensor?
	192.168.2.86 sg40 sg40.local.lan			//on mine, added “192.168.2.86 sg40” since couldn’t ping short name
cat /etc/resolv.conf						//can view DNS servers; “search local.lan”
vi /etc/inputrc							//disable inputrc
	#set bell none						//if you uncomment the first line, then you lose the “d” key and “d” key becomes a bell
								//ONLY uncomment the second line;      {SUDO,,} ⇒ bash shortcut to make lowercase
bind -f /etc/inputrc						//binds the keys OR restart computer

Replace IP in [baseurl=http://<repo_ip>/localstuff using %s		//missed morning class, so not sure about this %s stuff, so doing it manually...
	sg40:80008							//lab is to figure it out on our own using knowledge from this morning
sudo vi /etc/yum.repos.d/remote.repo
[local-WANdisco-git]
name=local-WANdisco-git
baseurl=http://sg40:8008/local-WANdisco-git/
enabled=1
gpgcheck=0

[local-base]
name=local-base
baseurl=http://sg40:8008/local-base/
enabled=1
gpgcheck=0

[local-elasticsearch-7.x]
name=local-elasticsearch-7.x
baseurl=http://sg40:8008/local-elasticsearch-7.x/
enabled=1
gpgcheck=0

[local-epel]
name=local-epel
baseurl=http://sg40:8008/local-epel/
enabled=1
gpgcheck=0

[local-extras]
name=local-extras
baseurl=http://sg40:8008/local-extras/
enabled=1
gpgcheck=0

[local-rocknsm-2.5]
name=local-rocknsm-2.5
baseurl=http://sg40:8008/local-rocknsm-2.5/
enabled=1
gpgcheck=0

[local-updates]
name=local-updates
baseurl=http://sg40:8008/local-updates/
enabled=1
gpgcheck=0

[local-virtualbox]
name=local-virtualbox
baseurl=http://sg40:8008/local-virtualbox/
enabled=1
gpgcheck=0

[local-zerotier]
name=local-zerotier
baseurl=http://sg40:8008/local-zerotier/
enabled=1
gpgcheck=0

[localstuff]
name=localstuff
baseurl=http://sg40:8008/localstuff/
enabled=1
gpgcheck=0	
sudo mkdir ~/repo_backup				//backup Centos repos
sudo cp -r C* ~/repo_backup/
ls ~/repo_backup/
cd /etc/yum.repos.d					//remove Centos repos
sudo rm -f C*
ls
yum clean all						//update repos
yum makecache fast					//no errors, so good to go!

sudo vi /etc/nginx/conf.d/proxy.conf			//Add reverse proxy in nginx to resolve port	 ---  on the repo laptop
	server {
	  listen 80;					//default port 
	  server_name sg40 sg40.local.lan repo
	
  proxy_max_temp_file_size 0;		//default limit is 1GB for nginx; set to unlimited
	
	  location / {
	    proxy_set_header X-Real_IP $remote_addr;
	    proxy_set_header Host $http_host;
	    proxy_pass http://127.0.0.1:8008;
	  }
	}
sudo vi /etc/yum.repos.d/remote.repo		//remove port numbers - sensor
yum makecache fast					//errors as expected - sensor
sudo systemctl restart nginx				//on repo laptop
yum makecache fast					//still have errors - sensor - no route
sudo firewall-cmd --add-port=80/tcp --permanent	//allow access from other machines
sudo firewall-cmd --reload
sudo firewall-cmd --list-ports				
	9200/tcp 9300/tcp 8008/tcp 80/tcp
yum makecache fast					//sensor - now works

TOMORROW - install suricata & stenographer
