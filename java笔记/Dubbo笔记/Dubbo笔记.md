# **1.什么是分布式应用**

**分布式应用程序是指：应用程序分布在不同计算机上，通过网络来共同完成一项任务。通常为服务器/客户端模式**

**应用程序拆分成不同的模块,然后每个模块都是一个单体应用,模块与模块之间可以互相调用,通过远程过程调用(Rpc),来实现模块与模块之间的通信,这样的应用程序我们称之为分布式应用**



![1558746904068](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558746904068.png)

# 2.Dubbo的概念以及简单使用

## 2.1什么是Dubbo

Dubbo是一款高性能的java RPC框架,它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

是为了解决我们分布式应用中各个服务之间的调度,方便管理各个服务的调度的一个服务治理框架



## 2.2 Dubbo的优点



- ####  面向接口代理的高性能RPC调用

  提供高性能的基于代理的远程调用能力，服务以接口为粒度，为开发者屏蔽远程调用底层细节。

- 

  #### 智能负载均衡

  内置多种负载均衡策略，智能感知下游节点健康状况，显著减少调用延迟，提高系统吞吐量。

- 

  #### 服务自动注册与发现

  支持多种注册中心服务，服务实例上下线实时感知。

- 

  #### 高度可扩展能力

  遵循微内核+插件的设计原则，所有核心能力如Protocol、Transport、Serialization被设计为扩展点，平等对待内置实现和第三方实现。

- 

  #### 运行期流量调度

  内置条件、脚本等路由策略，通过配置不同的路由规则，轻松实现灰度发布，同机房优先等功能。

- 

  #### 可视化的服务治理与运维

  提供丰富服务治理、运维工具：随时查询服务元数据、服务健康状态及调用统计，实时下发路由策略、调整配置参数。



## 2.3 Dubbo的治理流程

==如图:初始化阶段,我们Dubbo容器先启动进行初始化,然后把服务注册到注册中心中,然后服务消费者来到服务中心查看需要需要订阅的服务,异步的服务中心也会把回应消费者需要订阅的服务的服务状况,然后我们通过dubbo的负载均衡机制来调用该服务,最后呢.Dobbo监控中心记录我们调用的过程以及详细信息==

![1558748189712](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558748189712.png)

## 2.4如何使用dubbo

**安装zookeeper,安装dubbo服务治理Monitor**

**在项目中如何使用dubbo**

**首先你需要把你的项目进行服务接口化,也就是把你的所有接口都放入到一个项目中,然后你其他服务引入该项目**

**然后在你的项目中引入dubbo   引入注册中心,我们引入的是zookeeper注册中心**

```xml
 <!-- https://mvnrepository.com/artifact/com.alibaba/dubbo -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo</artifactId>
            <version>2.6.2</version>
        </dependency>
		<!--zookeeper的连接客户端程序-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>2.12.0</version>
        </dependency>
```



最后配置dubbo即可

==1.服务者的配置==

**根据步骤图来配置 我们先要启动dubbo,然后向注册中心注册服务**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd        http://dubbo.apache.org/schema/dubbo        http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- 提供方应用信息，用于计算依赖关系,也就是把该服务起一个名字,项目中每个服务的名字不能相同 -->
    <dubbo:application name="user-service-provider"  />
	
    <!-- 使用zookeeper注册中心暴露服务地址 -->
    <dubbo:registry address="zookeeper://127.0.0.1:2181" />

    <!-- 用dubbo协议在20880端口暴露服务  指定通信协议,以及通信协议的端口-->
    <dubbo:protocol name="dubbo" port="20880" />

    <!-- 声明需要暴露的服务接口   然后ref引入它的真正的实现-->
    <dubbo:service interface="service.UserService" ref="userServiceImpl" />

    <!-- 本地服务的真正实现, -->
    <bean id="userServiceImpl" class="com.zxh.service.impl.UserServiceImpl" />
</beans>
```



**启动该项目,通过dubbo服务管理系统查看该服务的状态**

![1558755163850](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558755163850.png)

![1558755201112](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558755201112.png)



==2.消费者的配置==

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo" xmlns:context="http://www.springframework.org/schema/context"

       xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd        http://dubbo.apache.org/schema/dubbo        http://dubbo.apache.org/schema/dubbo/dubbo.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="order-service-consumer"  />

    <!-- 使用zookeeper注册中心暴露发现服务地址 -->
    <dubbo:registry address="zookeeper://127.0.0.1:2181" />

    <!-- 生成远程服务代理，也就是指定需要消费的服务-->
    <dubbo:reference id="userService" interface="service.UserService"  />

    <!--包扫描-->
    <context:component-scan base-package="com.zxh.service.impl"/>
</beans>
```

![1558840544393](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558840544393.png)

## 2.5 开启dubbo的监控中心系统

在官网下载该ops项目

![1558843520052](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558843520052.png)

然后使用maven命令

```shell
mvn package   #打包该项目  然后修改配置,启动项目即可访问监控中心系统
```

![1558843584697](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558843584697.png)

如何在项目中引入监控中心,让监控中心与我们项目相结合

```xml
  <!--连接监控中心  通过注册中心自动连接-->
    <dubbo:monitor address="register"></dubbo:monitor>

    <!--通过地址来找到注册中心连接
    <dubbo:monitor address="127.0.0.1:7001"></dubbo:monitor>-->
```

## 2.6 Dubbo的配置文件的启动顺序

![1558920940427](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558920940427.png)

配置文件是虚拟机参数优先,然后是.xml优先,再然后是dubbo.xml

## 2.7 如何在启动服务时不去检查服务是否在注册中心中,或者是需要消费的服务是否在注册中心中

使用check参数来设置,默认缺省配置是ture,启动时检查,如何不通过则该项目启动失败,所以我们一般是需要设置其为false的

![1558921115217](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558921115217.png)

 

## 2.8 Dubbo的超时时间以及重试次数

首先超时时间是设置当该服务被调用的时候超过多少时间被调用完成就报错

重试次数,当超时之后,我们还需要调用多少次该服务.

```xml
<!--超时时间可以在服务提供方,消费方来书写  其他的配置也都一样
	但是规则是,1.精确优先(方法优先,接口级次之,全局配置最后
			 2.同一级别,消费方优先
-->
<dubbo:reference>
<dubbo:method  name="timeout"  value="1000" />
 </dubbo:reference>
<!--
	重试次数设置规则
	幂等(设置重试次数)[查询.删除.修改]  就是每次执行的结果都不会改变就是幂等操作,该操作可以设置重试次数
	非幂等(不设置) [新增]  每次执行的结果都不一样,该操作不能设置重试次数
-->
<dubbo:reference>
<dubbo:method  name="retries"  value="3" />
</dubbo:reference>
<!--
-->
```

==重试次数还有一条规则,在dubbo中,如果该服务存在于多台服务器或者多个端口时,如果访问主服务器超时之后,我们重试一次还是失败我们就会访问从服务器或者是其他端口,不会在一棵树上掉死==

## 2.9 Dubbo的多版本机制

我们服务的提供方的服务可能存在升级,也就是可能存在一个服务有多个不同功能的版本,我们如何指定该服务呢

在dubbo中有version这个参数,当设置接口暴露的时候,我们可以设置version参数

提供者配置

```xml
    <!--设置该服务的version版本-->
	<!-- 声明需要暴露的服务接口   然后ref引入它的真正的实现-->
    <dubbo:service interface="service.UserService" ref="userServiceImpl1"
    version="0.0.1"/>
	

    <!-- 本地服务的真正实现, -->
    <bean id="userServiceImpl1" class="com.zxh.service.impl.UserServiceImpl2" />
```

消费者配置

```xml
<!--然后在消费者中指定需要的版本服务  
	我们也可以在消费者中配置  version="*"  表示调用任意版本的服务
-->
<!-- 生成远程服务代理，也就是指定需要消费的服务-->
    <dubbo:reference id="userService" interface="service.UserService"  version="0.0.1"/>
```

## 2.10 Dubbo的本地存根概念以及使用

**本地存根:首先本地存根的出现是为了提供方想要执行一些逻辑验证而出现的,也就是当消费者调用提供方的服务的时候,先调用本地存根方法进行一些逻辑上面的验证,如果不通过,直接就不会调用服务了,我们可以返回一些提示信息等等,可以提高容错率,而不用一味的支持消费者的调用,及时错误的调用也要支持.**

本地存根的使用,本地存根一般在写在接口项目中

```java
package stub;

import bean.User;
import com.sun.org.apache.xpath.internal.operations.String;
import com.sun.xml.internal.ws.util.StringUtils;
import service.UserService;

import java.util.Arrays;
import java.util.List;

public class UserServiceStub implements UserService {

    private final UserService userService;

    public UserServiceStub(UserService userService){

        this.userService = userService;
    }

    public List<User> getUser(Integer id) {

        System.out.println("本地存根");
        if(id == null){
            System.out.println("没有参数哟");
        }
        List<User> user = userService.getUser(1);
        return  user;
    }
}

```

```xml
<!--
	想要本地存根生效,必须配置在需要的服务上 stub参数,
-->
<!-- 声明需要暴露的服务接口   然后ref引入它的真正的实现-->
    <dubbo:service interface="service.UserService" ref="userServiceImpl1"
    version="0.0.1" stub="stub.UserServiceStub"/>
```



消费者调用提供方接口,如果有本地存根,我们就调用本地存根



# 3. Dubbo的高可用

## 3.1 zookeeper宕机与dubbo直连



![1558926323589](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558926323589.png)

**当注册中心出现宕机的情况,我们消费者仍然是可以调用提供者的服务的,因为当第一次消费者调用完成的时候,dubbo会把服务者的url等信息记录下来,即使注册中心宕机,我们仍然可以使用本地缓存的这些数据进行通讯.**

**还有我们也可以使用dubbo直连的方式进行访问服务,也就是可以不用注册中心,直接使用dubbo进行服务的远程调用,使用一个叫做url参数,消费者通过该参数,里面写上服务者暴露服务的接口,然后我们就可以使用dubbo直连方式进行服务的远程调用**



## 3.2 Dubbo的负载均衡策略

![1558927933756](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558927933756.png)



![1558927971046](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558927971046.png)



**权重的随机负载均衡机制:在每台提供者的服务上面设置权重,然后所有相同服务权重的和的比例来进行负载均衡的**

==也就是如图:三台机器服务相同,权重和尾350==  

​			==当消费者调用服务的时候,有2/7的概率会调用第一台服务器上的服务==

​			==4/7的概率调用第二台服务器上的服务==

​			==1/7的概率调用第三台服务器上的服务==

![1558928073868](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558928073868.png)

**基于权重的轮询负载均衡**

​	==如图:==

​		==轮询就是每次请求都是从第一台服务器开始依次进行访问,但是基于权重会分配每台服务器接收请求的次数==

​		==如图:如果一次进来7次请求,根据权重  因为权重决定访问的次数,第一台服务器只能访问2/7数量的请求,第二胎服务器只能访问4/7数量的请求,第三台服务器只能访问1/7数量的请求,但是访问按照轮询机制.==

​		==第一次请求: 访问服务器1==

​		==第二次请求:访问服务器2==

​		==第三次请求:访问服务器3==

​		==第四次请求: 访问服务器1==

​		==第五次请求:访问服务器2==

​		==第六次请求:访问服务器2==

​		==第七次请求:访问服务器2==

![1558928409892](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558928409892.png)



**最小活跃数-负载均衡机制: 每次都查看每台服务器上的上次调用服务所花费的时间,然后找到花费最少的时间的服务器进行服务的调用.**

![1558930055675](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558930055675.png)

**一致性hash负载均衡机制:每个服务器都有一个hashcode值,然后每次请求调用的时候带上参数hash值,我们就会调用相对应的服务器的服务.**

![1558930214194](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558930214194.png)

![1558930543290](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558930543290.png)

## 3.3 Dubbo服务降级处理

![1558931115570](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558931115570.png)

**服务降级是为了提高服务器的容错率,当并发量过大的时候,服务器请求响应非常缓慢,我们可以降级一些不重要的服务,保证核心服务的正常运行,从而达到高可用,提高系统的稳定性.**

==dubbo的服务降级策略有俩种==

![1558931283090](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558931283090.png)

使用dubbo admin来进行服务的降级处理.

![1558931459633](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558931459633.png)



## 3.4 Dubbo的集群容错模式

![1558931751579](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558931751579.png)

![1558931781008](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558931781008.png)



dubbo中如何配置集群容错模式

在服务的配置上配置设定容错模式即可  **dubbo的默认集群容错模式为Failover Cluster 失败自动切换模式**

```xml
<dubbo:service  cluster="Falisafe Cluster"/> 
```



**整合该hystrix来进行服务容错**

消费者和服务者都需要导入这个包

![1558932443255](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558932443255.png)

**然后在 消费者和服务者启动类上贴上开启容错模式 @EnableHystrix**

![1558932502583](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558932502583.png)

**在提供者服务方法上面贴上HestrixCommand标签即可**

![1558935014110](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558935014110.png)

**当消费者调用可能出错的服务的时候,我们可以设置当出错之后我们需要做的处理**

在消费者服务实现方法中贴上@HystrixCommand  

​	然后指定fallbakcMethod参数 :里面写上名字  就是调用失败之后的回调方法

​	然后我们再创建该方法写上回调之后的逻辑即可.	

​	

![1558932997958](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558932997958.png)

![1558933245926](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558933245926.png)



# 4. Dubbo原理

## 4.1 RPC远程调用原理

![1558935279756](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558935279756.png)

![1558935294933](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558935294933.png)

