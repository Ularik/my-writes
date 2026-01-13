sudo nano /etc/nginx/sites-available/fishing - core is name of your file configuration:

```
server {
    listen 80;    # http port
    server_name 10.100.191.13;    # ip server or domain    

	location / {
        proxy_pass http://unix:/run/fishing.sock;  
        proxy_set_header Host $host;    # default headers config
        proxy_set_header X-Real-IP $remote_addr;    # default headers config
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /favicon.ico { access_log off; log_not_found off; }
    location /static/ {              # 'static/assets/css...'
        alias /home/kuba/fish/fishing/app/static/;    # your 
    }
    location /media/ {
        alias /home/kuba/fish/fishing/media;
    }

	location /socket {
		include proxy_params;
		proxy_pass http://unix:/run/fishing.sock;
	}
}

```
sudo nano /etc/nginx/nginx.conf:
```
user ular    # change user
...
http {
	#...
        client_max_body_size 800m;    # max size of media file
	#...
}
```

```
sudo ln -s /etc/nginx/sites-available/fishing /etc/nginx/sites-enabled
sudo nginx -t
sudo systemctl restart nginx
sudo ufw delete allow 8000
sudo ufw allow 'Nginx Full'
```

```
server {
    listen 80;    # http port
    server_name 10.100.191.8;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        alias /home/ular/certular/static/;
    }
    location /media/ {
        alias /home/ular/certular/media/;
    }

    location / {
        proxy_pass http://unix:/run/cert.sock;
        proxy_set_header Host $host;    # default headers config
        proxy_set_header X-Real-IP $remote_addr;    # default headers config
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /socket {
        include proxy_params;
        proxy_pass http://unix:/run/cert.sock;
    }
}

```

### Проверка домена или ip
```
curl -v -L -H "Host: 10.100.191.8" -H "User-Agent: Mozilla/5.0" http://10.100.191.8
tail -f /var/log/nginx/access.log
```