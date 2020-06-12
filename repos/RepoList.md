# Repos needed

curl -s https://packagecloud.io/install/repositories/rocknsm/2_5/script.rpm.sh | sudo bash  
yum -y install epel-release
touch /etc/yum.repos.d/elasticsearch.repo
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch  

vi /etc/yum.repos.d/elasticsearch.repo  

```repo
[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md
```  

yum install --enablerepo=elasticsearch elasticsearch  

vi /etc/yum.repos.d/logstash.repo

```repo
[logstash-7.x]
name=Elastic repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

PORTS

6379 - Redis
