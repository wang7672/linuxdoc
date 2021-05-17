# RBAC权限控制

### RBAC资源对象

```
Rule：规则，规则是一组属于不同API Group资源上的一组操作的集合

Role 和 ClusterRole：角色和集群角色，这俩个对象都包含上面的Rlues元素，二者的区别在于，在Role中，定义的规则只适用于单个命名空间，也就是和namespace关联的，而ClusterRole是集群范围内的，因此定义的规则不受命名空间的约束。另外Role和ClusterRole在Kubernetes中都被定义为集群内部的API资源，所以同样可以用Yaml文件来描述，用kubectl工具来管理

Subject：主题，对应集群中尝试操作的对象，集群中定义了3种类型的主题资源：
1.User Account：用户，这是有外部独立服务进行管理的，管理员进行私钥的分配，用户可以使用KeyStone或者Goolge账号，甚至一个用户名和密码的文件列表也可以。对于用户的管理集群内部没有一个关联的资源对象，所以用户不能通过集群内部的API来进行管理
2.Group：组，这是用来关联多个账户的，集群中有一些默认创建的组，比如cluster-admin
3.Service Account：服务账户，通过Kubernetes API来管理的一些用户账户，和namespace进行关联的，适用于集群内部运行的应用程序，需要通过API来完成权限认证，所以在集群内部进行权限操作，都需要关联到ServiceAccount

RoleBinding 和 ClusterRoleBinding：角色绑定和集群角色绑定，简单来说就是把声明的Subject和我们的Role进行绑定的过程(给某个用户绑定上操作的权限)，二者的区别也是作用范围的区别：RoleBinding只会影响到当前namespace下面的资源操作权限，而ClusterRoleBinding会影响到所有的namespace
```



### 只能访问某个namespace的普通用户

```shell
###username：test    group：yck###
$ openssl genrsa -out test.key 2048

$ openssl req -new -key cnych.key -out cnych.csr -subj "/CN=test/O=yck"

$ openssl x509 -req -in test.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out test.crt -days 3650

$ kubectl config set-context test-context --cluster=kubernetes --namespace=kube-system --user=test --kubeconfig=.kube/config

$ kubectl config use-context test-context --kubeconfig=.kube/config 

$ kubectl config set-cluster kubernetes --certificate-authority=/etc/kubernetes/pki/ca.crt --server=https://192.168.3.20:6443 --kubeconfig=.kube/config
```



###  只能访问某个namespace的ServiceAccount



### 可以全局访问的ServiceAccount











































































