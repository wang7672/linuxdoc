# Pod调度

一般情况下部署Pod是通过集群的自动调度策略来选择节点的，默认情况下调度器考虑的是资源足够，并且负载尽量平均，但是有的时候需要能够更加细粒度的去控制Pod的调度。这就需要使用一些调度方式来控制Pod的调度了，主要有俩个概念：**亲和性和反亲和性**，亲和性又分成节点亲和(nodeAffinity)和Pod亲和性(podAffinity)。



### nodeSelector

**nodeSelector**是一个非常常用的调度方式，我们知道label标签是kubernetes中一个非常重要的概念，用户可以非常灵活的利用label来管理集群中的资源，比如最常见的Service对象通过label去匹配Pod资源，而Pod的调度也可以根据节点的label来进行调度

**nodeSelector**调度的方式比较直观，但是还不够灵活，控制粒度偏大。



### 亲和性和反亲和性调度

亲和性调度可以分成**软策略**和**硬策略**俩种方式：

1. `软策略` 如果现在没有满足调度要求的节点的话，Pod就会忽略这条规则，继续完成调度过程，说白了就是满足条件最好，没有的话也无所谓
2. `硬策略` 如果没有满足条件的节点的话，就不断重试直到满足条件为止，简单的说就是你必须满足我的要求，不然就不干了

对于亲和性和反亲和性都有这两种规则可以设置： <font color=blue>preferredDuringSchedulingIgnoredDuringExecution</font> 和<font color=blue>requiredDuringSchedulingIgnoredDuringExecution</font>，前面的就是软策略，后面的就是硬策略。

```
匹配逻辑是label标签的值在某个列表中，操作符值如下
In：label的值在某个列表中
NotIn：label的值不在某个列表中
Gt：label的值大于某个值
Lt：label的值小于某个值
Exists：某个label存在
DoesNotExist：某个label不存在
```

#### 节点亲和性

节点亲和性(nodeAffinity)主要是用来控制Pod要部署在那些节点上，以及不能部署在哪些节点上的，它可以进城一些简单的逻辑组合，不止是简单的相等匹配

在匹配label标签时，需要注意如果<font color=blue>nodeSelectorTerms</font>下面有多个选项的话，满足任何一个条件就可以了，如果<font color=blue>matchExpressions</font>有多个选项的话，则必须同时满足这些条件才能正常调度Pod

#### Pod亲和性

Pod亲和性(podAffinity)主要解决Pod可以和哪些Pod部署在同一拓扑域中的问题(其中拓扑域用主机标签实现，可以是单个主机，也可以是多个主机组成的cluster、zone等等)，而Pod反亲和性主要是解决Pod不能和哪些Pod部署在同一个拓扑域总的问题，它们都是处理Pod和Pod之间的关系，比如一个Pod在一个节点上了，那么我这个Pod也得在这个节点上，或者一个Pod在这个节点上，那么我就不想和你待在同一个节点上

#### Pod反亲和性

Pod反亲和性(PodAntiAffinity)则是反着来的，比如一个节点上运行了某个Pod，那么我们的模版Pod则不希望呗调度到这个节点上面去，我们把podAffinity直接改成podAntiAffinity



###  污点与容忍

对于<font color=blue>nodeAffinity</font>无论是硬策略还是软策略方式，都是调度Pod到预期的节点上，而污点(Taints)恰好与之相反，如果一个节点标记为Taints，除非Pod也被标识可以容忍污点节点，否则该Taints节点不会被调度Pod。

```
NoSchedule：表示Pod不会被调度到标记为taints的节点
PreferNoSchedule：表示尽量不调度到污点节点上去
NoExecute：该选项意味着一旦Taint生效，如该节点内正在运行的Pod没有对应容忍(Tolerate)设置，则会被直接驱逐
```

对于tolerations属性的写法，其中key、value、effect与Node的Taint设置需要保持一致

如果operator的值是`Exists`，则value属性可省略

如果operator的值是`Equal`，则表示其key与value之间的关系是equal(等于)

如果不指定operator属性，则默认值为`Equal`

































































