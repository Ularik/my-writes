##### Install
```
sudo apt update
sudo apt-get install ssh
sudo apt install openssh-server
sudo systemctl enable sshd
```


Настроить ssh port
```
sudo nano /etc/ssh/sshd_config

#Port 22    - uncomment
```

### Открыть 22 порт на фаэрволе
```
sudo ufw allow 22
sudo ufw enable
```