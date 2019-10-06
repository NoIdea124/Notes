# docker_devops

Notes for using docker to deploy devops environemnt.

![img](https://img-blog.csdnimg.cn/20181114170553871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x5ZnF5cg==,size_16,color_FFFFFF,t_70)

### Docker install gitlab
docker pull
```shell
docker pull gitlab/gitlab-ce
docker network create gitlab_net
```
mkdir for volume
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
docker-compose.yml

```
git clone https://github.com/haxqer/jira.git
```
docker-compose
```
cd jira 
docker-compose up
```

### Docker install confluence 

docker-compose.yml
```
git clone https://github.com/EugenMayer/docker-image-atlassian-confluence.git
```
docker-compose
```
docker-compose up
```
docker exec
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
docker pull
```
docker pull neo4j
```
docker run
```
docker run -d -p 7474:7474 -p 7687:7687 neo4j
```

### Docker install Rocket.chat

docker-compose.yml
```
curl -L https://raw.githubusercontent.com/RocketChat/Rocket.Chat/develop/docker-compose.yml -o docker-compose.yml
```
docker-compose
```
docker-compose up
```

### Docker install Crawlab
docker-compose.yml
```
version: '3.3' 
services:  
  master:  
    image: tikazyq/crawlab:latest  
    container_name: master  
    environment: 
      CRAWLAB_API_ADDRESS: "localhost:8000" 
      CRAWLAB_SERVER_MASTER: "Y"  
      CRAWLAB_MONGO_HOST: "mongo" 
      CRAWLAB_REDIS_ADDRESS: "redis" 
    ports: 
      - "8080:8080"
      - "8000:8000"
    depends_on: 
      - mongo  
      - redis  
  worker: 
    image: tikazyq/crawlab:latest
    container_name: worker
    environment:
      CRAWLAB_SERVER_MASTER: "N"
      CRAWLAB_MONGO_HOST: "mongo"
      CRAWLAB_REDIS_ADDRESS: "redis"
    depends_on:
      - mongo
      - redis
  mongo: 
    image: mongo:latest 
    restart: always 
    ports: 
      - "27017:27017"
  redis: 
    image: redis:latest
    restart: always 
    ports:  
      - "6379:6379"
```

### Docker install ambari (hadoop+spark+hive)
docker pull
```
docker pull registry.cn-hangzhou.aliyuncs.com/guoyun/ambari
```
docker run
```
docker run -h fb34ae151c81 --name ambari_onenode -d -p 2222:22 -p 8888:8080 registry.cn-hangzhou.aliyuncs.com/guoyun/ambari /usr/sbin/sshd -D
```
docker exec
```
docker exec ambari_onenode /usr/init.sh
docker exec ambari_onenode /usr/start.sh
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
### Docker install MLflow
dockerfile
```
git clone https://github.com/Ycallaer/mlflowdocker
```
docker build
```
docker build -t mlflowserver -f Dockerfile . --no-cache
```
docker run 
```
docker run -p 5000:5000 --env MLFLOW_SERVER_DEFAULT_ARTIFACT_ROOT=<wasb> --env AZURE_STORAGE_ACCESS_KEY=<access_key> -it mlflowserver:latest
```
