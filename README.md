# jeedom-docker-raspberry

Installs a jeedom + mysql inside a docker

```
sudo mkdir -p /opt/jeedom/mysql
sudo mkdir -p /opt/jeedom/html
sudo apt install docker docker-compose
```

## use compose
```
docker-compose up
```

## Alternatively:

```
# create a mysql instance
docker run --name jeedom-mysql -v /opt/jeedom/mysql/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=<your_passwd> -e MYSQL_USER=jeedom -e MYSQL_PASSWORD=<your_passwd> --detach --publish 3306:3306 --restart always hypriot/rpi-mysql

# create the jeedom instance
docker create --net host --name jeedom-server \
--cap-add=NET_ADMIN --device /dev/net/tun:/dev/net/tun  # for openvpn\
-v /opt/jeedom/html:/var/www/html -e ROOT_PASSWORD=<your_passwd> -e APACHE_PORT=80 -e SSH_PORT=22 -e MODE_HOST=1 --restart always nricheton/jeedom-rpi

docker start jeedom-server-tst
```
