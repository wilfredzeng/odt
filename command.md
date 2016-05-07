# 课程所用到的命令

## 一、Docker 基础

安装curl命令

`which curl`

`apt-get install -y curl`

`yum -y install curl`

检查内核版本

`uname -r`

安装Docker

`curl -sSL https://get.docker.io/ | sh`

## 二、下载镜像

查看Docker命令帮助：

`docker —help`

下载`centos:7.2.1511`镜像

`docker pull centos:7.2.1511`

官方仓库地址：`hub.docker.com`

启动第一个容器

`docker pull hello-world`

`docker run -it --rm hello-world`

命令介绍

`start`,`stop`,`restart`,`exec`,`rename`,`update`,`logs`

## 三、Dockerfile介绍

oschina [Dockerfile详细介绍](http://t.cn/RU7Jlbf)

github [Dockerfile详细介绍](https://github.com/billycyzhang/Shell/blob/master/Dockerfile%E5%91%BD%E4%BB%A4%E8%AF%A6%E8%A7%A3.md)


## 四、构建镜像

`clone`代码

`git clone https://github.com/billycyzhang/odt.git`

构建base基础镜像：

```
cd centos7
docker build -t zcy/centos7 .
```

构建nginx+php-fpm镜像

```
cd php-fpm
docker build -t zcy/php-fpm .
```

构建Mariadb镜像

```
cd mariadb
docker build -t zcy/mariadb .
```

创建mariadb容器

```
docker run -d -p 3306:3306 --name mydb -e DB_USER=myuser -e DB_PASS=mypass -e REMOTE_ADMIN=true -e REMOTE_ADMIN_USER=zcy -e REMOTE_ADMIN_PASS=zcypaas --restart=always -v /db-data:/var/lib/mysql zcy/mariadb
```

构建wordpress镜像

```
cd wordpress
docker build -t zcy/wordpress: .
```

创建Wordpress容器

```
docker run -d -p 80:80 --name blog --restart=always -e WORDPRESS_DB_USER=zcy -e WORDPRESS_DB_PASSWORD=mypaas -e WORDPRESS_DB_HOST=172.17.0.1 -v /data/logs:/var/log/nginx zcy/wordpress
```

## 五、Docker Registry管理

`115.182.107.187 reg.zcy.com` >> /etc/hosts

`docker pull reg.zcy.com:8888/registry:2.4.0`
`docker tag reg.zcy.com:8888/registry:2.4.0 registry:2.4.0`

`mkdir registry && cd registry`

生成证书

```
mkdir certs
cd certs
openssl req -x509 -days 3650 -subj '/CN=reg.odt.com/' -nodes -newkey rsa:2048 -keyout domain.key -out domain.crt
```

生成密码

```
mkdir auth
docker run --entrypoint htpasswd registry:2.4.0 -Bbn zcy password > auth/htpasswd
```

创建registry容器

```
docker run -d -p 8888:5000 --restart=always --name registry \
  -v `pwd`/auth:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -v `pwd`/certs:/certs \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  -v /registry:/var/lib/registry \
  registry:2.4.0
```
配置Registry证书

`mkdir /etc/docker/certs.d/reg.odt.com:8888`

`cp /root/certs/domain.crt /etc/docker/certs.d/reg.odt.com:8888`

```
docker tag busybox reg.odt.com:8888/busybox
docker login reg.odt.com:8888
docker push reg.odt.com:8888/busybox
```

## 六、日志管理

下载`elk`镜像

`docker pull reg.zcy.com:8888/elk`

启动elk容器

`docker run -d -p 5601:5601 -p 9200:9200 -p 5044:5044 -p 5000:5000 --name elk reg.zcy.com:8888/elk`

> 登陆kibana管理面板: `http://<your-host>:5601`


下载`filebeat`镜像

`docker pull reg.zcy.com:8888/filebeat`

创建filebeat容器

`docker run -d --name filebeat -v /data/logs:/data/logs -v /var/lib/docker/containers:/var/lib/docker/containers --link elk:elk reg.zcy.com:8888/filebeat`

登陆kibana管理面板: `http://<your-host>:5601`,查看日志信息；

`logstash-*` to `filebeat-*`