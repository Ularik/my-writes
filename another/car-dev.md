journalctl -u car-cp-dev -f
systemctl status car-cp-dev -f
sudo systemctl status car-cp-dev.service
sudo systemctl start car-cp-dev.service
sudo systemctl stop car-cp-dev.service
sudo systemctl restart car-cp-dev.service

journalctl -u car-cp-dev.service -f

### Открыть айпи
iptables -L -n -v --line-numbers
iptables -A ispmgr_allow_ip -s "127.0.0.0/8" -j ACCEPT
iptables-save
sudo systemctl restart iptables.service
## Config for car-cp-dev tools:
### SSh configurations 
login: cp-car-dev
pass: nK1bE7xT1b
ip: 91.242.37.59

root path dir: /var/www/cp-car-dev/data/www/car-cp-dev.expcard.ru
### database dev:	
car-dev
car-dev-usr
oS2rI0jN5s

### admin
adm-car
E@3EQBb*

### front test:
mp
111qwe!

https://car-cp-dev.expcard.ru/admin

sudo systemctl status car-cp-dev.service

## Fast credentials
companies: 43890069, 14077196
groups: 42

Мотоциклы
Коммерческий транспорт
Грузовые и автобусы
Легковые автомобили

checks:
reg_num: В793ЕР01
sts: 9960741572

reg_num: Е703ОК123
sts: 9925080344

tickets regnum:
С380АВ550 ( здесь 2 пропуска день/ночь )
Р114ЕУ750
У910ЕЕ797
Т517КН198
Е886ВХ977
Н849МТ797
К339ХВ790
А683АА777 
О483РВ29 
О522РВ29
О490РВ29
О660РР190
А405СЕ69
С237КТ197

## Laximo
https://doc.laximo.ru/ru/general/principles
Подсказка для лаксимо:
https://github.com/alexandrkotov/laximo/blob/master/laximo_am.py

### База машин:
https://www.exist.ru/Catalog/Global/