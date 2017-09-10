# easy-start-with-laravel
Start your Laravel project during 30 minutes

After passing all steps of this tutorial you will have environment for developing PHP projects that includes: PHP, NGINX, MySQL and Redis. 

You will know how to start to use Laravel and its tools.

Required tools:
* Git https://git-scm.com/downloads
* Docker https://www.docker.com/ (Windows: https://www.docker.com/docker-windows, Mac: https://www.docker.com/docker-mac)
* PHPStorm https://www.jetbrains.com/phpstorm/download/

Please download and install them.

Clone this tutorial to you PC.
1) go to a folder where are you going to test this tutorial
2) clone the tutorial

```
cd ~
git clone https://github.com/ADiesel82/easy-start-with-laravel.git tutorial 
```
If you are windows user or installed this tutorial into another path, please change *~/tutorial* in the docker/docker-compose.yml config to your path 

*~* - home path

Let's install NGINX, Redis, MySQL and PHP with docker:
```
cd docker
docker-compose up -d
```

*"docker-compose up -d"* - runs docker containers in the background

For more information: https://docs.docker.com/compose/reference/overview/

To be sure that everything has been done correctly run **docker ps** and verify that **nginx, php-fpm, mysql, redis**
```
$docker ps

CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                                      NAMES
d5090c95537d        nginx                "nginx -g 'daemon ..."   About an hour ago   Up About an hour    0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   docker_nginx_1
64264e159f1a        lyberteam/php7-fpm   "php-fpm"                About an hour ago   Up About an hour    9000/tcp, 0.0.0.0:32793->7000/tcp          docker_fpm_1
fbd95a07c6de        mysql                "docker-entrypoint..."   10 hours ago        Up 2 hours          0.0.0.0:3306->3306/tcp                     docker_mysql_1
dbf082154d2d        redis                "docker-entrypoint..."   2 months ago        Up 2 hours          0.0.0.0:6379->6379/tcp                     docker_redis_1
```
Let's try to enter into docker container:
```
docker exec -it docker_fpm_1 bash
```
To understand exec parameters: https://docs.docker.com/engine/reference/commandline/exec/

*docker_fpm_1* you can find with using *docker ps* in the column *NAMES*. 

As result you will be logged into into fpm container as root.

Let's install Laravel with using composer. From instructions https://laravel.com/docs/5.5/installation#configuration let's do next:
```
composer create-project --prefer-dist laravel/laravel blog
```
Laravel uses *.evn* file as base configuration file in its application root. Let's create and fill it with ours environment variables.

Laravel has .evn file example, so let's use it:
```
# cd blog/
# cp .env.example .env
```  
Please open it for editing and do next:
* Set MySQL settings
```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=tutorial
DB_USERNAME=root
DB_PASSWORD=root
```
*DB_HOST=mysql* all services have own IPs and you can use service name instead of IP inside the docker. This means that you can simply write *mysql* instead of its IP. For example you can try command __ping nginx__ to get IP of the nginx container.
*DB_PASSWORD=root* this password has been set in docker/docker-compose.yml file: __MYSQL_ROOT_PASSWORD: root__.
For more info you can take a look docker image description: https://hub.docker.com/_/mysql/

To connect to mysql from your PC (outside the docker) you can use __localhost__ and the same password.

* Redis in the similar way
```
REDIS_HOST=redis  
```
Save these changes and let's try to use Laravel CLI tool __artisan__ to generate __APP_KEY__ for the .evn. To do this, please, run:
~~~
php artisan key:generate
~~~ 
That is all. You can see this key inside the .evn file.

To see all commands just run __php artisan__ and read more here: https://laravel.com/docs/5.5/artisan

Please go away from the fpm container with __exit__ command or typing __Ctrl+D__.

Let's use another command to create MySQL database:
~~~
docker exec -t docker_mysql_1 mysql -u root -proot -e "CREATE DATABASE tutorial"
~~~

Here you are. Everything is ready for use. Try to open in a browser: http://localhost/

Nginx config: docker/conf/nginx/conf.d/tutorial.conf