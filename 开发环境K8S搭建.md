# 开发环境K8S搭建
``` plantuml 
@startuml
   
   actor "运维工程师" as run
   actor "产品工程师" as p
   actor "架构师" as pp
   actor "项目经理" as ppp
   actor "开发工程师" as eng
   actor "高级开发工程师" as engh
   actor "测试工程师" as teng
   node "工程师开发环境-单机" as n0 {
        [minikube] as m
        [kubectl] as k
        [helm] as h
        [docker_desk] as d
        d <. m : 依赖
        k --> m : 调用
        h --> m : 调用
   }
   
   node "多项目测试环境-单机" as n1 {
        [vagrant] as vagrant
                 
  }
   
node "多项目测试环境-高可用" as n2 {
        [kubeadm] as k0
   }
   
package "生产环境-高可用" {
   node "控制平面*1" as n31 {
        [apiServer*3]
        [etcd*3]
        [contorller]
        [schedular]
   }
   node "数据平面*N" as n32 {
      [kubelet]
      [kubeProxy]
      [pod*N]
      
   }  

 }
 
run --> n0 : 创建&维护
run --> n1 : 创建&维护
run --> n2 : 创建&维护
run --> n31 : 创建&维护
run --> n32 : 创建&维护

   node "开发公用主机" {
     [gitlab] as gitlab
     [私有镜像仓库] as r
     [apifox私有API及文档服务] as api
     [项目管理] as pm
     gitlab ..> r : 自动打包镜像
  }
   
  eng ..> gitlab : PR请求
  engh --> gitlab : 审核及合并PR请求
  p ..> api : 定义API
  pp ..> api: 定义API
  ppp ..> api: 定义API
  engh --> api : 细化API
  eng <- api : 实现API
  teng --> api : 测试API
  run --> r : 创建及加载初始镜像
@enduml
```