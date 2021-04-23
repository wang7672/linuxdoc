# pod网络与共享文件描述

### Kubernetes中最基本的调度单元

> Pod只是一个逻辑概念。

> Kubernetes真正处理的，还是宿主机上Linux容器的Namespace和Cgroups，而并不存在一个所谓的pod的边界或者隔离环境

> 具体的说：Pod 里的所有容器，共享的是同一个 Network Namespace，并且可以声明共享同一个 Volume。

> 在Pod中，Infra 容器永远都是第一个被创建的容器，而其他用户定义的容器，则通过 Join Network Namespace 的方式，与 Infra 容器关联在一起

![container_join_to_infra-network left](./img/container_join_to_infra-network.jpg)



##### 总结来说也就是

1. 容器之间是可以直接使用 localhost 进行通信的
2. 看到的网络设备信息都是和 Infra 容器完全一致的
3. 在同一个 pod 下容器运行的多个进程不能绑定相同的端口
4. pod 的生命周期只和 Infra容器一致，而与其加入的其他容器无关



#### 文件系统在 kubernetes 中实现共享

> 默认情况下容器的文件系统是互相隔离的，要实现共享只需要在pod的顶层声明一个 Volume ，然后在需要时共享这个 Volume 的容器中声明挂载即可

<img src="./img/container_share_filesystem.jpg" alt="container_share_filesystem" style="zoom:50%;" />































