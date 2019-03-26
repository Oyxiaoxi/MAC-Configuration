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

![]()