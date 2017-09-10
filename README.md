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
mkdir -p tutorial/php
cd tutorial/php
git clone https://github.com/ADiesel82/easy-start-with-laravel.git tutorial 
```

Let's install NGINX, Redis, MySQL and PHP with docker:
```
cd docker
docker-compose up -d
```

*"docker-compose up -d"* - runs docker containers in the background

For more information: https://docs.docker.com/compose/reference/overview/

  