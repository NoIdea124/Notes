# docker_devops

Notes for using docker to deploy devops environemnt.

![img](https://img-blog.csdnimg.cn/20181114170553871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x5ZnF5cg==,size_16,color_FFFFFF,t_70)

### Docker install gitlab
docker pull
```shell
docker pull gitlab/gitlab-ce
docker network create gitlab_net
```
mkdir
```shell
mkdir /Users/apple/gitlab/config
mkdir /Users/apple/gitlab/logs
mkdir /Users/apple/gitlab/data
```
docker run
```shell
docker run --name gitlab -d --net=gitlab_net --publish 1443:443 --publish 18080:80 --restart always --volume /Users/apple/gitlab/config:/etc/gitlab --volume /Users/apple/gitlab/logs:/var/log/gitlab --volume /Users/apple/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce:latest
```

### Docker install Jenkins 

docker pull
```
docker pull jenkins
```

docker run
```
docker run -d -p 8080:8080 -p 50000:50000 -v /Users/apple/jenkins:/var/jenkins_home jenkins
```

### Docker install Jira
Get docker-compose.yml

```
git clone https://github.com/haxqer/jira.git
```
Docker-compose up
```
cd jira 
docker-compose up
```

### Docker install confluence 

Get docker-compose.yml
```
git clone https://github.com/EugenMayer/docker-image-atlassian-confluence.git
```
Docker-compose up
```
docker-compose up
```
Create Database
```
dc exec mysql
mysql -pverybigsecretrootpassword -e 'drop database confluencedb;'
mysql -pverybigsecretrootpassword -e 'CREATE DATABASE confluencedb CHARACTER SET utf8 COLLATE utf8_bin;'
```

### Docker install ElasticSearch+Kibana

docker pull
```
docker pull nshou/elasticsearch-kibana
```
docker run
```
docker run -d -p 9200:9200 -p 5601:5601 nshou/elasticsearch-kibana
```

### Docker install neo4j

```
docker run -d -p 7474:7474 -p 7687:7687 neo4j
```

### Docker install Rocket.chat
Get docker-compose.yml
```
curl -L https://raw.githubusercontent.com/RocketChat/Rocket.Chat/develop/docker-compose.yml -o docker-compose.yml
```
Docker-compose
```
docker-compose up
```

### Docker install Crawlab
docker-compose.yml
```
version: '3.3'  # Docker Compose 的版本号（请看后续说明）
services:  # 服务
  master:  # 服务名称
    image: tikazyq/crawlab:latest  # 服务对应的镜像名称
    container_name: master  # 服务对应的容器名称
    environment:  # 这里定义传入的环境变量
      CRAWLAB_API_ADDRESS: "localhost:8000"  # 前端调用的 API 地址，默认为 localhost:8000
      CRAWLAB_SERVER_MASTER: "Y"  # 是否为主节点，Y/N
      CRAWLAB_MONGO_HOST: "mongo"  # MongoDB host，由于在 Docker Compose 里，可以引用服务名称
      CRAWLAB_REDIS_ADDRESS: "redis"  # Redis host，由于在 Docker Compose 里，可以引用服务名称
    ports:  # 映射的端口
      - "8080:8080" # 前端端口
      - "8000:8000" # 后端端口
    depends_on: # 依赖的服务
      - mongo  # MongoDB
      - redis  # Redis
  worker:  # 工作节点，与主节点配置类似，不重复写了
    image: tikazyq/crawlab:latest
    container_name: worker
    environment:
      CRAWLAB_SERVER_MASTER: "N"
      CRAWLAB_MONGO_HOST: "mongo"
      CRAWLAB_REDIS_ADDRESS: "redis"
    depends_on:
      - mongo
      - redis
  mongo:  # MongoDB 服务名称
    image: mongo:latest  # MongoDB 镜像名称
    restart: always  # 重启策略为“总是”
    ports:  # 映射端口
      - "27017:27017"
  redis:  # Redis 服务名称
    image: redis:latest  # Redis 镜像名称
    restart: always  # 重启策略为“总是”
    ports:  # 映射端口
      - "6379:6379"
```

### Docker install ambari

```
Hadoop集群的搭建往往是一件很费力的事情，部署人员也许会消耗很长的时间部署和调试，Ambari把Hadoop集群的部署成本大大降低了，但是能否再更进一步的简化部署，降低部署的人力成本呢？
这时候我们想到了Docker技术。
先从单节点做起。
这里我做好了一个镜像放到了阿里云上
docker pull registry.cn-hangzhou.aliyuncs.com/guoyun/ambari
下载后 运行
docker run -h fb34ae151c81 --name ambari_onenode -d -p 2222:22 -p 8888:8080 registry.cn-hangzhou.aliyuncs.com/guoyun/ambari /usr/sbin/sshd -D
然后依次运行下列两个命令
docker exec ambari_onenode /usr/init.sh
docker exec ambari_onenode /usr/start.sh
脚本中包含了启动Ambari引擎，设置Ambari，启动Ambari所有服务等命令，当然也可以手动ssh到docker内部执行。
直接访问宿主机8888端口即可看到Ambari管理页面
单节点16g内存，包含常用的组件，如hdfs，hive，spark等。
```
### Docker install Rasa-UI 
docker-compose.yml
```
version: '3.0'

services:
  rasa:
    image: rasa/rasa:latest-full
    container_name: rasa
    networks: ['rasa-network']
    ports:
    - "5005:5005"
    volumes:
    - "./rasa-app-data/models:/app/models"
    - "./rasa-app-data/logs:/app/logs"
    command: "run --enable-api --debug"

  rasa_ui:       
    image: paschmann/rasa-ui:latest
    container_name: rasa_ui
    networks: ['rasa-network']
    ports:
      - "5001:5001"
    depends_on:
      - "rasa"
    environment:
      rasa_endpoint: "http://rasa:5005"

networks: {rasa-network: {}}
```
