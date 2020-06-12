# PacketBeat Reference  

## MAC Brew Reference  

[MAC Brew Install](https://www.elastic.co/guide/en/beats/packetbeat/current/packetbeat-installation.html)  

[Directory Layout](https://www.elastic.co/guide/en/beats/packetbeat/current/directory-layout.html)  

**Type** | **Description** | **Location**  
---      | ---             | ---  
**home** | Home of the packetbeat installation. |/usr/local/var/homebrew/linked/packetbeat-full
**bin** | The location for the binary files. | /usr/local/var/homebrew/linked/packetbeat-full/bin  
**config** | The location for configuration files. | /usr/local/etc/packetbeat  
**data** | The location for persistent data files. | /usr/local/var/lib/packetbeat  
**logs** | The location for the logs created by packetbeat. | /usr/local/var/log/packetbeat  

```zsh
brew services start elastic/tap/packetbeat-full  
```  
