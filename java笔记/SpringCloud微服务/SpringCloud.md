# 1. 微服务是什么

![1561020796638](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561020796638.png)

![1561021413905](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561021413905.png)



## 1.1 微服务Springcloud和dubbo的区别

==最大的区别:Spring Cloud抛弃了Dubbo 的RPC通信，采用的是基于HTTP的REST方式。==
==严格来说，这两种方式各有优劣。虽然在一定程度上来说，后者牺牲了服务调用的性能，但也避免了上面提到的原生RPC带来的问题。而且REST相比RPC更为灵活，服务提供方和调用方的依赖只依靠一纸契约，不存在代码级别的强依赖，这在强调快速演化的微服务环境下，显得更为合适。==

```
总结：
Dubbo和Spring Cloud并不是完全的竞争关系，两者所解决的问题域不一样：Dubbo的定位始终是一款RPC框架，而Spring Cloud的目的是微服务架构下的一站式解决方案。
非要比较的话，Dubbo可以类比到Netflix OSS技术栈，而Spring Cloud集成了Netflix OSS作为分布式服务治理解决方案，但除此之外Spring Cloud还提供了包括config、stream、security、sleuth等分布式服务解决方案。
当前由于RPC协议、注册中心元数据不匹配等问题，在面临微服务基础框架选型时Dubbo与Spring Cloud只能二选一，这也是两者总拿来做对比的原因。
Dubbo之后会积极寻求适配到Spring Cloud生态，比如作为SpringCloud的二进制通讯方案来发挥Dubbo的性能优势，或者Dubbo通过模块化以及对http的支持适配到Spring Cloud
```

![1561031936808](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561031936808.png)

## 1.2 微服务和微服务架构的区别

![1561026773691](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561026773691.png)



![1561026977116](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561026977116.png)



## 1.3 微服务的优缺点

**优点**

![1561027606837](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561027606837.png)

**缺点**

![1561027911604](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561027911604.png)



## 1.4 微服务的技术栈

![1561029090298](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561029090298.png)

![1561029108918](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561029108918.png)

# 2. SpringCloud

## 2.1 SpringCloud应用

**==一个微服务应用的架构图==**

![1561030479202](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561030479202.png)

![1561030427148](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561030427148.png)

![1561030536406](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561030536406.png)

**SpringCloud  是微服务架构下的一站式解决方案,是各个微服务架构落地技术的集合体,俗称微服务全家桶.**



## 2.2 SpringCloud和SpringBoot的关系

![1561030878776](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561030878776.png)



## 2.3 SpringCloud学习网站

官方文档中文版

http://springcloud.cc/spring-cloud-netflix.html

Api文档网站

http://springcloud.cc/spring-cloud-dalston.html

springcloud技术社区

http://springcloud.cn/

springcloud中文网

http://springcloud.cc/



# 3. 构建Springcloud工程

## 3.1 构建父工程 microservicecloud-parent

使用maven构建,该项目打包成pom文件,自定义

```pom
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.zxh.springcloud</groupId>
    <artifactId>microservice-parent</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
	
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.18</lombok.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-dependencies -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Dalston.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>1.5.19.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>5.0.4</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
            
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>1.0.13</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>1.3.0</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-core -->
            <dependency>
                <groupId>ch.qos.logback</groupId>
                <artifactId>logback-core</artifactId>
                <version>1.2.3</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/junit/junit -->
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
                <scope>test</scope>
            </dependency>
            <!-- https://mvnrepository.com/artifact/log4j/log4j -->
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
        


        </dependencies>
    </dependencyManagement>
    <modules>
        <module>microservice-api</module>
    </modules>

</project>
```

## 3.2 子工程microservicecloud-api

```pom
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <parent>
        <artifactId>micorservicecloud-parent</artifactId>
        <groupId>com.zxh.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../micorservicecloudparent/pom.xml</relativePath>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>micorservicecloud-api</artifactId>


    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
            <scope>provided</scope>
        </dependency>

    </dependencies>

</project>
```

引入lombok

```java
package com.zxh.springcloud.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.ToString;
import lombok.experimental.Accessors;

@Data
@AllArgsConstructor //创建一个全部参数的构造函数
@NoArgsConstructor	//创建一个没有构造函数
@Accessors(chain = true) //链式编程
@ToString	//tostring
public class Dept {


    private Long id;

    private String dname;//部门名称

    private String db_source;//来自哪个数据库,微服务架构可以一个服务对应一个数据库,同一个信息被存储到不同服务器上


}

```

## 3.3 micorservicecloud-provider-dept-8001

```pom

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.zxh.springcloud</groupId>
    <artifactId>micorservicecloud-parent</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>../micorservicecloudapi</module>
        <module>../micorservicecloudproviderdept8001</module>
    </modules>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.18</lombok.version>
        <project-version>1.0-SNAPSHOT</project-version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-dependencies -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Dalston.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>1.5.19.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>5.0.4</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>1.0.13</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>1.3.0</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-core -->
            <dependency>
                <groupId>ch.qos.logback</groupId>
                <artifactId>logback-core</artifactId>
                <version>1.2.3</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/junit/junit -->
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
                <scope>test</scope>
            </dependency>
            <!-- https://mvnrepository.com/artifact/log4j/log4j -->
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>

        </dependencies>
    </dependencyManagement>
    
</project>
```

mybatis主配置文件

```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <!--开启二级缓存!-->
        <setting name="cacheEnabled" value="true"/>
    </settings>
</configuration>
```

application.yml配置文件,springboot的主配置文件

```yaml
server:
  port: 8001
mybatis:
  config-location: classpath:mybatis/mybatis.cfg.xml
  type-aliases-package: com.zxh.springcloud.entities
  mapper-locations: mybatis/mapper/**/*.xml
spring:
  application:
    name: microservicecloud-dept   #非常重要,该配置就是对外暴露的服务2名称
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource  #Druid数据源
    driver-class-name: org.gjt.mm.mysql.Driver #mysql驱动包
    url: jdbc:mysql://localhost:3306/cloudDB01
    username: root
    password: 123456
    dbcp2:
      min-idle: 5                 #数据库连接池的最小维持连接数
      initial-size: 5             #初始化连接数
      max-total: 5                #最大连接数
      max-wait-millis: 200        #等待连接获取的最大超时时间



```



**然后就是他的dao,service,controller层方法**

**controller层方法 **      该服务器端口8001

```java
package com.zxh.springcloud.controller;


import com.zxh.springcloud.entities.Dept;
import com.zxh.springcloud.service.DeptService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
public class DeptController {


    @Autowired
    DeptService deptService;

    @PostMapping("/dept/add")
    //@RequestBody 将 HTTP 请求正文插入方法中，使用适合的 HttpMessageConverter 将请求体写入某个对象。
    public boolean add(@RequestBody  Dept dept){

        return  deptService.addDept(dept);
    }

    @GetMapping("/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id){

        return deptService.get(id);
    }

    @GetMapping("/dept/list")
    public List<Dept> list(){

        return deptService.list();
    }
}

```

## 3.4 子工程micorserviccloud-consumer-dept-80



**首先我们把Spring封装的Rest风格的模板注入到spring容器中**

```java
package com.zxh.springcloud.cfgbeans;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ConfigBean {

    /**
     * 把RestTemplate注入到spring容器中
     * @return
     */
    @Bean
    public RestTemplate getRestTemplate(){

        return new RestTemplate();
    }

}

```



==**使用rest风格来调用其他服务器的方法 **== 该服务器端口 80

```java

package com.zxh.springcloud.Controller;

import com.zxh.springcloud.entities.Dept;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
public class DeptController_Consumer {

	//rest的url地址前缀  调用8001端口的服务器,使用http请求rest风格调用
    public final static String REST_URL_PREFIX ="http://localhost:8001";
    /**
     *postForObject(url,requset,requestBody.class)
     * 使用rest风格的调用,url 请求url地址,
     * request 请求的参数,
     * requestBody.class  返回结果的字节码对象
     */
    //
    @Autowired
    private RestTemplate restTemplate;

    @RequestMapping("/consumer/dept/add")
    public boolean add(Dept dept){

        return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept,Boolean.class);
    }

    @RequestMapping("/consumer/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id){

        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id,Dept.class);
    }
    @RequestMapping("/consumer/dept/list")
    public List<Dept> list(){

        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
    }
}

```



# 4 Eureka: Springcloud的服务注册与发现的一组服务治理组件

**来自百度百科**

```text
Eureka是Netflix开发的服务发现框架，本身是一个基于REST的服务，主要用于定位运行在AWS域中的中间层服务，以达到负载均衡和中间层服务故障转移的目的。SpringCloud将它集成在其子项目spring-cloud-netflix中，以实现SpringCloud的服务发现功能。
Eureka包含两个组件：Eureka Server和Eureka Client。
Eureka Server提供服务注册服务，各个节点启动后，会在Eureka Server中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。
Eureka Client是一个java客户端，用于简化与Eureka Server的交互，客户端同时也就是一个内置的、使用轮询(round-robin)负载算法的负载均衡器。
在应用启动后，将会向Eureka Server发送心跳,默认周期为30秒，如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，Eureka Server将会从服务注册表中把这个服务节点移除(默认90秒)。
Eureka Server之间通过复制的方式完成数据的同步，Eureka还提供了客户端缓存机制，即使所有的Eureka Server都挂掉，客户端依然可以利用缓存中的信息消费其他服务的API。综上，Eureka通过心跳检查、客户端缓存等机制，确保了系统的高可用性、灵活性和可伸缩性。
```



**GitHub上面Eureka的解释**

![1561115760008](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561115760008.png)



## 4.1 Eureka的工作原理

**Eureka  C/S结构**

**==服务端 :Eureka-sever  注册中心,负责服务的注册以及发现==**

**==客户端: Eureka-client 也就是每一个服务提供者都是一个客户端,然后我们会把该服务注册到注册中心==**

![1561116298762](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561116298762.png)

**Eureka和Dubbo架构相比对就会发现非常相似,都是服务注册中心负责服务的注册以及发现,然后服务提供者注册服务到该注册中心中,服务消费者到注册中心找到服务并消费,然后拿到该rest风格地址直接调用.**

![1561116449361](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561116449361.png)

EurekaClinet 客户端

![1561116486457](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561116486457.png)

![1561116608912](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561116608912.png)



## 4.2 Eureka的应用  microservicecloud-eureka-7001

引入Eureka server端

```pom
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>micorservicecloud-parent</artifactId>
        <groupId>com.zxh.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../micorservicecloudparent/pom.xml</relativePath>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>micorservicecloud-eureka-7001</artifactId>

    <dependencies>
        <!--Eureka-server客户端  -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
        </dependency>
        <!--热部署工具,修改后立即生效-->

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>springloaded</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>
</project>
```



Eureka server 的配置文件,配置

```yaml
server:
  port: 7001
eureka:
  instance:
    hostname: localhost #设置Eureka的服务端的实例名称
  client:
    register-with-eureka: false #表示不向注册中心注册自己,默认是true ,表明自己是注册中心,不注册自己
    fetch-registry: false #表示自己端是注册中心,我的职责是维护服务实例,并不需要去检索服务,
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/ #设置Eureka server的外部访问地址

```



**当我们配置完成以后,我们需要启动该项目,并标注该项目为Eureka sever端,让springboot告诉其他微服务,该项目是注册中心,允许其他微服务注册.**

**使用标签来进行标识==@EnableEurekaServer==**

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer//当springboot启动的时候,告诉微服务该项目是Eureka服务端的程序,是注册中心,接收其他微服务注册
public class EurekaServer7001_App {

    public static void main(String[] args) {

        SpringApplication.run(EurekaServer7001_App.class,args);
    }
}

```

## 4.3 如何把其他微服务注册进Eureka服务端中

**首先修改pom文件,我们在需要注册微服务的pom文件中引入Eureka客户端以及配置**

```
<!--引入Eureka客户端  然后使该服务能够注册进Eureka服务端,也就是注册中心-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
```

**在修改服务的配置文件   加上eureka客户端 访问 服务端的地址 ,我们到该地址下,也就是eureka服务端暴露的地址下去注册我们的服务**

```yaml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
```



**然后在启动类上我们标注@EnbaleEurekaClient  然后就会自动把该微服务注册到服务中心**

```java
@SpringBootApplication
@EnableEurekaClient //启动Eureka客户端,然后该服务就会自动注册进注册中心
public class DeptProvider8001_APP {

    public static void main(String[] args) {

        SpringApplication.run(DeptProvider8001_APP.class,args);
    }
}
```

![1561120413525](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561120413525.png)

最后启动该微服务,我们就可以看见该微服务被注册进服务中心,服务名称在该服务中自定义



## 4.4 修改服务状态名称,修改服务在Eureka注册中心的status名称以及微服务的ip地址显示

![1561122296249](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561122296249.png)

**在服务提供者的yml文件中修改配置即可**

```yaml
eureka:  
  instance:
    instance-id: microservicecloud-dept8001   #修改服务status名称为该名称即可
     prefer-ip-address: true #设置ip地址显示,也就是设置在Eureka的注册中心的status中显示自己微服务的ip地址

```



## 4.5 在注册中心显示微服务的基本信息的配置

**也就是在注册中心的status中点击该服务连接能够查看到一些基本信息**

![1561192674777](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561192674777.png)

![1561192716425](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561192716425.png)

**首先在该微服务pom文件中引入boot Actuator组件,用于主管完善监控信息**

```pom

        <!--boot里面的组件  Actuator 主管监控信息完善,让注册中心中查看该微服务得人能够看到微服务的信息-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

```

**在到父项目中,也就是打包成pom文件的项目中引入maven插件**

```pom
 <!--在父项目中加入该工具,查找src/main/resources 下文件名带有$的,把该文件构建进项目中
    然后通过boot里面的 AActuator 主管监控信息完善,让注册中心中查看该微服务的人能够看到微服务的信息
    通过读取该文件名得到信息-->
    <build>
        <finalName>micorservicecloud-parent</finalName>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
            </resource>
        </resources>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <configuration>
                    <delimiters>
                        <delimit>$</delimit>
                    </delimiters>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

**最后再到该微服务中配置,也就是application.yml文件中配置 需要显示该微服务的基本信息即可**

==出现的问题:$project.artifactId$ 取不到本项目中坐标名   也取不到版本号?==

```yaml
  #定义本微服务的基本信息,用于方便给别人查看该服务是干什么的
info:
  app.name: zxh-micorservicecloudproviderdept-8001
  company.name: www.zxh.com
  build.artifactId: $project.artifactId$  #父工程中的构建工具读取resouce下的带有$的1文件
  build.version: $project.version$
```

## 4.6 Eureka的自我保护机制

==**默认当 *Eureka Server* 连续3次（默认心跳间隔是30s）没有收到该服务的心跳时，会自动将该实例注销（进入自我保护模式时除外）。**==
==**但是也可以通过手动发送 `DELETE` 请求到 *Eureka Server* 来注销服务实例。==**

格式如下：

```batch
curl -v -X DELETE http://{Eureka Server 地址}/eureka/apps/{Application 名}/{Eureka 实例的 ID}
```

==**自我保护模式:当Eureka server在运行期间会统计15分钟之内,心跳失败比例超过85%的服务提供者,如果有,Eureka就默认该服务网络出现故障,注册中心就会把这些服务保存起来,不会注销,但是也不会更新服务信息,等到一定时间网络状态良好之后,取消自我保护机制,并更新服务信息.**==

![1561193050014](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561193050014.png)

注意:除非特殊情况,要不然不要修改该配置,不要禁用自我保护机制

![1561193233697](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561193233697.png)



来自百度的描述

```text
Eureka自我保护的产生原因：
Eureka在运行期间会统计心跳失败的比例，在15分钟内是否低于85%,如果出现了低于的情况，Eureka Server会将当前的实例注册信息保护起来，同时提示一个警告，一旦进入保护模式，Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据。也就是不会注销任何微服务。
Kubernetes环境
但在Kubernetes环境下，如果某个节点的Kubelet下线，比较容易造成自我保护。阶段如下：

Kubelet下线，会造成大量的服务处于Unknow状态, Kubernetes为了维持Deployment指定的Pod数量，会在其他节点上启动服务，注册到Eureka,这个时候是不会触发自我保护的。
重新启动Kubelet进行让节点Ready,这时候Kubernetes发现Pod数量超过了设计值，然后Terminate原来Unknown的Pod,这个时候就会出现大量的服务下线状态，从而触发自我保护
而处于自我保护状态的Eureka不再同步服务的信息，同时也不再和另一个实例保持同步。
 这个是个比较核心的问题，如果这样的话，只能够手工删除Eureka实例让他重建，恢复正常状况。

所以在Kubernetes环境下，关闭服务保护，让Eureka和服务保持同步状态。

目前的解决办法

Eureka Server端：配置关闭自我保护，并按需配置Eureka Server清理无效节点的时间间隔。
eureka.server.enable-self-preservation# 设为false，关闭自我保护
eureka.server.eviction-interval-timer-in-ms # 清理间隔（单位毫秒，默认是60*1000）
 Eureka Client端：配置开启健康检查，并按需配置续约更新时间和到期时间 
eureka.client.healthcheck.enabled# 开启健康检查（需要spring-boot-starter-actuator依赖）
eureka.instance.lease-renewal-interval-in-seconds# 续约更新时间间隔（默认30秒）
eureka.instance.lease-expiration-duration-in-seconds # 续约到期时间（默认90秒）
 

原文如下， 我把结论翻译一下
我在自我保护方面的经验是，在大多数情况下，它错误地假设一些弱微服务实例是一个糟糕的网络分区。
自我保护永不过期，直到并且除非关闭微服务（或解决网络故障）。
如果启用了自我保留，我们无法微调实例心跳间隔，因为自我保护假定心跳以30秒的间隔接收。
除非这些网络故障在您的环境中很常见，否则我建议将其关闭（即使大多数人建议将其保留）。
```



## 4.7 Eureka服务发现

**什么是服务发现呢?就是服务提供者暴露一个接口,接口中有本微服务的一些基本信息,让其他消费者能够调用该接口来了解该服务的基本信息.**



**基本信息如下: 调用接口得到该信息**

```json
{"services":["microservicecloud-dept"],"localServiceInstance":{"host":"192.168.43.138","port":8001,"serviceId":"microservicecloud-dept","metadata":{},"uri":"http://192.168.43.138:8001","secure":false}}
```

**如何实现:首先我们在服务提供者中注入DiscoveryClient,然后我们暴露一个接口,返回该client,得到该微服务的基本信息**

```java
 @Autowired
    private DiscoveryClient client;


  @GetMapping("/dept/discovery")
    public Object discovery(){
        //获取Eureka注册中心上的所有服务
        List<String> list = client.getServices();
        System.out.println("---------"+list);
        //获取Eureka注册中心上的服务实例 参数传入服务名称  microservicecloud-dept
        List<ServiceInstance> instances = client.getInstances("MICROSERVICECLOUD-DEPT");
        for(ServiceInstance ele : instances){
            //然后获取该服务实例上的信息,比如服务id,也就是服务名称,服务主机号,端口号,以及服务地址
            System.out.println(ele.getServiceId()+"  "+ele.getHost()+"  "+ele.getPort()+"  "+ele.getUri());
        }
        return client;

    }

```

**还要在启动类上开启@EnableDiscoveryClient //开启服务发现**

```java
@SpringBootApplication
@EnableEurekaClient //启动Eureka客户端,然后该服务就会自动注册进注册中心
@EnableDiscoveryClient //开启服务发现
public class DeptProvider8001_APP {

    public static void main(String[] args) {

        SpringApplication.run(DeptProvider8001_APP.class,args);
    }
}
```



## 4.8 Eureka集群配置

**我们为什么要使用集群,当并发量过大时,我们一台服务器不够,我们就使用多个服务器,每个服务器上有相同的项目,这样保证我们服务的正常运转**

Eureka集群如何配置呢?

第一步:我们首先拷贝Eureka服务,拷贝三个

修改启动类名称,修改application.yml配置文件

需要修改的:hostname    我们使用域名映射:使用不同的host

**然后就是允许外部访问的url defaultzone:因为我们需要所有集群都要同步,所以我们需要访问其他集群.**

**其他集群地址:http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/** 

其他配置集群如下配置

```pom
server:
  port: 7001
eureka:
  instance:
    hostname: eureka7001.com #设置Eureka的服务端的实例名称


  client:
    register-with-eureka: false #表示不向注册中心注册自己,默认是true ,表明自己是注册中心,不注册自己
    fetch-registry: false #表示自己端是注册中心,我的职责是维护服务实例,并不需要去检索服务,
    service-url:
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/ #设置Eureka server的外部访问地址


```



**然后就是服务提供者修改配置,因为我们需要注册中心地址注册服务,然而集群之后,我们就需要配置所有集群地址,到所有集群地址里面注册我们的服务**

```pom
eureka:
  client:
    service-url:      					
    	defaultZone:
 http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/ #设置Eureka server的外部访问地址

```



## 4.9 Eureka与Zookeper的比对,好用在哪里

首先我们在比对之前,我们需要了解ACID CAP原则

在分布式系统,我们使用CAP原则

C(Consistency)强一致性

A(Availability)可用性

P(Partion telerance)分区容错性



Eureka使用AP原则,Zookeper使用CP原则

![1561203041323](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561203041323.png)

![1561203054621](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561203054621.png)



![1561203092661](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561203092661.png)



# 5 Ribbon(客户端负载均衡工具)

==**Ribbon是客户端负载均衡,也就是当用户需要访问服务的时候,我们按照某种算法(负载均衡算法)来访问服务**==

类比:去餐厅点餐窗口,有三个,一个全是人,一个少量人,一个没人,我们肯定是去没人的窗口点餐,这就是一种选择,也就是负载均衡算法在生活上的体现.

![1561206225516](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561206225516.png)

负载均衡:为了系统的高可用

![1561206253509](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561206253509.png)

负载均衡分为集中式(硬件)和进程式(软件)

![1561206310477](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561206310477.png)



![1561206342491](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561206342491.png)