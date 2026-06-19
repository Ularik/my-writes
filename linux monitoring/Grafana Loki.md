
Для этого нам понадобятся две программы:
1. **Loki** — сама база данных для хранения логов.
2. **Promtail** — агент, который «читает» файлы логов (например, `/var/log/nginx/access.log`) и отправляет их в Loki.
---
## 1. Установка Loki и Promtail

На «голом» Linux проще всего скачать бинарные файлы. Выполните эти команды:
```
# Создаем папки
sudo mkdir -p /etc/loki /etc/promtail /tmp/loki

# Скачиваем Loki
wget https://github.com/grafana/loki/releases/download/v3.0.0/loki-linux-amd64.zip
unzip loki-linux-amd64.zip
sudo mv loki-linux-amd64 /usr/local/bin/loki

# Скачиваем Promtail
wget https://github.com/grafana/loki/releases/download/v3.0.0/promtail-linux-amd64.zip
unzip promtail-linux-amd64.zip
sudo mv promtail-linux-amd64 /usr/local/bin/promtail
```

_(Если `unzip` не установлен: `sudo apt install unzip -y`)_

---

## 2. Настройка конфигурации

#### Для Loki (хранилище):

Создайте файл `sudo nano /etc/loki/loki-config.yml`:

```
auth_enabled: false
server:
  http_listen_port: 3100
common:
  instance_addr: 127.0.0.1
  path_prefix: /tmp/loki
  storage:
    filesystem:
      chunks_directory: /tmp/loki/chunks
      rules_directory: /tmp/loki/rules
  replication_factor: 1
  ring:
    kvstore:
      store: inmemory
schema_config:
  configs:
    - from: 2024-04-01
      store: tsdb
      object_store: filesystem
      schema: v13
      index:
        prefix: index_
        period: 24h
```

#### Для Promtail (сборщик):

Создайте файл `sudo nano /etc/promtail/promtail-config.yml`. Здесь мы укажем ему читать логи вашего Nginx:

```
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://localhost:3100/loki/api/v1/push

scrape_configs:
- job_name: system_logs
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*.log  # Читать все логи из этой папки

- job_name: nginx_logs
  static_configs:
  - targets:
      - localhost
    labels:
      job: nginx
      __path__: /var/log/nginx/*.log # Читать логи Nginx
```
---
## 3. Запуск как системных служб

Создайте два файла для `systemd`, чтобы всё работало в фоне.

**Для Loki:** 
`sudo nano /etc/systemd/system/loki.service`

Ini, TOML

```
[Unit]
Description=Loki service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/loki -config.file=/etc/loki/loki-config.yml
Restart=always

[Install]
WantedBy=multi-user.target
```

**Для Promtail:** `sudo nano /etc/systemd/system/promtail.service`

Ini, TOML

```
[Unit]
Description=Promtail service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/promtail -config.file=/etc/promtail/promtail-config.yml
Restart=always

[Install]
WantedBy=multi-user.target
```

**Запускаем:**

Bash

```
sudo systemctl daemon-reload
sudo systemctl enable --now loki promtail
```

---

## 4. Подключение в Grafana

1. Зайдите в Grafana (`порт 3000`).
2. **Connections** -> **Data Sources** -> **Add data source**.
3. Выберите **Loki**.
4. В поле URL введите: `http://localhost:3100`.
5. Нажмите **Save & Test**.

## 5. Как увидеть сами логи?

1. Нажмите на иконку компаса (**Explore**) в левом меню.
2. Сверху выберите **Loki** вместо Prometheus.
3. Нажмите на кнопку **Label browser** и выберите `job` -> `nginx` (или просто введите в строке `{job="nginx"}`).
4. Нажмите **Run query**.