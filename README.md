## (1) Docker version
**Docker:** 20.10.21<br>
**Docker Compose:** 2.12.2 


## (2) Docker files
The most important 3 docker files are:
```
- Dockerfile
- docker-compose.yml
- docker-compose/nginx/backend.conf
```
**Dockerfile:**<br>
installs system dependencies, php extensions, composer, sets user/group permissions

**docker-compose.yml:**<br>
*service* names and *container_name* need to be unique if working with more than one project on the same system

**docker-compose/nginx/backend.conf:**<br>
make sure *fastcgi_pass  app-container-name*<br>
where *app-container-name* is the name of the container app: backend
```
location ~ \.php$ {
    try_files $uri =404;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass backend:9000;
    fastcgi_index index.php;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param PATH_INFO $fastcgi_path_info;
}
```


Make sure to assign **ports** values accordingly if running more than one docker project so they don't try to use the same port:
```
ports:
    - "8001:80"
```
Each Docker project should use a different port, 8000, 8001, 8002 ,.. etc


## (3) .env DB configuration
Your DB_HOST needs to be the container name given in docker-compose.yml
```
DB_CONNECTION=mysql
DB_HOST=your-db-container-name
```

## (4) Build/Run Docker Container
(a) Build Container:
```
docker compose build
```
(b) Run Container:
```
docker compose up -d
```

## (5) Start/Stop/List Containers

List running containers only:
```
docker ps
```

List of all containers :
```
docker ps -a
```
Start Containers in current directory:
```
docker compose start
```
Stop Containers in current directory:
```
docker compose stop
```


## (6) Container operations
Enter container (bash, where *container-name* is the name of your container
```
docker exec -it container-name bash
```
Enter container mysql, this is the same as using regular mysql commands: mysql -u user -p which will prompt for a mysql user password
```
docker exec -it db-container-name mysql -u db-user -p
```

## (7) Project URL
Containers should now be running after running step (4):
```
http://localhost:8001
```