# 采用GraphQL作为聚合层
* GraphQL

 ``` plantuml 
@startuml
!include <cloudogu/common>
!include <cloudogu/dogus/jenkins>
!include <cloudogu/dogus/cloudogu>
!include <cloudogu/dogus/scm>
!include <cloudogu/dogus/smeagol>
!include <cloudogu/dogus/nexus>
!include <cloudogu/tools/k8s>
actor web
actor app

node node1 as "XXX应用-K8S集群" <<$cloudogu>> {
	DOGU_SMEAGOL(smeagol71, 网关) #ffffff
    DOGU_SMEAGOL(smeagol75, GraphQL-Server-N个) #ffffff
    DOGU_SMEAGOL(smeagol76, GraphQL-Server1-N个) #ffffff
    DOGU_SMEAGOL(smeagol77, 数据库) #ffffff
    DOGU_SMEAGOL(smeagol771, 微服务API) #ffffff
    DOGU_SMEAGOL(smeagol78,  微服务GRPC) #ffffff
    DOGU_SMEAGOL(smeagol79,  中间件API) #ffffff
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


@enduml
```