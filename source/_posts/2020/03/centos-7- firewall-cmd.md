---
title: 【转】centos 设置防火墙
date: 2020-03-27 15:18:07
tags: centos
---

```
// 查看防火墙状态
firewall-cmd --state

// 打开防火墙
systemctl start firewalld

// 查看当前防火墙开放的端口
firewall-cmd --list-ports

// 添加80端口
firewall-cmd --zone=public --add-port=80/tcp --permanent

// 重载防火墙数据
firewall-cmd --reload
```
————————————————
版权声明：本文为CSDN博主「燕双嘤」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_42192693/java/article/details/101789718
