### Шаг 1. Правильная установка Beyla

Скопируйте и выполните эти команды по очереди. Я вшил туда актуальную и точную ссылку на версию 3.1.2:
```
# 1. Скачиваем архив с правильным именем
wget https://github.com/grafana/beyla/releases/download/v3.1.2/beyla-linux-amd64-v3.1.2.tar.gz

# 2. Распаковываем его
tar -xzvf beyla-linux-amd64-v3.1.2.tar.gz

# 3. Переносим бинарный файл в системную папку
sudo mv beyla /usr/local/bin/

# 4. Проверяем, что всё работает
beyla --version
```


### Шаг 2. Настройка Beyla как системной службы (systemd)

Чтобы Beyla работала в фоне, запускалась при старте сервера и не зависела от открытого терминала, сделаем из нее демона.

1. **Создаем конфиг:**
```
sudo nano /etc/beyla.yml

prometheus_export:
  port: 8999

discovery:
  services:
    - open_ports: 80,443,8000,8080,22 # Порты для мониторинга

network:
  enable: true
```

2. Создаем файл службы:
```
sudo nano /etc/systemd/system/beyla.service
```

Вставляем описание службы (Beyla требует прав `root` для eBPF):
```
[Unit]
Description=Grafana Beyla eBPF Auto-Instrumenter
After=network.target

[Service]
ExecStart=/usr/local/bin/beyla --config /etc/beyla.yml
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```

3. Запускаем службу
```
sudo systemctl daemon-reload 
sudo systemctl enable --now beyla 
sudo systemctl status beyla
```

## Шаг 3. Установка и подключение Prometheus

Теперь ставим сборщик метрик, который будет опрашивать нашу Beyla. На чистом Linux это делается одной командой пакетов.

1. **Установка:**
    ```
      sudo apt update && sudo apt install prometheus -y
    ```
     
2. Настройка подключения к Beyla: Открываем конфиг Prometheus:
    ```
    sudo nano /etc/prometheus/prometheus.yml
    ```
    Спускаемся в самый низ к блоку `scrape_configs` и добавляем нашу Beyla (важно соблюдать отступы):
```
scrape_configs:
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "beyla"
    static_configs:
      - targets: ["localhost:8999"]

  
  - job_name: nginx
    static_configs:
    - targets:
        - localhost
      labels:
        job: nginx
        __path__: /var/log/nginx/*.log

```

3. **Перезапускаем Prometheus:**
```
sudo systemctl restart prometheus
```

### Проверка ошибки

##### Точечная проверка ошибки

У Prometheus есть встроенная утилита, которая покажет, в какой именно строчке вы ошиблись. Выполните:
```
promtool check config /etc/prometheus/prometheus.yml
```


```
sudo systemctl restart prometheus 
sudo systemctl status prometheus
```