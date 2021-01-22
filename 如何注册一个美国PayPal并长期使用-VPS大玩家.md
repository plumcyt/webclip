# 如何注册一个美国PayPal并长期使用-VPS大玩家
为什么要注册一个美国 PayPal？一般有以下几个原因：

1.  使用中国地址注册的 PayPal 无法在一些网站下单，比如联想美国站，详情见：[记一次通过联想 8 通道海淘 ThinkPad 的经历](https://www.vpsdawanjia.com/1937.html)
2.  一些美国网站搞活动时，只有美国 PayPal 才能享受折扣或者只有美国 PayPal 才能试用产品，比如 beenverified
3.  在阿里云国际、Google Play、App Store、Netflix 等平台付款
4.  美国 PayPal 提现余额到美国银行无手续费，而中国 PayPal 提现余额到美国银行要收 35 美元手续费
5.  中国 PayPal 间不能相互转帐
6.  有些网站不接受你的信用卡付款，但把信用卡绑定到 PayPal 以后却可以正常付款，很明显这是网站为了降低风险，把风险直接丢给 PayPal 处理。

这里的美国 PayPal 是指用美国地址注册的 PayPal。

由于 PayPal 的风控很严格，如果想注册一个美国 PayPal 并长期使用，注册前我们需要做好充分的准备，一般需要准备好以下资料和工具：

1.  一个美国电话，可以用 Google Voice
2.  一个电子邮箱，推荐用 gmail 或者 outlook
3.  美国地址，这个请在 Google 地图上找真实的地址，自己编的过不了 PayPal 的地址库验证。如果你知道[虚拟信用卡](https://www.vpsdawanjia.com/category/virtual-card)的帐单地址邮编，建议找邮编附近的地址。一般的美国礼品卡可以自己义邮编和帐单地址，而一些虚拟信用卡的邮编则是固定的，比如 BOA 的虚拟信用卡以及 WEX Bank 的万事达虚拟信用卡，当然也有一些虚拟信用卡可以随意指定邮编和帐单地址。另外，建议找居民区的地址，找好以后可以在 Google 街景上核实一下。具体方法参见：[如何在谷歌 (Google) 地图上找一个真实的美国地址](https://www.vpsdawanjia.com/2594.html)
4.  美国 IP 地址，最好固定 IP 地址，这样可以有效地避免触发 PayPal 的风控。我的解决方案是在[VPS 上安装 VNC 和 Firefox 浏览器](https://www.vpsdawanjia.com/465.html)，所有的操作直接在 VPS 上进行。如果你对 Linux 系统不熟悉，建议找可以安装 Windows 操作系统的 VPS 服务器，通过远程桌面连接上去操作。如果你使用[Google Fi](https://www.vpsdawanjia.com/aboutgooglefi)，那就更简单了，直接在手机上操作即可，也可以开热点给笔记本电脑使用，在电脑上注册美国 PayPal.
5.  用于验证的虚拟信用卡，可以使用 EasyPay 环球捷汇发行的[422813 虚拟信用卡](https://www.vpsdawanjia.com/3377.html)。

当你准备好以上资料和工具以后，就可以开始你的美国 PayPal 之旅了。美国 PayPal 官方网址 [www.paypal.com](https://www.paypal.com/)，打开以后选 Personal，然后点”Sign Up for Free”，在 VNC 上打开 PayPal 网站，图片失真很严重，我就不截图了。打开的注册界面是这样的：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/ppregister.png)

之前准备好的地址派上用场了，一个典型的美国地址是这样的：9301 Tampa Ave, Northridge, CA 91324（**不要使用这个地址，会注册失败，因为用这个地址的人太多了**），解释如下：

9301 Tampa Ave, 街道地址  
Northridge, 城市  
CA 州  
91324，邮编

如图：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/ppaddress.png)

提交地址信息以后打开如下界面：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/done.png)

点”Get Started” 来绑定你的虚拟信用卡，如图：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/addcvv.png)

绑定成功，可以开始消费了。

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/ppsuccess.png)

正式使用之前最好验证一下邮箱和手机号码，否则你会看到以下提醒：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/reminder.png)

这两步很容易，就不赘述。先去开个 Netflix 试用压压惊，选最贵的 $15.99，反正试用不要钱。非常意外，居然失败了，估计是 PayPal 刚注册，没有交易记录，无法开启预核准付款。Sorry，we weren’t able to set up preapproved payments aft this time. 如图：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/preapproved.png)

放弃是不可能放弃的，直接用 553232 的卡开免费试用成功，如图：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/netflix.png)

为了避免忘记取消试用，到期后自动扣费，建议删掉试用的卡。但是我还是不甘心，回到 Netflix 帐号里修改付款方式为 PayPal，继续验证，但依然提示：Sorry，we weren’t able to set up preapproved payments aft this time. 看来需要联系客服才能解决这个问题了，使用 Google Fi 打了 10 分钟的电话，虽然 PayPal 客服很想帮我解决这个问题，但是系统认为高风险，她也没办法处理，看来只有等几几天再试试。**根据开通阿里云国际的经验，新注册的 PayPal 有可能需要先消费一笔以后，才可以开通这种免费试用的 preapproved payments，比如先开通 beenverified.com 1 美元的试用。** 

再试试 beenverified.com 的试用，1 美元试用 5 天，5 天后扣款 $26.89，这个太贵了，必须取消。

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/beenverified.png)

再试试阿里云吧，使用注册 PayPal 时用的美国地址、美国电话，以 PayPal 为付款方式，一次性成功，如图：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/alibabacloud.png)

注册成功以后，在 PayPal 的 Automatic Payments 列表里可以看到 Alibaba Cloud US LLC 以及之前开通试用的 BeenVerified Inc，如图：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/automaticpayments.png)

开通试用服务器，如图：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/ppalibabacloud-1024x205.png)

我开的是 US West 1 Zone B，随便选了个试用，如图：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/ppaliyun-1024x176.png)

Apple id 和 Google Play 我就先不测试了，毕竟新注册的 PayPal，如果交易过于频繁，被风控的风险还是很大的，我们的目的是长期使用，而不是短暂的快感。

记下你注册 PayPal 时所用的姓名，地址，电话以及邮箱，在注册别的网站时尽量使用相同的资料，可以有效地避免交易被拒绝。

## 美国 PayPal 收款

没有绑定 SSN 或者 ITIN 的 PayPal 可以收款，但是不能存到余额，只能绑定美国银行帐号 (Checking 或者 Savings) 直接提现，PayPal 会打两笔小额的款项用于验证，但是在完成验证前就可以收款，完成验证后可用于付款。我用的是[Bank of America](https://www.vpsdawanjia.com/283.html)的 Savings，如图：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/verifyyourbankassouts.png)

验证你的美国银行帐号

一般一个工作日左右你就可以看到这两笔小额的 ACH 汇款，输入这两笔小额的汇款金额即可完成验证，此时你的 PayPal 变成 Verified。**Your PayPal account is Verified and you are a Verified PayPal member.**

根据 PayPal 的官方说法，验证过的 PayPal 更容易获得客户的信任，不过目前只用于收返利网站的钱和个人转帐，并不觉得有什么优势。

## PayPal 进阶

当你搞到 ITIN 以后，美国 PayPal 的可玩性又增加了：你可以激活 PayPal Cash Plus account，申请 PayPal Credit 以及 PayPal Cash Card，下面分别进行解释。

PayPal Cash Plus account，从 2019 年 4 月 1 日开始，你必须激活这个帐号以后才能保留 PayPal 余额、使用 PayPal 余额进行付款以及接收别人的 PayPal 转帐，否则别人付给你的钱不能存到余额，只能绑定美国银行帐号 (Checking 或者 Savings) 直接提现。激活后的 PayPal Cash Plus account 是这样的：

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/paypalbalance.png)

PayPal Credit： 类似于国内的支付宝花呗以及京东白条，上传了 ITIN 扫描件以后给了 2000 美元的额度。

[PayPal Cash Card](https://www.paypal.com/us/webapps/mpp/cash-card)：一张和你的 PayPal 绑定的美国万事达借记卡，用来海淘也不错。

PayPal Prepaid Mastercard，网址：[paypal-prepaid.com](https://www.paypal-prepaid.com/)，需要使用海外 IP 才能打开，有四个卡面可以选择，可以获得一个 Checking 银行帐号。

## PayPal 虚拟卡

没错，PayPal 也出虚拟信用卡了！可惜只有一个卡位，可以自定义 CVV(安全码)，可以换卡号。PayPal 的这个产品叫做 PayPal Key，~ 目前正在内测中，并不是所有人都有机会体验。获得内测资格的方法：从有 PayPal Key 功能的 PayPal 帐号转一笔钱到你的 PayPal，就可以获得内测资格。 ~ 目前已经上线，详情：[美国虚拟信用卡 PayPal Key：来自美国 PayPal 的虚拟卡](https://www.vpsdawanjia.com/4246.html)

![](https://www.vpsdawanjia.com/wp-content/uploads/2019/05/paypalKey.png)

## 常见问题

付款时出现以下提示：  
Your PayPal account is missing a payment method. Please add a card to continue.  
或者  
添加一张卡到您的账户，您的 paypal 账户缺少付款方式，请添加卡以继续  
这是被 PayPal 风控了，简单点说就是 PayPal 不信任你，怎么办？可以绑定一个美国银行帐户来验证 PayPal 帐号，把 PayPal 升级，比如 VELO。中国人可以申请 VELO，只需要提供身份证即可。

当然，养号也很重要，可以先去一些小网站或者要求低的网站下单，养一段时间以后再去风控比较严的网站下单。

再次强调，IP 要稳定，不要经常换登录环境。

在国内可以申请美国 ITIN，具体申请办法可以看这篇文章：[在国内申请美国 ITIN 的几个方法](https://www.vpsdawanjia.com/2731.html)

如果你想办理美国银行帐号，可以参考以下的文章：

1.  [2020 美国银行 BOA 开户攻略](https://www.vpsdawanjia.com/boa)
2.  [美国银行 (BOA) 虚拟卡（无开卡费）](https://www.vpsdawanjia.com/283.html)
3.  [美国国泰银行开户记 (国内见证开户，不需要美国签证)](https://www.vpsdawanjia.com/2051.html)
4.  [免费申请美国黑貂 Sable 银行帐户 (没有月管理费、没有最低存款要求)](https://www.vpsdawanjia.com/3830.html)
5.  [美国虚拟信用卡 Revolut 申请使用指南](https://www.vpsdawanjia.com/3873.html)  Revolut 自带一美国银行帐号（Metropolitan Commercial Bank）
6.  [国际汇款转帐服务平台 TransferWise 初体验](https://www.vpsdawanjia.com/1716.html) TransferWise 提供五个币种的虚拟银行帐号

都是不需要出国，在国内即可办理。

转载记得给个链接：[VPS 大玩家](https://www.vpsdawanjia.com/) » [如何注册一个美国 PayPal 并长期使用](https://www.vpsdawanjia.com/2070.html) 
 [https://www.vpsdawanjia.com/2070.html](https://www.vpsdawanjia.com/2070.html) 
 [https://www.vpsdawanjia.com/2070.html](https://www.vpsdawanjia.com/2070.html)
