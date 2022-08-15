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


package "Infrastructure" {
  component "<$master>\nmaster" as master
  component "<$etcd>\netcd" as etcd
  component "<$node>\nnode" as node
}
package "Kubernetes resources" {
  component "<$group>\ngroup" as group
  component "<$sa>\nsa" as sa
  component "<$user>\nuser" as user
  component "<$pv>\npv" as pv
  component "<$job>\njob" as job
  component "<$pod>\npod" as pod
  component "<$crb>\ncrb" as crb
  component "<$sts>\nsts" as sts
  component "<$ing>\ning" as ing
  component "<$role>\nrole" as role
  component "<$cm>\ncm" as cm
  component "<$netpol>\nnetpol" as netpol
  component "<$ns>\nns" as ns
  component "<$secret>\nsecret" as secret
  component "<$quota>\nquota" as quota
  component "<$rb>\nrb" as rb
  component "<$ep>\nep" as ep
  component "<$pvc>\npvc" as pvc
  component "<$c_role>\nc_role" as c_role
  component "<$cronjob>\ncronjob" as cronjob
  component "<$ds>\nds" as ds
  component "<$sc>\nsc" as sc
  component "<$svc>\nsvc" as svc
  component "<$deploy>\ndeploy" as deploy
  component "<$crd>\ncrd" as crd
  component "<$hpa>\nhpa" as hpa
  component "<$psp>\npsp" as psp
  component "<$vol>\nvol" as vol
  component "<$rs>\nrs" as rs
  component "<$limits>\nlimits" as limits
}
@enduml
```