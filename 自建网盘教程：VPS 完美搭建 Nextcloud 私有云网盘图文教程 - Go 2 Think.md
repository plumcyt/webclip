# 自建网盘教程：VPS 完美搭建 Nextcloud 私有云网盘图文教程 - Go 2 Think
## 前言

几年前还百家争锋的国内网盘市场，如今只剩下百度网盘一枝独秀了。虽然还有一些稳定的国外网盘，如 OneDrive、DropBox、Google Drive 等，但国内访问并不友好。

私有云和 NAS 这种完全掌握在自己手中的云端存储方案就体现其优势了。**本文就介绍下如何在 VPS 上快速使用 Nextcloud 搭建个人专属的私有云同步网盘。** 

**目录：** 

1.  自建网盘方案选择
2.  Nextcloud 搭建 方法
3.  方法一：一键安装
4.  方法二：手动搭建
5.  ​Tips

## 自建网盘方案选择

推荐的比较多的有三个：

-   Nextcloud
-   ownCloud
-   Seafile

适合个人和企业使用，基础版免费。都是成熟方案，主体功能上大同小异，且都支持在线查看 / 播放文件、插件扩展等功能。

这里选择安装 [Nextcloud](https://nextcloud.com/)。

![](https://go2think.com/wp-content/uploads/20181209/3334975-68b6caa90e3dd737.png)

**优势：** 

-   私密，自己全权管理所有文件；
-   稳定，不存在服务商关闭网盘服务的问题；
-   高速，直链下载，不限速；
-   功能丰富，可安装插件实现各种云端功能。

**不足：** 

-   需要自己维护备份；
-   存储容量多为几十 G，不适合做仓库盘。

## Nextcloud 搭建 方法

方法有两个：

1.  一键安装
2.  手动搭建

**一键安装：** 简单。好消息就是 Vultr 支持一键创建 Nextcloud 主机，“真 · 零基础” 安装！

**手动搭建：** 毫无疑问——繁琐。需要一步步手动安装配置，也不算太麻烦。

这里两种方法都会介绍，小白请直接选择 “一键安装”；手动安装方法面向有基础的用户。

**VPS 教程：** 

未使用过 VPS 请先看下面教程注册：

-   [Vultr 中文教程：Vultr 购买和使用及常见测试方法和问题](https://go2think.com/vultr-%E4%B8%AD%E6%96%87%E6%95%99%E7%A8%8B/)
-   [搬瓦工 VPS 购买使用教程](https://go2think.com/bwg-guide-1/)

## 方法一、一键安装

**概要：** 利用 Vultr 的一键部署应用功能，在创建 VPS 时直接部署安装 Nextcloud 应用。（仅 Vultr）

**Vultr 注册：[Vultr](http://www.vultr.com/?ref=7585919)**（目前有充值 $10 送 $25 活动）

### 1. 创建 VPS

1.  Server Location：洛杉矶、纽约、新加坡等自选；
2.  **Server Type -> Application -> Nextcloud；**
3.  Server Size：$5/mo 包含 25G SSD 硬盘，$10/mo 是 40G，按需选择。
4.  其他选项默认。

![](https://go2think.com/wp-content/uploads/20181209/3334975-120182f7d1eb2e1c.png)

选好后点击右下角 “Deploy Now” 开始创建，等待几十秒。

当 Status 显示 “Running”，即安装完成！点击 Manage 进入详情界面。

![](https://go2think.com/wp-content/uploads/20181209/3334975-fb4495071f96203c.png)

详情页面下方会列出 Nextcloud 的登录信息：登录地址、用户名、密码。记下来。

![](https://go2think.com/wp-content/uploads/20181209/3334975-e19c94fe13569ebe.png)

没错，此时已经可以开始使用 Nextcloud 了，就这么简单！

### 2. 使用 Nextcloud

使用上面记录下的账户信息，打开登录地址登录账户。

![](https://go2think.com/wp-content/uploads/20181209/3334975-bc14a9e116f3faab.png)

欢迎页面，可以在浏览器中使用，也可以安装客户端，支持 Win、Mac、Android、iOS、Linux 全平台。

![](https://go2think.com/wp-content/uploads/20181209/3334975-1b4b30cb4cede5a1.png)

网盘主页，附带了几个示例文档、图片、视频和 PDF，点击即可在线查看 / 播放。

![](https://go2think.com/wp-content/uploads/20181209/3334975-0828b0fc24995f51.png)

![](https://go2think.com/wp-content/uploads/20181209/3334975-6c2b1a7a9bc6bd84.png)

右上角打开设置、应用和用户管理选项。

![](https://go2think.com/wp-content/uploads/20181209/3334975-c627e14f1b4ca033.png)

-   **设置：** 账户管理、安全、隐私、分享设置等；
-   **应用：** 插件商店，包括日程表、To-List、Office、思维导图、流程图、阅读器、云笔记…… 各种插件一应俱全；
-   **用户：** 添加 / 管理多用户，家庭成员或公司多人使用。

## 方法二、手动搭建

**概要：** 和搭建网站类似，先安装好网站环境（LNMP），然后将 Nextcloud 程序下载解压到网站目录中。（VPS 均可安装）

### 1. 创建 VPS

节点自选；系统 CentOS 7×64；配置内存建议最少 1G。创建好连接 Xshell 安装网站环境。

### 2. 安装网站环境

使用宝塔面板安装 LNMP 网站环境，PHP 选择 7.0 版本。

宝塔面板一键安装脚本：

\|  \| 

yum install  -y  wget  &&  wget  -O  install.sh [http://download.bt.cn/install/install.sh](http://download.bt.cn/install/install.sh) && sh install.sh

 \|

进入宝塔面板，添加网站和数据库。域名没有的话可以编一个，我们通过 IP 访问。

![](https://go2think.com/wp-content/uploads/20181209/3334975-7d8c8637906c7a35.png)

设置默认站点，方便使用 IP 访问。

![](https://go2think.com/wp-content/uploads/20181209/3334975-dd303fb91d004210.png)

### 3. 安装 Nextcloud

首先进入网站目录，将自带的 4 个文件全部删除。

**Nextcloud 安装包：** 

-   完整安装包 – [下载](https://download.nextcloud.com/server/releases/nextcloud-13.0.4.zip)
-   在线安装器 – [下载](https://download.nextcloud.com/server/installer/setup-nextcloud.php)

下载 Nextcloud 安装包 / 安装器，上传并解压到网站目录。我使用的是在线安装器，体积小传的快一点。

![](https://go2think.com/wp-content/uploads/20181209/3334975-84b8b6c87e6f2dd8.png)

访问网站地址：`http://ip/setup-nextcloud.php`，根据提示和引导完成安装。

![](https://go2think.com/wp-content/uploads/20181209/3334975-0a56e014cd2f1be3.png)

填写管理员和数据库信息。（默认使用 SQLite，不用手动设置）

![](https://go2think.com/wp-content/uploads/20181209/3334975-4ad8cfde24c80a71.png)

进入网盘，完成！

## Tips

-   建议在设置中开启登陆二次验证；
-   节点选择：延迟一般的情况下，优先选择下载速度快的节点（[Vultr VPS 节点选择方法](https://go2think.com/vultr-vps-node-ping-test/)）；
-   为了数据安全，不建议使用不知名小商家的 VPS；
-   可以通过快照功能实现备份的目的；
-   一键安装，无法打开网盘地址：[ping.pe](http://ping.pe/) 测试 IP，被墙的话销毁重建（手动安装，连不上 Xshell 同理）；
-   [客户端下载](https://nextcloud.com/install/#install-clients)

## 结语

使用 VPS 搭建私有云盘，网盘完全归自己所有，有很多优势和方便之处，个人使用或家庭 / 公司共享都可以。

但是限制也很明显，即容量不会太大，一般几十 G ~ 100G 之间比较划算。如果要像百度网盘一样有 2T 空间，那费用可不低。

因此不适合把私有云盘当仓库的大量存储需求，更适合工作学习等资料文件的存储同步。（其实几十 G 也挺多了，度盘虽大，但真正经常用的文件又有多少呢…… 
 [https://go2think.com/how-to-install-nextcloud/](https://go2think.com/how-to-install-nextcloud/) 
 [https://go2think.com/how-to-install-nextcloud/](https://go2think.com/how-to-install-nextcloud/)
