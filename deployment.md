# Deployment architecture

## Networks

ODA deployment will consist of three networks:
1. **DMZ** contains gateways and access control services
2. **Application VLAN** contains ODA microservices
3. **Logging VLAN** contains ODA logging services and its log database

### Incoming connections to ODA

Each network will be protected by a firewall. Firewall rules for incoming 
TCP connections:

| Source | Destination                                               | Ports   |
| ------ | --------------------------------------------------------- | ------- |
| any    | DMZ/API Gateway                                           | 80, 443 |
| any    | Control/Jitsi Videobridge   | TCP 80, 443 and 4443, UDP 10000-20000 |
| any    | DMZ/X-Road Security Server                                |     443 |
| DMZ    | Application VLAN/Microservices                            |   6080- |
| DMZ    | Logging VLAN/Logging Service (Virtual IP)                 |    6084 |
| Application VLAN | Logging VLAN/Logging Service (Virtual IP)       |    6084 |

Port 22 (SSH) will be opened to every server to provide remote management 
access. Additionally, a monitoring server and/or agents will be included in the
environment (products, protocols and ports to be decided).

If connections from end user LAN are restricted, access to ODA API Gateway 
ports 443 and 80 (which redirects to 443) must be opened, as well as UPD ports
10000-20000 for video conferencing. 

*development/proto/alpha environment:*
- API Gateway: 185.166.28.130
- X-Road Security Server: 185.166.28.131

*beta environment:*
- API Gateway: 185.166.28.127
- Jitsi Videobridge: 85.166.28.70
- X-Road Security Server: 185.166.28.69

*production environment:*
- API Gateway: TBD
- X-Road Security Server: TBD

### Connections from ODA to local services

Source IP for development/pilot prototype/alpha environment is 185.166.28.68.

Source IP for beta environment is 185.166.28.133.

Source IP for production environment is to be decided.

ODA ESB will connect to local appointment booking service. Connections to other
EHR APIs might be required in the future. 

ODA Context Server module connects to local minimum context server.

Outgoing connections to standard ports 80 and 443 are allowed by default.
Connections to non-standard ports (eg. 8443) require firewall configuration
on ODAs end.

### Connections from ODA to Internet

ODA will connect to national services over X-Road and the public Internet. 
Operating system packages are installed and upgraded from official RedHat and 
Fedora update sites, as well as middleware provider sites (such as Nginx).  
Docker images are downloaded from Docker Hub. All these connections use HTTPS 
or HTTP protocol.

ODA will also require access to [MIKES] NTP servers. 

![](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/omahoito/definitions/master/deployment.plantuml?4)

[MIKES]: http://www.mikes.fi/julkinen-ntp-palvelu
