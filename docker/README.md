# Docker


### docker supervisor、docker-compose、 Docker Swarm、 k8s
- supervisor 用于在一个容器中管理启动多个进程.
- compose 用于单机管理一组容器，比如几个容器组成的project.
- Docker Swarm: Docker原生的编排工作，跨节点管理容器。
- k8s: google大厂出品，跨节点管理容器. 【推荐】

如果项目单一,只用部署到一个节点上，compose就够用了。
如果公司项目繁多，直接用k8s，最好是使用云服务商的k8s, 不要自己搭建。

