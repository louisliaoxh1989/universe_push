# 概述
这个一个基础的消息通信架构，只在解决服务端与客户端消息通信，可应用于消息推送，即时通信以及由此衍生出来的消息通信业务。本项目基于其他开源项目的基础上，
希望通过合理的分布式架构，解决大规模并发链接的问题，从而适应互联网用户不断增长的需求，本项目将会采用微服务的开发与设计模式进行架构设计，尽量保持各个业务的单一性和高可用性。
这样的目的也是基于业务扩展的方式，方便以后能够在基于此通信架构基础上衍生其他相关业务，从而保持业务的独立性，增加项目的可维护性。

> 为了开源以及方便部署,将更换RPC框架为`Dubbo`,并且全部基于`SpringBoot`

# 部署说明

## 准备工作
为了脚本能够正常工作,请先在你的服务器建立如下目录`/opt/boot`,这个是脚本自动查找spring boot工程的目录,该目录下存放所有spring boot工程,具体工程目录结构如下：

## SpringBoot Dubbo服务启动
由于dubbo严格遵守服务依赖启动顺序，请安装顺序启动如下服务

* 运行`mvn clean package -Dmaven.test.skip=true` 打包springboot jar 

* 启动`spring-boot-dubbo-push-subscribe`订阅服务
```shell
/opt/boot/push-sub
├── jvm.ini
├── push-ap //可执行启动脚本
└── log
   └── push-api.log //存放日志
└── lib
   └── push-api-1.0-SNAPSHOT.jar //可运行的jar
```

* 启动`spring-boot-dubbo-push-connector`链接服务

```shell
/opt/boot/push-api
├── jvm.ini
├── push-connector //可执行启动脚本
└── log
   └── push-connector.log //存放日志
└── lib
   └── push-connector-1.0-SNAPSHOT.jar //可运行的jar
```

## SpringBoot web项目

* 启动`spring-boot-web-push-api`开放推送服务
```shell
/opt/boot/push-sub
├── jvm.ini
├── push-api //可执行启动脚本
└── log
   └── push-api.log //存放日志
└── lib
   └── push-api-1.0-SNAPSHOT.jar //可运行的jar
```


* 启动`sping-boot-web-push-group`群组服务
```shell
/opt/boot/push-sub
├── jvm.ini
├── push-group //可执行启动脚本
└── log
   └── push-group.log //存放日志
└── lib
   └── sping-boot-web-push-group-1.0.0-SNAPSHOT.jar //可运行的jar
```

# 基础架构

![image](attachment/push-universe.png)

## Push-connector
### 基本功能
* 提供集群链接管理的能力
* 提供消息群发的功能
* 提供消息单发的功能
* 客户端管理服务

## Push-Cache[计划中]
### 基本功能
提供消息缓存服务
* 消息缓存
* 消息过期

## Push-API
### 基本功能
提供推送Http接口服务，调用推送网关发送推送消息，此接口采用springboot开发web服务


## Push-Group
* 单聊服务
* 群组服务



# 部署

## 常见问题

> 部署connector要同时更新，防止出现redisson发布订阅数据解析问题

> redisson 发布时最好使用统一个logger,不然会有序列化问题

# 参考资料
## 参考项目
* [T-io](https://github.com/tywo45/t-io)
* [蚂蚁通信框架实践](https://mp.weixin.qq.com/s/JRsbK1Un2av9GKmJ8DK7IQ)

