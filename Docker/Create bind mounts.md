
1. Open a terminal and change directory to the `your_image_directory` directory.
    
2. Run the following command to start `bash` in an `ubuntu` container with a bind mount.
    
    Mac / Linux / PowerShell Command Prompt Git Bash
    
    ---
    
    ```console
    $ docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash
    ```
    
    ---
    
    The `--mount type=bind` option tells Docker to create a bind mount, where `src` is the current working directory on your host machine (`getting-started-app`), and `target` is where that directory should appear inside the container (`/src`).
    
3. After running the command, Docker starts an interactive `bash` session in the root directory of the container's filesystem.
    
    ```console
    root@ac1237fad8db:/# pwd
    /
    root@ac1237fad8db:/# ls
    bin   dev  home  media  opt   root  sbin  srv  tmp  var
    boot  etc  lib   mnt    proc  run   src   sys  usr
    ```
    
4. Change directory to the `src` directory.
    
    This is the directory that you mounted when starting the container. Listing the contents of this directory displays the same files as in the `getting-started-app` directory on your host machine.
    
    ```console
    root@ac1237fad8db:/# cd src
    root@ac1237fad8db:/src# ls
    Dockerfile  node_modules  package.json  spec  src  yarn.lock
    ```
    
5. Create a new file named `myfile.txt`.
    
    ```console
    root@ac1237fad8db:/src# touch myfile.txt
    root@ac1237fad8db:/src# ls
    Dockerfile  myfile.txt  node_modules  package.json  spec  src  yarn.lock
    ```
    
6. Open the `getting-started-app` directory on the host and observe that the `myfile.txt` file is in the directory.
    
    ```text
    ├── getting-started-app/
    │ ├── Dockerfile
    │ ├── myfile.txt
    │ ├── node_modules/
    │ ├── package.json
    │ ├── spec/
    │ ├── src/
    │ └── yarn.lock
    ```
    
7. From the host, delete the `myfile.txt` file.
    
8. In the container, list the contents of the `app` directory once more. Observe that the file is now gone.
    
    ```console
    root@ac1237fad8db:/src# ls
    Dockerfile  node_modules  package.json  spec  src  yarn.lock
    ```
    
9. Stop the interactive container session with `Ctrl` + `D`.


1. Make sure you don't have any `getting-started` containers currently running.
    
2. Run the following command from the `getting-started-app` directory.
    
    ```console
    $ docker run -dp 127.0.0.1:3000:3000 \
        -w /app --mount type=bind,src="$(pwd)",target=/app \
        node:18-alpine \
        sh -c "yarn install && yarn run dev"
    ```
    
    The following is a breakdown of the command:
    
    - `-dp 127.0.0.1:3000:3000` - same as before. Run in detached (background) mode and create a port mapping
    - `-w /app` - sets the "working directory" or the current directory that the command will run from
    - `--mount type=bind,src="$(pwd)",target=/app` - bind mount the current directory from the host into the `/app` directory in the container
    - `node:18-alpine` - the image to use. Note that this is the base image for your app from the Dockerfile
    - `sh -c "yarn install && yarn run dev"` - the command. You're starting a shell using `sh` (alpine doesn't have `bash`) and running `yarn install` to install packages and then running `yarn run dev` to start the development server. If you look in the `package.json`, you'll see that the `dev` script starts `nodemon`.
3. You can watch the logs using `docker logs <container-id>`. You'll know you're ready to go when you see this:
    
    ```console
    $ docker logs -f <container-id>
    nodemon -L src/index.js
    [nodemon] 2.0.20
    [nodemon] to restart at any time, enter `rs`
    [nodemon] watching path(s): *.*
    [nodemon] watching extensions: js,mjs,json
    [nodemon] starting `node src/index.js`
    Using sqlite database at /etc/todos/todo.db
    Listening on port 3000
    ```
    
    When you're done watching the logs, exit out by hitting `Ctrl`+`C`.
    

---

### [Develop your app with the development container](https://docs.docker.com/get-started/06_bind_mounts/#develop-your-app-with-the-development-container)

Update your app on your host machine and see the changes reflected in the container.