# 					Spring Cloud笔记

## 零、SpringCloud教程地址

**Spring Cloud教程地址**

spring cloud 中文文档：https://springcloud.cc/spring-cloud-netflix.html

![img](D:\软件\腾讯\有道云笔记\有道云笔记数据\qqCEF95AFC5CA0532DB9887F977262334B\d6e086e64b4d4b6b87f471167945c0cc\clipboard.png)

spring cloud教程地址：https://springcloud.cc/spring-cloud-dalston.html

![img](D:\软件\腾讯\有道云笔记\有道云笔记数据\qqCEF95AFC5CA0532DB9887F977262334B\b27cbc32b26e498396cf419c8b0bf243\clipboard.png)

spring cloud中国社区：https://springcloud.cn/

spring cloud中文网：https://springcloud.cc/

![img](D:\软件\腾讯\有道云笔记\有道云笔记数据\qqCEF95AFC5CA0532DB9887F977262334B\a7c62eb64c7a463d9f264d33ca67122e\clipboard.png)

## **一、从面试题开始**

### **1.1、什么是微服务**

Spring Cloud是一系列框架的有序集合。它利用Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、智能路由、消息总线、负载均衡、断路器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。Spring Cloud并没有重复制造轮子，它只是将各家公司开发的比较成熟、经得起实际考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。

### **1.2、Spring Cloud 和dubbo区别?**

（1）服务调用方式 dubbo是RPC springcloud Rest Api

（2）注册中心,dubbo 是zookeeper springcloud是eureka，也可以是zookeeper

（3）服务网关,dubbo本身没有实现，只能通过其他第三方技术整合，springcloud有Zuul路由网关，作为路由服务器，进行消费者的请求分发,springcloud支持断路器，与git完美集成配置文件支持版本控制，事物总线实现配置文件的更新与服务自动装配等等一系列的微服务架构要素。

### **1.3、SpringBoot和SpringCloud的区别？**

SpringBoot专注于快速方便的开发单个个体微服务。

SpringCloud是关注全局的微服务协调整理治理框架，它将SpringBoot开发的一个个单体微服务整合并管理起来，

为各个微服务之间提供，配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等等集成服务

SpringBoot可以离开SpringCloud独立使用开发项目， 但是SpringCloud离不开SpringBoot ，属于依赖的关系

SpringBoot专注于快速、方便的开发单个微服务个体，SpringCloud关注全局的服务治理框架。

### **1.4、什么是 Hystrix？它如何实现容错？**

- Hystrix 是一个延迟和容错库，旨在隔离远程系统，服务和第三方库的访问点，当出现故障是不可避免的故障时，停止级联故障并在复杂的分布式系统中实现弹性。
- 由于某些原因，employee-consumer 公开服务会引发异常。在这种情况下使用Hystrix 我们定义了一个回退方法。如果在公开服务中发生异常，则回退方法返回一些默认值。

### **1.5、微服务优缺点**

- **优点：**

产出于Spring大家族，Spring在企业级开发框架中无人能敌，来头很大，可以保证后续的更新、完善

组件丰富，功能齐全。Spring Cloud 为微服务架构提供了非常完整的支持。例如、配置管理、服务发现、断路器、微服务网关等；

Spring Cloud 社区活跃度很高，教程很丰富，遇到问题很容易找到解决方案

服务拆分粒度更细，耦合度比较低，有利于资源重复利用，有利于提高开发效率

可以更精准的制定优化服务方案，提高系统的可维护性

减轻团队的成本，可以并行开发，不用关注其他人怎么开发，先关注自己的开发

微服务可以是跨平台的，可以用任何一种语言开发

适于互联网时代，产品迭代周期更短

- **缺点：**

微服务过多，治理成本高，不利于维护系统

分布式系统开发的成本高（容错，分布式事务等）对团队挑战大

### **1.6、Eureka和Zookeeper区别**

分布式系统的(CAP)原则的三个指标 

1、Consistency	一致性 在分布式系统中的所有数据备份，在同一时刻是否同样的值。（等同于所有节点 访问同一份最新的数据副本） 

2、Availability	可用性 在集群中一部分节点故障后，集群整体是否还能响应客户端的读写请求。（对数据 更新具备高可用性） 

3、Partition tolerance	分区容错性 以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据 一致性，就意味着发生了分区的情况，必须就当前操作在C和A之间做出选择，（自己的理解：当某些节点出现故障，其他依然可以正常访问）

- eureka：基于AP

每一个微服务中都有eureka client，用于服务的注册于发现

每一个微服务启动的时候，都需要去eureka server注册

当A服务需要调用B服务时，需要从eureka服务端获取B服务的服务列表，然后把列表缓存到本地，然后根据ribbon的客户端负载均衡规则，从服务列表中取到一个B服务，然后去调用此B服务 当A服务下次再此调用B服务时，如果发现本地已经存储了B的服务列表，就不需要再从eureka服务端获取B服务列表，直接根据ribbon的客户端负载均衡规则，从服务列表中取到一个B服务，然后去调用B服务

（自己的理解：Eureka优先保证可用性，Eureka各个节点是平等的，某几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务，而Eureka的客户端在向某个Eureka注册如果发现连接失败，则会自动切换至其他节点，要有一台还在，就能保证注册服务可用，只是查询的信息可能不是最新的。）

- zookeeper：基于CP

zookeeper也可以作为注册中心，用于服务治理（zookeeper还有其他用途，例如：分布式事务锁等）

每启动一个微服务，就会去zk中注册一个临时子节点，

每当有一个服务down机，由于是临时接点，此节点会立即被删除，并通知订阅该服务的微服务更新服务列表 （zk上有watch，每当有节点更新，都会通知订阅该服务的微服务更新服务列表） 每当有一个新的微服务注册进来，就会在对应的目录下创建临时子节点，并通知订阅该服务的微服务更新服务列表 （zk上有watch，每当有节点更新，都会通知订阅该服务的微服务更新服务列表）

（自己的理解：在Zookeeper中，当master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举，，但是问题在于，选举leader需要一定的时间，且选举期间整个Zookeeper集群都是不可用的。）

### 1.7、Ribbon 与 Nginx 的 区 别

​        Ribbon 是客户端的负载均衡工具，Nginx是服务端的负载均衡工具，而客户端负载均衡和服务端负载均衡最大的区别在于服务清单所存储的位置不同，在客户端负载均衡中，所有客户端节点下的服务端清单，需要自己从服务注册中心上获取，比如 Eureka 服务注册中心。同服务端负载均衡的架构类似，在客户端负载均衡中也需要心跳去维护服务端清单的健康性，只是这个步骤需要与服务注册中心配合完成。在 Spring Cloud 中， 由于 Spring Cloud 对Ribbon 做了二次封装，所以默认会创建针对 Ribbon 的自动化  

​		服务端的负载均衡提前配置好，要连接哪几台机器就已经知道了，客户端的负载均衡需要一个地址，从哪个地方获取那些地方需要负载均衡。

 Spring Cloud 中，Ribbon 主要与RestTemplate 对象配合起来使用，Ribbon会自动化配置 RestTemplate 对象，通过@LoadBalanced 开启 RestTemplate 对象调用时的负载均衡。

客户端需要调用服务端时，通过restTemplate调用ribbon调用服务端



## **二、微服务概述**

### 2.1、什么是微服务？

​		Spring Cloud是一系列框架的有序集合。它利用Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、智能路由、消息总线、负载均衡、断路器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。Spring Cloud并没有重复制造轮子，它只是将各家公司开发的比较成熟、经得起实际考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。

### 2.2、微服务架构

​		微服务架构是一种架构模式或者说是一种架构风格，它提供单一应用程序划分成一组小的服务，每个服务运行在独立的自己的进程中，服务之间互相协调、互相配合，为用户提供最终价值，服务之间采用轻量级的通信机制互相沟通（通常是HTTP的RESTfulAPI）。每个服务都围绕着具体业务进行构建，并且能够独立的部署到生产环境，类生产环境等。另外，应尽量避免统一的、集中的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建，可以有一个非常轻量级的集中式管理来协调这些服务，可以使用不同的语言来编写服务，也可以使用不同的数据存储。

### 2.3、微服务核心

​		微服务化的核心就是将传统的一站式应用，根据业务拆分成一个一个的服务，彻底地去耦合，每一个微服务提供对单个业务功能的服务，一个服务做一件事，从技术的角度看就是一种小而独立的处理过程，类似进程概述，能够自行单独启动或摧毁，拥有自己独立的数据库。

### 2.4、微服务

强调的是服务的大小，它关注的是某一个点，是具体解决某一个问题/提供落地对应服务的一个服务应用，狭意的看，可以看做Eclipse里面的一个微服务工程/或者Module

### 2.5、微服务技术栈

| 功能                                     | 技术                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| 服务接口调用（客户端调用服务的简化工具） | Feign等                                                      |
| 消息队列                                 | Kafka，RabbitMQ，ActiveMQ等                                  |
| 服务配置中心管理                         | SpringCluodConfig、Chef等                                    |
| 附录路由（API网关）                      | Zuul等                                                       |
| 服务监控                                 | Zabbix、Nagios、Metrics、Spectator等                         |
| 全链接追踪                               | Zipkin，Brave，Dapper等                                      |
| 服务部署                                 | Docker、OpenStack、Kubernetes等                              |
| 数据流操作开发包                         | SpringCluod Stream（封装与Redis，Rabbit、Kafka等发送接收消息 |
| 事件消息总线                             | Spring Cloud Bus                                             |

### 2.6、微服务技术

| 技术                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| Netflix             | 提供多个开源框架                                             |
| Eureka              | 基于REST服务的分布式中间件，主要用于服务管理                 |
| Hystrix             | 容器框架，通过添加延迟赋值以及容器的逻辑，帮助我们控制分布式系统间的交互 |
| Feign               | REST客户端，目的是为了简化WebService客户端的开发             |
| Rubbon              | 负载均衡框架，在微服务集群为各个客户端的通信提供支持，主要实现中间层应用程序的负载均衡。 |
| Zuul                | 为微服务集群提供代理，过滤，路由等功能。                     |
| Spring Cloud Config | 分布式配置中心。                                             |
| Spring Cloud Sleuth | 服务跟踪框架，可以与Zipkin，Apache，HTrace，ELK等数据分析，服务跟踪系统进行整合。 |
| Spring Cloud Stream | 用于构建消息驱动服务的框架。                                 |
| Spring Cloud Bus    | 连接消息代理集群消息总线。                                   |
| RestAPI             | 服务调用方式                                                 |
| 服务监控            | Spring Boot Admin                                            |

### 2.7、微服务

**什么是服务注册？**

将服务所在主机、端口、版本号、通信协议等信息登记到注册中心上。

**什么是服务发现？**

服务发现：服务消费者向注册中心请求已经登记的服务列表，然后得到某个服务的主机、端口、版本号、通信协议等信息，从而实现对具体服务的调用。

**Eureka是什么？**

Eureka是一个服务治理组件，它主要包括服务注册和服务发现，主要用来搭建服务注册中心。

Eureka是一个基于REST的服务，用来定位服务，进行中间层服务器的负载均衡和故障转移。

Eureka采用C-S的设计架构，Eureka客户端和Eureka服务端 

有了Eureka注册中心，系统的维护人员就可以通过Eureka Server来监控系统中各个微服务是否正常运行。

### 2.8、spring cloud整体架构

![1600227309457](C:\Users\ly138\AppData\Roaming\Typora\typora-user-images\1600227309457.png)

Service Provider： 暴露服务的服务提供方。

Service Consumer：调用远程服务的服务消费方。

EureKa Server： 服务注册中心和服务发现中心。

### 2.9、使用父级maven配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.dingyi</groupId>
    <artifactId>microservice-cloud-01</artifactId>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>../microservice-cloud-02-api</module>
        <module>../microvice-cloud-03-provider-product8001</module>
    </modules>
    <!--手动指定pom-->
    <packaging>pom</packaging>
    <!--springboot版本  2.0.7-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.3.RELEASE</version>
    </parent>
    <properties>
        <project.build.sourceEncoding>utf-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <!--spring Cloud  最新的稳定版  Finchley SR2-->
        <spring-cloud.version>Finchley.SR2</spring-cloud.version>
    </properties>
    <!--依赖声明-->
 <dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
 <!--maven不支持多继承，使用import来依赖管理配置-->
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>
        <!--数据源-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.12</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.15</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>
    </dependencies>
 </dependencyManagement>
</project>
```







DROP DATABASE IF EXISTS cloudDB01; CREATE DATABASE cloudDB01 CHARACTER SET UTF8; USE cloudDB01; CREATE TABLE dept {    deptno BIGINT NULL PRIMARY AUTO_INCREAMENT,    dname VARCHAR(60),    db_source VARCHAR(60) }; INSERT INTO dept(dname,db_source) VALUE('开发部',DATABASE()); INSERT INTO dept(dname,db_source) VALUE('人事部',DATABASE()); INSERT INTO dept(dname,db_source) VALUE('财务部',DATABASE()); INSERT INTO dept(dname,db_source) VALUE('市场部',DATABASE()); INSERT INTO dept(dname,db_source) VALUE('运维部',DATABASE()); SELECT * FROM dept; 



## 三、SpringCloud快速开发入门

模块说明：

Service Provider：暴露服务的服务提供方

Service Consumer：调用远程的服务消费方

EureKa Server：服务注册中心和服务发现中心

### 3.1、搭建和配置一个服务提供者

1.创建一个SpringBoot工程，并且添加SpringBoot相关依赖

项目名为：01-springcloud-service-provider

Group为：com.xzy.springcloud

父类pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--springboot的父级依赖-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    
    <!--网关项目的gav坐标-->
    <groupId>com.xzy.springcloud</groupId>
    <artifactId>01-springcloud-service-provider</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>01-springcloud-service-provider</name>
    <description>Demo project for Spring Boot</description>

    <!--Maven的属性配置-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>
    </properties>


    <dependencies>
        <!--springboot开发web项目的起步依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--springboot测试依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!--springboot打包编译插件-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

2.创建服务提供者的访问方法，也就是后续消费者如何访问提供者；

```java
@RestController
public class HelloController {
    @RequestMapping(value = "/service/hello" ,method = RequestMethod.GET)
    public String hello(){
        return "Hello Spring Cloud";
    }
}
```

### 3.2、搭建和配置一个服务消费者

项目名：02-springcloud-service-consumer

1.创建一个名为config的包，包下创建一个BeanConfig类

```java
//@Configuration:等价于一个Spring的applicationContext.xml配置文件
@Configuration
public class BeanConfig {
    /**
    * @Bean:等价于
     * <bean id="restTemplate" class="com.xzy.springcloud.config.RestTemplate"></bean>
    * @return:
    */
    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

2.创建一个ConsumerController的类

```java
@RestController
public class ConsumerController {
    @Autowired
    private RestTemplate restTemplate;

    @RequestMapping(value = "/web/hello" ,method = RequestMethod.GET)
    public String hello(){
        //调用SpringCloud的服务提供者
        return restTemplate.getForEntity("http://localhost:8080/service/hello",String.class).getBody();
    }
}
```

### 3.3、搭建与配置注册中心Eureka

Eureda，Consul，Zookeeper

1.创建项目名为：03-springcloud-eureka-server

添加依赖maven：idea中，选中Cloud Discovery目录下的Eureka Server

2.添加eureka依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/POM/4.0.0"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.xzy.springcloud</groupId>
    <artifactId>03-springcloud-eureka-server</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>03-springcloud-eureka-server</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
        <spring-cloud.version>Hoxton.SR8</spring-cloud.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <!--指定maven仓库-->
    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

</project>

```

3.在入口处添加配置

```java
@SpringBootApplication
@EnableEurekaServer // 声明这个应用是一个EurekaServer
public class EurekaDemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(EurekaDemoApplication.class, args);
	}
}
```

4.在application.yml文件中配置Eureka服务注册中心

```yml
server:
  port: 8761 # 端口

spring:
    application:
      name: eureka-server # 配置服务名称，会在Eureka中显示

eureka:
  instance:
    hostname: localhost #设置该服务注册中心的hostname
  client:
    register-with-eureka: false # 是否注册自己的信息到EurekaServer，默认是true
    fetch-registry: false # 是否拉取其它服务的信息，默认是true
    service-url: # EurekaServer的地址，现在是自己的地址，如果是集群，需要加上其它Server的地址。
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka
```

5、启动与测试服务，访问 http://localhost:8761/

DS Replicas：DS副本

leases:租约，租契

### 3.4、向Eureka服务注册中心注册服务

Eureka注册中心，步骤如下：

1.在该服务提供者中添加eureka的依赖，因为服务提供者向注册中心注册服务，需要连接eureka，所以需要eureka客户端的支持。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--springboot的父级依赖-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.xzy.springcloud</groupId>
    <artifactId>01-springcloud-service-provider</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>01-springcloud-service-provider</name>
    <description>Demo project for Spring Boot</description>

    <!--Maven的属性配置-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>
        <spring-cloud.version>Hoxton.SR8</spring-cloud.version>
    </properties>


    <dependencies>
        <!-- springcloud集成Eureka客户端的起步依赖 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--springboot开发web项目的起步依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--springboot测试依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <!--spring cloud依赖管理-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <!--springboot打包编译插件-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <!--指定maven仓库-->
    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

</project>


```

 2.激活Eureka客户端的功能

```java
@SpringBootApplication
@EnableDiscoveryClient // 开启EurekaClient功能
public class UserServiceDemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(UserServiceDemoApplication.class, args);
	}
}
```

3.配置服务名称和注册中心地址

```yaml
server:
  port: 8080

spring:
  application:
    name: 01-springcloud-service-provider # 配置服务名称，建议与项目名保持一致


eureka:
  client:
    service-url: # EurekaServer地址
      defaultZone: http://localhost:8761/eureka

```

4.启动并运行

### 3.5、从Eureka服务注册中心发现与消费服务

服务的发现有eureka客户端实现，二服务的消费有Ribbon实现，也就是说服务的调用需要eureka客户端和Ribbon两个配合起来才能实现。

**Ribbon是什么？**

Ribbon是一个基于HTTP和TCP的客户端负载均衡器，当使用Ribbon对服务进行访问的时候，他会扩展Eureka客户端的服务发现工鞥呢，实现从Eureka注册中心获取服务列表，并通过Eureka客户端来确定服务端是否已经启动，Ribbon在Eureka客户端服务发现的基础上，实现了对服务实例的选择策略，从而实现服务的负载均衡消费。

1.在该消费者项目中添加eureka的依赖，因为服务消费者从注册中心获取服务，需要eureka，所以需要eureka的支持

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.xzy.springcloud</groupId>
    <artifactId>02-springcloud-service-consumer</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>02-springcloud-service-consumer</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>
        <spring-cloud.version>Hoxton.SR8</spring-cloud.version>
    </properties>

    <dependencies>
        <!-- springcloud集成Eureka客户端的起步依赖 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <!--spring cloud起步依赖-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <!--指定maven仓库-->
    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

</project>

```

2.激活Eureka中的EnableEurekaClient功能

```java
@SpringBootApplication
@EnableDiscoveryClient // 开启Eureka客户端支持
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

3.配置服务的名称和注册中心的地址

```yaml
server:
  port: 8081
spring:
  application:
    name: 02-springcloud-servic-consumer # 配置服务名称

eureka:
  client:
    service-url: # EurekaServer地址
      defaultZone: http://localhost:8761/eureka

```

4.前面介绍了服务的发现有eureka客户端实现，二服务的真正调用有ribbon实现，所以需要在调用服务提供这是使用ribbon来调用

在前面包config包的BeanConfig类中的@Bean上添加@LoadBalanced注解

```java
//@Configuration:等价于一个Spring的applicationContext.xml配置文件
@Configuration
public class BeanConfig {
    /**
    * @Bean:等价于
     * <bean id="restTemplate" class="com.xzy.springcloud.config.RestTemplate"></bean>
    * @return:
    */
    @LoadBalanced
    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

加入了ribbon的支持，name在调用时，即可改为使用服务名称来访问

```java
@RestController
public class ConsumerController {
    @Autowired
    private RestTemplate restTemplate;

    @RequestMapping(value = "/web/hello" ,method = RequestMethod.GET)
    public String hello(){
        //调用SpringCloud的服务提供者
        return restTemplate.getForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/hello",String.class).getBody();
    }
}
```

5、完成上面的步骤后，我们就可以启动消费者的 SpringBoot 程序，main 方法运行；

 http://localhost:8081/web/hello 

6、启动成功之后，通过在浏览器地址栏访问我们的消费者，看是否可以正常调用远程服务提供者提供的服务

### 3.5.Eureka自我保护机制

​		在没有 Eureka 自我保护的情况下，如果 Eureka Server 在一定时间内没有接收到某个微服务实例的心跳，Eureka Server 将会注销该实例，但是当发生网络分区故障时，那么微服务与 Eureka Server 之间将无法正常通信，以上行为可能变得非常危险了——因为微服务本身其实是正常的，此时不应该注销这个微服务， 如果没有自我保护机制，那么 Eureka Server 就会将此服务注销掉。

禁用我保护机制：

```xml
eureka.server.enable-self-preservation = false
```

其他配置项

```xml
#每间隔 2s，向服务端发送一次心跳，证明自己依然"存活"

eureka.instance.lease-renewal-interval-in-seconds=2

#告诉服务端，如果我 10s 之内没有给你发心跳，就代表我故障了，将我踢出掉

eureka.instance.lease-expiration-duration-in-seconds=10
```

 

## **五、 Eureka 注册中心高可用集群概述  **

Eureka Server 的高可用实际上就是将自己作为服务向其他服务注册中心注册自己，这样就会形成一组互相注册的服务注册中心，进而实现服务清单的互相同步，往注册中心 A 上注册的服务，可以被复制同步到注册中心 B 上，所以从任何一台注册中心上都能查询到已经注册的服务，从而达到高可用的效果。

1.在03-springcloud-eureka-server中添加两个yml文件

application-eureka8761.yml

```yaml
server:
  port: 8761 # \u7AEF\u53E3

spring:
    application:
      name: eureka-server # 配置服务名称，会在Eureka中显示

eureka:
  instance:
    hostname: eureka8761 #设置该服务注册中心的hostname
  client:
    register-with-eureka: false # 是否注册自己的信息到EurekaServer，默认是true
    fetch-registry: false # 是否拉取其它服务的信息，默认是true
    service-url: # EurekaServer的地址，现在是自己的地址，如果是集群，需要加上其它Server的地址。
      defaultZone: http://eureka8761:8762/eureka
```

application-eureka8762.yml

```yaml
server:
  port: 8762 # \u7AEF\u53E3

spring:
    application:
      name: eureka-server # 配置服务名称，会在Eureka中显示

eureka:
  instance:
    hostname: eureka8762 #设置该服务注册中心的hostname
  client:
    register-with-eureka: false # 是否注册自己的信息到EurekaServer，默认是true
    fetch-registry: false # 是否拉取其它服务的信息，默认是true
    service-url: # EurekaServer的地址，现在是自己的地址，如果是集群，需要加上其它Server的地址。
      defaultZone: http://eureka8762:8761/eureka
```

2.然后在本地 hosts 文件配置  C:\Windows\System32\drivers\etc\hosts  

127.0.0.1 eureka8761

127.0.0.1 eureka8762

3.运行时，在运行配置项目 Program Arguments 中配置：

program argument active

**--spring.profiles.active=eureka8761**

**--spring.profiles.active=eureka8762**

3.在要进行注册的服务中配置：

```yaml
server:
  port: 8080

spring:
  application:
    name: 01-springcloud-service-provider # 配置服务名称


eureka:
  client:
    service-url: # EurekaServer地址
      defaultZone: http://eureka8761:8761/eureka/,http://eureka8762:8762/eureka/
```

4.进行测试

## **六、Ribbon负载均衡**

### 6.1、Ribbon的使用

我们通常说的负载均衡是指将一个请求均匀地分摊到不同的节点单元上执行，负载均衡分为硬件负载均衡和软件负载均衡：

1、启动多个服务提供者实例并注册到一个服务注册中心或是服务注册中心集群。

2、服务消费者通过被＠LoadBalanced 注解修饰过的 RestTemplate 来调用服务提供者。

这样，我们就可以实现服务提供者的高可用以及服务消费者的负载均衡调用。

3.就是创建多个同名的服务提供者，使用消费者进行访问，由于默认为轮询策略，会自动在两个提供者之间调用。

### 6.2、修改负载均衡策略

```java
//@Configuration:等价于一个Spring的applicationContext.xml配置文件
@Configuration
public class BeanConfig {
    /**
    * @Bean:等价于
     * <bean id="restTemplate" class="com.xzy.springcloud.config.RestTemplate"></bean>
    * @return:
    */
    @LoadBalanced
    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

    /**覆盖掉原来ribbon默认的轮询负载均衡策略*/
    @Bean
    public IRule iRule(){ return new RandomRule(); }//采用随机负载均衡策略
}
```

| 负载均衡策略              | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| RandomRule                | 随机                                                         |
| RoundRobinRule            | 轮询                                                         |
| AvailabilityFilteringRule | 先过滤掉由于多次访问故障的服务，以及并发连接数超过阈值的服务，然后对剩下的服  务按照轮询策略进行访问； |
| WeightedResponseTimeRule  | 根据平均响应时间计算所有服务的权重，响应时间越快服务权重就越大被选中的概率即越高，如果服务刚启动时统计信息不足，则使用 RoundRobinRule 策略，待统计信息足够会切换到该 WeightedResponseTimeRule 策  略； |
| RetryRule                 | 先按照 RoundRobinRule 策略分发，如果分发  到的服务不能访问，则在指定时间内进行重试，分发其他可用的服务； |
| BestAvailableRule         | 先过滤掉由于多次访问故障的服务，然后选  择一个并发量最小的服务； |
| ZoneAvoidanceRule         | 综合判断服务节点所在区域的性能和服务节  点的可用性，来决定选择哪个服务 |

### 6.3、RestTemplate 的 GET 请求

​        当我们从服务消费端去调用服务提供者的服务的时候，使用了一个极其方便的对象叫 RestTemplate，当时我们只使用了 RestTemplate 中最简单的一个功能getForEntity 发起了一个 get 请求去调用服务端的数据，同时，我们还通过配置@LoadBalanced 注解开启客户端负载均衡，RestTemplate 的功能非常强大，那么接下来我们就来详细的看一下 RestTemplate 中几种常见请求方法的使用  

​	第一种：**getForEntity**

该方法返回一个 ResponseEntity<T>对象，ResponseEntity<T>是 Spring 对HTTP 请求响应的封装，包括了几个重要的元素，比如响应码、contentType、contentLength、响应消息体等；

​	第二种：**getForObject()**

与 getForEntity 使用类似，只不过getForObject 是在

getForEntity 基础上进行了再次封装，可以将 http 的响应体 body信息转化成指定的对象，方便我们的代码开发，当你不需要返回响应中的其他信息，只需要 body 体信息的时候，可以使用这个更方便；

#### 1.getForEntity ()方法

​		第一个参数为要调用的服务的地址，即服务提供者提供的[**http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/hello** ](http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/hello)接口地址，注意这里是通过服务名调用而不是服务地址，如果改为服务地址就无法使用Ribbon 实现客户端负载均衡了。

​		第二个参数String.class 表示希望返回的body 类型是String

类型，如果希望返回一个对象，也是可以的，比如 User 对象；

```java
<T> T getForObject(URI url, Class<T> responseType) throws RestClientException;

<T> T getForObject(String url, Class<T> responseType, Object... uriVariables) throws
RestClientException;

<T> T getForObject(String url, Class<T> responseType, Map<String, ?> uriVariables)
throws RestClientException;
```

使用实例

```java
@RestController
public class ConsumerController {
    @Autowired
    private RestTemplate restTemplate;

    @RequestMapping(value = "/web/hello" ,method = RequestMethod.GET)
    public String hello(){
        //调用SpringCloud的服务提供者
        ResponseEntity<String> responseEntity = restTemplate.getForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/hello",String.class);
        String body = responseEntity.getBody();
        HttpStatus statusCode = responseEntity.getStatusCode();
        int statusCodeValue = responseEntity.getStatusCodeValue();
        HttpHeaders headers = responseEntity.getHeaders();

        System.out.println("返回内容："+body);
        System.out.println("状态码："+statusCode);
        System.out.println("状态值："+statusCodeValue);
        System.out.println("头部信息："+headers);
        return restTemplate.getForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/hello",String.class).getBody();
    }
}
```

```
//运行结果
返回内容：Hello Spring Cloud
状态码：200 OK
状态值：200
头部信息：[Content-Type:"text/plain;charset=UTF-8", Content-Length:"18", Date:"Wed, 16 Sep 2020 09:11:57 GMT", Keep-Alive:"timeout=60", Connection:"keep-alive"]
```

#### 2.getForEntity ()使用实例

1）在提供者新建一个model的包，新建User

```java
public class User {
    private Long id;
    private String name;
    /*setter与getter*/
}
```

2)提供者的HelloController中添加方法

```java
@GetMapping(value = "/service/user")
    public User user() {
        User user = new User();
        user.setId(1001L);
        user.setName("叶星云");
        return user;
    }
```

3)在消费者中新建一个model包和User类

4）在消费者的ConsumerController中添加代码

```java
@RequestMapping(value = "/web/user" ,method = RequestMethod.GET)
    public String user(){
        //调用SpringCloud的服务提供者
        ResponseEntity<User> responseEntity = restTemplate.getForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/user", User.class);
        User body = responseEntity.getBody();
        HttpStatus statusCode = responseEntity.getStatusCode();
        int statusCodeValue = responseEntity.getStatusCodeValue();
        HttpHeaders headers = responseEntity.getHeaders();

        System.out.println("返回内容："+body);
        System.out.println("状态码："+statusCode);
        System.out.println("状态值："+statusCodeValue);
        System.out.println("头部信息："+headers);
        return restTemplate.getForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/user",String.class).getBody();
    }
```

5）返回内容

```
返回内容：User{id=1001, name='叶星云'}
状态码：200 OK
状态值：200
头部信息：[Content-Type:"application/json", Transfer-Encoding:"chunked", Date:"Wed, 16 Sep 2020 09:54:47 GMT", Keep-Alive:"timeout=60", Connection:"keep-alive"]
```

#### 3.getForEntity请求绑定参数

```java
<T> T getForObject(URI url, Class<T> responseType) throws RestClientException;

//使用数组传参
<T> T getForObject(String url, Class<T> responseType, Object... uriVariables) throws
RestClientException;

//使用map传参
<T> T getForObject(String url, Class<T> responseType, Map<String, ?> uriVariables)
throws RestClientException;
```

举例

```java
//服务提供者
@RequestMapping("/service/getUser")
    public User getUser(@RequestParam("id")Long id,@RequestParam("name")String name) {
        User user = new User();
        user.setId(id);
        user.setName(name);
        return user;
    }
```

```java
//服务消费者
@RequestMapping(value = "/web/getUser" ,method = RequestMethod.GET)
    public User getUser(){
        //使用数组的方式
        String [] strArry = {"105","张三"};
        ResponseEntity<User> responseEntity = restTemplate.getForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/getUser?id={0}&name={1}", User.class,strArry);
        
        //使用map的方式
        Map<String,Object> paramMap = new ConcurrentHashMap<>();
        paramMap.put("id",1032);
        paramMap.put("name","王五");
        ResponseEntity<User> responseEntity = restTemplate.getForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/getUser?id={id}&name={name}", User.class,paramMap);
        
        User body = responseEntity.getBody();
        HttpStatus statusCode = responseEntity.getStatusCode();
        int statusCodeValue = responseEntity.getStatusCodeValue();
        HttpHeaders headers = responseEntity.getHeaders();

        System.out.println("返回内容："+body);
        System.out.println("状态码："+statusCode);
        System.out.println("状态值："+statusCodeValue);
        System.out.println("头部信息："+headers);
        
        //使用数组的方式
        return restTemplate.getForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/getUser?id={0}&name={1}",User.class,strArry).getBody();
        
        //使用map的方式
        return restTemplate.getForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/getUser?id={0}&name={1}",User.class,strArry).getBody();
    }
```

#### 4.getForObject()方法

与 getForEntity 使用类似，只不过getForObject 是在

getForEntity 基础上进行了再次封装，可以将 http 的响应体 body

信息转化成指定的对象，方便我们的代码开发；当你不需要返回响应中的其他信息，只需要 body 体信息的时候，可以使用这个更方便  

```java
<T> T getForObject(URI url, Class<T> responseType) **throws** RestClientException;
 
<T> T getForObject(String url, Class<T> responseType, Object... uriVariables) **throws**
RestClientException;

<T> T getForObject(String url, Class<T> responseType, Map<String, ?> uriVariables)
**throws** RestClientException;
```

实例

```java
Map<String,Object> paramMap = new ConcurrentHashMap<>();
paramMap.put("id",1032);
paramMap.put("name","王五");
User user = restTemplate.getForObject("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/getUser?id={id}&name={name}", User.class,paramMap);
```

### 6.4、RestTemplate的POST请求

传参使用以下MultiValueMap接收

```java
 MultiValueMap<String,Object> dataMap = new LinkedMultiValueMap<>();
dataMap.add("id",1032);
```

```
restTemplate.postForObject(url,paramMap.class) 
restTemplate.postForEntity() 
restTemplate.postForLocation()
```

服务提供者

```java
//@RequestMapping(value = "/service/getUser",method = RequestMethod.POST)
    @PostMapping("/service/addUser")
    public User addUser(@RequestParam("id")Long id,@RequestParam("name")String name) {
        User user = new User();
        user.setId(id);
        user.setName(name);
        return user;
    }
```

服务消费者

```java
@RequestMapping(value = "/web/addUser")
    public User addUser(){
        //使用map的方式
        Map<String,Object> paramMap = new ConcurrentHashMap<>();
        paramMap.put("id",1032);
        paramMap.put("name","王五");

        //添加参数
        MultiValueMap<String,Object> dataMap = new LinkedMultiValueMap<>();
        dataMap.add("id",1032);
        dataMap.add("name","王五");

        //使用postForEntity方式
        ResponseEntity<User> responseEntity = restTemplate.postForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/addUser",dataMap, User.class);
        User body = responseEntity.getBody();
        HttpStatus statusCode = responseEntity.getStatusCode();
        int statusCodeValue = responseEntity.getStatusCodeValue();
        HttpHeaders headers = responseEntity.getHeaders();

        //使用postForObject方式
        User user = restTemplate.postForObject("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/addUser",dataMap, User.class);
        System.out.println(user);

        return restTemplate.postForEntity("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/addUser",dataMap,User.class).getBody();
    }
```

### 6.5、RestTemplate的PUT请求(可用POST代替)

1.提供者进行修改操作

```java
@PutMapping("/service/updateUser")
    public User updateUser(@RequestParam("id")Long id,@RequestParam("name")String name) {
        User user = new User();
        user.setId(id);
        user.setName(name);
        return user;
    }
```

2.消费者，没有返回值，只有put方法

```java
@PutMapping(value = "/web/updateUser")
    public String updateUser(){
        //使用map的方式封装对象
        Map<String,Object> paramMap = new ConcurrentHashMap<>();
        paramMap.put("id",1032);
        paramMap.put("name","王五");

        //添加参数
        MultiValueMap<String,Object> dataMap = new LinkedMultiValueMap<>();
        dataMap.add("id",1032);
        dataMap.add("name","王五");

        //修改,没有返回值
        restTemplate.put("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/updateUser",dataMap);
        return "success";
    }
```

### 6.6、RestTemplate的DELETE请求(可用Get代替)

1.服务提供者

```java
@DeleteMapping("/service/deleteUser")
    public User deleteUser(@RequestParam("id")Long id) {
        //dao层删除用户
        User user = new User();
        user.setId(id);
        user.setName(name);
        return user;
    }
```

2.服务消费者

```java
@DeleteMapping(value = "/web/deleteUser")
    public String deleteUser(){
        //使用数组绑定参数
        String [] strArry = {"105","张三"};
        restTemplate.delete("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/deleteUser?id={0}",strArry);


        //使用map的方式绑定参数
        Map<String,Object> dataMap = new ConcurrentHashMap<>();
        dataMap.put("id",1032);

        //修改,没有返回值
        restTemplate.delete("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/deleteUser?id={id}",dataMap);

        return "success";
    }
```

## 七、服务熔断Hystrix

### 7.1、什么是Hystrix？

​		在微服务架构中，我们是将一个单体应用拆分成多个服务单元，各个服务单元之间通过注册中心彼此发现和消费对方提供的服务，每个服务单元都是单独部署， 在各自的服务进程中运行，服务之间通过远程调用实现信息交互，那么当某个服务的响应太慢或者故障，又或者因为网络波动或故障，则会造成调用者延迟或调用失败，当大量请求到达，则会造成请求的堆积，导致调用者的线程挂起，从而引发调用者也无法响应，调用者也发生故障。  

​		熔断器也有叫断路器，他们表示同一个意思，最早来源于微服务之父 Martin Fowler 的论文 CircuitBreaker 一文。“熔断器”本身是一种开关装置，用于在电路上保护线路过载，当线路中有电器发生短路时，能够及时切断故障电路，防止发生过载、发热甚至起火等严重后果。

​		微服务架构中的熔断器，就是当被调用方没有响应，调用方直接返回一个错误响应即可，而不是长时间的等待，这样避免调用时因为等待而线程一直得不到释放， 避免故障在分布式系统间蔓延  

​        Hystrix 具备服务降级、服务熔断、线程和信号隔离、请求缓存、请求合并以及服务监控等强大功能。  

## 7.2、Hystrix快速入门

1.引入依赖

首先在服务消费者中引入Hystix依赖：

```xml
<!--熔断器的起步依赖-->
<dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-hystrix</artifactId>
      <version>1.4.5.RELEASE</version>
</dependency>

<!--另一种方式-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

2.在消费者入口出使用注解开启熔断器

```
@EnableCircuitBreaker  //开启断路器
```

3.在调用方法上添加注解

```
@HystrixCommand(fallbackMethod="Fallback") //开启断路器,后面的Fallback是失败回调的方法名称。
```

实例：

```java
@RequestMapping(value = "/web/hystrix")
    @HystrixCommand(fallbackMethod="error") //开启断路器，
    public String hystrix(){
        return restTemplate.getForObject("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/hello",String.class);
    }

    //错误
    public String Fallback(){
        //访问远程服务失败，该如何处理
        return "error";
    }
```

4.也可以使用一个名为@SpringCloudApplication 的注解代替主类上的三个注解；  

```
@EnableDiscoveryClient等价于
@EnableDiscoveryClient
```

### 7.2、 服务消费者 Hystrix 测试  

hystrix 默认超时时间是 1000 毫秒，如果你后端的响应超过此时间，就会触发断路器；

修改 hystrix 的默认超时时间：

```java
@HystrixCommand(fallbackMethod="error", commandProperties={ @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",
value="1500")}) //熔断器，调用不通，回调 error()方法
public String webHello () {
}
```

或者配置时间

```yaml
hystrix:
  command:
  	default:
        execution:
          isolation:
            thread:
              timeoutInMillisecond: 6000 # 设置hystrix的超时时间为6000ms
```

### 7.3、 Hystrix 服务降级

有了服务的熔断，随之就会有服务的降级，所谓服务降级，就是当某个服务熔断之后，服务端提供的服务将不再被调用，此时由客户端自己准备一个本地的fallback 回调，返回一个默认值来代表服务端的返回；

这种做法，虽然不能得到正确的返回结果，但至少保证了服务的可用，比直接抛出错误或服务不可用要好很多，当然这需要根据具体的业务场景来选择；

### 7.4、 Hystrix的异常处理  

实例：

```java
@RequestMapping(value = "/web/hystrix")
    @HystrixCommand(fallbackMethod="error") //开启断路器
    public String hystrix(){
        int a = 10/0;
        return restTemplate.getForObject("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/hello",String.class);
    }

    //错误
    public String error(Throwable throwable){
        //访问远程服务失败，该如何处理
        System.out.println("打印异常信息："+throwable.getMessage());
        return "error";
    }

//打印异常信息：/ by zero
```

果远程服务有一个异常抛出后我们不希望进入到服务降级方法中去处理，而是直接将异常抛给用户，那么我们可以在@HystrixCommand 注解中添加忽略异常，如下：

忽略异常

```
@HystrixCommand(fallbackMethod="error", ignoreExceptions = Exception.class)
```

### 7.5、自定义 Hystrix 请求的服务异常熔断处理

消费者控制层

```java
@RequestMapping(value = "/web/hystrix2")
    @HystrixCommand()
    public String hystrix2(){
        MyCommand myCommand =new MyCommand(com.netflix.hystrix.HystrixCommand.Setter.withGroupKey(HystrixCommandGroupKey.Factory.asKey("")),restTemplate);
        String str = myCommand.execute();
        return str;
    }
```

创建一个包名为hystrix，类名为MyCommand

```java
public class MyCommand extends HystrixCommand<String> {
    @Autowired
    private RestTemplate restTemplate;

    public MyCommand(Setter setter,RestTemplate restTemplate){
        super(setter);
        this.restTemplate=restTemplate;
    }

    @Override
    protected String run() throws Exception {
        //调用远程服务
        return restTemplate.getForObject("http://01-SPRINGCLOUD-SERVICE-PROVIDER/service/hello",String.class);
    }

    @Override
    public String getFallback() {
        //实现服务熔断降级逻辑
        return "error";
    }
}
```

异步调用

```java
@RequestMapping(value = "/web/hystrix3")
    @HystrixCommand()
    public String hystrix3() throws ExecutionException, InterruptedException {
        MyCommand myCommand =new MyCommand(com.netflix.hystrix.HystrixCommand.Setter.withGroupKey(HystrixCommandGroupKey.Factory.asKey("")),restTemplate);
        //异步调用（该方法执行后，不会马上又远程的返回结果，将来会有结果）
        Future<String> future = myCommand.queue();
        //阻塞的方法，直到拿到结果
        String str = future.get();
        return str;
    }
```

在MyCommand类中添加如下代码

```java
public String getFallback() {
        Throwable throwable = super.getExecutionException();
        System.out.println("打印异常信息："+throwable.getMessage());
        System.out.println("打印异常状态："+throwable.getStackTrace());
        //实现服务熔断降级逻辑
        return "error";
    }
```

### 7.6、Hystrix 仪表盘监控

​		Hystrix 仪表盘（Hystrix Dashboard），就像汽车的仪表盘实时显示汽车的各项数据一样，Hystrix 仪表盘主要用来监控 Hystrix 的实时运行状态，通过它我们可以看到 Hystrix 的各项指标信息，从而快速发现系统中存在的问题进而解决它。要使用 Hystrix 仪表盘功能，我们首先需要有一个 Hystrix Dashboard，这个功能我们可以在原来的消费者应用上添加，让原来的消费者应用具备 Hystrix 仪表盘功能，但一般地，微服务架构思想是推崇服务的拆分，Hystrix Dashboard 也是一个服务，所以通常会单独创建一个新的工程专门用做 Hystrix Dashboard 服务；

 **搭建一个 Hystrix Dashboard 服务的步骤：**   

**第一步：**创建一个普通的 Spring Boot 工程

比如创建一个名为 springcloud-hystrix-dashboard 的 Spring Boot 工程，建立好基本的结构和配置；

**第二步：**添加相关依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
	<version>1.4.5.RELEASE</version>
</dependency>
```

**第三步：**入口类上添加注解  

@EnableHystrixDashboard

```java
@SpringBootApplication
@EnableHystrixDashboard
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

**第四步：**属性配置  

最后，我们可以根据个人习惯配置一下 application.properties 文件，如下：

```
server.port=3721
```

 至此，我们的 Hystrix 监控环境就搭建好了  

**第五步**：访问

```
通过路径：localhost:3721/hystrix
```

![1600335877604](C:\Users\ly138\AppData\Roaming\Typora\typora-user-images\1600335877604.png)

Hystrix 仪表盘工程已经创建好了，现在我们需要有一个服务，让这个服务提供一个路径为/actuator/hystrix.stream 接口，然后就可以使用 Hystrix 仪表盘来对该服务进行监控了；

1、消费者项目需要有 hystrix 的依赖：

```xml
<!--Spring Cloud 熔断器起步依赖-->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-hystrix</artifactId>
	<version>1.4.5.RELEASE</version>
</dependency>
```

2、需要有一个 spring boot 的服务监控依赖：

```xml
<!--spring boot健康检查依赖-->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

3、配置文件需要配置 spring boot 监控端点的访问权限：

```
//springboot的监控端点访问权限，*表示打开所有的访问端点都允许访问
management.endpoints.web.exposure.include=*
```

​        这个是用来暴露 endpoints 的，由于 endpoints 中会包含很多敏感信息，除了 health 和 info 两个支持直接访问外，其他的默认不能直接访问，所以我们让它都能访问，或者指定：  

```
management.endpoints.web.exposure.include=hystrix.stream
```

4、访问入口 http://localhost:8081/actuator/hystrix.stream

​        **注意**：这里有一个细节需要注意，要访问/hystrix.stream 接口，首先得访问consumer 工程中的任意一个其他接口，否则直接访问/hystrix.stream 接口时会输出出一连串的 ping:   ping:  …，先访问 consumer 中的任意一个其他接口，然后再访问/hystrix.stream 接口即可  

![1600336250599](C:\Users\ly138\AppData\Roaming\Typora\typora-user-images\1600336250599.png)

## **八、声明式服务消费 Feign**  

### 8.1、Feign是什么？

​        Ribbon 负载均衡、Hystrix 服务熔断是我们 Spring Cloud 中进行微服务开发非常基础的组件，在使用的过程中我们也发现它们一般都是同时出现的，而且配置也都非常相似，每次开发都有很多相同的代码，因此 Spring Cloud 基于 Netflix Feign 整合了 Ribbon 和 Hystrix 两个组件，让我们的开发工作变得更加简单， 就像 Spring Boot 是对 Spring+SpringMVC 的简化一样，Spring Cloud Feign 对 Ribbon 负载均衡、Hystrix 服务熔断进行简化，在其基础上进行了进一步的封装，不仅在配置上大大简化了开发工作，同时还提供了一种声明式的 Web 服务客户端定义方式  

### 8.2、使用Feign实现消费者

使用 Feign 实现消费者，我们通过下面步骤进行：

**第一步**：创建普通 Spring Boot 工程  

首先我们来创建一个普通的 Spring Boot 工程，取名为  ：

05-springcloud-service-feign  

**第二步**：添加依赖

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <java.version>1.8</java.version>
    <spring-cloud.version>Hoxton.SR8</spring-cloud.version>
</properties>

<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-feign</artifactId>
	<version>1.4.5.RELEASE</version>
</dependency>

<!--Spring Cloud 熔断器起步依赖-->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-hystrix</artifactId>
	<version>1.4.5.RELEASE</version>
</dependency>

<!--spring cloud的依赖管理-->
<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
</dependencyManagement>

<!--指定maven仓库-->
    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>
```

**第三步**：添加注解

在项目入口类上添加@EnableFeignClients 注解表示开启 Spring Cloud Feign

的支持功能；

**第四步**：声明服务

定义一个 HelloService 接口，通过@FeignClient 注解来指定服务名称，进而绑定服务，然后再通过 SpringMVC 中提供的注解来绑定服务提供者提供的接口， 如下

```java
//声明一个方法，这个方法就是远程服务提供者提供的方法
@FeignClient("01-springcloud-service-provider")
public interface HelloService { 
    @RequestMapping("/service/hello") 
	public String hello();
}
```

​        这相当于绑定了一个名叫 01-springcloud-service-provider (这里  

​        01-springcloud-service-provider   大小写      01-SPRINGCLOUD-SERVICE-PROVIDER  都可以 ) 的服务提供者提供的/service/hello 接口； 我们服务提供者提供的接口如下：

```java
@GetMapping("/service/hello")
public String hello() {
	System.out.println("服务提供者 1。。。。。。。"); 
	return "Hello, Spring Cloud，Provider 1";
}
```

**第五步**：使用 Controller 中调用服务

接着来创建一个 Controller 来调用上面的服务，如下：

```java
@RestController
public class FeignController { 
    @Autowired
	HelloService helloService; 
    @RequestMapping("/web/hello") 
    public String hello() {
	return helloService.hello();
	}
}
```

**第六步**：属性配置

在 application.properties 中指定服务注册中心、端口号等信息，如下：

```properties
server.port=8082

#配置服务的名称
spring.application.name=05-springcloud-service-feign

#配置eureka注册中心地址
eureka.client.service-url.defaultZone=http://eureka8761:8761/eureka/,http://eureka8762:8762/eureka/
```

**第七步**：测试

依次启动注册中心、服务提供者和 feign 实现服务消费者，然后访问如下地址：

http://localhost:8082/web/hello

### 8.3、 使用 Feign 实现服务测试

**负载均衡：**

​		我们知道，Spring Cloud 提供了 Ribbon 来实现负载均衡，使用 Ribbo 直接注入一个 RestTemplate 对象即可，RestTemplate 已经做好了负载均衡的配置； 在 Spring Cloud 下，使用 Feign 也是直接可以实现负载均衡的，定义一个注解有@FeignClient 注解的接口，然后使用@RequestMapping 注解到方法上映射远程的 REST 服务，此方法也是做好负责均衡配置的。

**服务熔断：**

1、添加熔断器依赖

```xml
<!--断路器依赖-->
<dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-hystrix</artifactId>
     <version>1.4.5.RELEASE</version>
</dependency>
```

2、在 application.properties 文件开启 hystrix 功能

```properties
#开启hystrix功能
feign.hystrix.enabled=true
```

3、指定熔断回调逻辑  

```java
@FeignClient(name="01-springcloud-service-provider", fallback = MyFallback.class)
public interface HelloService { 
    @RequestMapping("/service/hello") 
	public String hello();
}
```

```java
@Component
public class MyFallback implements HelloService { 
    @Override
	public String hello() {
		return "远程服务不可用，暂时采用本地逻辑代替......";
	}
}
```

4、服务熔断获取异常信息：

```java
@FeignClient(name="01-springcloud-service-provider", fallbackFactory = MyFallbackFactory.class)
public interface HelloService { 
    @RequestMapping("/service/hello") 
	public String hello();
}
```

```java
@Component
public class MyFallbackFactory implements FallbackFactory<HelloService> { @Override
	public HelloService create(Throwable throwable) {
		return new HelloService() { 
            @Override
			public String hello() {
				return throwable.getMessage();
			}
		};
	}
}

```

## 九：API 网关 Zuul

### 9.1、Spring Cloud 的 Zuul 是什么

​        它就像一个安检站一样，所有外部的请求都需要经过它的调度与过滤，然后 API 网关来实现请求路由、负载均衡、权限验证等功能；  

9.2、使用Zuul创建API网关

1、创建一个普通的 Spring Boot 工程名为 06-springcloud-api-gateway，然后添加相关依赖，这里我们主要添加两个依赖 zuul 和 eureka 依赖：

```xml
<!--添加 spring cloud 的 zuul 的起步依赖-->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-zuul</artifactId>
</dependency>
<!--添加 spring cloud 的 eureka 的客户端依赖-->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

<!--spring cloud依赖管理-->
<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

<!--指定maven仓库-->
    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>
```

2、在入口类上添加@EnableZuulProxy 注解，开启 Zuul 的 API 网关服务功能：

```java
@EnableZuulProxy //开启 Zuul 的 API 网关服务功能 
@SpringBootApplication
public class Application {
	public static void main(String[] args) { 
		SpringApplication.run(Application.class, args);
	}
}
```

3、在 application.properties 文件中配置路由规则：

```properties
#配置服务内嵌的 Tomcat 端口 
server.port=8080

#配置服务的名称 
spring.application.name=06-springcloud-api-gateway

#配置路由规则 
zuul.routes.api-wkcto.path=/api-wkcto/**		#所匹配的地址栏路径
zuul.routes.api-wkcto.serviceId=05-springcloud-service-feign	#找哪个服务

#配置 API 网关到注册中心上，API 网关也将作为一个服务注册到 eureka-server 上 
eureka.client.service-url.defaultZone=http://eureka8761:8761/eureka/,http:/
/eureka8762:8762/eureka/
```

​		以上配置，我们的路由规则就是匹配所有符合/api-wkcto/**的请求，只要路径中带有/api-wkcto/都将被转发到 05-springcloud-service-feign 服务上，至于05-springcloud-service-feign 服务的地址到底是什么则由 eureka-server 注册中心去分析，我们只需要写上服务名即可。

以我们目前搭建的项目为例，请求 http://localhost:8080/api-wkcto/web/hello 接口则相当于请求 http://localhost:8082/web/hello

(05-springcloud-service-feign 服务的地址为 http://localhost:8082/web/hello)，

路由规则中配置的 api-wkcto 是路由的名字，可以任意定义，但是一组 path 和

serviceId 映射关系的路由名要相同。

如果以上测试成功，则表示们的 API 网关服务已经构建成功了，我们发送的符合路由规则的请求将自动被转发到相应的服务上去处理。

### 9.2、使用 Zuul 进行请求过滤  

​        我们知道 Spring cloud Zuul 就像一个安检站，所有请求都会经过这个安检站， 所以我们可以在该安检站内实现对请求的过滤，下面我们以一个权限验证案例说这一点：  

1、我们定义一个过滤器类并继承自 ZuulFilter，并将该 Filter 作为一个 Bean：

```java
@Component
public class AuthFilter extends ZuulFilter { @Override
public String filterType() {
    return "pre";
}
    @Override
    public int filterOrder() {
        return 0;
    }
    @Override
    public boolean shouldFilter() {
        return true;
    }
    @Override
    public Object run() throws ZuulException {
        //获取上下文
        RequestContext ctx = RequestContext.getCurrentContext(); 
        //获取对象
        HttpServletRequest request = ctx.getRequest();
        //判断请求是否有token参数
        String token = request.getParameter("token");
        if (token == null) { 
            ctx.setSendZuulResponse(false);
            //设置响应码
            ctx.setResponseStatusCode(401);
            //设置响应头
            ctx.addZuulResponseHeader("content-type","text/html;charset=utf-8"); 
            //设置响应内容
            ctx.setResponseBody("非法访问");
        }
        return null;
    }
}
```

（1） filterType 方法的返回值为过滤器的类型，过滤器的类型决定了过滤器在哪个生命周期执行，pre 表示在路由之前执行过滤器，其他值还有 post、error、route 和 static，当然也可以自定义。

（2） filterOrder 方法表示过滤器的执行顺序，当过滤器很多时，我们可以通过该方法的返回值来指定过滤器的执行顺序。  

（3） shouldFilter 方法用来判断过滤器是否执行，true 表示执行，false 表示不执行。

（4） run 方法则表示过滤的具体逻辑，如果请求地址中携带了 token 参数的话，则认为是合法请求，否则为非法请求，如果是非法请求的话，首先设置 ctx.setSendZuulResponse(false); 表示不对该请求进行路由，然后设置响应码和响应值。这个 run 方法的返回值目前暂时没有任何意义，可以返回任意值。

2、通过 http://localhost:8080/api-wkcto/web/hello 地址访问，就会被过滤器过滤。

### 9.3、Zuul路由规则

（1）在前面的例子中  

```properties
#配置路由规则 
zuul.routes.api-wkcto.path=/api-wkcto/**
zuul.routes.api-wkcto.serviceId=05-springcloud-service-feign
```

05-springcloud-service-feign 服务上，不过两行代码有点麻烦，还可以简化为：

```properties
zuul.routes.05-springcloud-service-feign=/api-wkcto/**
```

zuul.routes 后面跟着的是服务名，服务名后面跟着的是路径规则，这种配置方式更简单。

（2）如果映射规则我们什么都不写，系统也给我们提供了一套默认的配置规则默认的配置规则如下  

```properties
#默认的规则 
zuul.routes.05-springcloud-service-feign.path = /05-springcloud-service-feign/ zuul.routes.05-springcloud-service-feign.serviceId = 05-springcloud-service-feign
```

(3)  默认情况下，Eureka 上所有注册的服务都会被 Zuul 创建映射关系来进行路由。

但是对于我这里的例子来说，我希望：

05-springcloud-service-feign 提供服务；

而 01-springcloud-service-provider 作为服务提供者只对服务消费者提供服务，不对外提供服务  

如果使用默认的路由规则，则 Zuul 也会自动为

01-springcloud-service-provider 创建映射规则，这个时候我们可以采用如下方式来让 Zuul 跳过 01-springcloud-service-provider 服务，不为其创建路由规则：

```properties
#忽略掉服务提供者的默认规则 
zuul.ignored-services=01-springcloud-service-provider
```

不给某个服务设置映射规则，这个配置我们可以进一步细化，比如说我不想给 /hello 接口路由，那我们可以按如下方式配置： 

```properties
#忽略掉某一些接口路径
zuul.ignored-patterns= /**/hello/**
```

此外，我们也可以统一的为路由规则增加前缀，设置方式如下：

```properties
#配置网关路由的前缀 
zuul.prefix=/myapi
```

此时我们的访问路径就变成了 http://localhost:8080/myapi/web/hello  

(4)  由规则通配符的含义：

| 通配符 | 含义               | 举例                             | 说明                                                         |
| ------ | ------------------ | :------------------------------- | ------------------------------------------------------------ |
| ？     | 匹配任意单个字符   | /05-springcloud-service-feign/?  | 匹配  /05-springcloud-service-feign/a,  /05-springcloud-service-feign/b,  /05-springcloud-service-feign/c 等 |
| *      | 匹配任意数量的字符 | /05-springcloud-service-feign/*  | 匹配  /05-springcloud-service-feign/aaa,  /05-springcloud-service-feign/bbb,  /05-springcloud-service-feign/ccc  等， 无法匹配  /05-springcloud-service-feign/a/b/c |
| **     | 匹配任意数量的字符 | /05-springcloud-service-feign/** | 匹配  /05-springcloud-service-feign/aaa,  /05-springcloud-service-feign/bbb,  /05-springcloud-service-feign/ccc  等， 也可以匹配  /05-springcloud-service-feign/a/b/c |

（5） 一般情况下 API 网关只是作为各个微服务的统一入口，但是有时候我们可能也需要在 API 网关服务上做一些特殊的业务逻辑处理，那么我们可以让请求到达 API 网关后，再转发给自己本身，由 API 网关自己来处理，那么我们可以进行如下的操作  

在 06-springcloud-api-gateway 项目中新建如下 Controller：

```java
@RestController
public class GateWayController { 
    @RequestMapping("/api/local") 
    public String hello() {
		return "exec the api gateway.";
	}
}
```

然后在 application.properties 文件中配置：

```properties
zuul.routes.gateway.path=/gateway/
zuul.routes.gateway.url=forward:/api/local
```

### 9.4. Zuul 的异常处理

​        Zuul 请求的生命周期图  

![1600413186534](C:\Users\ly138\AppData\Roaming\Typora\typora-user-images\1600413186534.png)

1. 正常情况下所有的请求都是按照 pre、route、post 的顺序来执行，然后由 post

返回 response

2. 在 pre 阶段，如果有自定义的过滤器则执行自定义的过滤器

3. pre、routing、post 的任意一个阶段如果抛异常了，则执行 error 过滤器我们可以有两种方式统一处理异常：

禁用 zuul 默认的异常处理 SendErrorFilter 过滤器，然后自定义我们自己的

Errorfilter 过滤器

```properties
//禁用 zuul 默认的异常处理 SendErrorFilter 过滤器
zuul.SendErrorFilter.error.disable = true
```

自定义我们自己的过滤器，新建一个ErrorFIleter类

```java
@Component
public class ErrorFilter extends ZuulFilter {
    private static final Logger logger = LoggerFactory.getLogger(ErrorFilter.class);
    @Override
    public String filterType() {
        return "error";
    }
    @Override
    public int filterOrder() {
        return 1;
    }
    @Override
    public boolean shouldFilter() {
        return true;
    }
    @Override
    public Object run() throws ZuulException {
        try {
            //获取请求上下文
            RequestContext context = RequestContext.getCurrentContext(); 
            //获取异常
            ZuulException exception = (ZuulException)context.getThrowable(); 
            //将异常记录到日志中
            logger.error("进入系统异常拦截", exception);
            //获取response
            HttpServletResponse response = context.getResponse();
            //设置格式
            response.setContentType("application/json; charset=utf8");
            //设置状态码
            response.setStatus(exception.nStatusCode);
            PrintWriter writer = null; try {
                writer = response.getWriter();
                writer.print("{code:"+ exception.nStatusCode +",message:\""+ exception.getMessage() +"\"}");
            } catch (IOException e) { e.printStackTrace();
            } finally {
                if(writer!=null){ writer.close();
                }
            }
        } catch (Exception var5) { ReflectionUtils.rethrowRuntimeException(var5);


        }
        return null;
    }
}
```

2、自定义全局 error 错误页面

```java
@RestController
public class ErrorHandlerController implements ErrorController {
    /**
     * 出异常后进入该方法，交由下面的方法处理 
     */
    @Override
    public String getErrorPath() {
        return "/error";
    }
    @RequestMapping("/error")
    public Object error(){
        RequestContext ctx = RequestContext.getCurrentContext(); ZuulException exception = (ZuulException)ctx.getThrowable(); return exception.nStatusCode + "--" + exception.getMessage();
    }
}
```

## 十、Spring Cloud Config分布式配置中心

### 10.1、什么是Spring CLoud Config？

对各个分布式项目配置文件进行统一管理  

### 10.2、构建 Springcloud config 配置中心

构建一个 spring cloud config 配置中心按照如下方式进行：

1、创建一个普通的 Spring Boot 项目

2、在 pom.xml 文件中添加如下依赖：

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

3、在入口类，也就是 main 方法的类上添加注解 **@EnableConfigServer**

4、在 application.properties 中配置一下 git 仓库信息，此处我们使用 GitHub

（ 也可以使用码云 gitee ） ， 首先在我的 Github 上创建一个名为

spring-cloud-config 的项目，创建之后，再做如下配置：

```properties
server.port=3721

spring.application.name=07-springcloud-config-server

spring.cloud.config.server.git.uri = https://github.com/myspring/sprin g-cloud-config.git
spring.cloud.config.server.git.search-paths = config-center spring.cloud.config.server.git.username = xxxx@163.com spring.cloud.config.server.git.password = xxxx123456
```

其中：

1. uri 表示配置中心所在仓库的位置

2. search-paths 表示仓库下的子目录

3. username 表示你的 GitHub 用户名

4. password 表示你的 GitHub 密码

至此我们的配置中心服务端就创建好了。

### 10.3、构建 Springcloud config 配置中心仓库  

​        接下来我们需要在 github 上设置好配置中心，首先在本地创建一个文件夹叫wkcto，然后在里面创建一个文件夹叫 config-center，然后在 config-center 中创建四个配置文件，如下：  

```
application.properties			//默认配置文件
application-dev.properties 		//开发配置文件
application-test.properties 	//测试配置文件
application-online.properties	//上线配置文件
```

在四个文件里面分别写上要测试的内容：  

```
url=http://www.wkcto.com
url=http://dev.wkcto.com
url=http://test.wkcto.com
url=http://online.wkcto.com
```

然后回到 wkcto 目录下，依次执行如下命令将本地文件同步到 Github 仓库中：

说明：在使用命令之前先从 https://git-scm.com/ 网站下载安装 git 的 window 客户端

1、添加提交人的账号信息，git 需要知道提交人的信息作为标识；

```properties
git config --global user.name 'junge'
git config --global user.email 'junge@163.com'
```

2、将该目录变为 git 可以管理的目录；

```properties
git init
```

3、将文件添加到暂存区；

```properties
git add config-center/
```

4、把文件提交到本地仓库；

```properties
git commit -m 'add config-center'
```

5、添加远程主机；

```properties
git remote add origin https://github.com/hnylj/spring-cloud-config.git
```

6、将本地的 master 分支推送到 origin 主机；

```properties
git push -u origin master
```

至此，我们的配置文件就上传到 GitHub 上了。

此时启动我们的配置中心，通过/{application}/{profile}/{label}就能访问到我们的配置文件了；

其中：

```
{application} 表示配置文件的名字，对应的配置文件即 application， 

{profile} 表示环境，有 dev、test、online 及默认， 

{label} 表示分支，默认我们放在 master 分支上， 
```

通过浏览器上访问 http://localhost:3721/application/dev/master

返回的 JSON 格式的数据：

```
name 表示配置文件名application 部分，

profiles 表示环境部分，

label 表示分支，

version 表示 GitHub 上提交时产生的版本号，
```

同时当我们访问成功后，在控制台会打印了相关的日志信息；

当访问成功后配置中心会通过 git clone 命令将远程配置文件在本地也保存一份，以确保在 git 仓库故障时我们的应用还可以继续正常使用。

### 10.4、构建 Springcloud config 配置中心客户端  

前面已经搭建好了配置中心的服务端，接下来我们来看看如何在客户端应用中使用。

前面已经搭建好了配置中心的服务端，接下来我们来看看如何在客户端应用中使用。

1、创建一个普通的 Spring Boot 工程 08-springcloud-config-client，并添加如下依赖：

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

2、创建 bootstrap.properties 文件，用于获取配置信息，文件内容如下：

（注意这些信息一定要放在 bootstrap.properties 文件中才有效）

```properties
server.port=3722

spring.application.name=application 
spring.cloud.config.profile=dev 
spring.cloud.config.label=master 
spring.cloud.config.uri=http://localhost:3721/
```

其中：

name 对应配置文件中的 application 部分，

profile 对应了 profile 部分，

label 对应了 label 部分，

uri 表示配置中心的地址。

 3、创建一个 Controller 进行测试：  

```java
@RestController @RefreshScope
public class ConfigController { 
    //将本地配置文件中的url注入进来
    @Value("${url}")
	private String url; 
    
    @Autowired
	private Environment env;

    @RequestMapping("/cloud/url")
    public String url () {
        return this.url;
    }
    @RequestMapping("/cloud/url2")
    public String url2 () {
        return env.getProperty("url");
    }
}
```

我们可以直接使用@Value 注解注入配置的属性值，也可以通过 Environment

对象来获取配置的属性值

### 10.5、Springcloud config 配置中心客户端测试  

通过客户端应用测试是否能够获取到配置中心配置的数据；

### 10.6、Springcloud config 的工作原理  

​        Spring cloud Config Server 的工作过程如下图所示：  

![1600422063333](C:\Users\ly138\AppData\Roaming\Typora\typora-user-images\1600422063333.png)

1、首先需要一个远程 Git 仓库，平时测试可以使用 GitHub，在实际生产环境中，需要自己搭建一个 Git 服务器，远程 Git 仓库的主要作用是用来保存我们的配置文件；

2、除了远程 Git 仓库之外，我们还需要一个本地 Git 仓库，每当 Config Server 访问远程 Git 仓库时，都会克隆一份到本地，这样当远程仓库无法连接时，就直接使用本地存储的配置信息；

3、微服务 A、微服务 B 则是我们的具体应用，这些应用在启动的时会从 Config Server 中获取相应的配置信息；

4.当微服务 A、微服务 B 尝试从 Config Server 中加载配置信息的时候，Config Server 会先通过 git clone 命令克隆一份配置文件保存到本地；

5、由于配置文件是存储在 Git 仓库中，所以配置文件天然具有版本管理功能；

### 10.7、Springcloud config 的安全保护  

​		生产环境中我们的配置中心肯定是不能随随便便被人访问的，我们可以加上适当的保护机制，由于微服务是构建在 Spring Boot 之上，所以整合 Spring Security 是最方便的方式。

1、在 springcloud config server 项目添加依赖：

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

2、在 springcloud config server 项目的 application.properties 中配置用户名密码：  

```properties
spring.security.user.name=wkcto
spring.security.user.password=123456
```

3、在 springcloud config client 上 bootstrap.properties 配置用户名和密码：   

```properties
spring.cloud.config.username=wkcto 
spring.cloud.config.password=123456
```

4、最后测试验证；