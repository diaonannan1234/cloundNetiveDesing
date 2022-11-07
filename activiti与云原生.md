# activiti与云原生
>
>流程也提供了上云的解决方案
>
>Activiti Cloud提供了基础构建模块的初始 Beta1 版本，包括的内容：
>
>>Activiti Cloud 运行时包；
>>
>>Activiti Cloud 查询；
>>
>>Activiti Cloud 审计；
>>
>>Activiti Cloud 连接器；

> Activiti Core 基于 Spring Boot 2.x +。
>
>> 计划采用 Spring 5.x ，使用背压、Flux、加快性能和开发效能。

>  云原生的已经从梦想来到现实、在我们身边不知不觉中，Spring、Activiti都在拥抱它。

``` plantuml 
@startuml
   
 node Activiti_cloud {
    [cloud-connectors] as 1 
    [cloud-messages-service]
    [cloud-modeling-service]
    [cloud-query-service]
    [cloud-runtime-bundle-service]
    [cloud-audit-service]
 } 
 
 node activiti_core as 21{
    [bpmn-converter]
    [bpmn-model]
    [activiti-engine]
    [process-validation]
    [json-converter]
 }
  node activiti_core1 as 22 {
    [bpmn-converter1]
    [bpmn-model1]
    [activiti-engine1]
    [process-validation1]
    [json-converter1]
 }
  node activiti_core2 as 23{
    [bpmn-converter12]
    [bpmn-model12]
    [activiti-engine12]
    [process-validation12]
    [json-converter2]
 }
 1 <-- 21
 22 ---> 1
 23 --> 1
@enduml
```