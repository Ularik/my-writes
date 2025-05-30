docker images - list of docker images in your ubuntu
docker container rm <name_container> - remove container
docker ps - list of containers which running now
docker ps -a - list containers which were running few moments earlier 
docker run 

docker run -it busybox - "it" - include with terminal output
docker run -d nginx -      "d" - start prossec in backsite
docker container prune - delete all stoped containers
docker container inspect <ip_container> | grep IPAddress - grep - filter 
docker exec -it nice_faradey bash    - exec - start additional prosec in container

in the terminal (for example bash): " hostname -i "    # select ip address

docker container prune    - delete all stoped containers