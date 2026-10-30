Если есть несколько vlan 2, 3 и dhcp сервер, то сначала настраиваем vlan на коммутаторе 2 уровня

```
vlan 2
name VLAN2
exit

vlan 3
name VLAN3
exit

int range fa0/1-2
switchport mode access
switchport access vlan 2
exit

int range fa0/3-4
switchport mode access
switchport access vlan 3
exit

vlan 4
exit
int fa0/5
switchport mode access
switchport access vlan 4

```

Далее порт для маршрутизатора настраиваем на trunk 

```
int gig0/1
	switchport mode trunk
	switchport trunk allowed vlan 2,3,4
```

На маршрутизаторе настраиваем сабинтерфейсы как [[Коммутатор с Маршрутизатором]]

#### Настраиваем dhcp сервер для получения vlan

Сам сервер во vlan 4 с ip 192.168.4.2

![[Pasted image 20261030113944.png]]

#### Маршрутизатор
Далее на каждом сабинтерфейсе перенаправляем поступающий трафик на ip адресс сервера
```
int gig0/0/0.2
	ip helper-address 192.168.4.2
	exit
int gig0/0/0.3
	ip helper-address 192.168.4.2
```