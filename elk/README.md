# 安装 (Installation)

## 下载ELK镜像

安装[Docker](https://docs.docker.com/),请查看官方文档；（Linux、Windows、OSX-使用`Boot2Docker` 或者 `Vagrant`）

**打开shell终端，从`Docker registry`上下载镜像**

`docker pull sebp/elk:latest`

> 镜像的源代码在[GitHub](https://github.com/spujadas/elk-docker)上,如果你想自己构建镜像，请查看[构建镜像]()章节；

**下载指定版本的镜像**

`Elasticsearch:1.7.3、Logstash:1.5.5、Kibana:4.1.2`，也可以直接下载tag是`E1L1K4`的镜像：`docker pull sebp/elk:E1L1K4`

镜像`tag`在 [Docker Hub's sebp/elk](https://hub.docker.com/r/sebp/elk/tags/) 镜像页面，或者在 [GitHub](https://github.com/spujadas/elk-docker)页面可查看；

# 使用 (Usage)

使用以下命令启动容器：

`docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -p 5000:5000 -it --name elk sebp/elk`

- 5601 Kibana web 端口

- 9200 Elasticsearch JSON 端口

- 5044 Logstash Beats 端口，详情请在[使用Filebeat章节中查看]()

- 5000 Logstash Lumbejack 端口，详情请在[使用Logstash forwarder章节中查看]()

> sebp/elk镜像也暴露了`Elasticearch`传输端口`9300`,如果要暴露此端口在创建容器时，再多添加一个参数：`-p 9300:9300`；

下图显示了整个架构：

![together](http://i.imgur.com/wDertsM.png)

通过`http://<your-host>:5601`访问`Kibana`web页面，

## 使用`docker compose`启动容器

## 使用`Kitematic`启动容器

## 创建一个虚拟日志路径

## 启动服务选项

## 改变启动变量

# 日志转发（Forwarding logs）

## 使用`Filebeat`转发日志

## 使用`Logstash forwarder`转发日志

## 连接应用容器到`ELK`容器

# 构建镜像（Building the image）

# 调整镜像（Tweaking the image）

## 更新`logstash`配置

## 安装`Elasticsearch`插件

## 安装`Logstash`插件

## 安装`Kibana`插件

# 日志数据持久化（Persisting log data）

# 设置Elasticsearch集群（Setting up an elasticsearch cluster）

## 运行`Elasticsearch`节点到不同的主机

## 运行`Elasticsearch`节点到一个主机

## 优化`Elasticsearch`集群

# 安全注意事项（Security considerations）

## 设置证书（Certificates）

## 禁用SSL/TLS

# 故障排查（Troublesshooting）

# 报告问题（Reporting issues）

# 参考文档（References）

# 关于（About）

未完......