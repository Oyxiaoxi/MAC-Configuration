# Laravel Docker



### Docker

相关文档 ： [laradock](https://docs.docker.com/install/ )

- [Mac OS 客户端](<https://hub.docker.com/editions/community/docker-ce-desktop-mac> )
- [Windows 客户端](<https://hub.docker.com/editions/community/docker-ce-desktop-windows> )
- Linux 需要选择对应的发行版，根据文档一步步安装就可以了参考 Ubuntu —[Linux](https://docs.docker.com/install/linux/docker-ce/ubuntu/) 

安装好 docker 以后，在命令行中 `docker --version` 查看版本号

```bash
docker --version
```

### Docker Compose

Docker  Compose 是一个工具，用来编排和启动多个容器，这个其实很好理解，我们需要的 Docker 容器有 
PHP、Nginx、Mysql、Redis 等等，每个容器使用的软件版本，账户密码，存储的目录等等都需要进行配置，肯定不能一个一个管理，Compose 就是这样的一个管理工具。

```bash
# 查看版本信息
docker-compose --version
```

### Laradock

[Laradock 文档](https://laradock.io/getting-started/#installation)

```bash
cd ~ && mkdir -p Code
cd Code && git clone https://github.com/laradock/laradock.git
```



#### 配置

##### env

Laradock 的环境配置文件也是 .env，你应该非常熟悉，示例文件是 `env-example`

```bash
cp env-example .env
```

注意两个配置 `APP_CODE_PATH_HOST` 和 `APP_CODE_PATH_CONTAINER`，Docker 会将本机目录的上级目录 ~/Code 目录映射到容器中的 `/var/www` 目录，这个对后面的配置比较重要。

##### nginx

```bash
cd laradock/nginx/sites/
```

![](/Volumes/WorkDate/Project/git/Configuration/images/nginx-sites.png)



目录中有四个文件，`.conf` 结尾的就是 nginx 站点的配置文件，`.example` 结尾的是给出的示例文件，`laravel.conf.example` 就是 Laravel 的示例配置文件。

```bash
cp laravel.conf.example package.conf
```

Laradock/nginx/sites/package.conf

```bash
server {

    ...

    server_name package.test;
    root /var/www/package/public;
    index index.php index.html index.htm;

	...

    error_log /var/log/nginx/package_error.log;
    access_log /var/log/nginx/package_access.log;
}
```



##### MySQL

```bash
cat laradock/.env | grep MySQL
```

这是 MySQL 相关的配置，版本是最新版本，目前是 8.0，默认的数据库是 `default`，用户名密码是 `default`/`secret`，端口是 3306，`root` 密码是 `root`。这些基础的配置可以根据自己的需要修改，不修改也是没有问题的。

``` bash
### MYSQL #################################################
MYSQL_VERSION=latest
MYSQL_DATABASE=default
MYSQL_USER=default
MYSQL_PASSWORD=secret
MYSQL_PORT=3306
MYSQL_ROOT_PASSWORD=root
MYSQL_ENTRYPOINT_INITDB=./mysql/docker-entrypoint-initdb.d
SOLR_DATAIMPORTHANDLER_MYSQL=false
```

但是会有一个问题，MySQL 8.0 用到了新的密码插件验证方式，5.7 时候是 `mysql_native_password`，8.0 变成了 `caching_sha2_password` ，这样就导致我们平时使用的 MySQL 客户端都连接不上数据库了，非常的不方便。所以可以简单的修改一下配置。

Laradock/mysql/my.cnf

```bash
[mysql]

[mysqld]
sql-mode="STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION"
character-set-server=utf8
default_authentication_plugin=mysql_native_password
```

这里将 `default_authentication_plugin` 修改为 `mysql_native_password`。可以参考一下这个 Issue <https://github.com/laradock/laradock/issues/1752> 。

##### Run

现在就可以启动了，使用 `docker-compose up` 命令启动，`-d` 参数意思是后台运行，之后的参数就是我们想启动的容器。一个基础的 PHP 环境包括了 PHP，Mysql，Nginx，Redis，启动的命令就可以是
`docker-compose up -d php-fpm nginx mysql redis`，但是任何一个 Web service 容器都依赖了 `php-fpm` ，所以可以省略 `php-fpm` 参数。

```bash
docker-compose up -d php-fpm nginx mysql redis phpmyadmin
```

##### hosts

```bash
127.0.0.1 laravel-bbs.test
127.0.0.1 package.test
```

##### phpmyadmin

<http://localhost:8080> 。 root , root 

注意项目中使用的用户是 default，所以还需要为 default 用户分配权限。

```sql
GRANT ALL ON `package`.* TO 'default'@'%'
```

或者查看 [文档](https://laradock.io/documentation/#create-multiple-databases-mysql)

```bash
cp mysql/docker-entrypoint-initdb.d/createdb.sql.example createdb.sql
```

*createdb.sql*

```sql
CREATE DATABASE IF NOT EXISTS `larabbs` COLLATE 'utf8mb4_unicode_ci' ;
GRANT ALL ON `larabbs`.* TO 'default'@'%' ;

CREATE DATABASE IF NOT EXISTS `package` COLLATE 'utf8mb4_unicode_ci' ;
GRANT ALL ON `package`.* TO 'default'@'%' ;

FLUSH PRIVILEGES ;
```

因为这个时候 mysql 容器已经启动了，相关的数据存放在了 `~/.laradock/data` 目录中，我们可以把这个目录删除，然后重新启动 mysql 容器。

```bash
rm -rf ~/.laradock/data/*
docker-compose restart mysql
```

### 更新

更新代码之后，可能需要重新 build 一下相关的容器，这里可以使用 down 命令删除所有的容器，在重新 build 一下 `php-fpm` 和 `workspace`，最后重新启动 (重新创建非常慢)

```bash
docker-compose down
docker-compose build --no-cache php-fpm workspace
docker-compose up -d nginx redis mysql phpmyadmin
```

### Redis 扩展冲突

```bash 
docker-compose exec workspace bash
php -m | grep redis
```

项目直接通过 Composer 安装了 predis 扩展包来链接 redis。所以如果你的项目安装了 predis 又同时安装了 PHP 的 redis 扩展，就会出现冲突。

```bash
cat laradock/.env | grep REDIS
```

```bash
...
WORKSPACE_INSTALL_PHPREDIS=true
...
PHP_FPM_INSTALL_PHPREDIS=false
...
```

修改了配置文件，记得要重新创建

```bash
docker-compose up -d --force-recreate --build workspace php-fpm
```

### Cron

配置 Cron 只需要修改 `workspace/crontab/laradock` 文件即可。

workspace/crontab/laradock

```bash
* * * * * laradock /usr/bin/php /var/www/package/artisan schedule:run >> /dev/null 2>&1
```

例如配置 package 项目的 Cron，因为 Cron 配置文件是在 build 的时候复制到指定目录的，所以同样需要重新 build。

``` bash
docker-compose up -d --force-recreate --build workspace
```

进入容器查看日志，确认 Cron 是否执行，日志文件在 `/var/log/cron.log`

```bash 
tail /var/log/cron.log
docker-compose logs
```

### 队列

另一个常用的容器是 `php-worker` ，容器运行了 Supervisor，帮助我们管理队列。首先需要修改一下配置，在 `php-worker/supervisord.d` 目录中。

目录中有两个文件：

- laravel-scheduler.conf.example —— 计划任务的配置示例文件；
- laravel-worker.conf.example —— 队列 worker 的示例配置文件。

![](/Volumes/WorkDate/Project/git/Configuration/images/supervisord.png)

查看一下 [文档](https://laradock.io/documentation/#run-laravel-scheduler)，在生产环境你不想启动 workspace 容器，那么可以使用 Supervisor 来配置 Cron，复制 `laravel-scheduler.conf.example` 配置文件就可以了。

另一个文件就是队列的配置文件，复制一份：

```bash
cp laravel-worker.conf.example package-worker.conf
```

package-worker.conf

```bash
[program:package-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/package/artisan queue:work --sleep=3 --tries=3 --daemon
autostart=true
autorestart=true
numprocs=8
user=laradock
redirect_stderr=true
```

启动容器

```bash
docker-compose up -d php-worker
```

### ElasticSearch

着是使用 ElasticSearch ，Laradock 也提供了容器，用的是 Elasticsearch 官方的镜像 <https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html> ，查看一下对应的 Dockerfile，版本还是 `6.2.3` ，目前最新的版本是 `6.6.0`，可以修改一下：

*elasticsearch/Dockerfile*

```bash
FROM docker.elastic.co/elasticsearch/elasticsearch:6.6.0

EXPOSE 9200 9300
```

你可以指定自己需要的版本，同时我们一般还会安装一个中文的分词插件 <https://github.com/medcl/elasticsearch-analysis-ik> ，可以继续调整 Dockerfile，插件的安装可以参考这里 <https://www.elastic.co/guide/en/elasticsearch/plugins/6.6/_other_command_line_parameters.html>。

*elasticsearch/Dockerfile*

```bash
FROM docker.elastic.co/elasticsearch/elasticsearch:6.6.0

RUN ./bin/elasticsearch-plugin install --batch https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.6.0/elasticsearch-analysis-ik-6.6.0.zip

EXPOSE 9200 9300
```

启动容器

```bash
docker-compose up -d elasticsearch
```

进入容器

```bash
docker-compose exec elasticsearch bash
bin/elasticsearch-plugin list
```

