На Cisco ASA 5505 DMZ (Demilitarized Zone) создаётся как отдельная логическая сеть с собственным VLAN-интерфейсом, IP-адресом и уровнем безопасности (`security-level`).

#### 0. Проверьте лицензию

```
show version | include VLANs
```

- **Base license** даёт 3 VLAN, и третий урезан: он может ходить только в одну из двух других зон. Поэтому для DMZ обязательно нужна команда `no forward interface` (см. ниже). Обычно запрещают DMZ → inside.
- **Security Plus** даёт 20 VLAN без ограничений, и `no forward` не нужен.

#### 1. Интерфейс DMZ

Допустим, inside — это Vlan1, outside — Vlan2, а под DMZ берём Vlan3 и порт Ethernet0/7.

```
interface Vlan3
 no forward interface Vlan1      ! только на Base license, ввести ДО nameif
 nameif dmz
 security-level 50
 ip address 212.212.3.1 255.255.255.1
 no shutdown
!
interface Ethernet0/7
 switchport access vlan 3
 no shutdown
```

#### 2. Настройка NAT (Static NAT / Port Forwarding)

Чтобы сервер в DMZ (например, веб-сервер `192.168.3.3`) был доступен из снаружи, пробрасываем порты (ASA 8.3+):

Cisco CLI

```
object network obj-dmz-webserver
 host 192.168.3.3
 nat (dmz,outside) static 212.212.1.2
```

#### 3. ACL для доступа извне
Правила межсетевого экрана (Access Control Lists) на ASA применяются к **реальным (внутренним)** IP-адресам серверов, а не к публичным. Выполните в глобальном режиме конфигурации:
```
access-list OUTSIDE-IN extended permit tcp any object obj-dmz-webserver eq www
access-list OUTSIDE-IN extended permit tcp any object obj-dmz-webserver eq https
access-group OUTSIDE-IN in interface outside
```

### Проверка конфигурации

Проверьте, что правила применились корректно:

Cisco CLI

```
show nat
show access-list OUTSIDE-IN
```