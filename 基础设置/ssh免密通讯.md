### ssh免密

```shell
1.生成公私钥
ssh-keygen    #一路回车默认回车 不设置私钥密码

2.发送公钥到所需服务器
ssh-copy-id -i ～/.ssh/id_rsa.pub root@xxx.xxx.xxx.xxx
```

