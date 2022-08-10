# 采用GraphQL作为聚合层

>  全栈工程师创建"应用服务层-WEB"，平台自动创建一个NameSpace。
>
> 用户可通过选择相应初始化模板，填入参数。由平台按照模板定义和用户输入参数，在这个命名空间下创建"网关","服务发现"，“日志采集”等服务。
>
> 用户可以自定义模板。
>
> 平台在K8S集群创建时为伴生创建一个"中间件API服务层"。该中间件API提供了数据库、缓存、MQ等等pod/pv/pvc/storeClass,通过提供RPC服务供应用服务层使用、
>
> 应用服务层在创建服务时，如需要存储、发送消息、事件处理等，可通过声明式API创建，平台会在服务所在的Namespacek中自动创建“中间件API服务层”的客户端。
>
>平台应用通过命令下发到K8S集群，K8S集群是已注册并且状态为正常运行的。
>
>由集群中的kubeAge去完成命令
>
>
>
>
>
>
>
>
>
>
>
>

* 从K8S层次与上层应用之间对照的关系

 ``` plantuml 
@startuml
!include <cloudogu/common>
!include <cloudogu/dogus/jenkins>
!include <cloudogu/dogus/cloudogu>
!include <cloudogu/dogus/scm>
!include <cloudogu/dogus/smeagol>
!include <cloudogu/dogus/nexus>
!include <cloudogu/tools/k8s>
actor 最终用户 as web
actor app
actor 集群运维管理员  as cadmin

package p2 as "控制平面-OAM标准" {
    DOGU_CLOUDOGU(cloudogu, K8S集群注册) #ffffff
    DOGU_CLOUDOGU(cloudogu001, 平台应用) #ffffff
}



package p1 as "K8S集群" {

node p3  as "公共命名空间" <<$cloudogu>> {
     DOGU_CLOUDOGU(cloudogu213,kubeAge ) #ffffff   
}

cadmin --> cloudogu001: 管理
cloudogu001 --> p3 : 命令下发

node node1 as "可编程基础设施层" <<$cloudogu>> {
	DOGU_SMEAGOL(smeagol75, GraphQL-Server-N个) #ffffff
    DOGU_SMEAGOL(smeagol76, GraphQL-Server1-N个) #ffffff
    DOGU_SMEAGOL(smeagol90,  日志服务) #ffffff
    DOGU_SMEAGOL(smeagol91,  服务编排Flow) #ffffff
    DOGU_SMEAGOL(smeagol92,  安全服务) #ffffff
     DOGU_SMEAGOL(smeagol93,  应用商店) #ffffff
      DOGU_SMEAGOL(smeagol94,  namespace创建模板) #ffffff
   }
   
node node11 as "可编程基础设施层-服务治理" <<$cloudogu>> {
	DOGU_SMEAGOL(smeagol71, 网关) #ffffff
    DOGU_SMEAGOL(smeagol90,  日志采集客户端服务) #ffffff
    DOGU_SMEAGOL(smeagol93,  配置服务) #ffffff
    DOGU_SMEAGOL(smeagol901, 服务注册与发现) #ffffff
    DOGU_SMEAGOL(smeagol902,  熔断) #ffffff
    }

node node2 as "应用服务层-WEB" <<$cloudogu>> {
 DOGU_SMEAGOL(smeagol101, 微服务-VUE) #ffffff
    DOGU_SMEAGOL(smeagol102,  微服务-NodeJS) #ffffff
}


node node4 as "应用服务层" <<$cloudogu>> {
 DOGU_SMEAGOL(smeagol771, 微服务API) #ffffff
    DOGU_SMEAGOL(smeagol78,  微服务GRPC) #ffffff
}

node node3 as "中间件API服务层" <<$cloudogu>> {
    DOGU_SMEAGOL(smeagol79,  缓存API) #ffffff
    DOGU_SMEAGOL(smeagol80,  数据持久API) #ffffff
    DOGU_SMEAGOL(smeagol81,  目录服务-LDAP-AD) #ffffff
}

web ---> smeagol71
app ---> smeagol71 

smeagol71 --> smeagol75 : 聚合服务
smeagol75 --> smeagol76 :  流 
smeagol75  --> smeagol77 : 异步
smeagol76 --> smeagol771
smeagol75 --> smeagol78 : 批量
smeagol76 --> smeagol78
smeagol75 ..> smeagol79 : 异步事件调度

}

p3 --> cloudogu : K8s集群启动时注册
@enduml
```