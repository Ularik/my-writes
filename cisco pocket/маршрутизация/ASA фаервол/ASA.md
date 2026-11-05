
Удаленный доступ
```
conf t
username admin password cisco
ssh 192.168.1.0 255.255.255.0 inside
aaa authentication ssh console LOCAL
```

подключение
```
ssh -l admin 192.168.1.1
```

#### Security level
Меняется на vlan е 
```
int vlan 1
	security-level 95
```


### Настройка ip address
```
int vlan 2 
	ip address 212.212.1.2 255.255.255.0
	exit

route outside 0.0.0.0 0.0.0.0 212.212.1.1  - ip роутера
```

#### На маршрутизаторе настраиваем пути (для проверки обратного доступа)
```
ip route 192.168.1.0 255.255.255.0 212.212.1.2
```

### Настройка инспектирования трафика
```
class-map inspection_default
match default-inspection-traffic
exit
policy-map global_policy
class inspection_default
inspect icmp
exit
service-policy global_policy global
```


### Настройка NAT 
```
object network FOR-NAT
subnet 192.168.1.0 255.255.255.0   - подсеть которую натим
nat (inside,outside) dynamic interface
``` 

