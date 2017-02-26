+++
date = "2017-02-08"
title = "Consul在服务发现中的应用"
draft = false
description = ""
tags        = ["Consul", "服务发现"]
categories = [ "技术"]
+++

Consul的基本原理及其在服务发现中的使用

<!--more-->
 

## 服务发现
 服务发现,通俗的讲,就是通过关键字查询特定的服务. 

## 服务发现的意义 

- 如果服务本身发生了变化, 只需要更新服务即可, 与服务相关的关键字不变, 因此服务的使用者不需要关心该变化

- 服务进行升级后, 如果服务要求的参数不变(平滑升级可以做到), 服务使用者只需要用新的关键字替换之前的关键字即可

- 同一个服务的多个版本可以同时存在, 使用者可以根据需求方便的切换版本

## Consul 是什么

Consul是 [HashiCorp](https://www.hashicorp.com/)开发的用于服务发现的系统. 它的主要特性有:

- 支持通过 DNS 和 HTTP 发现服务
- 提供服务的健康检查
- 键值存储
- 多数据中心

通过服务发现的方式为使用者提供服务, 稳定是最重要的考虑因素, Consul 的去中心化的多节点提供稳定的服务. 同时因为健康检查的存在, 可以保证服务的有效性.

## Consul 的部署

### 说明
每台服务器上至少有一个 Consul 的节点( Agent ), 节点有两种模式: 服务器模式和客户端模式. 

- 服务器模式提供服务注册和查找等功能, 是 Consul 去中心化的关键节点,推荐至少部署三个节点. 

- 客户端模式负责转发服务请求到服务器模式的节点. 

由于两种模式的节点负责功能的不同, 可以假设客户端模式的节点是稳定存在的, 而服务器模式的节点可能会异常退出.

### 安装

根据系统[下载](https://www.consul.io/downloads.html)合适的版本, 并解压至合适的目录.

假设有4台服务器, IP分别为 **172.16.1.34**, **172.16.1.35**, **172.16.1.37**, **172.16.1.69**

我们将在 34, 35, 69 上启动服务器模式节点, 在37上启动客户端模式节点, 启动命令如下:

##### 34
``` 
./consul agent -server -bootstrap-expect=3 -node=agent34 -data-dir=./ &
```

###### 35 
```
 ./consul agent -server -bootstrap-expect=3 -node=agent35 -data-dir=./ &  
 ```
##### 69 
```
 ./consul agent -server -bootstrap-expect=3 -node=agent69 -data-dir=./ &  
```
##### 37  
```
 ./consul agent  -node=agent37 -retry-join=172.16.1.35 -data-dir=./ & 
 ```

在35和34服务器上运行  
```
 ./consul join 172.16.1.69 
 ``` 
 三个服务器模式节点将选出一个 leader, 服务启动完成.

查看一下节点状态, 运行命令
```
 ./consul members 
``` 
结果类似如下:

```
Node     Address           Status  Type    Build  Protocol  DC
agent34  172.16.1.34:8301  alive   server  0.7.2  2         dc1
agent35  172.16.1.35:8301  alive   server  0.7.2  2         dc1
agent37  172.16.1.37:8301  alive   client  0.7.2  2         dc1
agent69  172.16.1.69:8301  alive   server  0.7.2  2         dc1

```

### 使用

##### 使用HTTP API

- 注册服务 [/v1/agent/service/register](https://www.consul.io/docs/agent/http/agent.html#agent_service_register)

- 查找服务 [/v1/health/service/<service>](https://www.consul.io/docs/agent/http/health.html#health_service)

##### 使用 SDK
系统中使用 Consul 需要对其 HTTP API 进行封装, 因此可以直接使用官方封装的 Go 版的 SDK

- 注册服务 参考[代码](https://github.com/ssor/consul_service_reg/blob/master/main.go)

- 查找服务 参考[代码](https://github.com/ssor/consul_query_service_example/blob/master/main.go)

### 总结
基本的使用方法介绍完毕, 下一步将对实际工程中的使用问题进行补充
