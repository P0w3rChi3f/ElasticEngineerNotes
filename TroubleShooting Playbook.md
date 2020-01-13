1) Check firewall to see if port is open (`firewall-cmd`)
2) Check netstat to see if port is listening (`ss -lnt`)

Journalctl -xeu stenographer  
Error: not type given for **node** zeek
    vi /etc/zeek/node.cfg


## Troubleshooting install

#### Check Stenographer Indicies
```
ls /data/steno
ls /data/steno/thread0
ls /data/steno/thread0/index
systemctl status stenographer
```

#### Check Suricata
```
cd /data/suricata
ll
systemctl status suricata
tcpdump -i enp2s0
```

#### Check Zeek
```
cd /data/zeek/current
ls
```

Stop Zeek and current folder will be cleared
```
zeekctl stop
```

Check data format

Comment out json script
```
vi /usr/share/zeek/site/local.zeek
zeekctl stop
zeekctl cleanup all
zeekctl deploy
```

#### fsf gotcha
```
vi /usr/lib/systemd/system/fsf.service  
```  
__PIDFile=/run/fsf/fsf.pid__


## Kafka
`/usr/share/kafka/bin/kafka-topics.sh`  
`kafka-consumer-perf-test.sh`  
`/usr/share/kafka/bin/kafka-topics.sh --list --bootstrap-server 172.16.90.100:9092`  
