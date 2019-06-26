## Docker-compose Kibana

本仓库基于docker-compose创建独立的Kibana容器，连接到Elasticsearch集群中，配合[我的博客](<https://www.cnblogs.com/hellxz/p/docker-compose_kibana.html>)食用，风味更佳

## 端口占用情况

| 目录名        | 容器名 | 占用端口号 |
| ------------- | ------ | ---------- |
| docker-kibana | kibana | 5601       |



## 文件结构

```bash
├── docker-compose.yml
└── .env
```

> 这可能是本次ELK集群中最少的配置了，哈哈

## 文件说明

`.env`为`docker-compose.yml`提供了需要连接的es-tribe节点的宿主机Ip

```properties
# just for kibana docker-compose.yml
# this host-ip is elasticsearch tribe-node's machine ip.
ES_TRIBE_HOST=10.2.114.110
```

对就到`docker-compose.yml`我们可以看到`.env`中的`ES_TRIBE_HOST`与9204进行组合出es-tribe暴露的节点位置

```yaml
version: "3"
services:
    kibana:
        image: kibana:7.1.0
        container_name: kibana
        environment:
            - ELASTICSEARCH_HOSTS=http://${ES_TRIBE_HOST}:9204 # connect the es-balance node
            - I18N_LOCALE=zh-CN #汉化
        ports:
            - "5601:5601"
        network_mode: "host"

```

这里因为只有一个目录，就不写启动和结束脚本了，必要性不是很高了。

## 使用说明

1. 确保ES集群es-tribe节点宿主机可以Ping通
2. 确保es-tribe节点处于提供服务状态
3. 修改`.env`的`ES_TRIBE_HOST`的value为es-tribe的宿主机Ip
4. 执行`docker-compose up -d` 以启动程序，执行`docker-compose down`以关闭程序