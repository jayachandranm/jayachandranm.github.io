----

## On this page
{:.no_toc}

- TOC
{:toc}

----


* TOC
{:toc}


## Useful Docker Commands

Quick listing of common docker commands.

### General

docker run -d --name dr7 --network=br_nw_1 -p 8080:80 drupal:7-apache  
(if the application runs on 127.0.0.1 it wont respond to request from outside docker, use 0.0.0.0 instead.)  
docker run -e MYSQL_ROOT_PASSWORD=[] -e MYSQL_DATABASE=dr7 -e MYSQL_USER=dr7 -e MYSQL_PASSWORD=[] -v mariadb:/var/lib/mysql --network=br_nw_1 -p3308:3306 -d --name mariadb mariadb  
docker exec -it dr7 bash  
docker exec -it --user root dr7 bash  
docker attach dr7  
iptables -t nat -L -n  
docker port dr7  
docker ps -l  
mysql -h0.0.0.0 -P3306 -udr7 -p  
docker volume ls  
docker image inspect <image>  
container rm $(docker container ls -a -q)  
docker history <image id>  
docker logs <instance id>  
docker ps -a (show inactive as well)  
### Swarm
docker swarm init  
docker service ls  
docker service ps <service>  
docker container ls -q  
docker container prune  
docker stack deploy -c docker-compose.yml getstartedlab  # creates overlay n/w.  
docker stack deploy -c docker-compose_comfortapp.yml comfortapp  
docker stack rm getstartedlab  
docker node ls  
docker swarm leave --force  
### Swarm on AWS
Connect to AWS, with right policies.  
Create new Swarm, specify manager, worker.  
Wait for resource creation. Click and get the terminal command to connect from console (dockercloud client).  
Execute command and export DOCKER_HOST. This won't set the env for console.  
sudo DOCKER_HOST=tcp://127.0.0.1:32769 docker stack deploy -c docker-compose.yml dr7web  
port mode: host for publishing a host port on each node, or ingress for a swarm mode port which will be load balanced.  
### Others
docker inspect --format='{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <app>  
 CTRL-p CTRL-q.  
docker network prune  
docker system prune (containers, images, network, volumes)  
brctl show docker0  
docker network disconnect -f  

nettool docker image  

docker build --no-cache -t my_image .  

docker image prune -a  
docker tag dr7nav {username}/dr7nav:1.0  
docker push {username}/dr7nav:1.0  
docker run -p 4000:80 username/repository:tag  
