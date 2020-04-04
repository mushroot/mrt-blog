---
title: 同时使用多个git账号
date: 2020-03-21 20:55:06
tags: git
---

# 一、清除本地的原有账户信息
```
# 查看是否存在全局配置
git config --global --list

# 存在则直接清除全局配置信息
git config --global --unset user.name
git config --global --unset user.email
```

# 二、生成ssh 密钥
进入用户目的.ssh目录下，创建每个账号使用的目录。
```
cd .ssh

# gitee 使用
mkdir gitee

# github 使用
mkdir github
```

# 三、生成对应账号的ssh密钥。
```
ssh-keygen -t rsa -C "email@email.com" -f github/id_rsa
```

> 1. -t 指定密钥类型，默认是 rsa ，可以省略。
> 2. -C 设置注释文字，比如邮箱。
> 3. -f 指定密钥文件存储文件名。

> 此时会要求输入密码，该密码是 push 的密码，一般留空就行

# 四、将秘钥添加到网站
复制秘钥
```
# Windows
clip < github/id_rsa.pub

# Powershell 中会提示： “<”运算符是为将来使用而保留的。
# 我们可以使用如下方式
Get-Content ./github/id_rsa.pub | clip
```
然后登陆账号找到 SSH key 填入

# 五、配置 ssh
在 .ssh 目录下添加一个 config 文件，内容如下

```
# gitee
Host gitee.com
  HostName gitee.com
  User email@email.com
  IdentityFile ./gitee/id_rsa

# github
Host github.com
  HostName github.com
  User email@email.com
  IdentityFile ./github/id_rsa
```

使用 PowerShell 可以直接复制上面的文字然后执行：
```
Get-Clipboard | Out-File config
```

# 六、修改仓库地址
如果之前使用的 https 方式访问，则需要修改仓库的远程地址：

```
git remote set-url origin git@github.com:<用户名>/<仓库名>.git
```

# 六、测试
```
ssh -T git@github.com
```

# 七、错误提示
> 如果你使用的是 PowerShell 可能会出现错误：
> line 1: Bad configuration option: \377\376h
> 那是因为默认使用的格式不对导致的。
> 我们需要在保存 config 文件之前，修改文本的格式为 'utf-8'：

```
$PSDefaultParameterValues['Out-File:Encoding'] = 'utf8'
```
