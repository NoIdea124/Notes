# docker_devops

Notes for using docker to deploy devops environemnt.

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

docker run
```
docker run -d -p 8080:8080 -p 50000:50000 -v /Users/apple/jenkins:/var/jenkins_home jenkins
```

