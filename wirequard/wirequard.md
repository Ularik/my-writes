wg-quick up wg0 - start vpn
wg-quick down wg1 - stop vpn
wg show - check working

```
sudo vim /etc/wireguard/wg1.conf
systemctl enable wg-quick@wg1.service
systemctl start wg-quick@wg0.service
systemctl status wg-quick@wg0.service
```

```
sudo systemstl stop wg-quick
sudo systemctl disable wg-quick@<config>
```

Wg панелька:

login: http://92.246.76.126:51821/
Пароль: v4m7M-r?tMv*sT
