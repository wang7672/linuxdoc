# SecurityContext的使用

```
有的时候在运行一个容器的时候，可能需要使用sysctl命令来修改内核参数，比如net、vm、kernel等参数，但是sysctl需要容器拥有超级权限才可以使用，在docker容器启动的时候可以加上 --privileged 参数来使用特权模式。
在kubernetes则需要使用到SecurityContext(安全上下文)主要是来限制容器非法操作宿主节点的系统级别的内容，使得节点的系统或者节点上其他容器组受到影响。kubernetes提供了三种配置安全上下文级别的方法
1.Container-level Security Context：仅应用到指定的容器
2.Pod-level Security Context：应用到Pod内所有容器以及Volume
3.Pod Security Policies(PSP)：应用到集群内部所有Pod以及volume
```

### Security Context 设置方式

1. 访问权限控制：根据用户ID(UID)和组ID(GID)来限制对资源的访问权限
2. Security Enhanced Linux(SELinux)：为对象分配 selinux 标签
3. 以 privileged(特权) 模式运行
4. Linux Capabilities：给某个特定的进程超级权限，而不用给root用户所有的privileged权限
5. AppArmor：使用程序文件来限制单个程序的权限
6. Seccomp：过滤容器中进程的系统调用(system call)
7. AllowPrivilegeEscalation(允许特权扩大)：此项配置是个布尔值，定义了一个进程是否可以比其父进程获得更多的特权，直接效果是，容器的进程上是否被设置 no_new_privs 标记。当容器以 privileged 模式运行或容器拥有 CAP_SYS_ADMIN 的 linux capability 则 AllowPrivilegeEscalation的值始终为true



### 为Pod设置Security Context

```yaml
securityContext:
	runAsUser: 1000
	runAsGroup: 3000
	fsGroup: 2000
```

在资源清单中添加了security-context字段，其中：

- runAsUser 字段指定了该Pod中所有容器的进程都以 UID1000 的身份运行，runAsGroup 字段指定了该 Pod 中所有容器的进程都以GID3000 的身份运行
  - 如果省略runAsGroup字段，则容器进程的GID为root(0)
  - 容器中创建的文件，其所有者为userID1000，groupID3000

- fsGroup 字段指定了该Pod的faGroup为2000，数据卷对应的挂载目录以及在该数据卷下创建的任何文件，其GID都为2000(还是要看挂载方式原目录的权限)

**除了在Pod中可以设置安全上下文之外，我们还可以单独为某个容器设置安全上下文，同样也是通过SecurityContext字段设置，当该字段的配置与Pod级别的SecurityContext配置相冲突时，容器级别的配置将覆盖Pod级别的配置，容器级别的SecurityContext不影响Pod中的数据卷**



### Kubernetes 配置 Capabilities





































































