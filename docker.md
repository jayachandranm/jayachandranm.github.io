## Useful Docker Commands

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

docker run -d --name dr7 --network=br_nw_1 -p 8080:80 drupal:7-apache

docker run -e MYSQL_ROOT_PASSWORD=[] -e MYSQL_DATABASE=dr7 -e MYSQL_USER=dr7 -e MYSQL_PASSWORD=[] -v mariadb:/var/lib/mysql --network=br_nw_1 -p3308:3306 -d --name mariadb mariadb

docker exec -it dr7 bash\s\s
docker attach dr7
iptables -t nat -L -n
docker port dr7
docker ps -l
mysql -h0.0.0.0 -P3306 -udr7 -p
docker volume ls
docker image inspect <image>
container rm $(docker container ls -a -q)         # Remove all containers
docker history <image id>
docker logs <instance id>
docker ps -a (show inactive as well)
