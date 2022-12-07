# Create-docker-private-registry-by-nexus-oss
**This document describe how to create docker private registry and use it in containerd**

The environment in which the program is installed has the following specifications:
```
Type: Virtual Machine
IP: 192.168.1.100
OS: OracleLinux 8.5
Memory: 4GB
CPU: 2*2 (4 Cores)
Nexus OSS 3.x
HDD: 100 GB for OS and 50GB for Nexus OSS (APP+Data)
```
![image](https://user-images.githubusercontent.com/16554389/206153734-43f3838b-d5cd-4566-a661-0f6820cb4ecb.png)

**Install steps**
```
Step1: Install Nexus OSS 3.x

1- mkdir /app
2- cd /app
3- curl -O https://sonatype-download.global.ssl.fastly.net/repository/downloads-prod-group/3/nexus-3.42.0-01-unix.tar.gz
4- tar xzfv  https://sonatype-download.global.ssl.fastly.net/repository/downloads-prod-group/3/nexus-3.42.0-01-unix.tar.gz
5- mv nexus* nexus
6- sed -i.bak 's/application-port=8081/application-port=8000/' /app/nexus/etc/nexus-default.properties
7- Create nexus service:

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

8- systemctl daemon-reload
9- systemctl enable nexus.service
10- systemctl start nexus.service
11- ss -tlpn | grep 8000
```
![image](https://user-images.githubusercontent.com/16554389/206165146-96ddc338-3693-4a67-8213-4cb1bfdbee29.png)

Step2: Create docker registry:
```
1- Login to Nexus OSS web ui on web browser:
http://192.168.1.100:8000
*Note: Deafult admin's password stored in "admin.password" file on "/app/nexus" and you must be change password after first login.*
```
![image](https://user-images.githubusercontent.com/16554389/206167618-ea87bf97-e5bc-47bd-9248-b7e31daa7605.png)

```
2- Create docker registry:
```
![image](https://user-images.githubusercontent.com/16554389/206169240-913082da-918e-4374-9a8e-080826b7fd42.png)


