1. Будет один внешний ip для сервера с обратным прокси
2. Покупаем домен для этого ip 'example.com'
3. Теперь у сервера есть выход к интернету по этому домену
4. Теперь про сам reverse proxy:

### Установка nginx
```

```

### Настройка файла конфигурации /etc/nginx/conf.d/default.conf
```
sudo nano /etc/nginx/conf.d/default.conf
```

Внутрь прописываем:
```
server {
    listen 80;   # 1
    server_name site1.local;   # 2
    location / {
        proxy_pass http://10.100.191.8:80;  # 3
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

}

```
1. listen 80;   # http port который будет прослушиваться reverse proxy сервером
2. server_name site1.local;   # поддомен который мы зарегистрировали когда покупали домен у DNS
3. proxy_pass http://10.100.191.7:80;   # это путь на который будет направляться запрос, его нет в интернете, он локально доступен для прокси сервера в одной подсети
4. proxy_set_header Host $host; # транслирует название Host (Например site1.local на адресс proxy_pass)
Можем создать второй файл (Например: "/etc/nginx/conf.d/site2.conf") или там же добавить блок:
```

server {
    listen 80;
    server_name site2.local;

    location / {
        proxy_pass http://10.100.191.7:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

```

### Теперь нужно настроить nginx на адресах:   http://10.100.191.8:80; и http://10.100.191.7:80;

```
server {
    listen 80;    # http port
    server_name site1.local;  # пишем название домена, который будет передаваться через Host: site1.local в нашем случае

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        alias /home/ular/certular/static/;
    }
    location /media/ {
        alias /home/ular/certular/media/;
    }

    location / {
        proxy_pass http://unix:/run/cert.sock;
        proxy_set_header Host $host;    # теперь Host: site1.local будет транслироваться на бэк django
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

```
server {
    listen 80;    # http port
    server_name site2.local;  # пишем название домена, который будет передаваться через Host: site2.local в нашем случае

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        alias /home/ular/CTI/oneappdatabase/static/;
    }
    location /media/ {
        alias /home/ular/CTI/oneappdatabase/media/;
    }

    location / {
        proxy_pass http://unix:/run/cti.sock;
        proxy_set_header Host $host$;    # теперь Host: site2.locl будет транслироваться на бэк django
        proxy_set_header X-Real-IP $remote_addr;    # default headers config
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /socket {
        include proxy_params;
        proxy_pass http://unix:/run/cti.sock;
    }
}
```

### В настройках django settings.py

добавляем site1.local:
```
ALLOWED_HOSTS = ['site1.local', '10.100.191.8', '127.0.0.1', 'localhost']
```
после чего перезапускаем gunicorn
```
sudo systemctl daemon-reload
sudo systemctl restart cert.service cert.socket
```
### Команды для проверки и перезапуска nginx
```
sudo nginx -s reload
curl -H "Host: site2.local" http://10.100.191.7 отправляет запрос на сервер под хостом site2.local
curl -v http://site2.local  # просто отправляет запрос на домен sit2.local

```



server {
    listen 443;    # http port
    server_name elk.cert.gov.kg;  

    location / {
        proxy_pass http://localhost:5601;
        proxy_set_header Host $host;    # теперь Host: site1.local будет транслироваться на бэк django
        proxy_set_header X-Real-IP $remote_addr;    # default headers config
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

