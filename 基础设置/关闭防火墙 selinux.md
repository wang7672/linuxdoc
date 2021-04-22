### 关闭防火墙 

```shell
systemctl stop firewalld
systemctl disable firewalld
```



### 关闭selinux

```shell
setenforce 0 && getenforce
sed -i 's/^SELINUX=.*/SELINUX=disabeld/' /etc/selinux/config
```

