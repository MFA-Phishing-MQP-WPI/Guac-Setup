# Guac-Setup

## Set Up Guac and Deps
```bash
get guac to work on ec2
 - use port 80 (http) (-p 80:8080)
 - config cert for 443 (-p 443:8443)
 - expose IP to internet (allow http and https connections)

install docker

install and setup xrdp (https://stackoverflow.com/questions/78074498/how-to-configure-xrdp-to-work-with-gnome-on-ubuntu)

run the commands_setup_guacdatabase_on_ubuntu
```

<br>

<br>

## Launch Guac

```bash
docker rm guacd guacamole mysql-server

docker run --name guacd --network guac-net -e GUACD_LOG_LEVEL=debug -d guacamole/guacd
// docker run --name guacd --network host -d guacamole/guacd

docker run --name mysql-server --network guac-net -v $HOME/mysql_init:/docker-entrypoint-initdb.d -v $HOME/mysql_data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=guacamole_root -e MYSQL_USER=guacamole -e MYSQL_PASSWORD=guacamole_password -d mysql:latest

docker exec -it mysql-server /bin/bash

bash-5.1#
mysql -p
<password for root> (password="guacamole_root")
	mysql> GRANT ALL ON guacamole.* TO 'guacamole'@'%';
	mysql> exit;
mysql -u guacamole -p
<password for guacamole> (guacamole_password)
	mysql> use guacamole
	mysql> show tables;
	mysql> exit;

exit

//default
docker run --name guacamole --network guac-net -e MYSQL_DATABASE=guacamole -e MYSQL_USER=guacamole -e GUACD_HOSTNAME=172.18.0.2 -e MYSQL_HOSTNAME=mysql-server -e  MYSQL_PASSWORD=guacamole_password -d -p 8080:8080 guacamole/guacamole

// alternative (for http access)
docker run --name guacamole --network guac-net -e MYSQL_DATABASE=guacamole -e MYSQL_USER=guacamole -e GUACD_HOSTNAME=172.18.0.2 -e MYSQL_HOSTNAME=mysql-server -e  MYSQL_PASSWORD=guacamole_password -d -p 80:8080 guacamole/guacamole

//  removed:
//  --network host
//  --link guacd:guacd
//  --link mysql-server:mysql


-- TO FIND IP:
ip addr (192.168.0.153)

-- url:https://stackoverflow.com/questions/78074498/how-to-configure-xrdp-to-work-with-gnome-on-ubuntu

// guacd_port:4822
// "IPAddress": "172.18.0.2"

// to leave gui so guac can work run ...
sudo init 3
// this might be an issue for ec2 as it will boot into gui
# 4ec2 -> sudo systemctl set-default multi-user

// connect via ssh so cp+pst works (ssh jacob@192.168.0.153)
```
