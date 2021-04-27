# Pod进阶实用化

### 静态Pod

```
静态pod --> Static Pod
静态pod直接由节点上的kubelet进程来管理，不受master节点上的apiverser控制。无法与我们常用的控制器Deployment或者DaemonSet进行关联。
配置文件放置在特定的目录下需要用 kubelet --pod-manifest-path=<static-pod-path>来指定目录，kubulet会定期的去扫描这个目录，根据这个目录下出现或消失的YAML/JSON文件来创建或删除静态pod
```



### 环境变量注入

```
1. env直接指定方式注入
2. Volume信息挂载写入
```

