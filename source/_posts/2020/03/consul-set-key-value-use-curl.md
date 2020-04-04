---
title: 利用 CURL 设置 consul 的 key/value
date: 2020-03-19 22:50:07
tags: consul
---

# 利用 CURL 设置 consul 的 key/value

> ___KV储存的最大不能超过 512kb。___

## 添加
```
# 设置 /config/dw 的值为 foo
$ curl -X PUT -d 'foo' http://localhost:8500/v1/kv/config/dw
```
- ?flag=33

## 查看全部的 key/value
```
$ curl http://localhost:8500/v1/kv/:key?recurse
```
参数说明：
- recurse (bool: false) 指定查看多个 KV
- keys (bool: false) 查看全部的 key 值
- key (string: "") 指定需要读取的 key 值
- dc (string: "") - 指定需要查询的数据中心。
- raw (bool: false) - 指定显示键的原始值。
- separator (string: "") - 指定用于递归查询的分隔符。


## 将 key/value 保存到文件 
```
$ curl http://localhost:8500/v1/kv?recurse -o data.json
```

## 查看单个
```
$ curl http://localhost:8500/v1/kv/config/dw/data
```
> 返回结果的 value 为 base64 编码，需要自己转义。

## 修改
> 方式和增加一样
```
$ curl -X PUT -d 'foo2' http://localhost:8500/v1/kv/config/dw/data
```

## 删除
1. 删除指定的 
```
$ curl -X DELETE http://localhost:8500/va/kv/config/dw/data1
```
2. 批量删除
```
$ curl -X DELETE http://localhost:8500/v1/kv/config/dw/?recurse
```
> 这里必须要使用 recurse
