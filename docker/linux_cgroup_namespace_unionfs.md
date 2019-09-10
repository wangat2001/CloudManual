## cgroups是什么？
> Cgroup是control group的简写，属于Linux内核提供的一个特性，用于限制和隔离一组进程对系统资源的使用，也就是做资源QoS，这些资源主要包括CPU、内存、block I/O和网络带宽。Cgroup从2.6.24开始进入内核主线，目前各大发行版都默认打开了Cgroup特性。

#### Cgroups提供了以下四大功能:
- 资源限制（Resource Limitation）：cgroups可以对进程组使用的资源总额进行限制。如设定应用运行时使用内存的上限，一旦超过这个配额就发出OOM（Out of Memory）。
- 优先级分配（Prioritization）：通过分配的CPU时间片数量及硬盘IO带宽大小，实际上就相当于控制了进程运行的优先级。
- 资源统计（Accounting）： cgroups可以统计系统的资源使用量，如CPU使用时长、内存用量等等，这个功能非常适用于计费。
- 进程控制（Control）：cgroups可以对进程组执行挂起、恢复等操作。
#### Example
```
1) -m or --memory: 设置内存的限制
2) --memory-swap: 设置内存+swap的限制
docker run -m 200M --memory-swap=300M ubuntu  #最多200M 内存，100MB的swap. 默认值都是-1, 即没有限制
3) -c or --cpu-shares: 设置container的cpu相对权重值，不指定则为1024
4) --cpu：用来设置工作线程的数量
```

refer: https://docs.docker.com/config/containers/resource_constraints/

## 命名空间 namespace
> 命名空间是 Linux 内核一个强大的特性。每个容器都有自己单独的命名空间，运行在其中的应用都像是在独立的操作系统中运行一样。命名空间保证了容器之间彼此互不影响。Linux使用了6种namespace.

- pid 命名空间: 隔离不同用户的进程的pid，不同namespace可以有相同的pid. 所以的LXC(linux container)进程具有不同的命名空间。
- net 命名空间：用来网络隔离。container共享host端口，每个net namespace有独立的网络设备，IP地址，路由表。
- ipc 命名空间：(interprocess communication - IPC)，让容器拥有自己的**共享内存**和**信号量**来实现进程间通信，而不会与host和其它container的IPC混在一起。
- mnt 命名空间：mnt命名空间允许不同命名空间的进程看到的文件结构不同，这样每个命名空间中的进程所看到的文件目录就被隔离开了。
- uts 命名空间：("UNIX Time-sharing System") 命名空间允许每个容器拥有独立的 hostname 和 domain name, 使其在网络上可以被视作一个独立的节点而非 主机上的一个进程。
- user 命名空间：每个容器可以有不同的用户和组id, 也就是说可以在容器内用容器内部的用户执行程序而非主机上的用户。

## 什么是UnionFS
> Union File System, 联合文件系统,它可以把多个目录(也叫分支)内容联合挂载到同一个目录下，而目录的物理位置是分开的。
> docker的镜像rootfs，和layer的设计就用到了UnionFS.

一个最常见的 rootfs，或者说容器镜像，会包括如下所示的一些目录和文件，比如 /bin，/etc，/proc:

```
1 ls /
2 bin dev etc home lib lib64 mnt opt proc root run sbin sys tmp usr var
```
Docker 在镜像的设计中，引入了层（layer）的概念。也就是说，用户制作镜像的每一步操作，都会生成一个层，也就是一个增量 rootfs。 用到的技术就是联合文件系统（Union File System）

![image](./image/unionfs.png)