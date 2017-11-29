title: dubbo环境隔离
date: 2017-11-29 16:09:32
tags: dubbo
---
## 需求简介
    使用dubbo来进行服务管理，都是在分布式应用的前提下，涉及到多个子系统之间的调用，DUBBO所做的事情就是维护各个子系统暴露的接口和自动发现对应接口的远程地址从而实现分布式RPC服务管理。
    在项目开发和测试过程中涉及接口的联调。如果每个子系统自己维护自己系统的联调环境，可能会导致别人调用接口的不稳定，因为环境是系统自己人来维护，可能挂了也可能调整接口没通知相关人员，这对开发接口联调测试是一个问题。

## 解决方案
### 1、直连
    服务消费端:

        DUBBO在消费端提供了一个url的属性来指定某个服务端的地址
        <dubbo:reference interface="com.alibaba.dubbo.demo.HelloWorldService" check="false" id="helloWorldService"/>
        默认的方式是从注册中心发现接口为com.alibaba.dubbo.demo.HelloWorldService的服务，但是如果需要直连，可以在dubbo.properties下面配置dubbo.reference.helloWorldService.url=dubbo://ip:port/com.alibaba.dubbo.demo.HelloWorldService可以通过配置dubbo.reference.url=dubbo://ip:port/来让某个消费者系统的服务都指向制定的服务器地址

    服务提供端：
        只需要在dubbo.properties里面添加dubbo.registry.register=false即表示当前系统的服务不发布到注册中心
        为了达到环境的隔离，最好不用为了切换环境而调整源码.所以通过配置在dubbo.properties
### 2、通过服务分组/版本号
    通过给每个子环境分配一个分组来实现各个子环境在一个组里面，从而实现各个环境的隔离
    服务消费方：
       <dubbo:reference interface="com.alibaba.dubbo.demo.HelloWorldService" check="false" id="helloWorldService"/>
       针对上面的接口只能调用指定的分组，可以在dubbo.properties中添加dubbo.reference.helloWorldService.group=test，那么该接口只会从test分组中发现对应接口的服务了。也可以将所有服务都指向某个分组dubbo.reference.group=test。

    服务提供方：
        <dubbo:service interface="com.alibaba.dubbo.demo.HelloWorldService" id="helloWorldRemote" ref="helloWorld"/>
        针对上面的接口发布到指定的分组，也是在dubbo.properties中添加dubbo.service.helloWorldRemote.group=test，那么该服务就发布到了test分组,同样也可以将当前系统所有服务发布到指定分组dubbo.service.group=test。

        而通过版本号也是类似的方案，只是配置的属性不是group而是version

     group在dubbo的定义是服务接口有多种实现而进行分组的（version也是类似），不是进行环境上面隔离的，所以虽然dubbo提供了这种功能，但是设计的目的不是做这种事情的，那么就不能这么硬拉过来，不然会导致团队开发理解不一直出现问题。另外这种方案会导致注册中心比较混乱，因为注册中心是所以环境公用的，那么会导致一个注册中心中存在多个环境的接口，也不便于维护。

### 3、注册中心分组
    通过dubbo.registry.group=test来实现注册中心的分组，但是这有个问题，如果配置了这个，那么当前系统的服务发现和服务注册都会到这个组里面来进行，不能分别对服务发现和服务注册单独配置，也不能对某个接口进行配置












