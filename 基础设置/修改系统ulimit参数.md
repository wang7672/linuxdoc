### 修改系统ulimit参数

```shell
编辑文件 /etc/security/limits.conf 添加     (普通用户修改/etc/security/limits.d/20-nproc.conf)
* soft nproc 65536
* hard nproc 65536
* soft nofile 65536
* hard nofile 65536

重启服务器
```

