Zeek - TCP -> (Executables to FSF)
Google Stenographeer - Disk
Suricata - Disk Eve.json
Logstash - 
ElasticSearch - Disk
Kibana - 
Filebeat - Read from Disk (reads Eve.json and sends to EleasticSearch)
FSF - Disk
    Client
    Server - Needs processing power
AF_Packet


Disk competition
    Suricata
    Kafka
    ElasticSearch
    Google Stenographer
    FSF

CPU 
    Zeek - 1 CPU == 100 mbs
    Suricata
    FSF
    ElasticSearch

Memory
    Zeek - 6 gig/core
    Logstash
    ElasticSearch


/ - remaining
/home - 10 G
/var - 10 G
/var/log - 2 G
/tmp - 2 G
/data/suricata
/data/elasticsearch
/data/steno
/data/kafka
/data/fsf
