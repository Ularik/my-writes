
### сертификат
openssl pkcs12 -in client.pfx -clcerts -nokeys -out client.crt

### ключ
openssl pkcs12 -in client.pfx -nocerts -nodes -out client.key

### (опционально) CA
openssl pkcs12 -in client.pfx -cacerts -nokeys -out ca.crt


1. получить ca.pfx или ca.key, ca.crt
2. Ввести команду для проверки:
```
curl https://api.example.com/secure \
  --cert-type P12 \
  --cert client.pfx:mypassword \
  --cacert ca.crt
```

После этого в Python:

```
import requests  

url = "https://api.example.com/secure"  

response = requests.get(url,cert=("client.crt", "client.key"),verify="ca.crt")

print(response.status_code) 
print(response.text)
```


### nginx

```
server {
    listen 443 ssl http2;    # ssl http2
    server_name localhost;    # ip server or domain

    ssl_certificate /home/ular/certs/server.crt;
    ssl_certificate_key /home/ular/certs/server.key;

    ssl_client_certificate /home/ular/certs/ca.crt;
    ssl_verify_client on;

    location / {
        proxy_pass http://unix:/run/connectors.sock;
        proxy_set_header Host $host;    # default headers config
        proxy_set_header X-Real-IP $remote_addr;    # default headers config
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /favicon.ico { access_log off; log_not_found off; }
    location /static/ {              # 'static/assets/css...'
        alias /home/ular/connectors/connectors/app/static/;    # your core directory
    }
    location /media/ {
        alias /home/ular/connectors/connectors/media/;
    }
```