# K5s使用流程

> 业务服务对应着OAM的一个App,k8s的Deploy/cm/pod/service的一个组合体。
>
>
>
>
>
>
>
>
``` plantuml 
@startuml
   
   actor "平台运维工程师" as a4
   actor "产品经理" as a1
   actor "项目经理" as a2
   actor "开发工程师" as a3
package 应用逻辑平面 as app0 {
    node "业务服务一" as n1 {
       [分布式ID] as p0  
    }
        node "业务服务二" as n2 {
       [分布式ID] as p1  
    }
        node "业务服务三" as n3 {
       [分布式ID] as p2  
    }
       
        node "应用文档管理" as n6 {
       [分布式ID] as p3  
    }
        node "应用项目任务管理" as n7 {
       [分布式ID] as p4  
    }
}

package K5S控制平面 as appk5s1{
     node "所有项目任务管理" as n21 {
         [项目计划] as p10
         [项目任务分配] as p101
         [项目组织成员] as p102  
    }
        node "所有项目文档管理" as n22 {
          [文档目录] as p11
          [文档模板管理] as p110
          [文档生成] as p111  
    }
        node "所有项目API管理" as n23 {
            [分布式ID] as p12 
    }
    
    node "应用管理" as n24 {
       [应用创建] as p13
       [应用服务创建] as p130  
    }
    
      [CI与CD] as m0
      [调度管理] as m1
}

package K8S集群数据平面 as appk8s1 {
        node "命名空间一" as namespaces1 {
       [业务服务一] as service10
       [业务服务二] as service11
       [业务服务三] as service12
    }
    
    node "命名空间二" as namespaces {
       [文档管理] as service2
       [项目任务管理] as service1
    }
    
}

a4 --> appk5s1 : 运维与管理
a4 --> appk8s1 : 运维与管理

a3--> p130 : 创建

n22 --> service2
n21 --> service1

a2 --> n21 : 制订项目计划，发布项目任务
n21 --> n7 : 分配
n1 --> service10
n2 --> service11
n3 --> service12


a1 --> p13 : 创建应用
p13 --> app0 : 系统自动创建应用、分配空间、资源。
p13 --> n6 : 系统自动创建、必选。
p13 --> n7 : 系统自动创建、必选。 

n6 --> n24: 注册
n7 --> n23 : 注册 
@enduml
```