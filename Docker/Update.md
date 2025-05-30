1. Build your updated version of the image, using the `docker build` command.
    
    ```console
    $ docker build -t name_of_your_image .
    ```
    you change application and get this command
    
 Remove a container using the CLI

1. Get the ID of the container by using the `docker ps` command.
    
    ```console
    $ docker ps
    ```
    
2. Use the `docker stop` command to stop the container. Replace `<the-container-id>` with the ID from `docker ps`.
    
    ```console
    $ docker stop <the-container-id>
    ```
    
3. Once the container has stopped, you can remove it by using the `docker rm` command.
    
    ```console
    $ docker rm <the-container-id>
    ```

Start the updated app container

1. Now, start your updated app using the `docker run` command.
    
    ```console
    $ docker run -dp 127.0.0.1:8000:80 name_of_your_image
    ```


1. Now, update docker compose command, if you change one or some containers within
    
    ```console
    $ docker compose up -d --build
    ```
	this command create new images for containers