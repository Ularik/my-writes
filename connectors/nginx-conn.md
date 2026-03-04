
```
server {
    listen 443 ssl http2;    # ssl http2
    server_name localhost;    # ip server or domain

    ssl_certificate /home/ular/certs/server.crt;
    ssl_certificate_key /home/ular/certs/server.key;

    ssl_client_certificate /home/ular/certs/ca.crt;
    ssl_verify_client on;

    location / {
        proxy_pass http://unix:/run/connector.sock;
        #proxy_pass http://127.0.0.1:8001;
        proxy_set_header Host $host;    # default headers config
        proxy_set_header X-Real-IP $remote_addr;    # default headers config
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /favicon.ico { access_log off; log_not_found off; }
    location /static/ {              # 'static/assets/css...'
        alias /home/ular/connector/connector/app/static/;    # your core directory
    }
    location /media/ {
        alias /home/ular/connector/connector/media/;
    }
}
```

sudo ln -s /etc/nginx/sites-available/connector /etc/nginx/sites-enabled


```
curl -X POST "https://10.100.191.25:5001/health" `
  --cert "/home/ular/certs/connector.crt" `
  --key "/home/ular/certs/connector.key" `
  --cacert "/home/ular/certs/ca.crt" `
  -H "Content-Type: application/json" `
  -d "{""user_name"":""admin"",""password"":""admin""}" `
  -k

```