# 防御GFW主动探测的实用指南
在近期的 IMC'20 的工作中 ([论文](https://gfw.report/publications/imc20/data/paper/shadowsocks.pdf), [演讲](https://gfw.report/talks/imc20/zh/))，我们揭示了中国的防火长城采用_流量分析_与_主动探测_相结合的手段来检测和封锁 Shadowsocks 服务器。

在这篇短文中，我们将分别向技术小白和翻墙软件开发者提供防御 GFW 主动探测的实用建议。 我们还将介绍 Len et al. 展示的[partitioning oracle 攻击](https://www.usenix.org/system/files/sec21summer_len.pdf#page=13)的缓解办法。 如果在遵循了本文的建议后，你的 Shadowsocks 服务器仍被封锁，请将封锁情况[汇报给 GFW Report](https://gfw.report/)以及相应的开发者。

## 给用户的建议[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E7%BB%99%E7%94%A8%E6%88%B7%E7%9A%84%E5%BB%BA%E8%AE%AE)

根据我们的测试和来自开发者的报告，在采用了适当的配置后，以下两个 Shadowsocks 实现的最新版本已经可以抵御来自 GFW 的主动探测：_Shadowsocks-libev_和_OutlineVPN_。

### 针对 Shadowsocks-libev 的使用建议[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E9%92%88%E5%AF%B9shadowsocks-libev%E7%9A%84%E4%BD%BF%E7%94%A8%E5%BB%BA%E8%AE%AE)

如果你决定使用 Shadowsocks-libev，我们强烈建议你根据这篇教程来[部署一台抗封锁的 Shadowsocks-libev 服务器](https://gfw.report/blog/ss_tutorial/zh/)。我们会时刻更新那篇教程的，以应对之后新出现的针对 Shadowsocks 的识别和攻击。

如果你已经拥有了一台 Shadowsocks-libev 服务器，你可以根据以下规则来确认你的服务器是否配置得可以对抗 GFW 的主动检测和封锁。

截止 2021 年 1 月，你需要做到以下几点来防止你的_Shadowsocks-libev_服务器被封锁：

1.  确保你的服务器版本为`v3.3.1`及以上。你可以通过以下命令查看服务器的版本`ss-server -h`。
2.  使用[_AEAD ciphers_](https://shadowsocks.org/en/spec/AEAD-Ciphers.html)， 而**不用** _stream ciphers_。换句话说，仅在以下几种加密方式中进行选择：`chacha20-ietf-poly1305` (推荐), `aes-256-gcm`, `aes-192-gcm`或者`aes-128-gcm`。

为了缓解针对 Shadowsocks 的[partitioning oracle 攻击](https://www.usenix.org/system/files/sec21summer_len.pdf#page=13)，你需要:

1.  使用一个长的随机密码。这样的密码可以用以下命令在终端生成： `openssl rand -base64 16`;
2.  禁用 UDP 模式。

注意：针对客户端没有特殊的要求，任何与 Shadowsocks-libev 服务器兼容的客户端均可。

### 针对 OutlineVPN 的使用建议[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E9%92%88%E5%AF%B9outlinevpn%E7%9A%84%E4%BD%BF%E7%94%A8%E5%BB%BA%E8%AE%AE)

为了防止你的[OutlineVPN](https://getoutline.org/)服务器被 GFW 封锁，你需要做到以下几点：

1.  使用最新版的从[官网](https://getoutline.org/)下载的服务端。
2.  使用最新版的从[官网](https://getoutline.org/)下载的客户端。

注意：

1.  Outline 会自动生成一个长的，随机的密码，因此你不必像为 Shadowsocks-libev 那样手动配置密码。
2.  Outline 服务端会自动更新，因此你不必手动升级服务端。
3.  Outline 只采用`chacha20-ietf-poly1305`这一种 AEAD cipher 作为加密方式，因此你不必手动选择加密方式。

## 给翻墙软件开发者的建议[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E7%BB%99%E7%BF%BB%E5%A2%99%E8%BD%AF%E4%BB%B6%E5%BC%80%E5%8F%91%E8%80%85%E7%9A%84%E5%BB%BA%E8%AE%AE)

下面我们介绍我们发现的 GFW 的最新审查能力，并附上我们给翻墙软件开发者的相应建议。这些建议不仅对 Shadowsocks，而且对其他许多翻墙软件都有用。 我们欢迎你加入我们的讨论，分享你的想法，评论，疑惑和关切。

### 正确的校验[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%A0%A1%E9%AA%8C)

首先，我们强烈建议翻墙软件的开发者们**彻底**废除不具备校验的加密构造。仅仅有保密性是不够的。

-   对于新开发的翻墙软件来说，这意味着根本不应考虑支持不具备校验的加密构造。
-   对于现存的翻墙软件来说，这意味着开发者应该勇敢地**移除**所有与不具备教研的加密构造相关的代码，即使以不向下兼容为代价。

我们这看似大胆的建议实际上出于合理的原因。如我们在[论文](https://gfw.report/publications/imc20/data/paper/shadowsocks.pdf#page=7)中所介绍的， 一些类型的 GFW 主动探测会利用 Shadowsocks 的 stream ciphers 的 malleability。这已经不是第一次不具备校验的加密结构造成漏洞了。事实上，不具备校验的加密结构是许多 Shadowsocks 和其他翻墙软漏洞的根本来源。

早在 2015 年 8 月，BreakWa11[发现了](https://web.archive.org/web/20160829052958/https://github.com/breakwa11/shadowsocks-rss/issues/38)一个关于 Shadowsocks 的 stream ciphers 的漏洞。这个漏洞是由于缺乏数据完整性保护而造成的（[英文总结](https://groups.google.com/d/msg/traffic-obf/CWO0peBJLGc/Py-clLSTBwAJ)）。 在 2020 年，类似的漏洞又被[发现](https://github.com/v2ray/v2ray-core/issues/2523)存在于 V2Ray 中（[总结](https://gfw.report/blog/v2ray_weaknesses/zh/)）。

当 Shadowsocks 开发者试图引入`one time auth`模式来缓解 2015 年的那个漏洞时，另一个因数据长度缺乏完整性保护的主动探测又被[引入了](https://web.archive.org/web/20191002190325/https://printempw.github.io/why-do-shadowsocks-deprecate-ota/)（[英文总计](https://groups.google.com/d/msg/traffic-obf/CWO0peBJLGc/Py-clLSTBwAJ)）。

2020 年 2 月，Zhejiang Peng[发现了](https://github.com/edwardz246003/shadowsocks)一个关于 Shadowsocks stream ciphers 的灾难性的漏洞，([英文总结](https://github.com/net4people/bbs/issues/24)）。 利用使用了 stream cipher 的 Shadowsocks 服务器作为 decryption oracle，攻击者可以在不需要密码的情况下，完全解密 Shadowsocks 会话。

其实早在 2017 年 2 月，AEAD ciphers 就已经成为了 Shadowsocks 协议的一部分。而校验问题也理应在那时就被解决了。但实际情况是，截止 2021 年，大量的服务器仍然因为使用被废弃的 stream ciphers 而存在着安全隐私漏洞，以及被准确识别的风险。

这样的现象表明，在实际操作中，许多的用户不能够正确的选择加密方式。这可能与使用过时的教程或一键脚本有关。 我们因此鼓励开发者从 Shadowsocks 各实现中彻底移除 stream ciphers，帮助用户做出正确的选择。

### 使用同时基于 nonces 和时间的重放过滤器[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E4%BD%BF%E7%94%A8%E5%90%8C%E6%97%B6%E5%9F%BA%E4%BA%8Enonces%E5%92%8C%E6%97%B6%E9%97%B4%E7%9A%84%E9%87%8D%E6%94%BE%E8%BF%87%E6%BB%A4%E5%99%A8)

我们建议翻墙软件的开发者们采用同时基于 nonces 和时间的重放过滤器。 因为采用基于时间的重放过滤器需要对 Shadowsocks 协议进行根本性地变动，我们建议开发者至少要此采用基于 nonces 的过滤器，并且做到：

1.  要么建议用户在每次过滤器重置后修改密码；
2.  要么开发一个机制，可以让重放过滤器在软件重启后依然记得之前的 nonces。

这些建议是基于以下的研究发现和推论。 如[论文的 section 3.5](https://gfw.report/publications/imc20/data/paper/shadowsocks.pdf#page=5)所介绍的， GFW 既可以在观察到一个合法连接的瞬间对其进行重放；也可以等待三周甚至更长时间后才重放。 因此，一个更加合理的主动探测模型应该允许审查者在任意时长后对合法连接进行重放。

这样的一个主动探测模型揭示了纯粹基于 nonces 的重放过滤所须要面对的不对称性。GFW 仅用少量资源就可以记录**一些**合法的连接，并且在经过任意的时长后再重放它们；但与此同时，Shadowsocks 需要大量的资源和相对复杂的实现来永久性地记住**所有的**合法链接，直至密码被更换。 注意，Shadowsocks 服务器必须在重启后还记住这些 nonces；否则重放过滤器不会过滤基于重启前的合法连接的重放。

幸运的是，这个不公平的局面可以通过同时引入基于时间的重放过滤机制来扭转：服务器只需要处理并验证时间戳未过期的连接，[就像 VMess 服务器那样](https://gfw.report/blog/v2ray_weaknesses/en/)。 如此一来服务器就不需要永久性地记住所有合法连接中的 nonces。

我们还想强调，对于那些仅仅短暂暴露变化的监听端口的翻墙服务器，重放过滤仍是必要的。因为 GFW 可以瞬时重放合法连接中的第一个数据包，而这时暴露的监听端口因为未完成数据传输，依然是开启的。

### 让服务器的反应保持一致[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E8%AE%A9%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9A%84%E5%8F%8D%E5%BA%94%E4%BF%9D%E6%8C%81%E4%B8%80%E8%87%B4)

我们建议开发者们确保翻墙服务器在正常运行时，和遇到不合法的连接时都反应一致。理想情况下，可以按照[Frolov et al.](https://censorbib.nymity.ch/pdf/Frolov2020a.pdf#page=12)的建议，让服务器遇到错误连接时 “read forever”。 这是因为，审查者会故意触发协议的边边角角等特殊情况，来识别服务器指纹。

除了我们在 Shadowsocks-libev 和 OutlineVPN 中发现的服务器反应指纹，[Frolov et al.](https://censorbib.nymity.ch/pdf/Frolov2020a.pdf#page=11)还展示了包括 Shadowsocks-python 和 OutlineVPN 在内的多种翻墙软件可以通过关闭连接时的 TCP flags 和连接时长来识别。studentmain[报告](https://github.com/net4people/bbs/issues/22#issuecomment-744704701)，直至 2020 年 12 月， 许多的 Shadowsocks 实现仍存在着我们在 Shadowsocks-libev 和 Outline 中发现的问题。

Frolov et al. 建议代理服务器在遇到错误连接时不要立即关闭连接，而是 “read forever”。这样做不但避免泄漏超时值，而且使得服务器发送与正常连接关闭时相同的 TCP flags 来关闭错误连接。

进一步说，“reading forever” 本身不会带来更加独特的指纹。因为 Frolov et al. 发现[互联网上大量的服务器都会有无限超时值（“infinite timeout”）的特征](https://censorbib.nymity.ch/pdf/Frolov2020a.pdf#page=12)。David Fifield[调查](https://github.com/net4people/bbs/issues/26#issuecomment-599712288)显示，许多流行的翻墙软件已经采取了 “read forever” 策略。这些软件包括 OSSH，obfs4，Outline 和 Lampshade。

### 强制采用强密码[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E5%BC%BA%E5%88%B6%E9%87%87%E7%94%A8%E5%BC%BA%E5%AF%86%E7%A0%81)

Len et al. 于 2020 年展示了针对 Shadowsocks 服务器的[partitioning oracle 攻击](https://www.usenix.org/system/files/sec21summer_len.pdf#page=13)。利用在 Shadowsocks 中使用的 non-committing AEAD，攻击者可以更高效地猜出密码。我们因此建议开发者强制采用强密码。一种可能的实现方式是要求用户密码的熵高于一定值。

### 主动探测你的实现[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E4%B8%BB%E5%8A%A8%E6%8E%A2%E6%B5%8B%E4%BD%A0%E7%9A%84%E5%AE%9E%E7%8E%B0)

如果你是一个不同于 Shadowsocks-libev 和 Outline 的翻墙软件开发者或贡献者，我们鼓励你检查同样的漏洞是否也存在于你的 Shadowsocks 实现中。我们开源了我们在[论文 Section 5.1](https://gfw.report/publications/imc20/data/paper/shadowsocks.pdf#page=8)中用到的[prober 模拟器](https://gfw.report/publications/imc20/data/code/prober_simulator/)。

## 鸣谢[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E9%B8%A3%E8%B0%A2)

我们想要感谢来自 Jigsaw 的 Vinicius Fortuna，来自 APNIC 的 Robert Mitchell 和 Dan Fidler，以及来自 Qv2ray 的 DuckSoft 和 Student Main 对本文提供的反馈。

## 联系[](https://gfw.report/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us#%E8%81%94%E7%B3%BB)

这篇报告首发于[GFW Report](https://gfw.report/blog/ss_advise/zh/)。我们还在 APNIC blog，[net4people](https://github.com/net4people/bbs/issues/58)以及[ntc.party](https://ntc.party/t/a-practical-guide-to-defend-against-the-gfws-latest-active-probing/847)同步更新了博文。

我们鼓励您或公开地或私下地分享您的评论或疑问。我们私下的联系方式可见[GFW Report](https://gfw.report/)的页脚。 
 [https://webcache.googleusercontent.com/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us](https://webcache.googleusercontent.com/search?q=cache:Oa6Pid1cM5IJ:https://gfw.report/blog/ss_advise/zh/+&cd=1&hl=en&ct=clnk&gl=us)
