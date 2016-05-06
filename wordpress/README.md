##wordpress

>基于php-fpm镜像构建，当前非常流行的博客系统

### 如何构建

```
git clone https://git.oschina.net/csphere/wordpress.git
cd wordpress
docker build -t csphere/wordpress:4.3 .
```

### 运行容器

```
docker run -d -p 80:80 --name myblog --restart=always -e WORDPRESS_DB_USER=myuser -e WORDPRESS_DB_PASSWORD=mypass -e WORDPRESS_DB_HOST=192.168.42.1 -v /data/logs:/var/log/nginx csphere/wordpress:4.3
```

或者

`docker-compose up -d`
