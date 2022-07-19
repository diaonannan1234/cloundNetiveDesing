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
    DOGU_SMEAGOL(smeagol771, 安全认证子系统) #ffffff
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
	DOGU_SMEAGOL(smeagol71, 网关&入口网关Ingress&出口网关Egress) #ffffff
    DOGU_SMEAGOL(smeagol75, 服务注册&发现) #ffffff
     DOGU_SMEAGOL(smeagol55, 熔断) #ffffff
      DOGU_SMEAGOL(smeagol54, 负载均衡) #ffffff
       DOGU_SMEAGOL(smeagol53, 请求路由) #ffffff
        DOGU_SMEAGOL(smeagol52, 故障注入) #ffffff
         DOGU_SMEAGOL(smeagol40, 故障处理) #ffffff
         DOGU_SMEAGOL(smeagol41, 可视化) #ffffff
         DOGU_SMEAGOL(smeagol42, 策略&遥测) #ffffff
         DOGU_SMEAGOL(smeagol43, 规则引擎) #ffffff
         
    DOGU_SMEAGOL(smeagol76, 日志采集) #ffffff
    DOGU_SMEAGOL(smeagol77, 服务度量) #ffffff
    DOGU_SMEAGOL(smeagol78,  长期任务&一次性任务) #ffffff
    DOGU_SMEAGOL(smeagol79,  配置) #ffffff
    DOGU_SMEAGOL(smeagol91,  声明式API) #ffffff
    DOGU_SMEAGOL(smeagol92,  GrapQL查询语言) #ffffff
    DOGU_SMEAGOL(smeagol81,  私有证书) #ffffff
    DOGU_SMEAGOL(smeagol82,  私有镜像仓库) #ffffff
    DOGU_SMEAGOL(smeagol83,  私用Helm仓库) #ffffff
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
!include <cloudogu/dogus/openldap>
!include <cloudogu/tools/k8s>


node node1 as "安全认证子系统" <<$auth>> {
	DOGU_SCM(smeagol70, +安全认证子系统API) #39b778
	DOGU_SMEAGOL(smeagol71, -认证API) #00a99e
    DOGU_SMEAGOL(smeagol75, -组织机构API) #00a99e
    DOGU_SMEAGOL(smeagol76, -权限API) #00a99e
    DOGU_OPENLDAP(smeagol77, -组织机构默认实现-Ldap) #f16522
    DOGU_SMEAGOL(smeagol78,  -缓存API) #00a99e
    DOGU_CLOUDOGU(smeagol79,  -缓存默认实现-redis) #f16522
    DOGU_SMEAGOL(smeagol91,  -拦截WebHook) #f7941d
    DOGU_JENKINS(smeagol92,  -资源API) #00a99e
    DOGU_SMEAGOL(smeagol81,  -资源默认实现-资源注册与管理) #f16522
    DOGU_SMEAGOL(smeagol82,  -资源默认实现-资源认证授权规则) #f16522
    
    smeagol70  <... smeagol71
    smeagol70  <... smeagol78
    smeagol78  <--- smeagol79
    smeagol70  <... smeagol92
    smeagol71  <... smeagol92
    smeagol76  <... smeagol92
    smeagol76  <... smeagol75
    smeagol70  <... smeagol91
      smeagol70  <... smeagol76
      smeagol71  <... smeagol75
      smeagol75  <--- smeagol77
      smeagol76  <... smeagol91
      smeagol92  <--- smeagol81
      smeagol92  <--- smeagol82
}

@enduml
```



@endmindmap
``` plantuml

* 服务端鉴权

``` plantuml
@startmindmap
*  服务端鉴权
** 认证
***  证书
***  Token令牌
***  OAuth2.0
** 授权
*** 网关认证过滤
*** 网关转换认证协议
*** 内部认证解析、加密、注册、创建
*** 每服务前置拦截
**** 实现方式
***** Istio：四层
***** AOP拦截器：六层
** 配置
*** 加密配置
*** 环境配置


@endmindmap
``` plantuml

* 客户端鉴权

``` plantuml
@startmindmap
*  客户端鉴权
** 认证
***  用户名密码认证
***  多因子认证
*** 手机认证
***  第三方认证
*** 图片认证码
** 授权
*** 企业员工
*** 企业资源
*** 企业组织机构
*** 权限模型
**** RBAC-基于触键的访问控制(Role Based Access Control)
***** RBAC0
****** 用户
****** 角色
****** 权限
***** RBAC1
****** 用户
****** 角色分层级
****** 权限
***** RBAC2
****** 用户
****** 角色
****** 权限
****** 静态职责分离SSD
****** 互斥角色限制：同一个用户在两个互斥角色中只能选一个
****** 基数限制：一个用户拥有的角色是有限的，一个角色拥有的资源也是有限的
****** 先决条件限制：用户想获得更高级别的角色，必须拥有低级角色
****** 动态职责分离DSD
****** 动态的限制用户及其拥有的角色：如一个用户可以拥有两个角色，但只能激活一个角色。
***** RBAC3
****** RBAC3是RBAC1和RBAC2的合集，所以RBAC3既有角色分层，也包括可以增加各种限制
***** RBAC扩展
****** 用户组：用户组分配角色，再把用户加入用户组。这样用户除了拥有自身的权限外，还拥有了所属用户组的所有权限
**** ABAC-基于属性的访问控制 (Attribute Based Access Control)
***** Attribute属性
***** Subject主体
***** Object资源
***** Operation对资源的操作
***** Policy权限规则
***** EnvironmentConditions条件上下文
@endmindmap
``` plantuml