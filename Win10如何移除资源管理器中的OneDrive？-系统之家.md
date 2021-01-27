# Win10如何移除资源管理器中的OneDrive？-系统之家
当前位置：[系统之家](http://www.xitongzhijia.net/) > [系统教程](http://www.xitongzhijia.net/xtjc/) > Win10 如何移除资源管理器中的 OneDrive

时间：2016-12-16 17:22:07 作者：chunhua 来源：[系统之家](http://www.xitongzhijia.net/) 1. 扫描二维码随时看资讯 2. 请使用手机浏览器访问： [http://m.xitongzhijia.net/article/89184.html](http://m.xitongzhijia.net/article/89184.html) ![](https://api.pwmqr.com/qrcode/create/?url=http://m.xitongzhijia.net/article/89184.html)
     手机查看  [评论](#mark)

　　OneDrive 是一个云存储服务，不过该功能在国内不能很好的使用，而又占据着资源，所以很多 Win10 用户想将 OneDrive 从资源管理器中移除，那么该如何操作呢？下面我们一起来看看具体的操作步骤。

![](http://www.xitongzhijia.net/uploads/allimg/161216/56-1612161H018-water.jpg)

**步骤如下：** 

　　1、使用 Win+R 打开运行对话框，输入 regedit 打开注册表编辑器；

![](http://www.xitongzhijia.net/uploads/allimg/161216/56-1612161H018-50-water.jpg)

　　2、定位到 HKEY_CLASSES_ROOT\\CLSID\\{018D5C66-4533-4307-9B53-224DE2ED1FE6}，在右边双击 System.IsPinnedToNameSpaceTree 这个键，将 System.IsPinnedToNameSpaceTree 中的值从 1 改为 0。

![](http://www.xitongzhijia.net/uploads/allimg/161216/56-1612161H018-51-water.jpg)

　　**PS：** 如果你觉得定位有点逆天，那么可以使用注册表查找大法，使用 Ctrl+F 打开搜索框，之后在搜索框里面输入以上的这个数值，电脑就会自动帮你定位到这个注册表的项目上。

![](http://www.xitongzhijia.net/uploads/allimg/161216/56-1612161H019-water.jpg)

![](http://www.xitongzhijia.net/uploads/allimg/161216/56-1612161H019-50-water.jpg)

　　3、之后打开 “此电脑”，那个恼人的“OneDirve” 就看不到了，如果无效的话，就重启你的电脑。

　　4、如果你突然想要使用 “OneDirve” 的话，只要在地址栏上输入 “OneDrive” 就可以了。

![](http://www.xitongzhijia.net/uploads/allimg/161216/56-1612161H238-water.jpg)

　　5、如果你怀念 “OneDrive”，也不用担心，只要按照上面的方法做，在第五步将 0 改回 1，那么“OneDirve” 又出现在了[](http://www.xitongzhijia.net/zt/59015.html)[任务管理器](http://www.xitongzhijia.net/zt/rwglq/)上面了。因为更改了注册表并不是将 “OneDrive” 卸载，这个应用还是存放在你的电脑里面的。

　　以上就是 Win10 系统下移除资源管理器中 OneDrive 的操作方法，大家同样可以按照此方法找回 OneDrive，只要修改 “System.IsPinnedToNameSpaceTree” 中的值即可。 
 [http://www.xitongzhijia.net/xtjc/20161216/89184.html](http://www.xitongzhijia.net/xtjc/20161216/89184.html) 
 [http://www.xitongzhijia.net/xtjc/20161216/89184.html](http://www.xitongzhijia.net/xtjc/20161216/89184.html)
