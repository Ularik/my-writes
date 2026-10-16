**По умолчанию всегда все порты на vlan1**

```
enable
  conf t
    vlan 2
      name buh
      exit
```

Подключаем компьютеры к первому и второму порту коммутатора fastethernet 0/1, 0/2
и настраиваем эти порты 
```
interface fastEthernet 0/1
  switchport mode access
  switchport access vlan 2
  exit

interface fastEthernet 0/2
  switchport mode access
  switchport access vlan 2
  exit

show vlan
or
show vlan brief
```

### Настраиваем ip на компьютерах, по правилу хорошего тона вторая цифра в ip = номеру vlan это 2

```
192.168.2.1
192.168.2.2
```

## Создание vlan 3

**Тоже самое делаем только с другими портами на коммутаторе**

enable
  conf t
    vlan **3**
      name users
      exit
  
interface fastEthernet 0/3
  switchport mode access
  switchport access vlan 3
  exit

interface fastEthernet 0/4
  switchport mode access
  switchport access vlan 3
  exit

show vlan
or
show vlan brief

## Подключение второго коммутатора через trunk port

Подключаем через перекрестный провод два коммутатора через gigabit порты
Заходим в настройки коммутаторов и ставим такие настройки
```
conf t
interface gig
interface gigabitEthernet ?
Switch(config)#interface gigabitEthernet 0/1
Switch(config-if)#switchport mode trunk ?
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed ?
Switch(config-if)#switchport trunk allowed vlan ?
Switch(config-if)#switchport trunk allowed vlan 2,3

```

Проверка
```
show vlan brief
show interfaces trunk
show interfaces status
```

При этом на втором коммутаторе тоже должны предварительно создастся vlan 2 и vlan 3

Ps. на картинке ошибка во vlan 3 вторая цифра везде 3, а не 2

![[Схема VLAN_ два коммутатора и trunk.png]]