# 使用 Scrapy 爬取豆瓣数据





## 环境搭建

###  安装 scrapy

``` powershell
> pip install scrapy
```

### 使用 crawl 模板生成 spider 文件

``` powershell
> scrapy startproject doubanSpider
> cd doubanSpider
> scrapy genspider -t crawl douban douban.com
```

### 使用 scrapy shell 测试

出现报错消息：No module named 'win32api'

解决方法：pip install pypiwin32

#### 调试

爬取豆瓣时需要设置 header 否则爬不到;

通过 chrome 查看请求的 header 找出 USER_AGENT 填在下面

```powershell
scrapy shell -s USER_AGENT="Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.80 Safari/537.36" https://book.douban.com/tag/%E5%B0%8F%E8%AF%B4
```

爬取成功！



### 分析需要爬取的内容

![1560342866285](C:\Users\15651\AppData\Roaming\Typora\typora-user-images\1560342866285.png)

格式比较清晰

title：

```python
response.xpath('//*[@class="subject-item"]/*/h2/a/text()').extract()
```

[PyCharm调试Scrapy项目方法](<https://www.cnblogs.com/lsdb/p/9122970.html>)

再使用 pip install scrapy 一下，可以找到之前的安装目录



运行时

**注意**：

要`item = DoubanspiderItem()` -> 初始化 item

同时设定正确的Rule -> 跟进规则



### 爬取详情

```python
response.xpath('//*[@class="intro"]').xpath('string(.)').extract()
```





### 爬虫爬取过程

1. 从详情页爬取一定的书籍信息，获取豆瓣对书籍的编号 id
2. 根据书籍 id 和链接结构，爬取 书籍详情 和 评论详情，同时记录下评论的用户 id
3. 根据用户 id 爬取用户信息
4. 爬取书籍封面
5. 爬取用户头像



| 爬取次数 | 爬取链接 | 内容 |
|---- | ------------------------------------------ | -------------- |
| 列表导航 | book.douban.com/tag/[类别]                 | 图书列表页     |
|1    | book.douban.com/subject/[书籍id]           | 图书详情页     |
|2    | book.douban.com/subject/[书籍id]/comments/ | 图书相关的评论 |
|3    | www.douban.com/people/[用户id]             | 用户详情页     |

<https://blog.csdn.net/joe8910/article/details/85159059>



提前终止爬虫<https://blog.csdn.net/Mrzhang0419/article/details/80253692>

