```

services:  
  db-postgres:  
    image: postgres:15-alpine  
    restart: always  
    environment:  
      POSTGRES_DB: ${POSTGRES_DB}  
      POSTGRES_USER: ${POSTGRES_USER}  
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}  
    volumes:  
      - postgres_data:/var/lib/postgresql/data  
    expose:  
      - "5432"  
  
  # Наш Django проект  
  django-app:  
    image: ularchik/connector-ts  
    env_file:  
      - .env  
    depends_on:  
      - db-postgres  
    command: >  
      sh -c "python manage.py migrate &&  
      python manage.py collectstatic --noinput &&  
      gunicorn --bind 0.0.0.0:8000 --workers 4 --access-logfile - project.wsgi:application"  
    expose:  
      - "8000"  # Открываем порт только внутри сети Docker  
    restart: always  
  
  nginx:  
    image: nginx:latest  
    ports:  
      - "443:443"  
    volumes:  
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro  
      - ./certs:/etc/nginx/certs:ro  
      - static_content:/var/www/static/:ro  # Nginx только читает отсюда  
    depends_on:  
      - django-app  
    restart: always  
  
volumes:  
  postgres_data:  
  static_content:
```

```
docker compose up -d    -фоновый режим
docker compose up -d --build   -пересборка при изменении dockerFIle
docker compose down     -остановка и удаление контейнеров
docker compose down -v  -Остановить и удалить вместе с данными (Volume) 
```

### При изменении Nginx файла: 
Файл обновился, но Nginx об этом не знает. Нужно просто перечитать конфиг внутри работающего контейнера: 
`docker compose exec nginx nginx -s reload`


### Обновление image

### Способ 1: Обновить всё и сразу (Рекомендуемый)

Это самый надежный способ обновить проект до последних версий из облака:

1. **Стянуть новые слои:** `docker compose pull`
    
2. **Пересоздать контейнеры на базе новых образов:** `docker compose up -d`