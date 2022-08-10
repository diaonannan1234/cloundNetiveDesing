# 应用场景
```
开发、测试、预发、生产环境应一致。
包括操作系统-OS、程序语言运行环境、各种中间件及版本。
生产数据脱敏后同步到测试环境，提高测试的准确性。
开发环境的Mock数据准备
应用配置可从各环境的环境变量中读取数据。

提供公用数据API服务：如交通、天气、税务。

错误处理：最终用户不应看到如404、302、500等错误信息。

通过对流量的控制、路由。按业务场景完成金丝雀、灰度等部署方式。

```

``` plantuml 
@startuml
title Unlabeled k8s sprites
!include <kubernetes/k8s-sprites-unlabeled-25pct>
!include <kubernetes/k8s-skinparam>


package "电商业务集群" {
  component "<$ns>\n网关" as b0
  component "<$ns>\n安全域" as b1
  component "<$ns>\n后台管理API服务" as a0
  component "<$ns>\n电商API服务" as a1
  component "<$ns>\n开发平台API服务" as a2 
   component "<$crd>\nGraphQL查询聚合层" as b2
  component "<$ns>\n物流核心域" as n1
  component "<$ns>\n订单核心域" as n2
  component "<$ns>\n商品核心域" as n3
}

package "数据持久" {
  component "<$ns>\n关系型数据库" as n01 
  component "<$ns>\nNosql数据库" as n02
  component "<$ns>\n大数据分布式存储" as n03
  component "<$ns>\n图数据库" as n04
  component "<$ns>\n文档数据库" as n05
  component "<$ns>\nKeyValue数据库" as n06
}

b0 ---> a0 : /admin
b0 ---> a1 : /ecp
b0 ---> a2 : /platform

b2 <--- n1 : 注册
b2 <--- n2 : 注册
b2 <--- n3 : 注册

a0 ---> b2 : 查询请求
a1 ---> b2 : 创建并更新请求
a2 ---> b2 : 订阅请求

@enduml
```

``` plantuml 
@startuml
title Unlabeled k8s sprites
!include <kubernetes/k8s-sprites-unlabeled-25pct>
!include <kubernetes/k8s-skinparam>


package "电商业务集群" {
  component "<$pv>\npv" as b0
  component "<$pvc>\npvc" as b1
  component "<$deploy>\ndeploy" as a0
  component "<$svc>\nsvc" as a1
  component "<$cm>\ncm" as a2 
   component "<$role>\nrole" as b2
  component "<$secret>\nsecret" as n1
  component "<$job>\njob" as n2
  component "<$crd>\ncrd" as n3
  component "<$ing>\ning" as n3
  component "<$pod>\npod " as n3
}


@enduml
```