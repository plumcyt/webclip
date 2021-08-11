# 如何部署一台抗封锁的Shadowsocks-libev服务器
这篇教程记录了如何安装，配置并维护一台 Shadowsocks-libev 服务器。 这篇教程的亮点在于， 按照这里的配置建议，你的 Shadowsocks-libev 服务器可以抵御各种已知的攻击， 包括[来自 GFW 的主动探测和封锁](https://gfw.report/talks/imc20/en/)以及[partitioning oracle 攻击](https://www.usenix.org/system/files/sec21summer_len.pdf#page=13)。 我们还在教程的最后加入了常见的有关 Shadowsocks-libev 部署的常见问题。

我们致力于更新和维护这篇教程。如果今后发现了新的针对 Shadowsocks-libev 的攻击，我们将在第一时间在这篇教程中加入缓解攻击的办法。 因此请考虑将这个页面加入到你的收藏夹中。 另外，我们希望这篇教程对技术小白同样友好，因此如果你在任何步骤卡住了，请[联系我们](https://gfw.report/)，或在下方评论区留言。我们会对教程作相应改进。

## 安装[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E5%AE%89%E8%A3%85)

### 安装 Snap 应用商店[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E5%AE%89%E8%A3%85snap%E5%BA%94%E7%94%A8%E5%95%86%E5%BA%97)

通过 Snap 应用商店安装 Shadowsocks-libev 是[官方推荐](https://github.com/shadowsocks/shadowsocks-libev#quick-start)的方式。

-   如果你的服务器运行 Ubuntu 16.04 LTS 及以上的版本，Snap 已经默认安装好了。
-   如果你的服务器运行了其他的 Linux 发行版，你只需跟着[对应的发行版安装 Snap core](https://snapcraft.io/core)。

现在来检测一下你的服务器已经安装了需要的`snapd`和 Snap `core`:

### 安装 Shadowsocks-libev[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E5%AE%89%E8%A3%85shadowsocks-libev)

现在我们安装最新的 Shadowsocks-libev:

```sh
sudo snap install shadowsocks-libev --edge

```

## 配置[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E9%85%8D%E7%BD%AE)

下面是我们推荐的 Shadowsocks-libev 服务器配置：

```txt
{
    "server":["::0","0.0.0.0"],
    "server_port":8388,
    "encryption_method":"chacha20-ietf-poly1305",
    "password":"ExamplePassword",
    "mode":"tcp_only",
    "fast_open":false
}

```

注意，你需要把里面的`ExamplePassword`替换成一个更强的密码。 强密码有助缓解最新发现的针对 Shadowsocks 服务器的[Partitioning Oracle 攻击](https://www.usenix.org/system/files/sec21summer_len.pdf#page=13)。 你可以用以下命令在终端生成一个强密码：`openssl rand -base64 16`。

你还可以考虑将`server_port`的值从`8388`改为`1024`到`65535`之间的任意整数。

现在打开通过 Snap 安装的 Shadowsocks-libev 默认的配置文件：

```sh
sudo nano /var/snap/shadowsocks-libev/common/etc/shadowsocks-libev/config.json

```

将上方替换过密码的配置信息复制粘贴到配置文件后， 按`Ctrl + x`退出。 退出时，文本编辑器将问你`"Save modified buffer?"`，请输入`y`然后按回车键。

可以看到，通过 Snap 安装的 Shadowsocks-libev 默认的配置文件路径太长了，不便于记忆。同时默认配置路径又没有在官方文档中标出。 我们因此建议你收藏此页面，以备今后查找。

## 防火墙[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E9%98%B2%E7%81%AB%E5%A2%99)

我们使用`ufw`来管理 Shadowsocks 服务器的防火墙。

在基于 Debian 的服务器上，可以通过如下命令安装`ufw`：

```sh
sudo apt update && sudo apt install -y ufw

```

然后开放有关`ssh`和`Shadowsocks-libev`的端口。 请注意，以下命令假设你在`/var/snap/shadowsocks-libev/common/etc/shadowsocks-libev/config.json`中的`server_port`的值为`8388`。 如果你的`server_port`用了其他的值，请对以下命令作相应的修改：

```sh
sudo ufw allow ssh
sudo ufw allow 8388/tcp

```

现在我们启动`ufw`:

启动时如果弹出`Command may disrupt existing ssh connections. Proceed with operation (y|n)?`，请输入`y`并按回车键。

最后，请用`sudo ufw status`检查一下你的配置是否和下面的一样：

```txt
Status: active

To                         Action      From
--                         ------      ----
SSH                        ALLOW       Anywhere
8388/tcp                   ALLOW       Anywhere
SSH (v6)                   ALLOW       Anywhere (v6)
8388/tcp (v6)              ALLOW       Anywhere (v6)

```

## 运行 Shadowsocks-libev[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E8%BF%90%E8%A1%8Cshadowsocks-libev)

现在我们启动 Shadowsocks-libev：

```sh
sudo systemctl start snap.shadowsocks-libev.ss-server-daemon.service

```

记得设置 Shadowsocks-libev 开机自启动：

```sh
sudo systemctl enable snap.shadowsocks-libev.ss-server-daemon.service

```

## 维护[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E7%BB%B4%E6%8A%A4)

### 检查运行状态和日志[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E6%A3%80%E6%9F%A5%E8%BF%90%E8%A1%8C%E7%8A%B6%E6%80%81%E5%92%8C%E6%97%A5%E5%BF%97)

以下命令可以查看 Shadowsocks-libev 的运行状态：

```sh
sudo systemctl status snap.shadowsocks-libev.ss-server-daemon.service

```

如果你看到绿色的`Active: active (running)`，那么你的 Shadowsocks-libev 服务器就在正常的运行； 如果你看到红色的`Active: failed`，请用跳至如下命令`journalctl -u snap.shadowsocks-libev.ss-server-daemon.service`的尾部查看问题出在哪里了。

### 重新加载配置文件[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E9%87%8D%E6%96%B0%E5%8A%A0%E8%BD%BD%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)

每当你修改过配置文件后，请用如下命令重启 Shadowsocks-libev 以加载修改后的文件：

```sh
sudo systemctl restart snap.shadowsocks-libev.ss-server-daemon.service

```

## 常见问题[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

##### Q: 为什么我用了教程里的配置，服务器还是被封了?[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#q%E4%B8%BA%E4%BB%80%E4%B9%88%E6%88%91%E7%94%A8%E4%BA%86%E6%95%99%E7%A8%8B%E9%87%8C%E7%9A%84%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%BF%98%E6%98%AF%E8%A2%AB%E5%B0%81%E4%BA%86)

A: 通常来讲，这种事情不会发生，因为通过这篇教程配置的 Shadowsocks-libev 服务器已经可以抵御已知的所有来自 GFW 的主动探测。但如果你的服务器确实被封锁了，那么很有可能审查者使用了未知的攻击手段。请[将你的封锁情况汇报给我们](https://gfw.report/)，我们会认真地调查。

##### Q: 我应不应该从发行版的仓库下载安装 Shadowsocks-libev?[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#q-%E6%88%91%E5%BA%94%E4%B8%8D%E5%BA%94%E8%AF%A5%E4%BB%8E%E5%8F%91%E8%A1%8C%E7%89%88%E7%9A%84%E4%BB%93%E5%BA%93%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85shadowsocks-libev)

A: 发行版仓库里的 Shadowsocks-libev 不一定是最新版的。比如，截止 2021 年 1 月，Debian buster 仓库的 Shadowsocks-libev 的版本为`v3.2.5`。而这个版本的 Shadowsocks-libev 是不够防御来自 GFW 的主动探测的（详见[Figure 10](https://gfw.report/publications/imc20/data/paper/shadowsocks.pdf#page=9)）。

##### Q: 我应该怎样更新用 Snap 安装的 Shadowsocks-libev?[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#q-%E6%88%91%E5%BA%94%E8%AF%A5%E6%80%8E%E6%A0%B7%E6%9B%B4%E6%96%B0%E7%94%A8snap%E5%AE%89%E8%A3%85%E7%9A%84shadowsocks-libev)

A: 因为 Snap 会每天自动更新通过其安装的软件，因此通常情况下你不需要手动更新。如若需要手动更新，请用： `sudo snap refresh`。

##### Q: 为什么用`chacha20-ietf-poly1305`作为加密方式?[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#q-%E4%B8%BA%E4%BB%80%E4%B9%88%E7%94%A8chacha20-ietf-poly1305%E4%BD%9C%E4%B8%BA%E5%8A%A0%E5%AF%86%E6%96%B9%E5%BC%8F)

A: 因为它是其中一种[AEAD ciphers](https://shadowsocks.org/en/wiki/AEAD-Ciphers.html)。而[AEAD ciphers 可以抵御来自 GFW 的主动探测](https://gfw.report/blog/ss_advise/zh/)。它同时也是 Shadowsocks-libev 及 OutlineVPN 的默认加密方式。

##### Q: 我应该用 Shadowsocks 的 stream cipher 吗?[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#q-%E6%88%91%E5%BA%94%E8%AF%A5%E7%94%A8shadowsocks%E7%9A%84stream-cipher%E5%90%97)

A: 完全不应该。因为 Shadowsocks 的 stream cipher 有着不可接受的安全隐私漏洞，并且可以被准确的主动探测。如[Figure 10](https://gfw.report/publications/imc20/data/paper/shadowsocks.pdf#page=9)所示，即使是最新版的 Shadowsocks-libev，在使用 stream cipher 时同样可以被准确识别。更具灾难性的是，[在不需要密码的情况下，攻击者可以完全解密被记录下来的 Shadowsocks 会话](https://github.com/net4people/bbs/issues/24)。

##### Q: 但为什么我用的机场仍在使用 stream cipher?[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#q-%E4%BD%86%E4%B8%BA%E4%BB%80%E4%B9%88%E6%88%91%E7%94%A8%E7%9A%84%E6%9C%BA%E5%9C%BA%E4%BB%8D%E5%9C%A8%E4%BD%BF%E7%94%A8stream-cipher)

A: 这清楚地说明你的机场缺乏安全意识和安全措施。请把[这篇教程](https://gfw.report/blog/ss_tutorial/zh/)，[这个演讲](https://gfw.report/talks/imc20/en/)，和这篇[总结](https://gfw.report/blog/ss_advise/zh/)，分享给你的机场主。

##### Q: 我应该把配置中的`server_port`改为像`443`这样的常见端口吗?[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#q-%E6%88%91%E5%BA%94%E8%AF%A5%E6%8A%8A%E9%85%8D%E7%BD%AE%E4%B8%AD%E7%9A%84server_port%E6%94%B9%E4%B8%BA%E5%83%8F443%E8%BF%99%E6%A0%B7%E7%9A%84%E5%B8%B8%E8%A7%81%E7%AB%AF%E5%8F%A3%E5%90%97)

A: 不应该。因为不论你使用哪个端口，GFW 都会检测并怀疑你的 Shadowsocks 流量。

##### Q: 为什么配置文件使用`tcp_only`模式?[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#q-%E4%B8%BA%E4%BB%80%E4%B9%88%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E4%BD%BF%E7%94%A8tcp_only%E6%A8%A1%E5%BC%8F)

A: 这是为了缓解最新发现的[Partitioning Oracle 攻击](https://www.usenix.org/system/files/sec21summer_len.pdf#page=13)，文中指出 “Shadowsocks 的开发者们已经采取措施，禁用了本默认开启的 UDP 代理模式（“the Shadowsocks developers took immediate action to disable UDP proxying where it was enabled by default."）。当然，如 Vinicius[所指出的](https://gfw.report/blog/ss_tutorial/en/#isso-57)，如果你使用了长的随机密码，那么 partitioning oracle 攻击就不能成功。因此也就不需要禁用 UDP 代理模式。开启 UDP 代理模式可能会让经过 Shadowsocks 代理的视频通话质量更佳。

##### Q: 为什么配置文件禁用了`fast_open`?[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#q-%E4%B8%BA%E4%BB%80%E4%B9%88%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%A6%81%E7%94%A8%E4%BA%86fast_open)

A: 我们推荐你阅读[这里的讨论](https://github.com/klzgrad/naiveproxy#why-not-use-go-node-etc-for-performance)。

## 联系[](https://gfw.report/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E8%81%94%E7%B3%BB)

这篇报告首发于[GFW Report](https://gfw.report/blog/ss_tutorial/zh/)。我们鼓励您或公开地或私下地分享您的评论或疑问。我们私下的联系方式可见[GFW Report](https://gfw.report/)的页脚。 
 [https://webcache.googleusercontent.com/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us](https://webcache.googleusercontent.com/search?q=cache:rEYMvGhmbD4J:https://gfw.report/blog/ss_tutorial/zh/+&cd=1&hl=en&ct=clnk&gl=us)
