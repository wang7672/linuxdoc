# kubectl自动补全

### 安装bash-completion包

```SHELL
yum -y install bash-completion
```



### 添加  completion bash 至环境变量 

```shell
echo "source <(kubectl completion bash)" >> ${HOME}/.bashrc
source ${HOME}/.bashrc
配置完成后需要重新登录终端
```





