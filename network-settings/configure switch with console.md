
1. Подключаем компьютер к коммутатору через консольный кабель
2. Задаем привилегированного пользователя:
	1. enable
	2. configure t
	3. enable password 'password'
	4. user admin privilege 15 secret 'admin'
3. Устанавливаем авторизацию на подключение к консоли:
	1. configure t
	2. line console 0
	3. login local
4. Задаем ip address устройства:
	1. conf t
	2. interface Vlan1
	3. ip address 192.168.1.(1-256) 255.255.255.0
	4. no shutdown
5. Выбираем тип удаленного подключения на логическом интерфейсе:
	 1. configure t 
	 2. line vty 0 4
	 3. transport input telnet
	 4. login local        - устанавливаем пароль на вход и используем локальную базу
6. Сохраняемся:
	1. end
	2. write memory

Разные команды:

```
vlan 2 - создали доп виртуальную локалку vlan 2 
name admin - указали имя
exit - выход
```

```
interface fastEthernet 0/9   - подключаемся к порту коммутатора 
switchport mode access - устанавливаем ему принимать подключение 
switchport access vlan 2 - устанавливаем ему вирт локалку
```

```
show mac address-table  - показать таблицу мак адрессов
show run 
```

Подключение двух коммутаторов и разделение vlan:
1. Подключаем их по gigabitEthernet0/1 
2. Заходим в настройки коммутатора:
	```
	configure t 
	interface gigabitEthernet 1/1
	switchport mode trunk
	switchport trunk allowed vlan 2,3
	```
3. Делаем то же самое и с другим коммутатором
4. Сохраняемся: 
	```
	write memory
	```
