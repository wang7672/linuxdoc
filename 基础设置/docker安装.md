### docker 安装

```shell
1.安装依赖
yum install -y yum-utils device-mapper-persistent-data lvm2

2.添加yum源
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
###wget -O /etc/yum.repos.d/docker-ce.repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo###

3.安装docker-ce
yum clean all
yum makecache fast
yum list docker-ce --showduplicates
yum -y install docker-ce




4.添加docker配置 /etc/docker/daemon.json
{
  "graph":"/data/docker",   #docker数据目录
  "storage-driver":"overlay2",  #docker数据落地协议
  "insecure-registries":["registry.access.redhat.com","quay.io"],   #docker非ssl源镜像管理仓库
  "registry-mirrors":["https://vspy7yqr.mirror.aliyuncs.com"],    #镜像下载加速地址
  "bip":"172.x.x.1/24",    #设置docker容器进程所使用的网段的网关地址
  "exec-opts":["native.cgroupdriver=systemd"],   #指定 cgroup驱动为systemd
  "live-restore":true   #docker engine down掉后不影响正在运行的docker容器进程
}

```

