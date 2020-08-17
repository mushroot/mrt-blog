---
title: 安卓app开发中使用 MITM proxy 进行抓包
date: 2020-08-16 22:44:55
tags: Proxy
---

# 一、安装 Mitmproxy

## 1. 使用 homebrew 方式安装
```
brew install mitmproxy
```

## 2. 或者使用python包管理安装
```
pipx install mitmproxy
```

> 更多方式参考 [官网](https://mitmproxy.org/)

# 二、 运行 mitmproxy
> 这里我使用 **mitmweb** 来启用插件，这样会打开一个默认的web页面，方便操作。
```
mitweb -web-port 8889 -p 8888
// -web-port 指定web端的端口，默认为8081，如果不需要改变端口可以忽略
// -p 代理监听端口
```

# 三、设置手机
> 可以是模拟器也可以是真机，在设置上并无本质上的差异，模拟器可以在菜单栏直接设置。

设置手机proxy地址为 当前电脑IP:8888 即可，此时就可以在打开的[WEB端](http://localhost:8889)看见请求信息了。

# 四、配置HTTPS证书
前面步骤的设置并无法看见https的请求，这里我们需要手动导入证书，
1. 打开手机浏览器（推荐使用chrome，其他浏览器可能会出现无法下载证书的情况。）
2. 打开网址 mitm.it 此时即可看见证书下载页面，选择对应系统的证书即可

{% asset_img 02.png %}

> 如果此时提示“无法安装该证书，因为无法读取证书文件”，可以直接去手机设置中的》
> “凭证储存” 选择使用 “从储存设备安装”即可（不同版本系统的位置可能不一样，一般可以在系统安装中找到）

{% asset_img 01.png %}

3. 五、修改配置
新版系统默认禁用了用户证书，我们需要手动开启。

1. 创建文件 res/xml/network_security_config.xml 并填写如下配置：
```
<network-security-config>
   <domain-config cleartextTrafficPermitted="true">
      <!-- 如果配置证书后无法连接 BUNDLE 服务，则需要对本地服务做单独配置 -->
      <domain includeSubDomains="true">localhost</domain>
      <domain includeSubDomains="false">10.0.2.2</domain>
      <domain includeSubDomains="false">10.0.3.2</domain>
      <domain includeSubDomains="false">192.168.11.11</domain>
   </domain-config>
   <debug-overrides>
      <trust-anchors>
         <certificates src="system" />
         <certificates src="user" />
      </trust-anchors>
   </debug-overrides>
</network-security-config>
```
2. 在 AndroidManifest.xml 中的 application 引入配置
```
<application
  android:networkSecurityConfig=”@xml/network_security_config"
  ...
  >
```
现在就可以正常使用SSL了。