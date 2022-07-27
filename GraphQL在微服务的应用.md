# GraphQL在微服务的应用
``` plantuml 
@startuml
   
   actor "Web端" as a1
   actor "移动端" as a2
   actor "桌面端" as a3
   
node "物资管理系统-SpringCloud" as n0 {
        [网关] as s1
        [配置服务] as s2
        [服务注册与发现] as s3
       [GraphQL-Scheam注册] as s4
        [业务原子服务一] as s5
        [业务原子服务二] as s6
        [业务原子服务三] as s7
        
        [BFF-WEB端服务] as s8
        [BFF-移动端服务] as s81
        [BFF桌面端服务] as s82
        
        [GraphQL行程服务] as s9
        [GraphQL仓库服务] as s91      
   }
   

   
a1 ---> s1 : 访问
a2 ---> s1 : 访问
a3 ---> s1 : 访问

s1 ---> s8

s8 ---> s9

s9 ---> s5
s9 ---> s6
s9 ---> s7

s5 ---> s4  : Scheam注册
s6 ---> s4 : Scheam注册
s7 ---> s4 : Scheam注册

s2 <-- s4 
s2 ---> s9 : 提供Scheam&服务信息上下文
s3 ---> s2

s5 ---> s3 : 服务注册
s6 ---> s3 : 服务注册
s7 ---> s3 : 服务注册 

s1 ---> s81 
s1 ---> s82

s91 --> s5
s91 --> s6
s91 --> s7

s81 ---> s9

s8 ---> s91
s81 ---> s91
s82 ---> s91

@enduml
```