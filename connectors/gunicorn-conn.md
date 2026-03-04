
```
sudo nano /etc/systemd/system/connector.socket
```

```

[Unit]
Description=connector socket

[Socket]
ListenStream=/run/connector.sock
SocketMode=0660
SocketUser=ular
SocketGroup=www-data

[Install]
WantedBy=sockets.target

```

sudo nano /etc/systemd/system/connector.service

```
[Unit]
Description=connector daemon
Requires=connector.socket
After=network.target

[Service]
User=ular
Group=www-data
WorkingDirectory=/home/ular/connector/connector/app
ExecStart=/home/ular/connector/venv/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/connector.sock \
          project.wsgi:application
StandardOutput=append:/var/log/connectors.log

[Install]
WantedBy=multi-user.target
```


