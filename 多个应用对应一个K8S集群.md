# 多个应用对应一个K8S集群
* 什么是标签？

> 标签是一种数据特征。比如用户的年龄、性别、地区等。
> 
> 这些特征在数据中具有一定的通用性和价值。
> 
> 为什么说是“一种”数据特征呢，因为针对的目标不同，会有不同的标签，比如用户标签、订单标签、收货地址标签（办公or家庭）。但是订单、地址这些是无法营销落地的（无法闭环也就无法体现数据价值），所以最后还是会落实到用户标签上来。后面讲到的也都是用户标签。

``` plantuml 
@startuml

   node "aggregation" {
        interface - [orderListA] 
   }
   
   node "cacheServer" {
     [orderListA] ---> [redis] : 只读
  }
   
   node "idGeneratr"{
     [orderListA] -> [idGeneratrs]    
   }
   
    node "User" {
      [orderListA] --> [userList]
     
   }
   node "Order" {
      [orderListA] --> [orderList]  
   } 
   
  
   
   node "MQ" {
     [orderList] ---|> [rocketMQ] : 订单创建事件
      [rocketMQ] -|> [redis] : 消费订单创建事件
   }
   


database "orderdbs" {
  orderList -- orderdb     
}

database "userdbs" {
  userList -- userdb   
}



@enduml
```