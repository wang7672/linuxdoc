# Yaml资源文件编写

### Yaml文件编写对象类型

```reStructuredText
Yaml文件内容主要分类俩种对象类型 list map
map -> key: value
list -> ['value','value']
```



### 资源清单语法查询

```shell
kubectl explain xxxx
会返回一个资源清单所需字段的文档，如果显示有 <required> 就证明该字段是必须填写的
```

