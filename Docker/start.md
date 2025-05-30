1. touch Dockerfile''' - create docker file

2. Using a text editor or code editor, add the following contents to the Dockerfile:
    
    ```dockerfile

FROM python:3.10.12
ENV PYTHONUNBUFFERED=1
WORKDIR /app            # create a directory 'app' in container
COPY requirements.txt /app
RUN pip install -r /app/requirements.txt
COPY . /app             # copy all in 'app' from '.'
EXPOSE 8000  
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
    ```
    Build the image.

```console
$ docker build -t name_of_your_image .
```
Run your container using the `docker run` command and specify the name of the image you just created:

```console
$ docker run -dp 127.0.0.1:8000:80 name_of_your_image
```


Run the following `docker ps` command in a terminal to list your containers.

```console
$ docker ps
```

