---
title: 在 windows 中安装 RockrtMQ
date: 2020-03-19 23:02:07
tags: RockrtMQ
---

# 在 windows 中安装 RockrtMQ

## 1、下载安装包
进入[官网](http://rocketmq.apache.org/)，选择最新版本，
并下载二进制安装包。
例如当前最新版本为 [4.7.0](https://www.apache.org/dyn/closer.cgi?path=rocketmq/4.7.0/rocketmq-all-4.7.0-bin-release.zip)

## 2、配置环境
解压安装包到指定位置：
> 例如：D:\App\Eva\rocketmq-all-4.7.0-bin-release

配置环境变量：
> 新增环境变量 ROCKETMQ_HOME 值为 D:\App\Eva\rocketmq-all-4.7.0-bin-release
> 
> - 同时可以将 D:\App\Eva\rocketmq-all-4.7.0-bin-release\bin 目录添加到 Path 环境变量中。
> - 此步骤是方便不用每次都需要进入 bin 目录启动相关服务，是非必须的。

## 3、启动 NameServer

```
mqnamesrv
```

> 当控制台输出 
> The Name Server boot success. serializeType=JSON
> 则表示服务启动成功。

## 4、启动 Broker
```
mqbroker -n 127.0.0.1:9876 autoCreateTopicEnable=true
```

> 当然控制台输出 
> boot success. serializeType=JSON and name server is 127.0.0.1:9876
> 则表示服务启动成功。

## 5、使用 rocketmq-console

```
# 拉取官方扩展仓库
# 速度慢的话 推荐使用码云的镜像仓库 https://gitee.com/mirrors/RocketMQ-Externals.git
git clone https://github.com/apache/rocketmq-externals.git

# 进入 rocketmq-console 目录
cd \rocketmq-externals\rocketmq-console

# 修改 src\main\resources\application.properties 文件
rocketmq.config.namesrvAddr=localhost:9876

# 运行
mvn spring-boot:run
```
执行成功后 直接访问 http://localhost:8080 即可

> 如果启动过程中出现 Failed to execute goal org.apache.maven.plugins:maven-checkstyle-plugin 错误
> 在 pom.xml 找到 org.apache.maven.plugins 部分直接注释掉即可
