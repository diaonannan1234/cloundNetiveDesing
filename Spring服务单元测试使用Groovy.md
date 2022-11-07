# Spring服务单元测试使用Groovy&Spock
Groovy是什么

Apache的Groovy是Java平台上设计的面向对象编程语言。这门动态语言拥有类似Python、Ruby和Smalltalk中的一些特性，可以作为Java平台的脚本语言使用，Groovy代码动态地编译成运行于Java虚拟机（JVM）上的Java字节码，并与其他Java代码和库进行互操作。由于其运行在JVM上的特性，Groovy可以使用其他Java语言编写的库。Groovy的语法与Java非常相似，大多数Java代码也符合Groovy的语法规则，尽管可能语义不同。 Groovy 1.0于2007年1月2日发布，并于2012年7月发布了Groovy 2.0。从版本2开始，Groovy也可以静态编译，提供类型推论和Java相近的性能。Groovy 2.4是Pivotal软件赞助的最后一个主要版本，截止于2015年3月。Groovy已经将其治理结构更改为Apache软件基金会的项目管理委员会
——维基百科
Spock是什么

1.Spock是一个可以应用于java或groovy的测试框架
2.在编写单元测试过程中去掉依赖的类/对象/资源，专注测试类本身，实现解耦
3.Spock测试框架基于Groovy并吸收了Junit、TestNG、Mockito等测试框架的优点
4.Spock编写的单元测试层次清晰，优雅，代码量少，可读性好
什么使用Groovy+spock

groovy是一种非常类似java的语言，但是语法更加轻，每句代码结尾不用分号结束，也不需要用public修饰，因为pubilc是默认的。
在spock中mock对象非常容易，只需要使用Mock(class)
Groovy可以使用字符串文本来命名方法，这个特性使测试方法更加容易被阅读和理解。
expect可以看做精简版的when+then
“>>” 表示模拟对象的返回值
“>>>” 表示同一方法多次按顺序调用返回不同值
下划线“_”表示匹配所有的输入值
Spock不使用Assert来校验结果，then声明后面的表达式result=="1"就相当于Junit中的Assert.assertTrue(result.equals("1"))
n * . 表示期望对对象的方法n次调用
groovy的方法命名可以是中文或者是 语句（this is query record）类似这种