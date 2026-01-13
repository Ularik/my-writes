
```
    map $http_origin $cors_origin {
        default "";
        "~^https?://localhost:5173$"        $http_origin; # указываем свой домен фронта
    }

    upstream orch_upstream {
        server 192.168.0.109:7200;   # ip бэкенда куда будет проксироваться
    }

    server {
        listen 8443 ssl;    # порт на который идут запросы от фронта 
        http2 on;
        server_name 192.168.0.101 localhost;   # ip на котором работает сам фронт

        ssl_certificate     C:/Users/SAMURAI/Downloads/front-certs/front.crt;
        ssl_certificate_key C:/Users/SAMURAI/Downloads/front-certs/front.key;

        # mTLS с клиента тебе сейчас не нужен — оставляем off
        ssl_client_certificate C:/Users/SAMURAI/Downloads/front-certs/ca.crt;
        ssl_verify_client off;

        location / {
            # --- Preflight (OPTIONS) ---
            if ($request_method = OPTIONS) {
                add_header Access-Control-Allow-Origin      "$cors_origin" always;
                add_header Access-Control-Allow-Methods     "GET, POST, PUT, PATCH, DELETE, OPTIONS" always;
                add_header Access-Control-Allow-Headers     "Authorization, Content-Type, Accept" always;
                add_header Access-Control-Allow-Credentials "true" always;   # если не используешь cookies, можно убрать
                add_header Content-Length 0;
                add_header Content-Type   text/plain;
                return 204;
            }

            # --- Проксирование к бэкенду ---
            proxy_pass https://orch_upstream;   # указывали сверху
            proxy_ssl_server_name on;
            proxy_ssl_name back.local;  # указываем тот что при создании сертификата для бэка 
  
            proxy_set_header Host               $host;
            proxy_set_header X-Real-IP          $remote_addr;
            proxy_set_header X-Forwarded-For    $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto  $scheme;

            proxy_ssl_certificate         C:/Users/SAMURAI/Downloads/front-certs/front.crt;
            proxy_ssl_certificate_key     C:/Users/SAMURAI/Downloads/front-certs/front.key;
            proxy_ssl_trusted_certificate C:/Users/SAMURAI/Downloads/front-certs/ca.crt;
            proxy_ssl_verify on;
            proxy_ssl_verify_depth 2;
        }
    }
```