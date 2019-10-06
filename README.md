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

```
git clone https://github.com/haxqer/jira.git
cd jira 
docker-compose up

```
