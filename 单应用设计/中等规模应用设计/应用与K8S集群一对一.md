# 一个应用对应一个K8S集群

* 应用、K8S集群、K8S命名空间之间关系

>一个应用对应一个K8S集群。
> 一个应用由多个命名空间组合而成。
> 一个命名空间由多个服务构成。
> 命名空间内部服务之间如何通信？
> 命名空间相互间如何通信是？
> 应用或集群如何与外部交互？
> 存储设计方案？
> 集群内部网络设计方案？

* 最小架构

 ``` plantuml 
@startuml
!include <cloudogu/common>
!include <cloudogu/dogus/jenkins>
!include <cloudogu/dogus/cloudogu>
!include <cloudogu/dogus/scm>
!include <cloudogu/dogus/smeagol>
!include <cloudogu/dogus/nexus>
!include <cloudogu/tools/k8s>

node node1 as "XXX应用-K8S集群" <<$cloudogu>> {
	DOGU_SMEAGOL(smeagol71, 子系统1-K8S命名空间) #ffffff
    DOGU_SMEAGOL(smeagol75, 子系统2-K8S命名空间) #ffffff
    DOGU_SMEAGOL(smeagol76, 子系统3-K8S命名空间) #ffffff
    DOGU_SMEAGOL(smeagol77, 通用子系统) #ffffff
    DOGU_SMEAGOL(smeagol78,  存储子系统) #ffffff
    DOGU_SMEAGOL(smeagol79,  外部投影子系统) #ffffff
}

@enduml
```

*  通用子系统内部结构

 ``` plantuml 
@startuml
!include <cloudogu/common>
!include <cloudogu/dogus/jenkins>
!include <cloudogu/dogus/cloudogu>
!include <cloudogu/dogus/scm>
!include <cloudogu/dogus/smeagol>
!include <cloudogu/dogus/nexus>
!include <cloudogu/tools/k8s>

node node1 as "通用子系统" <<$cloudogu>> {
	DOGU_SMEAGOL(smeagol71, 网关) #ffffff
    DOGU_SMEAGOL(smeagol75, 服务注册&发现) #ffffff
    DOGU_SMEAGOL(smeagol76, 日志采集) #ffffff
    DOGU_SMEAGOL(smeagol77, 服务度量) #ffffff
    DOGU_SMEAGOL(smeagol78,  长期任务&一次性任务) #ffffff
    DOGU_SMEAGOL(smeagol79,  配置) #ffffff
    DOGU_SMEAGOL(smeagol81,  私有证书) #ffffff
    DOGU_SMEAGOL(smeagol82,  私有镜像仓库) #ffffff
    DOGU_SMEAGOL(smeagol83,  私用Helm仓库) #ffffff
}

@enduml
```