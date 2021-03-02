# Aria2基础上手指南 - 知乎
**前言**

这是一篇小白写给小白看的 Aria2 教程，旨在帮助不熟悉命令行、代码的用户也能用上 Aria2。各位大神请不要打脸，错误之处请指教。

## **1. 为什么用 Aria2？**

知道 Aria2 也有一段时间了，曾经抱着尝鲜的心态去试用，看着各种代码、命令行直接就放弃了。那时迅雷极速版还挺好用，下载都能跑满带宽。对上手这个极客范、连个软件界面都没有的下载工具就没多少动力。

之后迅雷极速版不能加速了，逼着用户升级迅雷 9 浏览器。这种 UI 实在是无法接受，只能另觅其他的下载工具。Free Download Manager、uTorrent、qBittorrent、BitComet 等等全部折腾了一遍，包括多年前的脱兔都用上了...... 从速度、稳定性、资源占用各方面比较下来，最终决定使用 qBittorrent。uTorrent 因为有广告，绿色版的话不知道是否安全，所以用免费、开源的 qBittorrent。qBittorrent 还有个不错的功能就是安装 Python 后，可以根据文件名搜索种子，挑个健康度好的下载。

![](https://pic1.zhimg.com/v2-0e88df21db6b92cd29cac944fc4384bc_b.jpg)

但 qBittorrent 对 http 下载有文件大小限制，必须再找一款 http 下载工具，偶尔应付大文件下载。当然用 Aria2 下载磁链或者 BT 都是没有问题的，只是在操作便捷性和任务管理上没有 qBittorrent 方便、响应及时。另外用 Aria2 下载磁链，在下载目录会多一个磁链信息文件，强迫症表示，每次用完都要去删有点繁琐。

**Aria2 强悍的下载性能**

我之前在[《如何看待迅雷 U 享版？》](https://www.zhihu.com/question/67077152/answer/250592879)这个问题的回答中做了 Aria2 和迅雷 U 享版的对比测试，下载同一 http 资源，V7 会员迅雷 U 享版速度只有 Aira2 的 60% 左右。

![](https://pic4.zhimg.com/v2-347ebbcb544572c8b2410ee9d998331f_b.jpg)
![](https://pic4.zhimg.com/v2-6bda99f0a35ad807a328aae4cf960b13_b.jpg)

好久没见到的满带宽下载速度。现阶段的网络环境，使用 Aria2 的确是一个不错的选择。

## 2.Aria2 使用难点

2.1 教程对于非程序员用户难度高

一开始上手 Aria2 的过程中，难以找到一篇面向小白的教程。多数教程的画风是这样的：

![](https://pic1.zhimg.com/v2-fa9c3d035a380fdb9bb3dda1183cb824_b.jpg)
![](https://pic2.zhimg.com/v2-d370f88d90310fc971a05c64a7277c49_b.jpg)

一条命令就行了？那要在哪里输入命令？“target url” 要怎么填？等等一堆堆问题就扑面而来了。而且不同操作系统，配置方法有所区别。更加增添了上手难度。

上图的举例，并没有任何质疑和不尊重的意思，对于熟悉代码的人肯定一眼就知道该怎么弄了。但是对不熟悉代码和命令行操作的用户而言有点无从下手。

2.2 容易混淆的地方

下载 Aria2 压缩包之后，运行解压出的可执行文件后，没有任何反应，打开 WebUI 也显示配置错误。曾经就是在这一步放弃。

**迫于寻找下载工具的刚需，认真学习了几个教程之后，终于搞明白 Aria2 要怎么用了，其实本质很简单的，只是被一堆代码、命令行所迷惑。** 

## 3. 一步一步安装配置 Aria2（Win10 环境）

我学习的教程包括：

[Aria2 & YAAW 使用说明](https://link.zhihu.com/?target=http%3A//aria2c.com/usage.html)

[Windows 下如何配置 Aria 2](https://zhuanlan.zhihu.com/p/21831960)

这两篇帮助很大，完全明白了整个安装流程，其实并不复杂，其中又有一些不同的地方，操作步骤做了一些修改更适合新手用户。

3.1 官网下载 Aria2 程序 [Aria2 1.33.0](https://link.zhihu.com/?target=https%3A//aria2.github.io/)

解压后如下图所示

![](https://pic3.zhimg.com/v2-55636887e17284f7c2d4f32a56ffb48e_b.jpg)

其实我们只需要其中的 aria2c 这个可执行文件，这就是最新 64 位版本的 Aria2

3.2 下载[Aria2 & YAAW 使用说明](https://link.zhihu.com/?target=http%3A//aria2c.com/usage.html)这篇教程中，Aria2 相关下载 -[Windows 懒人包下载](https://link.zhihu.com/?target=http%3A//aria2c.com/archiver/aria2.zip)

这一步主要是需要当中的一个启动文件，**黑色图标那个。没有找到这个软件的官网，只好引用了教程原作者的链接。** 

![](https://pic4.zhimg.com/v2-ca5a7c9389c7d17efabb518ba7e443af_b.jpg)

**这个可执行文件的作用是让你不用写命令行就可以运行 Aria2，可以简单理解为 Aira2 的启动器或快捷方式，添加到快捷启动栏后，需要下载时点击一下这个图标，Aria2 就启动了。启动后会图标会现实中右下角的通知区域，不需要使用 Aria2 时，右键退出就行。** 

![](https://pic1.zhimg.com/v2-24adc81d0f3ceee7d56c606a21b74a08_b.jpg)

3.3 所下载的 Windows 懒人包里的 Aria2 并不一定是最新版。所以要用官网下载的最新版 aria2c.exe 替换。或者也可以新建一个文件夹，把最新版的 aria2c.exe、和黑色图标的 aria2.exe 放入，这个文件夹可视为绿色版的下载软件了，按照你的习惯放到软件存放目录。可以跟你的下载文件夹在不同盘符，没有影响的。

3.4 添加 aria2.session 文件。这个文件记录了任务记录，必须在下载目标文件夹中。用记事本新建一个空白文件，然后另存为 - aria2.session，保存类型为：所有文件即可。

![](https://pic4.zhimg.com/v2-088a59403a9887e83631e1c695598437_b.jpg)

**把这个文件放到你的下载目标文件夹**

3.5 在 Aria2 软件存放文件夹，放置了两个可执行文件的那个。用记事本新建一个空白文件，然后另存为 - aria2.conf，保存类型为：所有文件即可。

用记事本打开用 aria2.conf，此时是空白的，粘贴如下代码，此代码引用自《[Aria2 & YAAW 使用说明](https://link.zhihu.com/?target=http%3A//aria2c.com/usage.html)》，按需修改保存即可。**当中 #号代表注释内容，删除了 #号的注释项才会生效。** 

```text
## '#'开头为注释内容, 选项都有相应的注释说明, 根据需要修改 ##
## 被注释的选项填写的是默认值, 建议在需要修改时再取消注释  ##

## 文件保存相关 ##

# 文件的保存路径(可使用绝对路径或相对路径), 默认: 当前启动位置
dir=~/downloads
# 启用磁盘缓存, 0为禁用缓存, 需1.16以上版本, 默认:16M
#disk-cache=32M
# 文件预分配方式, 能有效降低磁盘碎片, 默认:prealloc
# 预分配所需时间: none < falloc ? trunc < prealloc
# falloc和trunc则需要文件系统和内核支持
# NTFS建议使用falloc, EXT3/4建议trunc, MAC 下需要注释此项
#file-allocation=none
# 断点续传
continue=true

## 下载连接相关 ##

# 最大同时下载任务数, 运行时可修改, 默认:5
#max-concurrent-downloads=5
# 同一服务器连接数, 添加时可指定, 默认:1
max-connection-per-server=5
# 最小文件分片大小, 添加时可指定, 取值范围1M -1024M, 默认:20M
# 假定size=10M, 文件为20MiB 则使用两个来源下载; 文件为15MiB 则使用一个来源下载
min-split-size=10M
# 单个任务最大线程数, 添加时可指定, 默认:5
#split=5
# 整体下载速度限制, 运行时可修改, 默认:0
#max-overall-download-limit=0
# 单个任务下载速度限制, 默认:0
#max-download-limit=0
# 整体上传速度限制, 运行时可修改, 默认:0
#max-overall-upload-limit=0
# 单个任务上传速度限制, 默认:0
#max-upload-limit=0
# 禁用IPv6, 默认:false
#disable-ipv6=true
# 连接超时时间, 默认:60
#timeout=60
# 最大重试次数, 设置为0表示不限制重试次数, 默认:5
#max-tries=5
# 设置重试等待的秒数, 默认:0
#retry-wait=0

## 进度保存相关 ##

# 从会话文件中读取下载任务
input-file=/etc/aria2/aria2.session
# 在Aria2退出时保存`错误/未完成`的下载任务到会话文件
save-session=/etc/aria2/aria2.session
# 定时保存会话, 0为退出时才保存, 需1.16.1以上版本, 默认:0
#save-session-interval=60

## RPC相关设置 ##

# 启用RPC, 默认:false
enable-rpc=true
# 允许所有来源, 默认:false
rpc-allow-origin-all=true
# 允许非外部访问, 默认:false
rpc-listen-all=true
# 事件轮询方式, 取值:[epoll, kqueue, port, poll, select], 不同系统默认值不同
#event-poll=select
# RPC监听端口, 端口被占用时可以修改, 默认:6800
#rpc-listen-port=6800
# 设置的RPC授权令牌, v1.18.4新增功能, 取代 --rpc-user 和 --rpc-passwd 选项
#rpc-secret=<TOKEN>
# 设置的RPC访问用户名, 此选项新版已废弃, 建议改用 --rpc-secret 选项
#rpc-user=<USER>
# 设置的RPC访问密码, 此选项新版已废弃, 建议改用 --rpc-secret 选项
#rpc-passwd=<PASSWD>
# 是否启用 RPC 服务的 SSL/TLS 加密,
# 启用加密后 RPC 服务需要使用 https 或者 wss 协议连接
#rpc-secure=true
# 在 RPC 服务中启用 SSL/TLS 加密时的证书文件,
# 使用 PEM 格式时，您必须通过 --rpc-private-key 指定私钥
#rpc-certificate=/path/to/certificate.pem
# 在 RPC 服务中启用 SSL/TLS 加密时的私钥文件
#rpc-private-key=/path/to/certificate.key

## BT/PT下载相关 ##

# 当下载的是一个种子(以.torrent结尾)时, 自动开始BT任务, 默认:true
#follow-torrent=true
# BT监听端口, 当端口被屏蔽时使用, 默认:6881-6999
listen-port=51413
# 单个种子最大连接数, 默认:55
#bt-max-peers=55
# 打开DHT功能, PT需要禁用, 默认:true
enable-dht=false
# 打开IPv6 DHT功能, PT需要禁用
#enable-dht6=false
# DHT网络监听端口, 默认:6881-6999
#dht-listen-port=6881-6999
# 本地节点查找, PT需要禁用, 默认:false
#bt-enable-lpd=false
# 种子交换, PT需要禁用, 默认:true
enable-peer-exchange=false
# 每个种子限速, 对少种的PT很有用, 默认:50K
#bt-request-peer-speed-limit=50K
# 客户端伪装, PT需要
peer-id-prefix=-TR2770-
user-agent=Transmission/2.77
# 当种子的分享率达到这个数时, 自动停止做种, 0为一直做种, 默认:1.0
seed-ratio=0
# 强制保存会话, 即使任务已经完成, 默认:false
# 较新的版本开启后会在任务完成后依然保留.aria2文件
#force-save=false
# BT校验相关, 默认:true
#bt-hash-check-seed=true
# 继续之前的BT任务时, 无需再次校验, 默认:false
bt-seed-unverified=true
# 保存磁力链接元数据为种子文件(.torrent文件), 默认:false
bt-save-metadata=true
```

看着是一大堆代码，其实很简单。就是文字界面的 Aria2 软件设置。

**路径可以使用绝对路径，我的 Aria2 软件是放在 G 盘，下载目录是 D 盘，所以在 aria2.conf 这个配置文件中，路径和进度保存要设置在同一个文件夹：** 

```text
dir=D:/Aria2/

.......

## 进度保存相关 ##

# 从会话文件中读取下载任务
input-file=D:\aria2\aria2.session
# 在Aria2退出时保存`错误/未完成`的下载任务到会话文件
save-session=D:\aria2\aria2.session
```

另外我希望打开 DHT 功能，代码就改几个字母

理解为一个文字化的软件设置界面就可以了。顺着一行行看一下，根据自己的需求设定数值或者填写 true 或 false 就行，几分钟就可以设置完成。

需要注意的是，因软件限制，每台服务器最大连接数只能填 16，超过这个数值就会报错。（部分魔改版可以突破这个限制，但个人觉得速度已经够用）

```text
max-connection-per-server=16
```

3.6 根据自身需求修改完参数后保存。**确保 aria2.conf、aria2c.exe(Aria2 主程序)、aira2.exe（Aria2 启动器），在同一目录下。aria2.session 在下载目标目录下。** 现在 Aria2 已结配置好了，点击 aira2.exe（Aria2 启动器）图标，此时 Aria2 已开始运行。

![](https://pic4.zhimg.com/v2-ca9f85b187ca46c46eb9f7aeed650437_b.jpg)

3.7 开始下载

Aira2 相当轻量化，没有软件界面。怎样添加下载任务呢？十分简单，打开浏览器，输入网址[aria2c.com](https://link.zhihu.com/?target=http%3A//aria2c.com/)就可以打开操作界面了。可以把这个网址放到书签中，方便使用。

![](https://pic4.zhimg.com/v2-cfa0e4aa7a1c9dfe09f5cd18fc9e3a7b_b.jpg)

可以添加 http、https、磁链、BT 种子（Aira2 不支持 ed2k 链接）开始下载。成功安装后不会有报错提示，右上角可以看到 Aira2 的版本号。

**上面步骤写的很多，其实总结下思路，就是获得 Aria2 启动器、最新版主程序 - 修改配置文件 - 启动 - 打开 WebUI 控制页 - 下载。** 

## **4. Aria2 进阶**

[aria2c(1) - aria2 1.33.0 documentation](https://link.zhihu.com/?target=https%3A//aria2.github.io/manual/en/html/aria2c.html) 可访问官方文档获得更多信息

4.1 使用不同的 WebUI

1、webui-aria2：[https://github.com/ziahamza/webui-aria2](https://link.zhihu.com/?target=https%3A//github.com/ziahamza/webui-aria2)

![](https://pic4.zhimg.com/v2-6f97b92c1e6ebeb4153270f9727f82c3_b.jpg)

中文版界面，使用无压力，如果报错的话，记得修改服务器地址：

![](https://pic4.zhimg.com/v2-ca1a1d0de2705349536e86136a3f244f_b.jpg)

2、AriaNg [https://github.com/mayswind/AriaNg](https://link.zhihu.com/?target=https%3A//github.com/mayswind/AriaNg)

![](https://pic3.zhimg.com/v2-0f833c107dc6900b51e34e4e36da7d02_b.jpg)

和传统下载软件相似的界面

使用方法：下载 zip 包，解压后直接运行 index.html 就可打开 WebUI 界面，同样可以收藏到书签，方便使用。

4.2 [解决 Aria2 BT 下载速度慢没速度的问题](https://link.zhihu.com/?target=http%3A//www.senra.me/solutions-to-aria2-bt-metalink-download-slowly/)

[Senra の小窝 | 初闻天籁之音，未使心之将来。](https://link.zhihu.com/?target=http%3A//senra.me/)博主介绍的方法，添加 Tracker 列表，优化 DHT 缓存。

获得最新的 Tracker 列表信息后，直接粘贴到 aria2.conf 文件的最后，记得看过一个帖子要求用英文逗号隔开：

![](https://pic3.zhimg.com/v2-51710ceb8c3f75f7fe773b2b9604ab16_b.jpg)

方便复制粘贴，2017 年 11 月 02 日最新的 20 个 Tracker 地址：

```text
bt-tracker=udp://tracker.skyts.net:6969/announce,udp://tracker.safe.moe:6969/announce,udp://tracker.piratepublic.com:1337/announce,udp://tracker.pirateparty.gr:6969/announce,udp://tracker.coppersurfer.tk:6969/announce,udp://tracker.leechers-paradise.org:6969/announce,udp://allesanddro.de:1337/announce,udp://9.rarbg.com:2710/announce,http://p4p.arenabg.com:1337/announce,udp://p4p.arenabg.com:1337/announce,http://tracker.opentrackr.org:1337/announce,udp://tracker.opentrackr.org:1337/announce,udp://public.popcorn-tracker.org:6969/announce,udp://tracker2.christianbro.pw:6969/announce,udp://tracker1.xku.tv:6969/announce,udp://tracker1.wasabii.com.tw:6969/announce,udp://tracker.zer0day.to:1337/announce,udp://peerfect.org:6969/announce,udp://tracker.mg64.net:6969/announce,udp://open.facedatabg.net:6969/announce
```

希望有所帮助。

对上文中引用的教程原作者表示感谢，若您觉得此引用不合适，请联系，将致歉并及时处理。

《Aria2 & YAAW 使用说明》 [http://aria2c.com/usage.html](https://link.zhihu.com/?target=http%3A//aria2c.com/usage.html)

《Windows 下如何配置 Aria 2》 知乎作者：[Youth](https://www.zhihu.com/people/hewenjie-1996) [https://zhuanlan.zhihu.com/p/21831960](https://zhuanlan.zhihu.com/p/21831960)

《[解决 Aria2 BT 下载速度慢没速度的问题](https://link.zhihu.com/?target=http%3A//www.senra.me/solutions-to-aria2-bt-metalink-download-slowly/)》 [Senra の小窝 | 初闻天籁之音，未使心之将来。](https://link.zhihu.com/?target=http%3A//senra.me/) [http://www.senra.me/solutions-to-aria2-bt-metalink-download-slowly/](https://link.zhihu.com/?target=http%3A//www.senra.me/solutions-to-aria2-bt-metalink-download-slowly/) 
 [https://zhuanlan.zhihu.com/p/30666881](https://zhuanlan.zhihu.com/p/30666881) 
 [https://zhuanlan.zhihu.com/p/30666881](https://zhuanlan.zhihu.com/p/30666881)
