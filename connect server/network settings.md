В ubuntu по дефолту файл отвечающий за сеть это:
```
/etc/netplan/50-cloud-init.yaml
```
Внутрь прописываем след настройки:
```
network:
  version: 2
  ethernets:
    ens160:  - название вашей сети(проверить через ip a)
      addresses:
      - 10.100.191.8/24 - static free ip
      routes:
      - to: default
	    via: 10.100.191.254
	  nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```
далее применяем изменения:
```
sudo netplan apply
```
Если не проходит пинг на сервера google: 
```
ping -c 4 google.com - проверка

```
temporary failure in name resolution указывает на проблему с DNS
```
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```
это перепишет DNS сервер указанный в resolv.conf

```
nslookup google.com    - проверка серверов google
```