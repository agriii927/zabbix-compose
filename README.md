# Zabbix-compose
Docker compose file for quick installation zabbix system monitoring (zabbix-server/mysql/nginx) based on linux alpine images.

## What's Zabbix?
Zabbix is a mature and effortless enterprise-class open source monitoring solution for network monitoring and application monitoring of millions of metrics.
![изображение](https://user-images.githubusercontent.com/67045661/164970908-7a0635b9-41d3-4f41-935b-f372684a503d.png)


## How to deploy & verify it works:
1. Run docker compose up -d command:
```bash
docker-compose -f docker-compose.yaml up -d 
```
(This works also without -f parameter if the name is docker-compose.yaml)

2. Wait until all the containers will be running in terminal/command line.
```bash
Creating network "zabbix-net" with the default driver
Creating mysql-server           ... done
Creating zabbix-server-mysql    ... done
Creating agent                  ... done
Creating zabbix-web-nginx-mysql ... done
```
3. Check whether all containers are up and healthy:
```bash
docker ps -a

CONTAINER ID   IMAGE                                             COMMAND                  CREATED          STATUS                    PORTS                                             NAMES
293945ad7076   zabbix/zabbix-web-nginx-mysql:alpine-5.4-latest   "docker-entrypoint.sh"   29 minutes ago   Up 29 minutes (healthy)   8443/tcp, 0.0.0.0:80->8080/tcp, :::80->8080/tcp   zabbix-web-nginx-mysql
5683a5f0c8af   zabbix/zabbix-agent:alpine-latest                 "/sbin/tini -- /usr/…"   29 minutes ago   Up 29 minutes             0.0.0.0:10050->10050/tcp, :::10050->10050/tcp     agent
a20f16505a61   zabbix/zabbix-server-mysql:alpine-5.4-latest      "/sbin/tini -- /usr/…"   29 minutes ago   Up 29 minutes             0.0.0.0:10051->10051/tcp, :::10051->10051/tcp     zabbix-server-mysql
32ffdbd8ac09   mysql:8.0                                         "docker-entrypoint.s…"   29 minutes ago   Up 29 minutes (healthy)   3306/tcp, 33060/tcp                               mysql-server
```
4. Go to http://0.0.0.0:80 address to check Zabbix web interface.
5. Wait until MySQL DB will be up. There could be some DB errors like "Unable to determine database" or "Unable to select configuration", it will be resolved in quick time.
6. Go to Configuration -> Hosts page in Zabbix interface and change agent interface from IP address to DNS name "agent".
7. Wait until the availability parameter of your host will change its status to "Available", from red indicator to green.

## How to destroy:
Run docker compose down & wait until all the containers and networks are removed
```bash
docker-compose -f docker-compose.yaml down

Stopping zabbix-web-nginx-mysql ... done
Stopping agent                  ... done
Stopping zabbix-server-mysql    ... done
Stopping mysql-server           ... done
Removing zabbix-web-nginx-mysql ... done
Removing agent                  ... done
Removing zabbix-server-mysql    ... done
Removing mysql-server           ... done
Removing network zabbix-net
```
## Variables
      - MYSQL_PASSWORD=zabbix_pwd
      - MYSQL_USER=zabbix
      - MYSQL_DATABASE=zabbix
      - MYSQL_ROOT_PASSWORD=root_pwd
      - DB_SERVER_HOST=mysql-server
      - ZBX_SERVER_HOST=zabbix-server-mysql
      

That's all, you're done!



