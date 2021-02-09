# 8000+字的FREENAS个人的入门及建议与安装后延伸搭建可靠备份_NAS存储_什么值得买
2020-10-25 15:02:30 74 点赞 657 收藏 50 评论

> 是返乡过年？还是就地过年？最新一届[**#双面过节指南#**](https://www.smzdm.com/tag/tr0lj3l/post/)开始啦！本次征稿活动分为 A 面返乡和 B 面就地，大家可以根据自己的情况，分享自己的春节攻略，优秀的投稿文章还有可能能获得优厚的大奖哦，[**快点击查看活动详情 &lt;&lt;&lt;**](https://post.smzdm.com/p/a6lxxn8o/)

**创作立场声明：** 本文所购买的所有产品均为自用，购买渠道也写明了，要是有赞助就好了，可惜并没有，一套组下来心在滴血啊。

## 目录

1.  **提要**
2.  **配置**
3.  **常见问题**


1.  **ECC 内存要求**
2.  **单盘 / 条带建 Pool**
3.  **FreeNAS 怎么装其他软件**


5.  **遇到的困难与解决**


1.  **Jail 不互通**
2.  **权限配置问题 / 挂载无编辑或写权限**
3.  **Scrub/SMART 任务**
4.  **虚拟机的 Debian 安装花屏**
5.  **UPS 配置**
6.  **硬盘休眠停转设置**
7.  **Jail 的 NAT 映射**
8.  **安装插件以外的软件**


7.  **服务搭建**


1.  **使用 Duplicati 备份**
2.  **云端服务供应商**
3.  **Duplicati 配置备份**
4.  **Duplicati 性能**


9.  **额外内容**
10. **后记**

## 提要

想拼台 NAS 来存家里照片和重要文件挺久的了，终于在 Amazon Prime Day 把硬件买完了，组了这套完整的 FreeNAS 系统。没上群晖，黑群兼容性是个值得思考的问题，而且升级麻烦，安装麻烦，不洗白缺功能，白群价格不便宜，性能不够高，于是就去用基于 ZFS 的 FreeNAS 了（不知道以前叫 NAS4FREE 的 XigmaNAS 怎么样）。

## 配置

[![](https://qnam.smzdm.com/202010/24/5f9398987dd435744.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_2/)

FreeNAS 的推荐最低配置是多核 64 位处理器，16GB ECC 内存，独立系统盘，NAS 专用盘，这套配置算是基本满足了（另外我买了块 BX500 120GB（22.59 刀）拿来亮机，居然忘记加进去了）。  

ZFS 已经负责 RAID 的功能，所以 FreeNAS 不需要也最好不要上 硬 RAID 卡，LSI 的 HBA 卡是官方推荐。  

机箱是真的贵。

## 常见问题

### ECC 内存要求：

FreeNAS 乃至于 ZFS 对 ECC 的要求一直都备受争论，有传闻说基于 ZFS 必须上 ECC 不然数据很容易坏，还有理论说 ZFS 的 Scrub（类似数据校验）这个设计会导致写入被认为 "正确" 的错误数据（"Scrub of Death"），**实际上并不会发生**（如有兴趣可以参考[这篇文章](https://jrs-s.net/2015/02/03/will-zfs-and-non-ecc-ram-kill-your-data/)），ZFS 的 Co-founder Matthew Ahrens 也说过**ZFS 对 ECC 内存的要求并不特殊，没有比其他文件系统更需要 ECC**。

虽然是家用，我还是上了 Unbuffered ECC 内存（Registered ECC 对 CPU 和主板的要求导致基本只有 E5 还有 Eypc 这种才能用），因为 E5 系列太耗电，E3 或者 I3 要上工作站主板（实在太贵）才能用 Unbuffered ECC，于是就挑了 AMD 的 Ryzen 3 Pro 2200GE，AMD 的 Ryzen 系列的有一部分是支持直接在家用级主板上用 Unbuffered ECC 内存的。然而 Picasso 和 Raven Ridge，也就是大部分 APU 是不支持 Unbuffered ECC 的（虽然有传闻说 ECC 功能能正常运行，只是不显示），但是 Raven Ridge 和 Picasso 的**Pro 系列（一般用在 OEM 平台上）是支持 ECC 的**。因为 Ebay 没（便宜或者从美国发的）货，于是我就从 AliExpress（也就是淘宝海外版）上买了一块，等了快一个月才从中国送到美国。Unbuffered 的 ECC 还是贵，毕竟要用双倍的颗粒。

ECC 仍然是推荐上，不上也不是严重影响运行，ECC 只能上道保险，遇到**双 Bit 翻转 ECC 内存也是无法修正的**（只能报错，单 Bit 翻转可以修复），还有 “垃圾进，垃圾出” 这种情况也无济于事（原本输入的内容就是损坏的）。顺便提到，装好 NAS 后我第一次检验 65000 多张照片时发现有 10 多张是损坏的（不完整或翻转导致图片变样），校验多个备份都发现一样的问题，有些是十几年前就留下的问题。

如果你是给公司企业用，或者生产环境，保存重要数据，**强烈建议上 ECC 内存，不要拿别人的数据去冒险**。

### 单盘 / 条带建 Pool：

单盘 / 条带（RAID-0）建 Pool 是**无法使用 Scrub 纠错**的，只能用其校验，因为 Scrub 在发现错误后需要用已有的拷贝（RAID-1，RAID-10）或者奇偶校验（RAID-5 和 RAID-6 在 FreeNAS 不存在，只有替代的 RAID-Z1，RAID-Z2，RAID-Z3）来修复。

### FreeNAS 怎么装其他软件：

基于 FreeBSD 的 FreeNAS 自身是不让你装软件的，为了安全和隔离。所以直接在主机上用 pkg install 是无效的（你要修改配置启用也没问题），一般安装其他软件或者插件都在 FreeBSD 自带的 Jail 上，在 Plugins 里面提供了不少官方或者非官方插件（例如 Transmission，Plex 等等）。Jail 和 Docker 也是系统层虚拟化创建独立环境，当然 FreeNAS 也提供了 FreeBSD 的硬件虚拟化 Bhyve，让你随便装 BSD，Linux，Windows。  

## 遇到的困难与解决

### Jail 不互通：

最好解决的一个，只需要给 Jail 加挂载点即可。

[![](https://qnam.smzdm.com/202010/24/5f93a69c2d2692809.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_3/)

[![](https://qnam.smzdm.com/202010/24/5f93a8004a6572265.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_4/)

[![](https://qnam.smzdm.com/202010/24/5f93a8026f2ab1121.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_5/)

先将 Jail 关掉，然后进入 Mount Points，右上角选择 Add，选好原始路径和目标路径后确认，重启即可  

### 权限配置问题 / 挂载无编辑或写权限：

权限配置是最让我头痛的一个问题（甚至考虑过放弃 FreeNAS，因为一开始不知道怎么配），因为一开始我以为无法不修改拥有者就让所有规定的用户具备写权限。这也导致一开始 Transmission 无法下载，Plex 无法删除文件。

参考 iXSystems 官方社区后，于是决定来让组权限具有修改和写权限，解决了所有问题

因为 FreeNAS 本身的部分组和 Jail 的组 名称和 GID 是相同的，于是我就直接用了 wheel 这个组（FreeNAS 默认关闭了 wheel 组的 sudo 权限，而且大部分文件 组用户和其他用户 都是一样只有读和运行的权限，所以不用担心安全问题）

先将你挂载到 Jail 的 Dataset 的权限修改成 775（组用户可读写），并设置到你需要的拥有者和组

在 Jail 的 Shell 里面输入以下内容，即可将 Plex（范例，修改成 Jail 里软件需要的用户）加入 Wheel 组（范例，可以修改成你需要的组）  

> pw groupmod wheel -m plex

如果你想添加的组不是 Jail 和 FreeNAS 自带有相同名称和 GID 的组，你需要在 Jail 里面添加这个组（FreeNAS 也要有这个组），按照 FreeNAS 里的组名称（范例 EXAMPLE_GROUP）和 ID（范例 5555）添加即可

> pw groupadd -n EXAMPLE_GROUP -g 5555

不建议在 Jail 上直接用 root 权限运行程序，不建议在任何情况用 root 权限运行程序，**权限配置不周是安全隐患**。

### Scrub/SMART 任务：

FreeNAS 默认没有自动规划 Scrub 任务和 SMART 任务 (也是我在看到 Pool 的 Scrub Status 一直不存在才意识到的)，不过设置起来不难，直接在 Task - S.M.A.R.T Tests 和 Scrub Tasks 里面设置就行，右上角分别添加 Long Test，Short Test，还有每个 Pool 的 Scrub。

直接参考了其他人的配置，个人的配置是 SMART 选全盘，短测试每个月 5，12，19，26 号 早上 4 点，长测试每个月 8，22 号 早上 4 点。Scrub 任务也是把所有 Pool 在每个月 1，15 号 早上 4 点测试，阈值（Threshold）设 10 天（阈值是成功测试后过多少天才再次测试，例如每天执行一次 Scrub 任务，但是因为阈值是十天，如果第一次成功执行第二次实际执行是在十天后，如果第一次失败了，第二天又会马上执行，每天执行直到成功）。

在添加界面 Schedule 选 Custom，然后输入需要的日期。范例是 每个月 5，12，19，26 号 早上 4 点执行，不需要更改 Preset 或者设置 Months 和 Days of Week。

[![](https://qnam.smzdm.com/202010/25/5f9469cf07eae2019.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_6/)

### 虚拟机的 Debian 安装花屏：

很迷，不知道为什么只有 Debian 安装会花屏。装 Debian 是为了用 OpenConnect 的一键包来方便连回家（我是真的懒 ^ ^），可能是和显卡，VNC，Bhyve，Debian 之中哪几个不兼容吧。从 Debian 官网试过不少 ISO 都是花屏的，实在没办法。  

于是就去用了[netboot.xyz](https://netboot.xyz/)的网络安装，里面直接网络安装的 Debian 就不花屏，也真是奇怪。Netboot.xyz 提供了不少 Linux 发行版，BSD，还有 Windows 的安装，挺方便的，不过有个 Bug（不知道是 Netboot 还是 Debian 的关系）就是其安装的引导位置有点奇怪，导致第二次启动的时候进不了系统，不过网上有教程很好解决，移动一下引导文件就行（其实是我已经忘记具体情况了，也懒得再去重现一次了）。

### UPS 配置：

先把能智能管理的 UPS 用线（例如网线或者 USB）接到 NAS 的机器上，我用的是 APC 的 BE 550G，用的是 USB 线，所以用 dmesg 查看 UPS 用的 USB 端口。

> dmesg | grep -i usbus

然后在 Service - UPS 里面进行修改。  

[![](https://qnam.smzdm.com/202010/25/5f946fd8d54348104.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_7/)

UPS 模式不用改，ID 不要改（乱改识别不出来，我已经受过教训了），Driver 用[这个硬件表](https://networkupstools.org/stable-hcl.html)找对应 UPS 型号，如果是 USB，直接输入 / dev/ugenX.X（对应 dmesg 里显示的 UPS 所在的端口），Shutdown Mode 我选了 UPS 被使用 10 分钟（图中的 600 秒）后自动关闭，你也可以选择 UPS 低电量自动关机。Monitor Password 随便改下（大多数人都不需要，但是为了安全建议修改默认密码），就可以了，如果有需要可以设置邮件通知（需要 SMTP，此处就不再叙述了）。

然后启动服务，把 Auto Start 给勾上，然后在 Shell 或者 SSH 输入以下指令，UPS 就会返回状态了，可以算完成了。

> upsc ups@localhost  

### 硬盘休眠停转设置：

很多人都说 Spin Down（停转）伤硬盘，没办法这里电费太贵了，全天候跑着想想就费钱（纯属个人心理想法，频繁 Spin Down 和 Spin Up 对硬盘也不好，因为启动硬盘的电压之类的，一直 7x24 不停跑可能还更稳一些）  

不过因为设错某些个设置，导致硬盘还没买回来 1 个月 04（启停次数），C0（磁头关闭次数），C1（负载周期次数）猛飙（04 和 C0 370 多，C1 790 多），心疼硬盘。

研究了一下发现是因为 SMART 服务（与 SMART Task 不同，SMART 服务在 Service 那个界面，互不影响），我设置的是所有硬盘 30 分钟检测一次，无论状态，导致我的硬盘全部都要 30 分钟启动一次。马上就把 Power Mode（即设置的状态不运行 SMART）从 Never 改成了 Standby（Standby 状态的硬盘下不运行 SMART 服务）。

[![](https://qnam.smzdm.com/202010/25/5f9476f60b9499250.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_8/)

修改完后至少 04，C0，C1 飙的没那么快了，但是在运行任务的时候会莫名其妙涨，理论上运行任务是不应该停转的啊？然后才意识到硬盘的 APM(Advanced Power Management，智能供电管理) 设置导致运行中间还停转。  

[![](https://qnam.smzdm.com/202010/25/5f9479cc44ee861.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_9/)

然后马上就把 APM 改到 128（不智能自动 Spin Down，只自动进入 Idle 状态），这里解释一下，APM 和 HDD Standby 是互相独立的两个参数，HDD Standby 只在硬盘不工作后多少时间 Spin Down 节能，APM（1，64，127）是在某个时间后自动进入 Spin Down 休眠的（导致任务还没结束就开始 Spin Down，然后又马上重启）。这里我仍然保留了 HDD Standby 为 30 分钟，这也硬盘不工作 30 分钟后才会停转。APM 是 128（128 和 192）也就是不自动在某时间后停转，只进入 IDLE 或者 IDLE_B 的状态（磁头归位（IDLE_B）但是保持盘片旋转），如果在 30 分钟内有需要能马上启动，30 分钟后然后再通过 HDD Standby 让盘片停转，以避免 APM 自动频繁进入 Standby 状态关闭盘片旋转。  

此问题开头我也说了，在 7x24 小时的情况下，一直让硬盘转不 Spin Down 可能对硬盘更有利，自己**酌情设置或者禁用**，这个没有什么标准的。

### Jail 的 NAT 映射：

Jail 的网络可以选择直接用 DHCP 从你的路由器获取，也可以通过 NAT 的方式（把 FreeNAS 当路由器）给每个 Jail 分配内网 IP。很多应用我都选了 NAT，因为觉得所有 Jail 全在实际的路由器下不美观，也占 IP。看个人喜好来吧。  

[![](https://qnam.smzdm.com/202010/25/5f9481f39f1e74952.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_10/)灰色是因为我开着机

在 Jail 关机状态下，选择 Edit，然后选 Network Properties，最底下有个 NAT Port Forwarding，勾上之后添加协议（TCP 或者 UDP）程序所要的 Jail 端口和你想要的 Host 端口，然后保存并重新启动 Jail，访问 FreeNAS 的 IP:Host 端口就可以访问到服务了，下为例子

> [http://FREENAS 的 IP:Host 端口](http://FREENAS的IP:Host端口)

### 安装插件以外的软件：

很多软件都一键化了，网上也有其他教程如何在 Jail 里装特定软件例如 Aria2 和 Aria2NG。  

这里提醒一下，装了插件的 Jail 是不能直接用 pkg install 装别的软件的，需要修改一下配置。修改此路经的 FreeBSD.conf 这个文件

> /usr/local/etc/pkg/repos/FreeBSD.conf

然后把里面的 no 改成 yes 即可，即可使用包管理系统了

> FreeBSD: {enabled: yes}

## 服务搭建

安装 Plex，Transmission 这种就不在讲述了，人人都会，而且插件直接能一键安装，不麻烦，只需要搞定权限和挂载就行。Jail 不行就直接用 Bhyve 虚拟机，更完整更隔离。  

### 使用 Duplicati 备份：

因为要保证数据安全性（读作固执\~~），就决定用 Duplicati 来备份到云端，FreeNAS 上有提供很多备份的插件，但都不是很熟悉，所有没试过，Rclone 在 FreeNAS 没有官方或者非官方插件，看着配置也挺麻烦的，于是决定用一键的 Duplicati。Duplicati 是增量备份，每次文件有变动才会上传修改的部分。

### 云端服务供应商：

然后我最后选择了两个云端备份方案：Amazon S3 Glacier Deep Archive 和 OneDrive for Business。前者是商业化的方案，对后者的稳定性我是有所保留。

Amazon S3 Glacier Deep Archive 是非常冷的冷备，本身 Amazon S3 Glacier 就是储存长期备份用的，Deep Archive 对数据访问的要求是更低（AWS 明确写了 "适用于长期保存每年访问一两次且可在 12 小时内恢复的数据"），这就意味着备份被导入后，无法即时访问到数据，取出需要 12（标准取出，价格是批量的 8 倍）-48（批量取出，更便宜）个小时。因为 Amazon S3 Glacier Deep Archive 是作为极端情况（如火灾，水淹等不可抗力导致本地 NAS 报废，再加 OneDrive for Business 订阅报废，才会从 Amazon S3 Glacier Deep Archive 取出备份）的避险备份，基本上只输入数据到 Amazon S3 Glacier，基本不再取回了。

Amazon 标称 99.999999999% 的对象持久性（多少个 9 了都），然后再加 “一个 AWS 区域中不同地理位置的至少三个物理可用区”（如果我的理解无误的话也就是一个大概地方（例如弗吉尼亚北部）的不同机房（例如 Equinix DC1, DC12, Coresite VA1 等等这些机房 AWS 都有托管和 PoP）的至少三个物理可用区（AWS 自己的可用区，在弗吉尼亚北部有六个））。如果再出什么问题，那我就真的没话说了。

Amazon S3 Glacier Deep Archive 的价格也很便宜，存储 1TB 数据最低只要约 1.01 美元一个月（1GB 0.00099 USD，在弗吉尼亚北部，俄亥俄，俄勒冈，爱尔兰，斯德哥尔摩才有这个价，其他则在 0.0018 USD 到 0.002 USD（包括亚太），也就翻个倍，在南美是 0.0032USD，如果是从中国连的话建议选东京和新加坡，中国电信有直连（不知道现在怎么样），而且有 Lightsail（很重要））。上传（Put Object）每 1000 个文件是 0.05 美元，获取 (Get Object) 每 1000 个文件是 0.0004 美元。但是 Glacier 和 Glacier Deep Archive**需要取回文件**，批量取回每 1000 个文件 0.025 美元 + 每 GB 0.0025 美元（两者都要收，一个是算请求费用，一个是算数据量的费用），在 48 小时内返回数据，标准取回每 1000 个文件 0.10 美元 + 每 GB 0.02 美元（4 倍和 8 倍），在 12 小时内返回数据（加急费哈哈哈哈哈哈）。不过 AWS 还有免费额度，每个月给你 2000 的 Put, Copy, Post,List 请求（不包括 Deep Archive 的 Put 请求），还有 20000 的 Get 请求。

[![](https://qnam.smzdm.com/202010/25/5f94a2c28f8402052.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_11/)

然而需要特别注意，AWS S3 传入无流量费用，**传出是有流量费用的**（免费套餐有 15GB，不过够谁用啊？），每 GB 0.09 美元，传出 1TB 要 90 美金（亚洲更贵）！不过还有别的方法，如果**传出到 \*\***AWS S3 同区域的 EC2 上是免费。\*\* 虽然 EC2 的流量也是一个价，基于 EC2 的 Lightsail 只需要 3.5 美元就自带 1TB 流量包。只要用 Lightsail 中继一下（比如 东京 S3 - 东京 Lightsail - 你），这笔流量钱就省下了，只需要付 3.5 美元也就是 Lightsail 到你这段的费用，不过 Lightsail 一定要是同区域的 (不选香港 S3 是因为 Lightsail 没有香港分区)。

另外提早删除也会收费，Amazon S3 Glacier Deep Archive 中的文件最短须储存 180 天（Glacier 为 90 天），**如果提早删除，须将 180 天的储存费用付完**（例如有 1GB 的数据在储存 30 天后删除，你仍然需要为这 1GB 付剩下 150 天的费用），超过 180 天则不需要付此费用。

[![](https://am.zdmimg.com/202010/25/5f94a37a681134519.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_12/)

[![](https://qnam.smzdm.com/202010/25/5f94a3807eec47352.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_13/)我的账单

OneDrive for Business 的 SLA 是 99.9% 的可用性，不过因为订阅的关系不知道以后会怎么样。反正免费，不存白不存，如果出问题可以随时下载，比 Amazon S3 Glacier 要方便，顺序肯定是优先 OneDrive for Business 再 S3 Glacier Deep Archive 取数据。  

### Duplicati 配置备份：

安装完后在浏览器打开 FreeNAS 地址: 8200 即可，输入默认密码 duplicati，然后就可以开始配置了。

Duplicati 的网页端自带中文，选新增备份，配置新备份。

[![](https://qnam.smzdm.com/202010/25/5f94aa4f629ec7686.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_14/)

给备份起名，然后添加加密（可选，也可以不加密，性能低的情况下加密可能有影响）

[![](https://qnam.smzdm.com/202010/25/5f94aa612c5b23122.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_15/)

在 Amazon 的 S3 管理面板上创建新的 S3 储存桶，给储存桶起名字，选位置（中国大陆推荐选日本和新加坡），除了默认加密可以打开，其他都不建议打开。

[![](https://qnam.smzdm.com/202010/25/5f94ab323bb9b4666.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_16/)

选择 Amazon S3，启用 SSL（照理说应该是默认的），Bucket 名称和创建区域用刚才新建时你填入的内容，存储类型选 Deep Archive，文件夹路径可选填写（开头不需要加斜杠），AWS 访问 ID 和 AWS 访问密钥在 My Security Credentials - 访问密钥 里创建并填入，测试连接成功后即可

[![](https://qnam.smzdm.com/202010/25/5f94aab0800843665.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_17/)

选择你挂载到 Jail 的文件夹

[![](https://qnam.smzdm.com/202010/25/5f94acab213ff4490.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_18/)

设置自动运行备份的时间（按你的想法来），也可以手动运行。Duplicati 是增量备份，以前备份过无变动的文件是不会上传的。

[![](https://qnam.smzdm.com/202010/25/5f94acdbb077d505.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_19/)

按你的需求设置远程卷大小（这个大小是每个压缩加密后的文件包的最大大小，与你实际的文件大小或者实际最大备份大小无关），默认是 50MB，因为 AWS S3 是按照文件个数收费的，所以我设置的是 2GB，185GB 的所有文件最后大概是 183 个文件（里面包含校验文件和列表文件）。备份保留选择永久保留不需要改（增量备份只上传修改的部分）。高级设置要开，把 no-auto-compact 和 no-backend-verification 打开。Auto Compact 会把没达到压缩文件包最大大小的文件包合并，Backend Verification 会从云下载文件来校验以确保完整性。乍一看是挺好的，但是因为 Glacier 性质的关系，**需要取回文件才能下载**，这些功能实际上是没法用的。同理，Duplicati 无法直接使用 恢复功能 恢复存在 AWS S3 Glacier 的文件。保存完后就可以开始备份了。

[![](https://qnam.smzdm.com/202010/25/5f94ad44579fb6335.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_20/)

如果是 OneDrive for Business 的话，只需要在第二步选 Microsoft OneDrive v2，第五步可以调整远程卷大小，无需设置高级设置，因为 OneDrive for Business 可以即时下载。

[![](https://qnam.smzdm.com/202010/25/5f94af931df4a7230.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_21/)

### Duplicati 性能：

Duplicati 的性能还是可以接受的，备份到 Amazon S3 平均 120Mbps 上传，备份到 OneDrive for Business 平均 40Mbps 上传（因为需要压缩和加密，此外我的 OneDrive for Business 节点在亚太，上传速度有影响），宽带是 Verizon FIOS 1Gbps 对等。  

[![](https://qnam.smzdm.com/202010/25/5f94b04541837165.png_e680.jpg)
](https://post.smzdm.com/p/a5k4gwe8/pic_22/)

额外内容  

* * *

自己有那么多照片要备份，于是我决定去检查照片完整性，看看有多少照片是损坏的了。用到了 jpeginfo，最后发现 65000 多张照片，有 10 多张是损坏的，问题留下很久了，也不知道是哪里出的问题，心疼啊。  

直接在 Jail 里安装

> pkg install jpeginfo

然后在你的照片主目录下运行此命令，会递归查找此目录（包括子目录）下所有照片，只反馈损坏的照片

> find . -iname "\*.jpg"-exec jpeginfo -c {} ; | grep -E"WARNING|ERROR"

## 后记

FreeNAS 配置完了还是挺好用的，就是有些地方有些麻烦。用 Amazon 还有 OneDrive 备份至少是让我自己安心了。折腾了 2-3 个月，写了这篇 8000 多字的文章，就和这一路折腾过来一样。

![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png) 
 [https://post.smzdm.com/p/a5k4gwe8/](https://post.smzdm.com/p/a5k4gwe8/) 
 [https://post.smzdm.com/p/a5k4gwe8/](https://post.smzdm.com/p/a5k4gwe8/)
