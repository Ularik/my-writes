sudo nano /etc/systemd/system/website

```
sudo nano /etc/systemd/system/fishing.socket
```

```

[Unit]
Description=fishing socket

[Socket]
ListenStream=/run/fishing.sock

[Install]
WantedBy=sockets.target

```

sudo nano /etc/systemd/system/fishing.service

```

[Unit]
Description=fishing daemon
Requires=fishing.socket
After=network.target

[Service]
User=kuba
Group=kuba
WorkingDirectory=/home/kuba/fish/fishing/app
ExecStart=/home/kuba/fish/venv/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/fishing.sock \
          project.wsgi:application   # корневая папка с settings.py

[Install]
WantedBy=multi-user.target

```


### Если вдруг сокет не запускается
sudo systemctl disable --now connector.socket
sudo systemctl daemon-reload
sudo systemctl restart fishing.service
curl --unix-socket /run/fishing.sock http://localhost/


sudo systemctl start fishing.socket
sudo systemctl enable fishing.socket

curl --unix-socket /run/fishing.sock localhost