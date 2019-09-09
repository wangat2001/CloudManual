#### Docker 的最佳实践
1. 修改docker存储路径, 不要使用默认的/var/lib/docker.
  - sudo systemctl status docker  # 查看真实生效的docker.service配置
![image](./images/docker_service_config.png)
  - sudo vi /etc/systemd/system/docker.service.d/override.conf  #修改该文件
```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -g /home/myname/apps/docker -H fd:// -H tcp://0.0.0.0:12375  -H unix:///var/run/docker.sock
```

- sudo systemctl daemon-reload   # 重新加载配置
- sudo systemctl restart docker    # 重启docker
- doker info  # 查看Docker Root Dir的值是否为: /home/myname/apps/docker 