# Create-docker-private-registry-by-nexus-oss
**This document describe how to create docker private registry and use it in containerd**
The environment in which the program is installed has the following specifications:
```
Type: Virtual Machine
OS: OracleLinux 8.5
Memory: 4GB
CPU: 2*2 (4 Cores)
Nexus OSS 3.x
HDD: 100 GB for OS and 50GB for Nexus OSS (APP+Data)
```
![image](https://user-images.githubusercontent.com/16554389/206153734-43f3838b-d5cd-4566-a661-0f6820cb4ecb.png)

**Install steps**
```
1- mkdir /app
2- cd /app
3- curl -O https://sonatype-download.global.ssl.fastly.net/repository/downloads-prod-group/3/nexus-3.42.0-01-unix.tar.gz
4- tar xzfv  https://sonatype-download.global.ssl.fastly.net/repository/downloads-prod-group/3/nexus-3.42.0-01-unix.tar.gz
5- mv nexus* nexus
6- sed -i.bak 's/application-port=8081/application-port=8000/' /app/nexus/etc/nexus-default.properties
7- Create nexus service:
```
cat <<EOF> /etc/systemd/system/nexus.service
[Unit]
Description=nexus service
After=network.target
  
[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/app/nexus/bin/nexus start
ExecStop=/app/nexus/bin/nexus stop 
User=nexus
Restart=on-abort
TimeoutSec=600
  
[Install]
WantedBy=multi-user.target
EOF
```
8- systemctl daemon-reload
9- systemctl enable nexus.service
10- systemctl start nexus.service

