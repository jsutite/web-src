---
layout: page
title: "ITE NTP (网络授时) 服务使用说明"
author: Peter Pan
permalink: /help/ntp/
---

### 服务总览

ITE 现提供一台时间服务器：

# 192.168.6.190

服务器位于江理工校内，仅提供 IPv4 服务。校内师生可以使用这一服务进行一般的时间校准工作，但需要进行校园网认证，故不建议在宿舍网内使用。

校外或宿舍网使用NTP服务需使用[远程接入](http://www.jstu.edu.cn/3412/list.htm)。

### 服务介绍

NTP (网络时间协议, network time protocol) 是网络中保持时间同步的协议 ([How does it work](http://www.ntp.org/ntpfaq/NTP-s-algo.htm))。简单来说，客户端通过向服务器发送带有时间戳的数据包和服务器回复带有时间戳的数据包，来获得客户端发送时间，服务端接收时间、服务端发送时间、客户端接收时间 4 个时间戳。客户端系统时间偏移量定义为 δ = (t₃ - t₀) - (t₂ - t₁)。如果运行 ntpd 服务，一般来说 ntpd 会逐渐调整时钟，避免时间跳变。这对于运行计费系统、交易系统或者其他对时间准确性和事件先后顺序敏感的操作十分重要。

### Linux 客户端配置

使用 systemd-timesyncd 的用户需要修改 `/etc/systemd/timesyncd.conf`，将其中 `NTP=` 一行取消注释，修改为 `NTP=192.168.6.190` 。同时建议校内用户在 `FallbackNTP` 一栏中添加服务总览中提到的备用 NTP。修改好后，可使用 `systemctl restart systemd-timesyncd` 使配置生效。

使用 ntpd 的用户需要在 `/etc/ntp.conf` 中添加一行 `server 192.168.6.190` 。（若您的发行版使用 Chrony，请修改对应的配置文件 `/etc/chrony.conf`。）

为了确保 ntpd 服务正在运行，使用你的发行版的 initscripts 脚本或 `systemctl`（若有）进行检查和修正。

如果你的机器的时钟发生跳变不会有严重后果 (例如在你的笔记本上)，你可以使用 `sudo ntpdate 192.168.6.190` 进行一次性的同步。

### Mac 客户端配置

在“系统配置 > 日期与时间 > 自动设置日期与时间”一栏，填入 `192.168.6.190`。

在 macOS Mojave 及更新的系统，你可以使用 sudo sntp -sS 192.168.6.190 来进行一次性的同步，否则，使用 `sudo ntpdate 192.168.6.190` 进行同步。

### Windows 客户端配置

Windows 7 及以下版本的配置方式可以参看中国科学院提供的[教程](http://www.cas.cn/tz/201809/t20180921_4664344.shtml)。  

#### Windows 10 客户端配置

在“控制面板 > 时钟、语言和区域 > 日期和时间 > Internet时间 > 更改设置”中勾选“与 Internet 时间服务器同步”，在“服务器”一栏填入 `192.168.6.190`。  

您也可以通过在命令提示符中使用  `w32tm /config /manualpeerlist:192.168.6.190 /syncfromflags:manual /update` 来将此服务器设置为您的时间服务器.
