# 查询 Google Pixel 保修信息以及判断是否为 Verizon （运营商） 版 - Eric
如果你想了解关于运营商版和无锁版 Pixel 的不同点，你可以展开下列内容看看：

运营商版 Pixel 的相关资料

* * *

> -   根据你购买手机的区域，你可能需要解锁你的 Google Pixel 来切换运营商网络。所有**通过 Google Store 购买**的 Pixel 都会自动解锁，所以你不必考虑解锁运营商网络的问题——你可以自由地使用任一运营商。
> -   对于那些通过特定运营商或其他零售商购买 Pixel 的人，SIM 卡最长可能被锁定 2 年。在这种情况下，除非销售商解锁 SIM 卡或合约到期，否则手机只能使用该服务提供商提供的移动服务。
> -   如果您想在销售合约到期前解锁手机的 SIM 卡，请与您的移动服务提供商联系，以商讨可行的方案。
> -   “开发者选项”中的 “OEM 解锁” 选项，无法在 Verizon 版的 Pixel 机型上启用。 Verizon 版 Pixel 的 bootloader 经过加密无法解锁。
>
> **资料来源**：
>
> -   1.  [搭配任意移动网络使用 Pixel 手机](https://support.google.com/pixelphone/answer/7107188?hl=zh-Hans)
> -   2.  [How to unlock a Google Pixel phone, so you can switch carrier networks](https://www.businessinsider.com/how-to-unlock-google-pixel)

## [](#保修信息查询 "保修信息查询")保修信息查询

1.  点击上面的网站链接，会让你登录谷歌账号，随后跳转到美国地区的查询链接 `https://store.google.com/us/repair` 。如果你的 Pixel 是其他地区购买的， 你可能需要把链接里的 `us` 改成购买所在地的区域代码才能查询到信息，如 `https://store.google.com/gb/repair` （gb, Great Britain 的简写，为英国的区域代码）。
2.  用手机拨号器拨打 `*#06#` ，可以看到手机的 IMEI 和序列号。IMEI 处的内容为 `*************** / **`，我们这里取 `/` 前面的内容（也就是前 15 位），输入到网站里即可获知 Pixel 的**容量大小**，**机身颜色**和**保修截止日期**等。有部分人可以查到 Unlocked（无运营商锁），Verizon（Verizon 运营商版），或者是 Refurbished（官翻机）之类的字样，而我这里却什么都查不到🤣。

[![](https://cdn.jsdelivr.net/gh/ericclose/images/Pixel-repairs-and-carriers/1.png)
](https://cdn.jsdelivr.net/gh/ericclose/images/Pixel-repairs-and-carriers/1.png)Pixel 保修信息查询结果

## [](#确认是否可以解锁-bootloader "确认是否可以解锁 bootloader")确认是否可以解锁 bootloader

1.  前往「**Settings**（设置） -> **About phone**（关于手机） -> **Build number**（版本号）」，连续点击版本号 7 次即可开启「**开发者选项**」。
2.  确保手机能够正常访问国际互联网（如路由器翻墙，或手机开代理软件等）
3.  前往「**Settings**（设置） -> **System**（系统） -> **Adavanced**（高级） -> **Developer options**（开发者选项）」，**开启**「**OEM unlocking**」选项。如若选项显示为灰色，且为不可开启的状态，则很可能是 Verizon 版（带运营商锁的版本），我手中的这台 Pixel 就是这样😂。

![](https://cdn.jsdelivr.net/gh/ericclose/images/Pixel-repairs-and-carriers/2.png)

无法开启的 OEM unlocking 选项

4.  开启「**OEM unlocking**」选项后，手机进入 bootloader 模式（**手机在重启或开机的过程中，长按音量下键即可进入 bootloader 模式**），可以看到 Product/Variant: sailfish-US-PVT 。US 指的是美国，PVT 则是指 **产品验证测试阶段** （production validation test），这么来说也算是正常的销售商品；如若是 EVT 的话则是指 **工程验证测试阶段** （engineering validation test），那么这种手机也就是俗称上的工程机了。

[![](https://cdn.jsdelivr.net/gh/ericclose/images/Pixel-repairs-and-carriers/3.png)
](https://cdn.jsdelivr.net/gh/ericclose/images/Pixel-repairs-and-carriers/3.png)

5.  为了完成下面的操作，请确保满足以下条件：

你的电脑已经配置好 ADB 工具的环境变量（不懂的话就请网上自行搜索 “ADB 环境变量”）

6.  完成上述步骤后，手机在 bootloader 模式尝试解锁 bootloader 的命令，电脑打开 cmd 执行：

```bash
fasboot flashing unlock
```

7.  如果你前面正常启用「**OEM unlocking**」选项的话，这时手机应该就会询问你是否解锁 bootloader。由于安全设计，**如果你确认解锁 bootloader 手机将会删除所有的数据（即恢复出厂设置）**，取消则不会解锁 bootloader。（而我这边输入命令后，直接报错 `FAILED (remote: Flashing Unlock is not allowed)` ，也就根本不会询问是否解锁 bootloader。这也是正常的，因为我前面根本无法开启 OEM unlocking 的选项🥺。经过上面的操作，我基本可以确认我这台 Pixel 就是 Verizon 版的了…）

![](https://cdn.jsdelivr.net/gh/ericclose/images/Pixel-repairs-and-carriers/unlock-bootloader.jpg)

询问是否解锁 bootloader（图片来自互联网）

## [](#更简单的方法确认是不是-Verizon-版 "更简单的方法确认是不是 Verizon 版")更简单的方法确认是不是 Verizon 版

最后我查阅资料发现有个简单的方法确认 Pixel 是不是 Verizon 版本的，只需要这么做：

1.  前往「**Settings**（设置） -> **System**（系统） -> **Adavanced**（高级） -> **Developer options**（开发者选项）」，开启「**USB Debuging**（USB 调试）」的选项。
2.  然后 cmd 执行：

```bash
adb start-server
```

3.  期间手机会弹框问询是否允许此电脑对其进行调试，勾选「**Always allow from this computer**（总是允许此电脑调试）」 ，然后点击 **Allow** 即可。

![](https://cdn.jsdelivr.net/gh/ericclose/images/Pixel-repairs-and-carriers/4.png)

4.  最后 cmd 执行命令：

```bash
adb shell getprop ro.boot.cid
```

5.  如果返回的值为 `VZW__001` ，那么很抱歉这个 Pixel 是 Verizon 版的；无运营商锁的版本的返回值应为 `11111111`。唉，不说了，我这台就是😂。

![](https://cdn.jsdelivr.net/gh/ericclose/images/Pixel-repairs-and-carriers/5.png)

**后续**：最后我要说的是，尽管不能通过正常渠道解锁 Verizon 版 Pixel 的 bootloader。但是 Pixel 1 还是有办法解锁的（即使是 Pixel 1 最后更新的系统版本 —— 2019 年 12 月），我去了某宝找奸商花费了 50 RMB 成功解锁（为了避免广告嫌疑这里就不说店名了，可以搜索关键词：_Pixel bl 锁_ 
 [https://ericclose.github.io/Pixel-repairs-and-carriers.html](https://ericclose.github.io/Pixel-repairs-and-carriers.html) 
 [https://ericclose.github.io/Pixel-repairs-and-carriers.html](https://ericclose.github.io/Pixel-repairs-and-carriers.html)
