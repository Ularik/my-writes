```
ip dhcp excluded-address 192.168.2.1 192.168.2.20
ip dhcp excluded-address 192.168.3.1 192.168.3.20

ip dhcp pool VLAN2
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 8.8.8.8
 exit

ip dhcp pool VLAN3
 network 192.168.3.0 255.255.255.0
 default-router 192.168.3.1
 dns-server 8.8.8.8
 exit
```