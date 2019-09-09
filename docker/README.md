# Docker


### docker supervisor、docker-compose、 Docker Swarm、 k8s
- supervisor 用于在一个容器中管理启动多个进程.
- compose 用于单机管理一组容器，比如几个容器组成的project.
- Docker Swarm: Docker原生的编排工作，跨节点管理容器。
- k8s: google大厂出品，跨节点管理容器, 可扩展，滚动更新. 【推荐】

如果项目单一,只用部署到一个节点上，compose就够用了。
如果公司项目繁多，直接用k8s，最好是使用云服务商的k8s, 不要自己搭建。

### Dokcer 常用命令
- login
`docker login registry.yourcorp.com`
- we can create Secret for us to access
`kubectl create secret docker-registry mysecret --docker-server='registry.yourcorp.com' --docker-username=myname --docker-password=mypasswd`
- pull image
`docker pull registry.yourcorp.com/centos/nginx:1.0`
`docker image inspect registry.yourcorp.com/centos/nginx:1.0`
- rm images
`docker image rm {name}:{tag}`
- build images
`docker build -t nginx:1.0 .`
- run image as container
`docker run --name nginx-test -d -p 8090:8080 nginx:1.0`
- debug container
`docker run --rm -it --name nginx-debug nginx:1.0 bash`
`docker logs [OPTIONS] CONTAINER`
- stop container 
`docker ps -a`
`docker stop {container_ID}`
`docker rm {container_ID}`
- publish image
```
$ docker images
$ docker tag [REPOSITORY(:TAG)] registry.yourcorp.com/[REPOSITORY(:TAG)]
$ docker login registry.yourcorp.com
$ docker push registry.yourcorp.com/[REPOSITORY(:TAG)]
```

- stop all containers:
`docker kill $(docker ps -q)`
- remove all containers
`docker rm $(docker ps -a -q)`
- remove all docker images
`docker rmi $(docker images -q)`