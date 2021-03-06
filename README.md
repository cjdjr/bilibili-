

# bilibili番剧分析

**基于Bilibili番剧爬虫实现简单的番剧推荐系统和数据可视化分析**



##介绍

该项目具体实现了针对b站番剧信息的网络爬虫，抓取了2015~2017年所有番剧的名称、播放量、追番人数、总弹幕数、标签和评分，并将其写入本地的数据库中。然后基于Flask框架实现了一个简单的番剧推荐系统，用户写入自己感兴趣的番剧标签，系统从数据库中找出标签对应的番剧显示给用户。该项目还根据爬取的数据，基于pyecharts模块，进行数据的可视化形成图表，作出一些分析。



## 所用到的模块

* bequests
* bs4
* json
* time
* random
* sqlite3
* flask
* pyecharts



## 程序清单

* **app.py** 推荐系统的入口程序
* **spider.py** 爬虫程序
* **recommend.py** 推荐算法程序
* **data_to_chart.py** 将数据转成图表的程序
* **api分析.txt** 分析b站番剧索引的api参数
* **“图表”文件夹** 存放数据转成的图表
* **“template”文件夹** 装有生成推荐系统页面的模板网页
* **2015.db** 2015年b站番剧信息
* **2016.db** 2016年b站番剧信息
* **2017.db** 2017年b站番剧信息



## 推荐系统

### 推荐算法的编写

推荐算法在**recommend.py**中实现，调用其中的**recommend**函数，输入用户感兴趣的动漫标签组成的字符串，函数返回用户可能感兴趣的动漫信息。

首先算法将标签字符串按空白符分割，将输入的标签单独提取出来放入一个列表里。然后循环遍历3个数据库中的所有番剧，考虑番剧的标签和输入的这些标签的相似程度。

* 若用户输入的标签只有1个，那么算法按照播放数从高到低选取5个含有此标签的番剧推荐给用户；
* 若用户输入的标签超过1个，设其为x个。那么若一个番剧的所有标签中，有大于等于x-1个标签和输入的标签相同，那么该番剧被定义为“用户可能感兴趣的番剧”。将这些用户可能感兴趣的番剧按照播放数从高到低选取5个推荐给用户。

### 推荐系统的实现及部署

基于Flask框架的推荐系统简单网页的编写在app.py中实现。其在本地的127.0.0.1:5000生成一个推荐系统网页。

![番剧搜索界面](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/番剧搜索界面.png)

![系统推荐的番剧](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/系统推荐的番剧.png)



## 数据分析

利用pyecharts模块进行数据可视化，进行一些分析。Pyecharts是将数据生成html格式的文件，以下所有html文件均保存在“图表”文件夹中。生成这些图表的代码在data_to_chart.py中实现。



### 2015~2017三年的b站番剧总数的对比

![bilibili年度番剧总数](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili年度番剧总数.png)

从表中可以看出，b站每年的番剧量平均大约是200部，每个季度50部，相比较于爱奇艺和优酷土豆，b站对动漫番剧区还是大力支持的。

但是从整体趋势来看，2017年的番剧总数相较于2015 2016年呈下降趋势。通过上网搜索爱奇艺、优酷土豆等视频网站这三年对番剧引进的数据来看，是因为2017年其它平台引进了更多的“付费独播”的番剧，bilibili的资金无法和其它更大的视频平台相比，就导致bilibili2017年引进的番剧变少。



### 2015~2017三年的b站番剧播放、追番数据对比

![bilibili番剧平均评分](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili番剧平均评分.png)

![bilibili番剧平均播放量](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili番剧平均播放量.png)

![bilibili番剧平均弹幕量](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili番剧平均弹幕量.png)

![bilibili番剧平均追番人次i](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili番剧平均追番人次.png)

通过统计信息可知，bilibili近3年的番剧平均评分大致相同（无评分的番剧没有纳入统计）。平均弹幕量和平均播放量的变化趋势基本相同，都是稳步增长。但是平均追番人次有着明显的差距，这三年间平均追番人次上涨得很快，2017年每部番的平均追番人次是2015年的两倍，这说明越来越多bilibili番剧区的用户覆盖范围越来越广。



### 2015~2017三年的b站热门番剧类型



![bilibili热门类型番剧平均追番人数](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili热门类型番剧平均追番人数.png)

从统计数据可以看出，恋爱、后宫、热血、搞笑、日常等类型的番剧是用户们最喜爱的类型。



![bilibili热门类型番剧总数](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili热门类型番剧总数.png)

从统计数据可以看出，b站最多的番是热血、奇幻、战斗、日常等类型的番剧，基本迎合用户们喜爱的口味，值得注意的是用户们十分喜爱的恋爱和后宫类型的番剧在b站中数量并不多。



![bilibili热门类型番剧平均评分](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili热门类型番剧平均评分.png)

从统计数据中可以看出，各个类型的番剧质量比较接近，但追番人数最多的后宫类型的番剧评分却比较低，主要因为这类类型的番剧大多是轻改的动漫，有一个成型的模板，故事的剧情和人物形象都很模板化，虽然用户喜欢看，但也认为这些作品没有什么亮点，评分自然也就上不去。



### 2015~2017三年的b站评分系统分析



![bilibili番剧评分分布](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili番剧评分分布.png)

![bilibili番剧评分分布(饼图)](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili番剧评分分布(饼图).png)

从评分的分布中可以看出，大多数用户对番剧的评分都很高，大部分番剧的评分都在8.0甚至9.0以上，这和豆瓣之类的评分网站相比，可以说评分都不是很严苛。所以我们可以初步认为那些评分<8.0分的番是用户们公认的“差评番”。

既然没有多少番剧的评分低于8分，那我们不妨来看下之前的那些类型的番剧中，哪些类型的番剧评分低于8分的数量最多。



![bilibili番剧评分低于8.0分的比例统计](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/bilibili番剧评分低于8.0分的比例统计.png)

从数据中可以看窗户，差评率最多的番剧类型是泡面和科幻类型的番。泡面番因其时间过于段的原因，用户给其评分时，一般不会给比较高的评分。科幻类型的番剧容易高开低走，在开始的时候营造一个很庞大的世界观，但编剧无法驾驭其应有的剧情，结局就草草收尾，导致用户对其印象很差，评分自然不高。相比之下治愈、恋爱、励志这些描述生活和业界的番剧，引起了很多用户的共鸣，评分用户对它们的印象也就很好。



我们来看一看这3年的番剧中评分最高的TOP5和最低的TOP5(评分相同按追番人数排序)：

* **2015年**

![2015年bilibili番剧评分最高TOP5](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/2015年bilibili番剧评分最高TOP5.png)

![2015年bilibili番剧评分最低TOP5](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/2015年bilibili番剧评分最低TOP5.png)

* **2016年**

![2016年bilibili番剧评分最高TOP5](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/2016年bilibili番剧评分最高TOP5.png)

![2016年bilibili番剧评分最低TOP5](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/2016年bilibili番剧评分最低TOP5.png)

* **2017年**

![2017年bilibili番剧评分最高TOP5](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/2017年bilibili番剧评分最高TOP5.png)

![2017年bilibili番剧评分最低TOP5](https://github.com/cjdjr/bilibili-anime-analysis/blob/master/images/2017年bilibili番剧评分最低TOP5.png)



从数据中可以看出，在评分最高的TOP5中，有一部分是播放量、追番量都很高的热门番，也有一部分是小众番。但在评分最低TOP5中，大部分番剧都是播放量不低的热门番，但评分却是低得惊人，仔细翻了一翻这些评分很低的番剧的评论，比如《迷家》，发现主要是由于剧情的混乱和故弄玄虚引起了用户两种截然不同的看法，一些觉得该作很烂的用户大肆舆论渲染，使得一些人跟风评分。
