
 1. Create the network.
    
    ```console
    $ docker network create caps_network
    ```
2. create container with postgres and include it with network

   sudo  docker run -d \
    --network manga_network --network-alias postgres \
    -v manga-postgres-data:/var/lib/postgres \
	-e POSTGRES_DB=admin \
	-e POSTGRES_USER=ular \
    -e POSTGRES_PASSWORD=admin\
    postgres 

in django settings ypu need to change DATABASES:

ALLOWED_HOSTS = ['*']    - change this list

DATABASES = {  
    'default': {  
        'ENGINE': 'django.db.backends.postgresql',  
        'NAME': 'comix',  # Имя вашей базы данных -e POSTGRES_DB=comix  
        'USER': 'ular',  # Имя вашего пользователя  
        'PASSWORD': 'admin',  # Ваш пароль  
        'HOST': 'comix-postgres',  # Хост, имя докер контейнера с postgres --network-alias  
        'PORT': '5432',  # Порт (по умолчанию 5432)  
    }  
}

3. create image with your Dockerfile      

  sudo docker build -t <caps_shop> .
  
4. create container and include with network 
sudo docker run -p 127.0.0.1:8000:8000 -d --name <my_container> --network <caps_network> <caps_shop>

5. enter into project container
sudo docker exec -it <my_project> bash    - skip this step

6. sudo docker logs -f <name_container>



## Compose

1. create compose.yaml file in current directory.
    
    ```yaml
    services:  
  db:                               # name your database
    image: postgres:latest          # image - create container by using image
    restart: always                 # if container failed, he restarted
    environment:                    # databases settings
      POSTGRES_DB: manga-postgres  
      POSTGRES_USER: ular  
      POSTGRES_PASSWORD: admin  
    volumes:  
      - postgres_data:/var/lib/postgresql/data/  # defaults postgres volume
  
  
  web:  
    build: ./core                        # build our custom image 
    command: python manage.py migrate &&    # commands which starts after enter
             python manage.py runserver 0.0.0.0:8000  
    volumes:  
      - ./core:/app      # where will be built in container
    ports:  
      - "8000:8000"  
    depends_on:  
      - db  
    image: ularchik/manga-compose    # use whis method if you want to push
                                       #your compose image in docker push
  
volumes:  
  postgres_data:
    ```
WARNING! delete all comments in yaml file!

2. docker compose build 
3. docker compose up
4. docker compose down

### push it on docker hub
1. docker-compose build
2. docker-compose push