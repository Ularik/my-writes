Если на коммутаторе 3 уровня делаем маршрутизацию, то ACL будет ставится на конкретный vlan вместо обычного порта как на маршрутизаторе
```
ip access-list extended FOR-SERVER
	permit ip 192.168.20.0 0.0.0.255 any

int vlan 10
	ip access-group FOR-SERVER
```