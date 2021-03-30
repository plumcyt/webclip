# 【推荐收藏】倾心整理的Python量化资源大合集 - 知乎
**01 引言**

本公众号原名为 “**CuteHand**”（智能助手），后来在 Python 和金融方面的文章分享多了，就更名为 “**Python 金融量化**”，致力于分享 Python 在金融量化领域的应用，尤其是股票量化投资。

随着 Python 编程语言的流行和普及，越来越多人对如何应用 Python 做金融数据分析和量化交易充满兴趣。但是不少人对量化投资本身存在一定的误解或认识不清，有的人过于异想天开，认为可以躺着挣钱（怕是只有岛国老师吧）；有的人则因循守旧，认为没啥卵用；也有的人盲目追求模型的复杂性，在编程和数学中迷失了方向。

简单理解，量化投资就是利用计算机科技并采用一定的数学模型去实现投资理念、实现投资策略的过程。所以量化投资只是一种工具，只是用数量化的方法去实践投资理念，交易的本质并没有发生变化。量化投资的优势在于提高了我们分析的广度和深度，通过历史回测获取概率优势，同时自动交易过程可以规避人性中的诸多弱点。随着大数据和人工智能的发展，量化投资将成为市场的主流投资工具，并且将与传统的基本面分析和技术分析深度结合。

那么量化投资应该如何系统的学习呢？网上关于 Python 和量化（Quant）的资源汗牛充栋，十分庞杂，让很多踏入这一领域的人不知所措。那些已经掌握了 Python 编程基础的人，却不知如何切入量化的实际场景；而那些具备一定金融基础和策略思路的人，却不知如何使用 Python 来实现策略。因此，本文主要结合个人经验和网上公开资料，为大家分享 Python 和量化投资的学习资源，由于精力和关注度有限，经供参考，欢迎大家补充和分享。

![](https://pic2.zhimg.com/v2-826956336cd7d6bf7ed7518fc4ebfa51_b.jpg)

**02 量化资源分享**

![](https://pic2.zhimg.com/v2-8e014651efeb574c8c3f19dbfe8b7c7d_b.jpg)

量化可以简单分为**数据管理**、**策略分析**和**策略执行**三个模块，数据是基础，策略分析是核心，其中策略自动化执行（算法交易）在国内由于政策限制实施起来比较麻烦。从 Python 的角度看，数据层往下分解，要学习的模块主要有 Pandas、Numpy、tushare、pandas_datareader 以及一些爬虫库等。策略层往下分解，要掌握的基础工具包括 Pandas、matplotlib、scikit-learn、TA-lib、statsmodels 等等。当然，在学习上述金融量化常用库前，系统的掌握 Python 编程基础是很有必要的。从策略的角度看，**光会玩 Python 是远远不够的，必须有自己的策略思路和逻辑**。那么策略的灵感来自哪里？除了自身实践总结外，各种量化平台、论坛、博客和学术期刊可能会提供一定的借鉴参考。下面将围绕 Python 编程、数据源、量化平台、策略来源等方面分享相关的学习资源。

**01Python 编程**

**搭建 Python 环境**

-   **Anaconda，**推荐使用：

[https://www.anaconda.com/](https://link.zhihu.com/?target=https%3A//www.anaconda.com/)

一直使用其自带的 Jupyter Notebook 来做策略分析和写公众号文章。

-   **Pycharm**，用的人也很多：

[https://www.jetbrains.com/pycharm/](https://link.zhihu.com/?target=https%3A//www.jetbrains.com/pycharm/)

**入门学习**

-   **廖雪峰官方网站**：

[https://www.liaoxuefeng.com/wiki/1016959663602400](https://link.zhihu.com/?target=https%3A//www.liaoxuefeng.com/wiki/1016959663602400)

-   **菜鸟教程**：

[https://www.runoob.com/python3/python3-basic-syntax.html](https://link.zhihu.com/?target=https%3A//www.runoob.com/python3/python3-basic-syntax.html)

-   **GitHub 项目**：

[https://github.com/goodchinas/pyquant](https://link.zhihu.com/?target=https%3A//github.com/goodchinas/pyquant)

分享 notebook 格式小项目，从入门到 numpy、pandas、matplotlib 等各种库的讲解和练习，非常适合新手入门。

**高阶学习书籍**

-   **Python for Finance**，2014,Yves Hilpisch 中文版：Python 金融大数据分析，人民邮电出版社
-   **Mastering Python for Finance**，2015，James Ma Weiming
-   **Personal Finance with Python**，2018，Max Humber
-   **Python for Finance**，2017，Yuxing Yan
-   **Derivatives Analytics with Python**，2015，Yves Hilpisch
-   **QuantEcon Lectures**，2019，Thomas J. Sargent and John Stachurski
-   **量化投资以 Python 为工具**，2017，蔡立耑
-   **零起点 Python 大数据与量化交易**，2017，何海群
-   **量化交易之路用 Python 做股票量化分析**，2017，阿布

**02 量化数据源**

金融量化数据源主要有三种：一是大数据网站，一般只有日线级数据；二是专业金融数据公司，如通联和万德，收费价格高但数据齐全且比较稳定；三是开源数据模块库，如 Tushare，pandas-datareader，ccxt 数字货币等，github 上还有很多不一一列举。

**Python 开源数据**

-   **TuShare pro**，中文财经数据接口包，有积分限制。  
    [https://tushare.pro/](https://link.zhihu.com/?target=https%3A//tushare.pro/)
-   **BaoStock**，与 tushare 类似，主要提供国内股票行情数据、公司基本面和宏观数据：[http://baostock.com/](https://link.zhihu.com/?target=http%3A//baostock.com/)
-   **Quandl** :[https://www.quandl.com/](https://link.zhihu.com/?target=https%3A//www.quandl.com/)

国际金融和经济数据。

-   **pandas_datareader**：[https://pandas-datareader.readthedocs.io/en/latest/](https://link.zhihu.com/?target=https%3A//pandas-datareader.readthedocs.io/en/latest/)

从 pandas 中独立出来的数据开源库，丰富的数据源，包括美股、A 股、宏观数据等。

-   **yfinance**：[https://pypi.org/project/yfinance/](https://link.zhihu.com/?target=https%3A//pypi.org/project/yfinance/)

雅虎财经数据 api 的修复。

-   **ccxt**：[https://github.com/ccxt/ccxt](https://link.zhihu.com/?target=https%3A//github.com/ccxt/ccxt)  
    python 数字货币开源接口

**其他数据源**

-   通达信 （免费）
-   聚宽：jqdatasdk（免费）
-   新浪、雅虎、东方财富网（免费）
-   Wind 资讯 - 经济数据库（收费）
-   东方财富 Choice 金融终端（收费）
-   同花顺金融数据终端 （可免费导出）

**03 在线量化平台和开源框架**

平台之间大同小异，可以重点关注各大平台的策略大赛（练手）、社区（借鉴参考优秀项目）和学院（系统学习量化知识框架）板块。  

**国内平台（排名不分先后）：** 

-   **BigQuant** ：[https://bigquant.com/](https://link.zhihu.com/?target=https%3A//bigquant.com/)

人工智能量化平台，社区和学院提供了较丰富的资源。

-   **聚宽** ：[https://www.joinquant.com/](https://link.zhihu.com/?target=https%3A//www.joinquant.com/)

免费量化数据、投研工具、量化学习体系

-   **优矿** ：[https://uqer.io/](https://link.zhihu.com/?target=https%3A//uqer.io/)

特色是深度报告、量化学堂和量化社区

-   **万矿** ：[https://www.windquant.com/](https://link.zhihu.com/?target=https%3A//www.windquant.com/)

金融大数据、策略研究和数据可视化

-   **Ricequant** ：[https://www.ricequant.com/welcome/](https://link.zhihu.com/?target=https%3A//www.ricequant.com/welcome/)

涵盖金融数据、投资组合管理与风险分析、量化投研交易模块

-   **掘金量化** ：[https://www.myquant.cn/](https://link.zhihu.com/?target=https%3A//www.myquant.cn/)
-   **Factors** ：[http://factors.chinascope.com/](https://link.zhihu.com/?target=http%3A//factors.chinascope.com/)

专注于多因子分析，界面操作，黑盒子。

**国外量化平台：** 

国外量化平台非常多，这里只推荐两个。

-   **Quantopian** ：[https://www.quantopian.com/posts](https://link.zhihu.com/?target=https%3A//www.quantopian.com/posts)

比较知名的平台，旗下有量化三大件：pyFolio，zipline，alphalens

-   **Quantstart**：[https://www.quantstart.com/](https://link.zhihu.com/?target=https%3A//www.quantstart.com/)

平台文章提供了构建自己量化交易系统的思路框架

**开源框架（实现本地化）：** 

一般是直接在终端（cmd）上使用 pip install xxx(库名）进行安装，有些可能需要下载安装包离线安装。

-   **Zipline** - 回测框架
-   **vnpy** - python 开源开发框架
-   **easytrader** - 自动程序化股票交易
-   **pyalgotrade** - 基于事件驱动回测框架
-   **quantmod** - 量化金融建模
-   **backtrader** - 量化回测框架

**04 策略来源**

**量化投资专业网站、博客、论坛**

-   **ARQ**：[https://www.aqr.com/](https://link.zhihu.com/?target=https%3A//www.aqr.com/)
-   **Quantivity**：[https://quantivity.wordpress.com/page/2/](https://link.zhihu.com/?target=https%3A//quantivity.wordpress.com/page/2/)
-   **QuantLib**:[http://www.implementingquantlib.com/](https://link.zhihu.com/?target=http%3A//www.implementingquantlib.com/)
-   **NuclearPhynance**: [http://www.nuclearphynance.com/](https://link.zhihu.com/?target=http%3A//www.nuclearphynance.com/)
-   **QuantNet Community**：  
    [https://quantnet.com/](https://link.zhihu.com/?target=https%3A//quantnet.com/)
-   **Udacity** ：  
    [https://www.udacity.com/course/machine-learning-for-trading--ud501](https://link.zhihu.com/?target=https%3A//www.udacity.com/course/machine-learning-for-trading--ud501)
-   **Quant At Risk** ：  
    [http://www.quantatrisk.com/](https://link.zhihu.com/?target=http%3A//www.quantatrisk.com/)
-   **经管之家**：  
    [https://bbs.pinggu.org/forum-2166-1.html](https://link.zhihu.com/?target=https%3A//bbs.pinggu.org/forum-2166-1.html)
-   **知乎 - 宽客**：  
    [https://bbs.pinggu.org/forum-2166-1.html](https://link.zhihu.com/?target=https%3A//bbs.pinggu.org/forum-2166-1.html)
-   **知乎 - 量化：**  
    [https://www.zhihu.com/topic/19815465/hot](https://www.zhihu.com/topic/19815465/hot)
-   **GitHub** : [https://github.com/](https://link.zhihu.com/?target=https%3A//github.com/)
-   **FMZ 发明者量化交易平台**: [https://www.fmz.com/bbs](https://link.zhihu.com/?target=https%3A//www.fmz.com/bbs)

**量化投资书籍**

如果完全不懂金融投资理论，就谈量化投资，很容易流于形式，画出来漂亮的图表和策略，也就能忽悠一下外行而已。一直强调 Python 只是工具，不要舍本逐末，量化投资核心是策略和思路，而策略的来源需要一定的统计和投资学的积累与沉淀。

-   曼昆的**宏微观经济学**、米什金的《**货币金融学**》、罗斯《**公司理财**》、博迪《**投资学**》、《**金融工程**》、索罗斯《**金融炼金术**》）
-   《**计量经济学导论：现代观点**》

主要学习时间序列分析、多元统计线性回归，可结合 Python 的 statsmodels、scipy、sklearn 模块进行学习。

-   **多因子模型**：基础好的话可以阅读砝码三因子的 PAPER。

此外，Barra 风险模型（多因子模型扩展）是现在非常主流的量化模型，有很多可以参考的资料，如《**Barra Risk Model Handbook（US）**》。

**投资相关书籍**

-   《打开量化投资的黑箱》 里什 · 纳兰
-   《宽客》\[美] 斯科特 · 帕特森
-   《解读量化投资：西蒙斯用公式打败市场的故事》忻海
-   《漫步华尔街》麦基尔
-   《海龟交易法则》柯蒂斯 · 费思
-   《交易策略评估与最佳化》罗伯特 · 帕多
-   《统计套利》 安德鲁 · 波尔
-   《信号与噪声》纳特 • 西尔弗
-   《量化投资—策略与技术》丁鹏
-   《量化投资策略: 如何实现超额收益 Alpha》吴冲锋
-   《以交易为生》埃尔德
-   《高级技术分析》布鲁斯 · 巴布科克
-   《积极型投资组合管理》格里纳德，卡恩
-   《金融计量学: 从初级到高级建模技术》斯维特洛扎
-   《量化交易如何建立自己的算法交易事业》欧内斯特 · 陈
-   《聪明的投资者》 本杰明 · 格雷厄姆
-   《期权、期货和其他衍生品》 约翰 · 赫尔

注：金融投资书籍及金融史，可在公众号里分别回复 “**金融书籍**”、“金**融历史**” 获取。

**学术期刊**

金融三大顶级期刊：

-   Journal of Finance、
-   Journal of Financial Economics、
-   Review of Financial Studies

其他金融投资期刊：

-   Journal of Accounting and Economics、Journal of Financial and Quantitative Analysis、Financial Analysts 、Journal Financial Management、Journal of Empirical Finance、Quantitative Finance、Journal of Alternative Investments、Journal of Fixed Income、Journal of Investing、Journal of Portfolio Management、Journal of Trading、Review of Asset Pricing Studies

**国内期刊：** 国内专门讨论量化投资的学术文章比较少，可关注**经济研究、经济学（季刊）、金融研究、管理世界、会计研究、投资研究**。

**微信公众号资源**

微信公众号也提供了很多量化资源可供学习参考，下面分享几个自己关注的，排名不分先后。  

![](https://pic2.zhimg.com/v2-8c6f09eb013e81658e66224266d64025_b.jpg)

**03 结语**

**量化只是一种工具或手段，量化投资则是目前比较流行的一种交易分析框架，需要掌握的知识体系还真比较庞杂**。值得关注的是，证券投资的专业性远没有工科类的专业性那么可靠。比如说，你拥有制造汽车的专业技能，就能造出一部汽车；但当你掌握了证券投资的专业知识，却不一定能在股票市场上赚到钱。一个不难观察到的现象是，很多金融专业人士在股票投资这个领域不一定能干出好的成绩，而一些非金融背景的人却表现优异。这可能与证券投资的非逻辑性（艺术而不是科学）有关，交易市场的本质是零和博弈（不考虑手续费等），尤其是期货交易。但是不得不说这已经是个对专业要求越来越高的行当，因为在高收入的引诱下，各种高精尖人才都挤破脑袋往这个行业里钻。尤其是近几年，大型券商和基金招人连清北复交也不怎么鸟了，指定北美前五十高校，玩的模型和技术也越来越花了。**尽管专业的金融背景只是投资成功的必要条件（非充分条件），但是如果连基本的经济金融基础也没有，要想与市场上的其他人玩，成为韭菜的概率就更高了。** 

**参考资料**

**知乎**：守株待兔《_史上最全 Quant 资源整理_》，原始出处已无从考证：

[https://zhuanlan.zhihu.com/p/26179943](https://zhuanlan.zhihu.com/p/26179943)

**关于 Python 金融量化**

专注于分享 Python 在金融量化领域的应用。加入知识星球，可以免费获取 30 多 g 的量化投资视频资料、量化金融相关 PDF 资料、公众号文章 Python 完整源码、量化投资前沿分析框架，与博主直接交流、结识圈内朋友等。 
 [https://zhuanlan.zhihu.com/p/90933955](https://zhuanlan.zhihu.com/p/90933955)
