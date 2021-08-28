---
layout: page
title: "ITE MIR189 (开源镜像) 服务使用说明"
author: Peter Pan
permalink: /help/mirrors/
---

### 服务总览

ITE 现提供一台**内网**开源镜像服务器。

# [192.168.6.189](http://192.168.6.189/)

受服务器性能影响，当前仅为校内提供 `CentOS, Kali-images, Ubuntu-release` 镜像服务。

## CentOS 镜像使用帮助

该文件夹只提供 CentOS 7 与 8，架构仅为 `x86_64` ，如果需要较早版本的 CentOS，请参考清华源 [centos-vault](https://mirrors.tuna.tsinghua.edu.cn/help/centos-vault/) 的帮助，若需要其他架构，请参考清华源 [centos-altarch](https://mirrors.tuna.tsinghua.edu.cn/help/centos-altarch/) 的帮助。

建议先备份 `/etc/yum.repos.d/` 内的文件（CentOS 7 及之前为 `CentOS-Base.repo`，CentOS 8 为`CentOS-Linux-*.repo`）

然后编辑 `/etc/yum.repos.d/` 中的相应文件，在 `mirrorlist=` 开头行前面加 `#` 注释掉；并将 `baseurl=` 开头行取消注释（如果被注释的话），把该行内的域名（例如`mirror.centos.org`）替换为 `192.168.6.189`。

以上步骤可以被下方的命令一步完成

```
sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
         -e 's|^#baseurl=http://mirror.centos.org|baseurl=https://192.168.6.189|g' \
         -i.bak \
         /etc/yum.repos.d/CentOS-*.repo
```

注意其中的`*`通配符，如果只需要替换一些文件中的源，请自行增删。

注意，如果需要启用其中一些 repo，需要将其中的 `enabled=0` 改为 `enabled=1`。

最后，更新软件包缓存

```
sudo yum makecache
```
