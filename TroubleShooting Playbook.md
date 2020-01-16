# Troubleshooting

## Common Tools

1) Check firewall to see if port is open:
```
 firewall-cmd --list-ports
 ```
2) Check netstat to see if port is listening:
```
ss -lnt
```
3) Check the errors on an application starting:
```
Journalctl -xeu <Application>
```

## Troubleshooting installs

### Check Stenographer Indicies
```
ls /data/steno
ls /data/steno/thread0
ls /data/steno/thread0/index
systemctl status stenographer
```

### Check Suricata
```
cd /data/suricata
ll
systemctl status suricata
tcpdump -i enp2s0
```

### Check Zeek
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

### fsf gotcha
```
vi /usr/lib/systemd/system/fsf.service  
__PIDFile=/run/fsf/fsf.pid__
```

### Kafka
```
/usr/share/kafka/bin/kafka-topics.sh
kafka-consumer-perf-test.sh  
/usr/share/kafka/bin/kafka-topics.sh --list --bootstrap-server 172.16.90.100:9092
```

## Filebeat
```
yum install yamllint
```

look for fsf and suricat topics at:
``` 
/data/kafka
firewall-cmd --list-ports
ss -lnt
```

## ElasticSearch


## Kibana


## Logstash

Zeek and kafka
```
curl 172.16.90.100:9200/_cat/indicies
zeekctl status
ll /data/zeek/
/usr/share/kafka/bin/kafka-topics.sh --list --bootstrap-sverer 172.16.90.100:9092
/usr/share/kafka/bin/kafka-topics.sh --describe  --bootstrap-sverer 172.16.90.100:9092 -topic zeek-raw
/usr/share/kafka/bin/kafka-console-consumer.sh --bootstrap-sverer 172.16.90.100:9092 -topic zeek-raw --from-beginning
```

```
curl 172.16.90.100:9200/_cat/indices
cat /var/log/logstash/logstash-plain.log | grep error
/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/100-input-zeek.conf -t
cat *zeek*
```
