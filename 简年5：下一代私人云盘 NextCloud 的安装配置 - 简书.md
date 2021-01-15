# 简年5：下一代私人云盘 NextCloud 的安装配置 - 简书
[![](https://upload.jianshu.io/users/upload_avatars/137499/3de68ca8-5a91-400a-a4f5-4fd365205c1a.png?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)
![](https://upload.jianshu.io/group_image/7976179/e6271015-5486-4f2f-ae35-01faf5575088?imageMogr2/auto-orient/strip|imageView2/1/w/134/h/134/format/webp)
](https://www.jianshu.com/u/e213f00c7c35)

0.3032017.01.24 23:44:24 字数 878 阅读 63,303

> 之前看到一个名为 Nextcloud 的项目，没有注意，只是感觉和 Owncloud 的界面非常相似，大概是有千丝万缕的关系。  
> 然后最近看新闻才知道 Owncloud 母公司破产了，原团队已经出走，新的项目名为 Nextcloud，所以本文将介绍 Nextcloud 的安装配置。  
> 需要说明一下的是，Owncloud 并没有停止开发，而是由原来的德国团队接手了，所以你可以继续使用 Owncloud。之前的关于 Owncloud 的介绍：[http://www.jianshu.com/p/792a5c1fa44b](https://www.jianshu.com/p/792a5c1fa44b)

## 1. 安装 Docker 与 Compose

一贯的风格首先安装 Docker：

    curl -sSL https://get.docker.com/ | sh 

然后安装 Compose：

    curl -L https://github.com/docker/compose/releases/download/1.10.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

    chmod +x /usr/local/bin/docker-compose 

这样我们的基本工具就搞定了。

#### 1.1. 配置 Docker 镜像源

接下来我们会拉取几个镜像，默认的镜像仓库在海外，速度不理想，所以我们使用国内的镜像源，这里以中科大的为例：  
首先编辑文件 /etc/docker/daemon.json，在该配置文件中加入下面内容（没有该文件的话建一个）：

    {
      "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
    } 

保存之后最好重启一下 Docker 服务，这样就可以使用国内镜像源拉取镜像了。

#### 1.2. 部署 NextCloud

首先为 Nextcloud 建立一个独立的容器网络：

    docker network create nextcloud 

接下来新建一个文件夹，名字随便，这里演示为 cloud，然后在文件夹里面新建一个文件，文件名为 Caddyfile，文件内容如下：

    example.com {
      proxy / 233.233.233.233:2333 {
          proxy_header Host {host}
          proxy_header X-Real-IP {remote}
          proxy_header X-Forwarded-Proto {scheme}
      }
      log /var/log/caddy.log
      gzip
    } 

因为使用 Caddy 部署应用不需要花费诸位太多时间去配置 Web 服务器环境，所以我这里使用 Caddy，实际上如果你喜欢 Nginx，自己修改下面的配置就好了。Caddy 适合不想写配置或者懒得动手申请 SSL 的读者。

接下来再新建一个文件，名为 docker-compose.yml，文件的内容如下：

    version: '2'
    services:
      db:
        container_name: cloud_db
        image: mysql
        volumes:
          - "./data/cloud/mysql:/var/lib/mysql"
        restart: always
        environment:
          MYSQL_ROOT_PASSWORD: 这里填写你的密码
          MYSQL_DATABASE: nextcloud
      app:
        container_name: cloud_app
        depends_on:
          - db
        image: nextcloud
        volumes:
          - ./data/cloud/config:/var/www/html/config
          - ./data/cloud/data:/var/www/html/data
          - ./data/cloud/apps:/var/www/html/apps
        links:
          - db
        ports:
          - "2333:80"
        restart: always
      cron:
        container_name: cloud_cron
        image: nextcloud
        links:
          - db
        volumes_from:
          - app
        user: www-data
        entrypoint: |
          bash -c 'bash -s <<EOF
          trap "break;exit" SIGHUP SIGINT SIGTERM
          while /bin/true; do
            /usr/local/bin/php /var/www/html/cron.php
            sleep 900
          done
          EOF'
        restart: always
      web:
        container_name: cloud_web
        image: abiosoft/caddy
        volumes:
          - ./Caddyfile:/etc/Caddyfile
          - ~/.caddy:/root/.caddy
        ports:
          - 80:80
          - 443:443
        restart: always
    networks:
      default:
        external:
          name: nextcloud 

保存文件之后，一句话启动它～～

上面文件内容只有那个密码是需要你改的，其他不变即可。  
稍微去喝杯茶，一会回来你就可以看到 Nextcloud 部署成功了。

![](https://upload-images.jianshu.io/upload_images/137499-ac09a0c5ce270c6f.png)

安装界面

配置数据库自己根据需要修改，打算一个人用，就用 Sqlite，很多人用就用 MySQL 之类的吧。

![](https://upload-images.jianshu.io/upload_images/137499-8a39225333fab316.png)

配置数据库

你的用户名就是 root，数据库地址是 db，不是 localhost。

## 2. 配置 NextCloud

安装完成还要做两件事，当然不是必须的，但是为了安全起见，最好再折腾一下。

#### 2.1. 配置两步验证

两步验证可以防暴力入侵什么的，建议使用。首先在后台启用两步验证，然后手机安装下面的应用：  
[https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2](https://link.jianshu.com/?t=https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2)  
登录时需要手机上的离线验证码验证，安全有了多一层保障。

#### 2.2. 客户端安装与使用

首先客户端几乎是全平台的，地址在下面：  
[https://nextcloud.com/install/#install-clients](https://link.jianshu.com/?t=https://nextcloud.com/install/#install-clients)  
因为开启了二步验证，在客户端直接使用帐号密码肯定无法登录了，所以需要在后台设置应用密码，地址格式：  
[http:// 你的地址 / index.php/settings/personal#apppasswords](http://你的地址/index.php/settings/personal#apppasswords)  
在设置中设置应用一次性密码，使用随机密码登录客户端即可。  

![](https://upload-images.jianshu.io/upload_images/137499-37f46f99cdfa7b1e.png)

设置客户端一次性密码

更多精彩内容下载简书 APP

"不回评论，有事请发邮箱（即时收到）：i@zuolan.me"

还没有人赞赏，支持一下

[![](https://upload.jianshu.io/users/upload_avatars/137499/3de68ca8-5a91-400a-a4f5-4fd365205c1a.png?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/webp)
![](https://upload.jianshu.io/group_image/7976179/e6271015-5486-4f2f-ae35-01faf5575088?imageMogr2/auto-orient/strip|imageView2/1/w/140/h/140/format/webp)
](https://www.jianshu.com/u/e213f00c7c35)

[左蓝](https://www.jianshu.com/u/e213f00c7c35 "左蓝")不签约、不写书、不约稿、不互粉 不看简信、不回评论、不接广告、转载随意 还有事情，请联...

总资产 24 (约 1.08 元) 共写了 10.3W 字获得 4,694 个赞共 1,762 个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

-   demo 地址：[https://github.com/songshijun1995/minio-demo\\\[https..](https://github.com/songshijun1995/minio-demo\[https..).

    [![](https://upload-images.jianshu.io/upload_images/24191368-00350d2d41b5ff3d.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)
    ](https://www.jianshu.com/p/c0bf5facde51)
-   前置工作：插上你的移动硬盘，为 NTFS 分区取一个简单直观的英文名称，例如，我的移动硬盘是 4T 的，为了科学利...

    [![](https://upload.jianshu.io/users/upload_avatars/4080991/f6df1e44785d.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/webp)
    松撻](https://www.jianshu.com/u/2a51602d2555)阅读 174 评论 1 赞 0

    [![](https://upload-images.jianshu.io/upload_images/4080991-6543071a69b5242d.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)
    ](https://www.jianshu.com/p/4052b39b2cd8)
-   摘要 平时经常用 Docker 来部署各种环境，发现从 DockerHub 上下载镜像有时候比较慢。第三方的镜像还可以使用...

    [![](https://upload-images.jianshu.io/upload_images/20063139-6640d64c3c251a90?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)
    ](https://www.jianshu.com/p/1ead73e5103e)
-   平时经常用 Docker 来部署各种环境，发现从 DockerHub 上下载镜像有时候比较慢。第三方的镜像还可以使用一些国...

    [![](https://upload-images.jianshu.io/upload_images/1939592-56b0e152270542ab.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)
    ](https://www.jianshu.com/p/9bae5aefe1b9)
-   CI/CD 应该是大部分公司进行迭代开发的模式。个人理解就是系统规范流程化地去进行迭代开发，而中间又涉及到多个部分，...
       [![](https://upload-images.jianshu.io/upload_images/16219863-963b8e11e358af90.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)
       ](https://www.jianshu.com/p/521dd58c2567) 
    [https://www.jianshu.com/p/a2798b1ac8a4](https://www.jianshu.com/p/a2798b1ac8a4) 
    [https://www.jianshu.com/p/a2798b1ac8a4](https://www.jianshu.com/p/a2798b1ac8a4)
