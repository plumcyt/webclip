# FreeNas中使用rsync同步文件 | NASGEEK
> 声明：本文为本站【[NASGEEK](https://www.nasgeek.cn/)】原创文章，未经许可不得转载！

　　FreeNas 上有各种方法可以备份同步文件，比如云同步任务、安装 syncthing 插件等。这里介绍一个十分简便的同步方法：rsync。这是 FreeNas 系统自带的，你不用额外安装或者设置。

### 　　一、FreeNas 内部文件同步

　　为了提高常用文件访问速度，我在 FreeNas 中用了一块固态硬盘存储照片等小文件。不过由于固态硬盘价格比较贵，我不想再弄一块做镜像（穷），因此我需要系统自动定期将固态硬盘里的文件同步到机械硬盘中。rsync 命令就很适合做这个工作。

　　点击左侧菜单栏【任务】–>【计划任务】，再点右上角的【添加】按钮，出现添加计划任务的页面：

![](https://www.nasgeek.cn/wp-content/uploads/2020/06/1-1-1024x486.png)

　　“描述” 自己随便填一个。

　　“命令” 使用 “rsync”：

rsync -avz --delete /mnt/SSD/personal/ /mnt/STORE/personal/

rsync -avz --delete /mnt/SSD/personal/ /mnt/STORE/personal/

    rsync -avz --delete /mnt/SSD/personal/ /mnt/STORE/personal/

　　rsync 命令的使用和参数网上很多，我这里就不引用了，请自行学习。我这里是将 “/mnt/SSD/personal” 这个目录下的所有文件包括权限和属性全部同步到 “/mnt/STORE/personal” 目录下。注意：命令行中目录最后带上斜杆和不带斜杆是不一样的，不带表示的是同步整个目录包括目录本身，带上斜杆表示的是同步目录中的文件，不包括目录本身。

　　“以用户身份运行” 里选择 root 用户；“安排计划任务” 里选择要运行的周期，我这里是 “每天”；其它几个地方按图上选择就行了，然后保存。

![](https://www.nasgeek.cn/wp-content/uploads/2020/06/2-1024x457.png)

　　点击 “立即运行” 可以马上运行这条命令，然后你可以使用 winscp 之类的工具看看不是开始同步了。我们在添加任务时没有勾选 “隐藏标准错误” 这个选项，因此如果运行中有错误可以在日志文件 “/var/log/cron” 中看到。如果你想看到每次同步了哪些文件，可以取消勾选 “隐藏标准输出” 这个选项。

### 　　二、FreeNas 与 windows 之间文件同步

　　FreeNas 与 windows 之间同步文件也很方便。windows 上使用 cwrsync 这个开源工具。

#### 　　1、FreeNas 中的设置

　　点击左侧菜单栏【服务】，找到 “Rsync”，点击后面的动作按钮：

![](https://www.nasgeek.cn/wp-content/uploads/2020/06/3-1024x61.png)

　　进入配置界面后，打开 “Rsync Module” 页面，点击【添加】按钮，出现如下页面：

![](https://www.nasgeek.cn/wp-content/uploads/2020/06/4-1024x518.png)

　　“名称”自己随便填一个；“路径”选择需要同步的目录；“访问模式”有三种选择，我这里是要将 freenas 中的文件同步到 windows 中，因此我选择 “Read Only” 就行了，如果你需要将 windows 里的文件同步到 freenas 中，就选择 “Read and Write”；“用户” 和“群组”选择 “root” 和“wheel”；“允许主机”中填写你只允许连接的主机，这样安全一点。然后保存。

　　然后转到 “服务” 页面，打开 “Rsync” 服务，并选择“自动启动”。

#### 　　2、windows 中的设置

　　到[https://www.itefix.net/cwrsync-free-edition](https://www.itefix.net/cwrsync-free-edition)下载 “cwrsync” 免费版，然后解压。这个下载链接是官方的，可以看[https://www.rsync.net/resources/howto/windows_rsync.html](https://www.rsync.net/resources/howto/windows_rsync.html)这个页面。

　　在 cwrsync 目录下新建一个 bat 文件，内容如下：

.\\bin\\rsync -avz --progress --delete --port 873 root@freenas::personal /cygdrive/d / 个人 /

@echo off echo. echo 开始同步数据，请稍等... echo. cd /d %~dp0 .\\bin\\rsync -avz --progress --delete --port 873 root@freenas::personal /cygdrive/d / 个人 / echo. echo 数据同步完成 echo. pause

    @echo off
    echo.
    echo 开始同步数据，请稍等...
    echo.
    cd /d %~dp0
    .\\bin\\rsync -avz  --progress  --delete  --port 873 root@freenas::personal /cygdrive/d/个人/
    echo.
    echo 数据同步完成
    echo.
    pause

　　第 6 行 “root@freenas::personal” 中，“root”是 “Rsync Module” 页面中设置的用户，“freenas”是我的 freenas 主机名，“personal”是 “Rsync Module” 页面中设置的名称。后面 “/cygdrive/d / 个人 /”，是我 windows 中的同步目录，“cygdrive” 指的是本机上的硬盘。

　　如果你想反过来，将计算机中的文件同步到 freenas 中，只需将第 6 行中的两个目录对换一下前后位置就行了，需要注意的是在设置 “Rsync Module” 时候 “访问模式” 需要选择可写。

　　接下来，双击运行这个 bat 文件就可以开始同步了。当然你可以把这个 bat 文件添加到 windows 的计划任务中，实现自动同步。

　　我这里使用的是 module 模式进行同步，还有一种是使用 ssh 进行连接，可以参考[https://www.rsync.net/resources/howto/windows_rsync.html](https://www.rsync.net/resources/howto/windows_rsync.html)里的说明进行设置。 
 [https://www.nasgeek.cn/?p=664](https://www.nasgeek.cn/?p=664) 
 [https://www.nasgeek.cn/?p=664](https://www.nasgeek.cn/?p=664)
