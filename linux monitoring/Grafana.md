
## Шаг 1. Установка Grafana

Для чистого Linux (Ubuntu/Debian) выполните эти команды:

```
# 1. Добавляем ключ репозитория Grafana
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null

# 2. Добавляем сам репозиторий
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list

# 3. Устанавливаем и запускаем
sudo apt update && sudo apt install grafana -y
sudo systemctl enable --now grafana-server
```

---

## Шаг 2. Вход и настройка

1. Откройте в браузере: `http://IP_ВАШЕГО_СЕРВЕРА:3000`.
2. Логин/пароль по умолчанию: **admin / admin**.
3. При первом входе система попросит сменить пароль — придумайте что-то надежное.
#### Подключаем Prometheus:

1. В левом меню выберите **Connections** -> **Data Sources**.
2. Нажмите **Add data source** и выберите **Prometheus**.
3. В поле **Connection** укажите: `http://localhost:9090`.
4. Прокрутите вниз и нажмите **Save & Test**. Если появилась зеленая галочка — связь установлена!

---

## Шаг 3. Импорт готового дашборда Beyla

Вам не нужно рисовать графики вручную. Разработчики Beyla уже сделали это за вас.

1. В левом меню нажмите на значок **Dashboards**.
2. Нажмите синюю кнопку **New** -> **Import**.
3. В поле **Import via grafana.com** введите ID: **19923** и нажмите **Load**.
4. Внизу в выпадающем списке выберите ваш источник данных **Prometheus**.
5. Нажмите **Import**.

## Что вы теперь увидите?

Теперь на одном экране у вас будут:
- **Rate (RPS):** сколько запросов в секунду прилетает на каждый порт.
- **Errors:** сколько запросов упало с ошибкой (5xx или 4xx).
- **Duration (P99):** время ответа вашего приложения (вы сразу увидите, если база данных начала тормозить).
- **Topology:** (в некоторых версиях) карта того, кто к кому обращается.

---

## Один важный совет (Про безопасность)

Теперь у вас открыто три порта: **9090** (Prometheus), **8999** (Beyla) и **3000** (Grafana). Если ваш сервер смотрит в интернет, **обязательно** закройте порты 9090 и 8999 фаерволом (`ufw`), оставив доступ только к 3000 (Grafana).

```
sudo ufw allow 3000/tcp
sudo ufw deny 9090/tcp
sudo ufw deny 8999/tcp
```

_Prometheus и Grafana всё равно будут общаться между собой через `localhost`._