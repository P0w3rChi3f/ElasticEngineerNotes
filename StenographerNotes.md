Install Stenographer
```
sudo yum install stenographer
```
Configure Stenographer
```
cd /etc/stenographer/
ls
vi config  

```

Config file

```
{
  "Threads": [
    { "PacketsDirectory": "/data/steno/thread0/packets/"
    , "IndexDirectory": "/data/steno/thread0/index/"
    , "MaxDirectoryFiles": 30000
    , "DiskFreePercentage": 10
    }
  ]
  , "StenotypePath": "/usr/bin/stenotype"
  , "Interface": "enp2s0"
  , "Port": 1234
  , "Host": "127.0.0.1"
  , "Flags": []
  , "CertPath": "/etc/stenographer/certs"
}
```
Certificates
```
sudo stenokeys.sh stenographer stenographer
ll /data/
sudo chown -R stenographer: /data/steno
sudo systemctl start stenographer  
sudo systemctl status stenographer  

```