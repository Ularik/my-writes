- docker network create caps_network
- sudo  docker run -d \    <!-- create postgres container and tie with network-->
    --network comix-network --network-alias postgres \   <!-- HOST_name:-->
    -v comix-data:/var/lib/postgres \
	-e POSTGRES_DB=comix \    <!-- NAME:-->
	-e POSTGRES_USER=ular \
    -e POSTGRES_PASSWORD=admin\
    postgres 


Django.settings:
	- CACHES = {  
	    "default": {  
	        "BACKEND": "django.core.cache.backends.redis.RedisCache",  
	        "LOCATION": "==redis://redis-container:6379/1==",  # Используем имя контейнера Redis  
	    }  
	}


- sudo docker build -t <'name-image'> .
<!--create redis container-->
- sudo docker run -d --name <'redis-container'> --network <'comix-network'> redis   
-  sudo docker run -p 127.0.0.1:8000:8000 --name comix-container --network comix-network --link redis-container comix-image
- sudo docker exec -it <my_project> bash 
- python manage.py migrate


## compose.yaml

- create compose.yaml
```
services:  
  
  comix-postgres:                               # name your database host  
    image: postgres:latest          # image - create container by using image  
    restart: always                 # if container failed, he restarted  
    environment:                    # databases settings  
      POSTGRES_DB: comix  
      POSTGRES_USER: ular  
      POSTGRES_PASSWORD: admin  
    volumes:  
      - postgres_data:/var/lib/postgres/data/  # defaults postgres volume  
  
  
  redis-comix:  
    image: redis:latest  
  
  
  
  comix-container:  
    build: .  
    command: bash -c "python /comix/core/manage.py migrate &&   
             python /comix/core/manage.py runserver 0.0.0.0:8000"  
    working_dir: /comix/core  
    volumes:  
      - .:/comix/core  
    ports:  
      - 8000:8000  
    depends_on:  
      - comix-postgres  
      - redis-comix  
  
  
volumes:  
  postgres_data:


```

- create Dockerfile:
```	
FROM python:3.10.12  
ENV PYTHONUNBUFFERED=1  
WORKDIR /comix/core  
COPY requirements.txt /comix/core  
RUN pip install -r /comix/core/requirements.txt  
COPY . /comix/core  
EXPOSE 8000```
```

- docker compose build 
-  docker compose up
-  docker compose down