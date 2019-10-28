---
title: 爬虫demo
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-23 18:37:01
password:
summary:
copyright: true
tags:
  - python
  - spider
categories:
  - python
  - spider
---

### 爬虫基础 

-  模拟客户端（浏览器）发送网络请求，获取响应，按照规则提取数据的程序
- 模拟客户端（浏览器）发送网络请求：照着浏览器发送同样的请求，获取和浏览器同样的数据
- 爬虫数据去向
  - 网页或者`APP`上的呈现
  - 寻找规律，进行其他的分析等

<!--MORE-->

### 浏览器请求

- url
  - 在`Chrome`中点击检查，点到network
  - `url`解码工具进行解码
  - `url` = 请求的协议(`http、https`) + 网站的域名(www.baidu.com) + 资源路径 + 参数

- 浏览器请求`url`地址
  - 当前`url`对应的响应 + `js/css` + 图片------>elements中的内容
- 爬虫请求`url`地址
  - 当前`url`对应的响应
  - `elements`的内容和爬虫获取的`url`地址的响应不同
  - 爬虫中需要以当前的`url`地址对应的响应的数据为准
  - 当前`url`地址对应的响应在response

### HTTP/HTPPS

#### HTTP

超文本传输协议以明文的形式传输效率高，不安全

#### HTTPS

`HTTP + SSL`；其中`SSL`指的是安全套接字层，传输数据之前先进行加密，之后解密再获取内容
效率低，但是安全

### get和post请求的区别

- get请求无请求体；post请求有；请求体就是指携带的数据
- get请求把数据放在url地址中；
- post请求常用于登录注册；post请求携带大量数据，常用于传输大文本

### HTTP协议之请求

- 请求行
- 请求头
  user-agent:用户代理，对方服务器能够知道当前是什么浏览器进行资源的访问
  如果需要使用手机版的浏览器进行访问，把user-agent改成对应的手机版浏览器
- cookie：用来存储用户信息，每次请求会被带上发送给对方的浏览器
      （比如登录JD之后保存了cookie，下次直接登录）
  - 要获取登录之后才能访问的页面
  - 对方的服务器会根据cookie来判断是不是爬虫
- 浏览器
  - 获取登录之后才能访问的页面
  - 服务器会通过cookie来判断是不是爬虫
- 请求体
  - 携带数据就是请求体
  - get请求无请求体，post有

- http响应
  - 响应头
    set-cookie:对方服务器通过该字段设置cookie到本地
  - 响应体
    url地址对应的响应
- 模拟浏览器:带上headers即可

### demo

```python
# daouban

from parse import parse_url
import json

"""
    找出最初的url地址
    发送请求，获取响应
    提取数据
    保存数据
    构造下一页，循环上面的步骤
    
"""
class Doubanspider:
    
    def __init__(self):   # 初始化方法
        self.temp_url = "https://m.douban.com/rexxar/api/v2/subject_collection/tv_american/items?os=ios&for_mobile=1&start={}&count=18&loc_id=108288&_=0"
        
    def get_content_list(self, html_str):   # 提取数据
        dict_data = json.loads(html_str)
        content_list = dict_data["subject_collection_items"]
        total = dict_data["total"]
        return content_list
    
    def save_content_list(self,content_list):
        with open("douban.json", "a", encoding="utf-8") as f:   # a模式表示追加
            for content in content_list:
                f.write(json.dumps(content, ensure_ascii=False))
                f.write("\n")
                
        print("保存成功")
    
    def run(self):  # 实现主要逻辑
        num = 0
        total = 100
        while num < total+18:
            # 1、start_url
            url = self.temp_url.format(num)
            print(url)
            # 2、发送请求获取响应
            html_str = parse_url(url)
            # 3、提取数据
            content_list = self.get_content_list(html_str)
            # 4、保存
            self.save_content_list(content_list)
            # 5、构造下一页的地址，循环2-5步
            num += 18
     
            
if __name__ == "__main__":
    douban = Doubanspider()
    douban.run()

```

```python
# qiubai

import requests
import json
from lxml import etree

class QiushiSpider:
    
    # 初始化url和headers
    def __init__(self):
        self.start_url = "https://www.qiushibaike.com/8hr/page/{}/"
        self.headers = {"User-Agent": "  "}
   # 构造url_list
    def get_url_list(self):
       url_list = [self.start_url.format(i) for i in range(1,14)]
       return url_list
   
   # 发送请求，获取响应
    def parse_url(self, url):
        print("parsing", url)
        response = requests.get(url, headers=self.headers)
        return response.content.decode("utf-8", "ignore")
   
    def get_content_list(self, html_str):
        # 先用etree模块的HTML方法
        html = etree.HTML(html_str)
        div_list = html.xpath("//div[@id='content']/div/div[2]/div[@class='recommend-article']")
        # print(div_list)
        content_list = []
        for div in div_list:
            item = {}
            item["author_name"] = div.xpath(".//a[@class='recmd-user']/span/text()")[0].strip() if len(div.xpath(".//a[@class='recmd-user']/span/text()")) > 0 else None
            item["content"] = div.xpath(".//a[@class='recmd-content']/text()")
    
            # content是类表不能用strip；但是可以对列表中的每个元素进行strip
            item["content"] = [i.strip() for i in item["content"]]
            item["stats_vote"] = div.xpath(".//div[@class='recmd-num']/span[1]/text()")
            item["stats_vote"] = item["stats_vote"][0] if len(item["stats_vote"]) > 0 else None
            item["stats_comments"] = div.xpath(".//div[@class='recmd-num']/span[4]/text()")
            item["stats_comments"] = item["stats_comments"][0] if len(item["stats_comments"]) > 0 else None
            item["img"] = div.xpath(".//a[@class='recmd-user']//img/@src")
            item["img"] = "https:" + item["img"][0] if len(item["img"]) > 0 else None
            
            content_list.append(item)
        return content_list
   
    def save_content_list(self, content_list):
        with open("qiushibaike.txt", "a", encoding="utf-8") as f:
            for content in content_list:
                f.write(json.dumps(content, ensure_ascii=False))
                f.write("\n")
        print("保存成功")
    
    def main(self):
        # 根据url的规律，构造url_list
        url_list = self.get_url_list()
        for url in url_list:
            # 发送请求，获取响应
            html_str = self.parse_url(url)
            # 提取数据
            content_list = self.get_content_list(html_str)
            # 保存数据
            self.save_content_list(content_list)
            
            
if __name__ == '__main__':
    qiubai = QiushiSpider()
    qiubai.main()
```



```python
# maoyan

import json
from lxml import etree
import requests
import xlwt

"""
    通过建立一个类，多个函数 实现
    代码实现通过lxml和xpath对猫眼电影top100的爬取
    保存成TXT和Excel表格中
"""
class MaoyanSpider:
    
    # 初始化url和headers
    def __init__(self):
        self.start_url = 'https://maoyan.com/board/4?offset={}'
        self.headers = {'User-Agent': '  '}
        
    
    # 根据url的规律，构造url_list
    def get_url_list(self):
        url_list = [self.start_url.format(10*i) for i in range(10)]
        return url_list
      
    # 发送请求，获取响应
    def parse_url(self, url):
        print("parsing...", url)
        response = requests.get(url=url, headers=self.headers)
        return response.content.decode('utf-8', 'ignore')
    
    # 获取数据
    def get_content_list(self, html_str):
        html = etree.HTML(html_str)
        div_list = html.xpath('//*[@id="app"]/div/div/div[1]/dl/dd')
        content_list = []
        for div in div_list:
                item = {'film_name': '', 'film_actor': '', 'film_time': ''}
                item["film_name"] = div.xpath('.//div/div/div[1]/p[1]/a/text()')[0].strip('[]')
                item["film_actor"] = div.xpath('.//div/div/div[1]/p[2]/text()')[0].replace('\n ', '').strip().split("主演：")[1]    # 取出来的数据先去掉换行再去掉空格最后去掉主演：
                item["film_time"] = div.xpath('.//div/div/div[1]/p[3]/text()')[0].strip('[]').split("上映时间：")[1]
                content_list.append(item)
                
        return content_list

    # 保存数据
    def save_content_list(self, content_list):
        with open("maoyan2.txt", "a", encoding="utf-8") as f :
            for content in content_list:
                f.write(json.dumps(content, ensure_ascii=False))
                f.write("\n")
                
        print("保存成功")
        
    # 数据保存到Excel中，使用xlwt（用于写入Excel中）
    def save_to_excel(self, content_list):
        workbook = xlwt.Workbook(encoding='utf-8')
        sheet = workbook.add_sheet('maoyan_film') # 设置表名
        head = ['电影名称','主演','上映时间']  # 设置表头
        for h in range(len(head)):
            sheet.write(0, h, head[h])

        length = len(content_list)
        for j in range(1,length+1):
            sheet.write(j,0,content_list[j-1]["film_name"])
            sheet.write(j,1,content_list[j-1]["film_actor"])
            sheet.write(j,2,content_list[j-1]["film_time"])

        workbook.save('./maoyan2.xls')
        print('写入Excel成功')
    
        
    def main(self):
        # 获得url_list
        url_list = self.get_url_list()
        # 在url_list中进行请求的发送，内容的获取以及保存数据
        content_lists = []
        for url in url_list:
            html_str = self.parse_url(url)
            content_list = self.get_content_list(html_str)
            # extend()将两个列表进行合并;append()是在末尾进行追加元素
            self.save_content_list(content_list)
            content_lists.extend(content_list)
        self.save_to_excel(content_lists)
                
if __name__ == '__main__':
    maoyan = MaoyanSpider()
    maoyan.main()
```

### re模块

#### compile()

```python
import re

"""
    常见的匹配模式：
        \w      匹配字母数字及下划线
        \W      匹配非字母数字及下划线
        \s      匹配任意空白字符，等价于[\t\n\r\f]
        \S      匹配任意非空字符
        \d      匹配任意数字，等价于[0-9]
        \D      匹配任意非数字
        \A      匹配字符串开始
        \Z      匹配字符串结束；如果存在换行，则匹配换行之前的字符串
        \z      匹配字符串开始
        \G      匹配最后完成匹配完成的位置
        \n      匹配换行符
        \t      匹配制表符，就是空格
        ^       匹配字符串的开头
        $       匹配字符串的末尾
        .       匹配任意的字符串，除了换行符;当re.DOTALL标记被指定的时候，匹配包含换行符的任意字符
        [...]   用来表示一组字符，单独列出
        [^...]  不包含在[]中的字符
        *       匹配0个或者多个的表达式
        +       匹配1个或者多个表达式
        ?       匹配0个或者1个由前面的正则表达式定义的片段，非贪婪模式
        {n}     精确匹配n个前面的字符
        {n,m}   匹配n-m次由前面的正则表达式定义的片段，贪婪模式
        a|b     匹配a或者b
        （）     匹配括号内的表达式，也表示一个组
"""

# compile模块主要是将正则字符串编译成正则对象，以便复用该匹配模式
content = '''Hello 1234567 World_This
 is a Regex Demo'''

pattern = re.compile("^Hello.*Demo$", re.S)
result = re.match(pattern, content)
# result = re.match("Hello.*Demo", content, re.S)
print(result)
```

#### findall()

```python
import re
import requests

url = "https://maoyan.com/board/4?offset=90"

response = requests.get(url=url)

html = response.content.decode()
# print(html)

# re.findall()
# 搜索字符串，以列表的形式返回所有的能匹配的子串
result_list = re.findall('<li.*?href=(.*?).*?singer="(.*?)">(.*?)</a>', html, re.S)
for result in result_list:
    print(result)
    print(result[0], result[1], result[2])
```

#### match()

match()函数：从字符串的第一个位置开始匹配；如果不是起始位置匹配成功的话，返回是None

```python
import re

# re.match(pattern, string, flags=0)  第一个是正则表达式  第二个是匹配的目标字符串 第三个匹配模式

# 最常规的匹配
content = "Hello 123 4567 World_This is a Regex Demo"
print(len(content))
result = re.match("^Hello\s\d\d\d\s\d{4}\s\w{10}.*Demo$", content)
print(result)
print(result.group())
print(result.span())


# 泛匹配
# 其中^表示开头，$表示结尾，.*表示任意字符
result1 = re.match("^Hello.*Demo$", content)
print(result)
print(result.group())
```

#### search()

```python
import re

content = "Extra stings Hello 1234567 World_This is a Regex Demo Extra stings"
# result = re.match('Hello.*?(\d+).*?Demo', content)    开始就匹配失败直接返回None
# 能用search就不用match
result = re.search('Hello.*?(\d+).*?Demo', content)
print(result)
```

