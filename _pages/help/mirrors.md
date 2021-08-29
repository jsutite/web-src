---
layout: page
title: "ITE MIR189 (开源镜像) 服务使用说明"
author: Peter Pan
permalink: /help/mirrors/
---

### 服务总览

ITE 现提供一台**内网**开源镜像服务器。

# [192.168.6.189](http://192.168.6.189/)

受服务器性能影响，当前仅为校内提供 `CentOS, EPEL` 镜像服务。

## CentOS 镜像使用帮助

该文件夹只提供 CentOS 7 与 8，架构仅为 `x86_64` ，如果需要较早版本的 CentOS，请参考清华源 [centos-vault](https://mirrors.tuna.tsinghua.edu.cn/help/centos-vault/) 的帮助，若需要其他架构，请参考清华源 [centos-altarch](https://mirrors.tuna.tsinghua.edu.cn/help/centos-altarch/) 的帮助。

建议先备份 `/etc/yum.repos.d/` 内的文件（CentOS 7 及之前为 `CentOS-Base.repo`，CentOS 8 为`CentOS-Linux-*.repo`）

然后编辑 `/etc/yum.repos.d/` 中的相应文件，在 `mirrorlist=` 开头行前面加 `#` 注释掉；并将 `baseurl=` 开头行取消注释（如果被注释的话），把该行内的域名（例如`mirror.centos.org`）替换为 `192.168.6.189`。

以上步骤可以被下方的命令一步完成

```
sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
         -e 's|^#baseurl=http://mirror.centos.org|baseurl=http://192.168.6.189|g' \
         -i.bak \
         /etc/yum.repos.d/CentOS-*.repo
```

注意其中的`*`通配符，如果只需要替换一些文件中的源，请自行增删。

注意，如果需要启用其中一些 repo，需要将其中的 `enabled=0` 改为 `enabled=1`。

最后，更新软件包缓存

```
sudo yum makecache
```

## EPEL 镜像使用帮助

EPEL(Extra Packages for Enterprise Linux)是由Fedora Special Interest Group维护的Enterprise Linux（RHEL、CentOS）中经
常用到的包。


下面以CentOS 7为例讲解如何使用本镜像站的epel镜像。

首先从CentOS Extras这个源（[本镜像站](http://192.168.6.189/centos)也有镜像）里安装epel-release：

```
yum install epel-release
```

<!-- 当前ITE已经在epel的官方镜像列表里，所以不需要其他配置，mirrorlist机制就能让你的服务器就近使用ITE的镜像。-->
修改`/etc/yum.repos.d/epel.repo`，将`mirrorlist`和`metalink`开头的行注释掉。

接下来，取消注释这个文件里`baseurl`开头的行，并将其中的`http://download.fedoraproject.org/pub`替换成`http://192.168.6.189`。

可以用如下命令自动替换：（来自 https://github.com/tuna/issues/issues/687） 

```
sed -e 's!^metalink=!#metalink=!g' \
    -e 's!^#baseurl=!baseurl=!g' \
    -e 's!//download\.fedoraproject\.org/pub!//192.168.6.189!g' \
    -i /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel-testing.repo
```

修改结果如下：（仅供参考，不同版本可能不同）

```
[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
baseurl=http://192.168.6.189/epel/7/$basearch
#mirrorlist=http://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch
failovermethod=priority
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7

[epel-debuginfo]
name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
baseurl=http://192.168.6.189/epel/7/$basearch/debug
#mirrorlist=http://mirrors.fedoraproject.org/metalink?repo=epel-debug-7&arch=$basearch
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1

[epel-source]
name=Extra Packages for Enterprise Linux 7 - $basearch - Source
baseurl=http://192.168.6.189/epel/7/SRPMS
#mirrorlist=http://mirrors.fedoraproject.org/metalink?repo=epel-source-7&arch=$basearch
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1
```

运行 `yum update` 测试一下吧。
