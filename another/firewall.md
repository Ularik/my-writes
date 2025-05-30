1. sudo iptables -A INPUT -p tcp --dport 443 -j DROP - запретить входящие соединения к браузеру
2. sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT - РАЗРЕШИТЬ ВХОДЯЩИЕ СОЕДИНЕНИЯ