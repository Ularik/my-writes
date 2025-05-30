apt install nmap - tools to view open ports in servers
nmap google.com
ifconfig - view your ip address
telnet <'python.org'> <'80'> - connecting to port 80
nslookup <'python.org'> - view all servers with this name <'python.org'>
cat /etc/passwd - check all users which open some programs
tail -F /var/log/nginx/access.log - check all proccess in our project

## gunicorn
pip install gunicorn
 gunicorn --bind 0.0.0.0:8000 ==myproject==.wsgi - test working
 sudo vim /etc/systemd/system/cert.service - create conf file:
	
```

[Unit]
Description=cert-gov-kg daemon
Requires=cert.socket
After=network.target

[Service]
User=ular
Group=ular
WorkingDirectory=/home/ular/certular/project
ExecStart=/home/ular/venv/bin/gunicorn --workers 9 --bind unix:/run/cert.sock project.wsgi:application
StandardOutput=append:/var/log/cert.log

[Install]
WantedBy=multi-user.target
```

1. sudo systemctl start gunicorn.socket
2. sudo systemctl enable gunicorn.socket

 

