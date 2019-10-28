---
title: NBA之旅1-透视表参数详解
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-13 09:45:02
password:
summary:
copyright: true
tags:
  - NBA
  - pivot_table
  - pandas
  - 数据分析
categories:
  - pandas
  - 实战
---

> 数据是从网上获取的，关于一份NBA的数据

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```


```python
path = r"D:\Python\datalearning\利用Python进行数据分析\NBA_Data.csv"
```

<!--MORE-->

### 读取数据


```python
data = pd.read_csv(path, encoding='utf-8', names=[
    '球员姓名','赛季','胜负','对手名称','对手总得分','己方总得分',
    '己方名称','首发','上场时间','投篮命中率','投篮命中数','投篮出手数',
    '三分命中率','三分命中数','三分出手数','罚球命中率','罚球命中数','罚球次数',
    '总篮板数','前场篮板数','后场篮板数','助攻数','抢断数','盖帽数','失误数','犯规数','得分'
])
df = pd.DataFrame(data)
```

### 查看数据

- `head()`：前5行

- `tail()`：后5行


```python
df.head()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>球员姓名</th>
      <th>赛季</th>
      <th>胜负</th>
      <th>对手名称</th>
      <th>对手总得分</th>
      <th>己方总得分</th>
      <th>己方名称</th>
      <th>首发</th>
      <th>上场时间</th>
      <th>投篮命中率</th>
      <th>...</th>
      <th>罚球次数</th>
      <th>总篮板数</th>
      <th>前场篮板数</th>
      <th>后场篮板数</th>
      <th>助攻数</th>
      <th>抢断数</th>
      <th>盖帽数</th>
      <th>失误数</th>
      <th>犯规数</th>
      <th>得分</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>黄蜂</td>
      <td>115</td>
      <td>129</td>
      <td>湖人</td>
      <td>1</td>
      <td>32</td>
      <td>57.9</td>
      <td>...</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>9</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>27</td>
    </tr>
    <tr>
      <th>1</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>奇才</td>
      <td>106</td>
      <td>124</td>
      <td>湖人</td>
      <td>1</td>
      <td>34</td>
      <td>55.0</td>
      <td>...</td>
      <td>2</td>
      <td>7</td>
      <td>0</td>
      <td>7</td>
      <td>14</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>23</td>
    </tr>
    <tr>
      <th>2</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>国王</td>
      <td>106</td>
      <td>111</td>
      <td>湖人</td>
      <td>1</td>
      <td>35</td>
      <td>40.9</td>
      <td>...</td>
      <td>9</td>
      <td>11</td>
      <td>2</td>
      <td>9</td>
      <td>11</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>29</td>
    </tr>
    <tr>
      <th>3</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>篮网</td>
      <td>111</td>
      <td>106</td>
      <td>湖人</td>
      <td>1</td>
      <td>37</td>
      <td>32.0</td>
      <td>...</td>
      <td>12</td>
      <td>9</td>
      <td>3</td>
      <td>6</td>
      <td>14</td>
      <td>1</td>
      <td>1</td>
      <td>8</td>
      <td>3</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>尼克斯</td>
      <td>124</td>
      <td>123</td>
      <td>湖人</td>
      <td>1</td>
      <td>35</td>
      <td>42.3</td>
      <td>...</td>
      <td>13</td>
      <td>6</td>
      <td>2</td>
      <td>4</td>
      <td>8</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>33</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 27 columns</p>
</div>

### 修改列属性名称

- 将对手名称改成对手：`对手名称--->对手`，`球员姓名--->姓名`

- 同时修改多个属性，用字典的形式

```python
df.rename(columns={"对手名称": "对手", "球员姓名": "姓名"})
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>姓名</th>
      <th>赛季</th>
      <th>胜负</th>
      <th>对手</th>
      <th>对手总得分</th>
      <th>己方总得分</th>
      <th>己方名称</th>
      <th>首发</th>
      <th>上场时间</th>
      <th>投篮命中率</th>
      <th>...</th>
      <th>罚球次数</th>
      <th>总篮板数</th>
      <th>前场篮板数</th>
      <th>后场篮板数</th>
      <th>助攻数</th>
      <th>抢断数</th>
      <th>盖帽数</th>
      <th>失误数</th>
      <th>犯规数</th>
      <th>得分</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>黄蜂</td>
      <td>115</td>
      <td>129</td>
      <td>湖人</td>
      <td>1</td>
      <td>32</td>
      <td>57.9</td>
      <td>...</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>9</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>27</td>
    </tr>
    <tr>
      <th>1</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>奇才</td>
      <td>106</td>
      <td>124</td>
      <td>湖人</td>
      <td>1</td>
      <td>34</td>
      <td>55.0</td>
      <td>...</td>
      <td>2</td>
      <td>7</td>
      <td>0</td>
      <td>7</td>
      <td>14</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>23</td>
    </tr>
    <tr>
      <th>2</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>国王</td>
      <td>106</td>
      <td>111</td>
      <td>湖人</td>
      <td>1</td>
      <td>35</td>
      <td>40.9</td>
      <td>...</td>
      <td>9</td>
      <td>11</td>
      <td>2</td>
      <td>9</td>
      <td>11</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>29</td>
    </tr>
    <tr>
      <th>3</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>篮网</td>
      <td>111</td>
      <td>106</td>
      <td>湖人</td>
      <td>1</td>
      <td>37</td>
      <td>32.0</td>
      <td>...</td>
      <td>12</td>
      <td>9</td>
      <td>3</td>
      <td>6</td>
      <td>14</td>
      <td>1</td>
      <td>1</td>
      <td>8</td>
      <td>3</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>尼克斯</td>
      <td>124</td>
      <td>123</td>
      <td>湖人</td>
      <td>1</td>
      <td>35</td>
      <td>42.3</td>
      <td>...</td>
      <td>13</td>
      <td>6</td>
      <td>2</td>
      <td>4</td>
      <td>8</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>33</td>
    </tr>
    <tr>
      <th>5</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>猛龙</td>
      <td>111</td>
      <td>98</td>
      <td>湖人</td>
      <td>1</td>
      <td>32</td>
      <td>52.2</td>
      <td>...</td>
      <td>7</td>
      <td>4</td>
      <td>0</td>
      <td>4</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>29</td>
    </tr>
    <tr>
      <th>6</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>公牛</td>
      <td>107</td>
      <td>123</td>
      <td>湖人</td>
      <td>1</td>
      <td>33</td>
      <td>65.2</td>
      <td>...</td>
      <td>5</td>
      <td>10</td>
      <td>1</td>
      <td>9</td>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>36</td>
    </tr>
    <tr>
      <th>7</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>凯尔特</td>
      <td>120</td>
      <td>107</td>
      <td>湖人</td>
      <td>1</td>
      <td>28</td>
      <td>56.5</td>
      <td>...</td>
      <td>7</td>
      <td>10</td>
      <td>2</td>
      <td>8</td>
      <td>12</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>30</td>
    </tr>
    <tr>
      <th>8</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>掘金</td>
      <td>115</td>
      <td>99</td>
      <td>湖人</td>
      <td>1</td>
      <td>31</td>
      <td>59.1</td>
      <td>...</td>
      <td>8</td>
      <td>7</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>4</td>
      <td>31</td>
    </tr>
    <tr>
      <th>9</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>快船</td>
      <td>113</td>
      <td>105</td>
      <td>湖人</td>
      <td>1</td>
      <td>42</td>
      <td>50.0</td>
      <td>...</td>
      <td>12</td>
      <td>8</td>
      <td>0</td>
      <td>8</td>
      <td>6</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>27</td>
    </tr>
    <tr>
      <th>10</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>太阳</td>
      <td>118</td>
      <td>109</td>
      <td>湖人</td>
      <td>1</td>
      <td>41</td>
      <td>47.6</td>
      <td>...</td>
      <td>7</td>
      <td>9</td>
      <td>0</td>
      <td>9</td>
      <td>16</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>27</td>
    </tr>
    <tr>
      <th>11</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>雄鹿</td>
      <td>131</td>
      <td>120</td>
      <td>湖人</td>
      <td>1</td>
      <td>39</td>
      <td>52.6</td>
      <td>...</td>
      <td>10</td>
      <td>7</td>
      <td>2</td>
      <td>5</td>
      <td>10</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>31</td>
    </tr>
    <tr>
      <th>12</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>鹈鹕</td>
      <td>119</td>
      <td>125</td>
      <td>湖人</td>
      <td>1</td>
      <td>37</td>
      <td>54.2</td>
      <td>...</td>
      <td>8</td>
      <td>6</td>
      <td>1</td>
      <td>5</td>
      <td>10</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>33</td>
    </tr>
    <tr>
      <th>13</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>灰熊</td>
      <td>110</td>
      <td>105</td>
      <td>湖人</td>
      <td>1</td>
      <td>40</td>
      <td>34.8</td>
      <td>...</td>
      <td>10</td>
      <td>12</td>
      <td>3</td>
      <td>9</td>
      <td>11</td>
      <td>3</td>
      <td>0</td>
      <td>5</td>
      <td>3</td>
      <td>24</td>
    </tr>
    <tr>
      <th>14</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>鹈鹕</td>
      <td>128</td>
      <td>115</td>
      <td>湖人</td>
      <td>1</td>
      <td>39</td>
      <td>64.7</td>
      <td>...</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>7</td>
      <td>12</td>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>27</td>
    </tr>
    <tr>
      <th>15</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>火箭</td>
      <td>106</td>
      <td>111</td>
      <td>湖人</td>
      <td>1</td>
      <td>40</td>
      <td>47.8</td>
      <td>...</td>
      <td>10</td>
      <td>12</td>
      <td>4</td>
      <td>8</td>
      <td>6</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>29</td>
    </tr>
    <tr>
      <th>16</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>老鹰</td>
      <td>117</td>
      <td>113</td>
      <td>湖人</td>
      <td>1</td>
      <td>43</td>
      <td>40.0</td>
      <td>...</td>
      <td>11</td>
      <td>11</td>
      <td>2</td>
      <td>9</td>
      <td>16</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>28</td>
    </tr>
    <tr>
      <th>17</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>凯尔特</td>
      <td>128</td>
      <td>129</td>
      <td>湖人</td>
      <td>1</td>
      <td>38</td>
      <td>52.4</td>
      <td>...</td>
      <td>5</td>
      <td>12</td>
      <td>2</td>
      <td>10</td>
      <td>12</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>4</td>
      <td>28</td>
    </tr>
    <tr>
      <th>18</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>步行者</td>
      <td>136</td>
      <td>94</td>
      <td>湖人</td>
      <td>1</td>
      <td>30</td>
      <td>58.3</td>
      <td>...</td>
      <td>3</td>
      <td>7</td>
      <td>0</td>
      <td>7</td>
      <td>9</td>
      <td>1</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>18</td>
    </tr>
    <tr>
      <th>19</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>快船</td>
      <td>120</td>
      <td>123</td>
      <td>湖人</td>
      <td>1</td>
      <td>40</td>
      <td>40.9</td>
      <td>...</td>
      <td>7</td>
      <td>14</td>
      <td>1</td>
      <td>13</td>
      <td>9</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>24</td>
    </tr>
    <tr>
      <th>20</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>勇士</td>
      <td>101</td>
      <td>127</td>
      <td>湖人</td>
      <td>1</td>
      <td>21</td>
      <td>54.5</td>
      <td>...</td>
      <td>8</td>
      <td>13</td>
      <td>2</td>
      <td>11</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>17</td>
    </tr>
    <tr>
      <th>21</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>灰熊</td>
      <td>107</td>
      <td>99</td>
      <td>湖人</td>
      <td>1</td>
      <td>36</td>
      <td>57.1</td>
      <td>...</td>
      <td>4</td>
      <td>14</td>
      <td>1</td>
      <td>13</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>22</td>
    </tr>




----


### 查看某个属性的种类

查看和每个对手分别比赛了多少场次，降序排列


```python
df['对手名称'].value_counts()   # 默认降序
# df['对手名称'].value_counts(ascending=True)  改成升序
```


    公牛     56
    雄鹿     56
    步行者    56
    活塞     55
    老鹰     55
    魔术     55
    篮网     54
    猛龙     53
    尼克斯    53
    凯尔特    51
    奇才     50
    76人    44
    热火     38
    黄蜂     36
    山猫     35
    太阳     32
    马刺     31
    森林狼    31
    掘金     31
    灰熊     31
    开拓者    30
    火箭     30
    湖人     30
    快船     30
    爵士     30
    国王     28
    勇士     28
    小牛     26
    雷霆     19
    骑士     14
    鹈鹕     13
    超音速     8
    独行侠     4
    Name: 对手名称, dtype: int64

### 查看得分的最高和最低分


```python
print(max(df['得分']))
print(min(df['得分']))
```

    61
    3


###  查看胜负场数总和


```python
print((df["胜负"]=='胜').sum())
print((df["胜负"]=='负').sum())
```

    787
    406


### 得分柱状图


```python
# 解决中文乱码问题
plt.rcParams['font.sans-serif']=['SimHei']
plt.rcParams['font.family']='sans-serif'
plt.rcParams['axes.unicode_minus']=False

# 画布大小，防止横纵坐标重叠
plt.figure(figsize = (12, 7))

# 图标设置
plt.title("得分次数统计图")
# 横坐标标签
plt.xlabel('得分')
# 纵坐标标签
plt.ylabel('得分次数')

score = df['得分'].value_counts()
score.plot(kind='bar', rot=0)

# 图片保存
plt.savefig('./score.png', bbox_inches='tight')
plt.show()
plt.close()
```


![png](https://s2.ax1x.com/2019/10/13/ujgYmF.png)


### 对手得分前5名

通过`sort_values()`函数进行排序，`ascending=False`指定为降序


```python
df.sort_values(by='对手总得分', ascending=False).head(3)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>球员姓名</th>
      <th>赛季</th>
      <th>胜负</th>
      <th>对手名称</th>
      <th>对手总得分</th>
      <th>己方总得分</th>
      <th>己方名称</th>
      <th>首发</th>
      <th>上场时间</th>
      <th>投篮命中率</th>
      <th>...</th>
      <th>罚球次数</th>
      <th>总篮板数</th>
      <th>前场篮板数</th>
      <th>后场篮板数</th>
      <th>助攻数</th>
      <th>抢断数</th>
      <th>盖帽数</th>
      <th>失误数</th>
      <th>犯规数</th>
      <th>得分</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>89</th>
      <td>勒布朗-詹姆斯</td>
      <td>17-18</td>
      <td>负</td>
      <td>雷霆</td>
      <td>148</td>
      <td>124</td>
      <td>骑士</td>
      <td>1</td>
      <td>38</td>
      <td>47.1</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>18</td>
    </tr>
    <tr>
      <th>51</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>负</td>
      <td>马刺</td>
      <td>143</td>
      <td>142</td>
      <td>湖人</td>
      <td>1</td>
      <td>43</td>
      <td>44.0</td>
      <td>...</td>
      <td>11</td>
      <td>8</td>
      <td>2</td>
      <td>6</td>
      <td>14</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
      <td>32</td>
    </tr>
    <tr>
      <th>81</th>
      <td>勒布朗-詹姆斯</td>
      <td>17-18</td>
      <td>胜</td>
      <td>森林狼</td>
      <td>138</td>
      <td>140</td>
      <td>骑士</td>
      <td>1</td>
      <td>48</td>
      <td>72.7</td>
      <td>...</td>
      <td>2</td>
      <td>10</td>
      <td>1</td>
      <td>9</td>
      <td>15</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
      <td>37</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 27 columns</p>
</div>

### 查看数据的多种信息

- 总数
- 中位数
- 平均值
- 最小值
- ......


```python
df.describe()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

![](https://s2.ax1x.com/2019/10/13/uj27gx.png)

以上为部分数据截图

### 关于index使用

#### 两个属性的层次化索引

- "对手名称"是第一层索引

- "赛季"是第二层索引

**数据含义：同一对手在不同赛季下的各种数据**


```python
pd.pivot_table(df, index=['对手名称','赛季'])
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>三分出手数</th>
      <th>三分命中数</th>
      <th>三分命中率</th>
      <th>上场时间</th>
      <th>前场篮板数</th>
      <th>助攻数</th>
      <th>后场篮板数</th>
      <th>失误数</th>
      <th>对手总得分</th>
      <th>己方总得分</th>
      <th>...</th>
      <th>投篮出手数</th>
      <th>投篮命中数</th>
      <th>投篮命中率</th>
      <th>抢断数</th>
      <th>犯规数</th>
      <th>盖帽数</th>
      <th>罚球命中数</th>
      <th>罚球命中率</th>
      <th>罚球次数</th>
      <th>首发</th>
    </tr>
    <tr>
      <th>对手名称</th>
      <th>赛季</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="14" valign="top">76人</th>
      <th>03-04</th>
      <td>3.333333</td>
      <td>2.000000</td>
      <td>40.466667</td>
      <td>40.666667</td>
      <td>1.000000</td>
      <td>6.666667</td>
      <td>3.000000</td>
      <td>4.666667</td>
      <td>85.000000</td>
      <td>83.333333</td>
      <td>...</td>
      <td>18.666667</td>
      <td>9.666667</td>
      <td>49.800000</td>
      <td>1.666667</td>
      <td>1.333333</td>
      <td>1.000000</td>
      <td>3.333333</td>
      <td>68.533333</td>
      <td>5.333333</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>04-05</th>
      <td>3.666667</td>
      <td>0.666667</td>
      <td>11.100000</td>
      <td>44.666667</td>
      <td>1.666667</td>
      <td>7.333333</td>
      <td>7.666667</td>
      <td>5.000000</td>
      <td>98.000000</td>
      <td>89.333333</td>
      <td>...</td>
      <td>23.666667</td>
      <td>9.000000</td>
      <td>37.366667</td>
      <td>4.333333</td>
      <td>0.666667</td>
      <td>0.000000</td>
      <td>10.333333</td>
      <td>88.866667</td>
      <td>11.666667</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>05-06</th>
      <td>5.750000</td>
      <td>1.750000</td>
      <td>23.400000</td>
      <td>43.000000</td>
      <td>2.000000</td>
      <td>7.000000</td>
      <td>4.250000</td>
      <td>4.500000</td>
      <td>106.750000</td>
      <td>112.250000</td>
      <td>...</td>
      <td>21.750000</td>
      <td>12.500000</td>
      <td>57.600000</td>
      <td>2.250000</td>
      <td>2.000000</td>
      <td>0.750000</td>
      <td>7.500000</td>
      <td>76.950000</td>
      <td>9.750000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>06-07</th>
      <td>4.666667</td>
      <td>1.333333</td>
      <td>20.833333</td>
      <td>43.333333</td>
      <td>1.000000</td>
      <td>7.333333</td>
      <td>5.333333</td>
      <td>4.000000</td>
      <td>101.666667</td>
      <td>107.000000</td>
      <td>...</td>
      <td>20.333333</td>
      <td>10.333333</td>
      <td>50.166667</td>
      <td>0.333333</td>
      <td>3.000000</td>
      <td>0.333333</td>
      <td>5.333333</td>
      <td>58.566667</td>
      <td>9.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>07-08</th>
      <td>1.666667</td>
      <td>0.666667</td>
      <td>33.333333</td>
      <td>42.000000</td>
      <td>1.333333</td>
      <td>6.666667</td>
      <td>5.000000</td>
      <td>3.000000</td>
      <td>90.000000</td>
      <td>89.333333</td>
      <td>...</td>
      <td>18.000000</td>
      <td>10.333333</td>
      <td>57.433333</td>
      <td>1.666667</td>
      <td>2.666667</td>
      <td>1.000000</td>
      <td>3.333333</td>
      <td>83.333333</td>
      <td>5.333333</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>08-09</th>
      <td>4.666667</td>
      <td>1.333333</td>
      <td>27.766667</td>
      <td>37.666667</td>
      <td>1.666667</td>
      <td>7.333333</td>
      <td>2.666667</td>
      <td>0.666667</td>
      <td>85.666667</td>
      <td>97.000000</td>
      <td>...</td>
      <td>20.000000</td>
      <td>9.000000</td>
      <td>45.166667</td>
      <td>0.333333</td>
      <td>3.333333</td>
      <td>2.000000</td>
      <td>8.666667</td>
      <td>81.933333</td>
      <td>10.666667</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>09-10</th>
      <td>6.666667</td>
      <td>2.666667</td>
      <td>43.333333</td>
      <td>40.333333</td>
      <td>0.333333</td>
      <td>8.666667</td>
      <td>6.000000</td>
      <td>3.000000</td>
      <td>95.666667</td>
      <td>101.666667</td>
      <td>...</td>
      <td>21.333333</td>
      <td>10.666667</td>
      <td>50.033333</td>
      <td>1.333333</td>
      <td>2.333333</td>
      <td>1.333333</td>
      <td>6.333333</td>
      <td>66.166667</td>
      <td>9.666667</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>10-11</th>
      <td>3.666667</td>
      <td>0.666667</td>
      <td>15.000000</td>
      <td>40.000000</td>
      <td>1.000000</td>
      <td>5.000000</td>
      <td>7.000000</td>
      <td>5.000000</td>
      <td>92.000000</td>
      <td>102.333333</td>
      <td>...</td>
      <td>14.666667</td>
      <td>7.000000</td>
      <td>46.566667</td>
      <td>2.333333</td>
      <td>1.666667</td>
      <td>1.000000</td>
      <td>8.000000</td>
      <td>93.933333</td>
      <td>8.666667</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>11-12</th>
      <td>1.250000</td>
      <td>0.250000</td>
      <td>12.500000</td>
      <td>37.250000</td>
      <td>1.500000</td>
      <td>6.250000</td>
      <td>7.000000</td>
      <td>2.750000</td>
      <td>85.500000</td>
      <td>98.750000</td>
      <td>...</td>
      <td>19.500000</td>
      <td>11.750000</td>
      <td>60.150000</td>
      <td>2.250000</td>
      <td>0.750000</td>
      <td>2.000000</td>
      <td>5.500000</td>
      <td>72.350000</td>
      <td>7.750000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>12-13</th>
      <td>3.250000</td>
      <td>1.250000</td>
      <td>41.675000</td>
      <td>33.250000</td>
      <td>1.500000</td>
      <td>7.250000</td>
      <td>6.250000</td>
      <td>2.500000</td>
      <td>91.000000</td>
      <td>105.000000</td>
      <td>...</td>
      <td>16.250000</td>
      <td>9.250000</td>
      <td>57.450000</td>
      <td>1.500000</td>
      <td>1.250000</td>
      <td>0.750000</td>
      <td>4.000000</td>
      <td>73.325000</td>
      <td>5.250000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>13-14</th>
      <td>4.000000</td>
      <td>2.000000</td>
      <td>28.550000</td>
      <td>37.000000</td>
      <td>0.500000</td>
      <td>11.500000</td>
      <td>5.500000</td>
      <td>3.500000</td>
      <td>100.000000</td>
      <td>105.500000</td>
      <td>...</td>
      <td>15.000000</td>
      <td>8.000000</td>
      <td>53.350000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>0.500000</td>
      <td>5.000000</td>
      <td>81.250000</td>
      <td>6.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>14-15</th>
      <td>4.000000</td>
      <td>1.000000</td>
      <td>25.000000</td>
      <td>37.000000</td>
      <td>1.000000</td>
      <td>8.500000</td>
      <td>6.000000</td>
      <td>5.500000</td>
      <td>85.000000</td>
      <td>92.000000</td>
      <td>...</td>
      <td>20.500000</td>
      <td>7.000000</td>
      <td>34.300000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.500000</td>
      <td>4.000000</td>
      <td>66.650000</td>
      <td>6.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>15-16</th>
      <td>4.000000</td>
      <td>1.500000</td>
      <td>30.000000</td>
      <td>32.750000</td>
      <td>0.000000</td>
      <td>9.250000</td>
      <td>6.250000</td>
      <td>2.250000</td>
      <td>93.250000</td>
      <td>104.500000</td>
      <td>...</td>
      <td>20.000000</td>
      <td>11.500000</td>
      <td>57.225000</td>
      <td>2.750000</td>
      <td>1.250000</td>
      <td>0.750000</td>
      <td>3.750000</td>
      <td>77.100000</td>
      <td>5.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>16-17</th>
      <td>5.333333</td>
      <td>1.666667</td>
      <td>29.300000</td>
      <td>36.000000</td>
      <td>1.666667</td>
      <td>11.000000</td>
      <td>7.333333</td>
      <td>4.000000</td>
      <td>104.666667</td>
      <td>112.000000</td>
      <td>...</td>
      <td>21.333333</td>
      <td>10.666667</td>
      <td>50.033333</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>5.333333</td>
      <td>71.166667</td>
      <td>7.333333</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="16" valign="top">公牛</th>
      <th>03-04</th>
      <td>3.500000</td>
      <td>0.750000</td>
      <td>22.925000</td>
      <td>37.250000</td>
      <td>1.250000</td>
      <td>6.250000</td>
      <td>3.000000</td>
      <td>4.000000</td>
      <td>88.250000</td>
      <td>91.750000</td>
      <td>...</td>
      <td>19.750000</td>
      <td>7.500000</td>
      <td>37.925000</td>
      <td>1.750000</td>
      <td>1.250000</td>
      <td>2.500000</td>
      <td>5.500000</td>
      <td>71.875000</td>
      <td>7.500000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>04-05</th>
      <td>2.250000</td>
      <td>0.750000</td>
      <td>43.750000</td>
      <td>41.750000</td>
      <td>1.500000</td>
      <td>7.750000</td>
      <td>3.750000</td>
      <td>3.000000</td>
      <td>95.000000</td>
      <td>92.750000</td>
      <td>...</td>
      <td>19.250000</td>
      <td>8.500000</td>
      <td>45.200000</td>
      <td>2.750000</td>
      <td>2.250000</td>
      <td>0.750000</td>
      <td>6.750000</td>
      <td>65.550000</td>
      <td>10.500000</td>
      <td>1.0</td>
    </tr>



<b>limit_output extension: Maximum message size of 10000 exceeded with 34500 characters</b>


#### 交换两个属性的位置

数据显示的是在同一个赛季的情况下，不同的对手的数据情况


```python
pd.pivot_table(df, index=['赛季', '对手名称'])
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>三分出手数</th>
      <th>三分命中数</th>
      <th>三分命中率</th>
      <th>上场时间</th>
      <th>前场篮板数</th>
      <th>助攻数</th>
      <th>后场篮板数</th>
      <th>失误数</th>
      <th>对手总得分</th>
      <th>己方总得分</th>
      <th>...</th>
      <th>投篮出手数</th>
      <th>投篮命中数</th>
      <th>投篮命中率</th>
      <th>抢断数</th>
      <th>犯规数</th>
      <th>盖帽数</th>
      <th>罚球命中数</th>
      <th>罚球命中率</th>
      <th>罚球次数</th>
      <th>首发</th>
    </tr>
    <tr>
      <th>赛季</th>
      <th>对手名称</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="28" valign="top">03-04</th>
      <th>76人</th>
      <td>3.333333</td>
      <td>2.000000</td>
      <td>40.466667</td>
      <td>40.666667</td>
      <td>1.000000</td>
      <td>6.666667</td>
      <td>3.000000</td>
      <td>4.666667</td>
      <td>85.000000</td>
      <td>83.333333</td>
      <td>...</td>
      <td>18.666667</td>
      <td>9.666667</td>
      <td>49.800000</td>
      <td>1.666667</td>
      <td>1.333333</td>
      <td>1.000000</td>
      <td>3.333333</td>
      <td>68.533333</td>
      <td>5.333333</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>公牛</th>
      <td>3.500000</td>
      <td>0.750000</td>
      <td>22.925000</td>
      <td>37.250000</td>
      <td>1.250000</td>
      <td>6.250000</td>
      <td>3.000000</td>
      <td>4.000000</td>
      <td>88.250000</td>
      <td>91.750000</td>
      <td>...</td>
      <td>19.750000</td>
      <td>7.500000</td>
      <td>37.925000</td>
      <td>1.750000</td>
      <td>1.250000</td>
      <td>2.500000</td>
      <td>5.500000</td>
      <td>71.875000</td>
      <td>7.500000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>凯尔特</th>
      <td>4.500000</td>
      <td>1.000000</td>
      <td>21.675000</td>
      <td>41.500000</td>
      <td>0.500000</td>
      <td>4.000000</td>
      <td>3.750000</td>
      <td>3.000000</td>
      <td>98.000000</td>
      <td>89.750000</td>
      <td>...</td>
      <td>19.000000</td>
      <td>7.250000</td>
      <td>36.950000</td>
      <td>1.250000</td>
      <td>2.250000</td>
      <td>0.250000</td>
      <td>7.000000</td>
      <td>73.400000</td>
      <td>8.750000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>勇士</th>
      <td>5.000000</td>
      <td>2.500000</td>
      <td>50.000000</td>
      <td>44.500000</td>
      <td>2.500000</td>
      <td>8.000000</td>
      <td>4.500000</td>
      <td>2.500000</td>
      <td>111.000000</td>
      <td>101.000000</td>
      <td>...</td>
      <td>24.500000</td>
      <td>10.000000</td>
      <td>40.500000</td>
      <td>2.000000</td>
      <td>0.500000</td>
      <td>0.500000</td>
      <td>9.000000</td>
      <td>74.100000</td>
      <td>12.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>国王</th>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>42.000000</td>
      <td>2.000000</td>
      <td>9.000000</td>
      <td>4.000000</td>
      <td>2.000000</td>
      <td>106.000000</td>
      <td>92.000000</td>
      <td>...</td>
      <td>20.000000</td>
      <td>12.000000</td>
      <td>60.000000</td>
      <td>4.000000</td>
      <td>3.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>33.300000</td>
      <td>3.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>太阳</th>
      <td>5.000000</td>
      <td>1.500000</td>
      <td>30.000000</td>
      <td>41.500000</td>
      <td>1.500000</td>
      <td>7.000000</td>
      <td>5.500000</td>
      <td>6.000000</td>
      <td>99.000000</td>
      <td>86.000000</td>
      <td>...</td>
      <td>19.000000</td>
      <td>8.000000</td>
      <td>42.600000</td>
      <td>2.000000</td>
      <td>0.500000</td>
      <td>0.500000</td>
      <td>5.500000</td>
      <td>67.450000</td>
      <td>8.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>奇才</th>
      <td>1.500000</td>
      <td>0.750000</td>
      <td>25.000000</td>
      <td>40.500000</td>
      <td>2.500000</td>
      <td>6.750000</td>
      <td>2.750000</td>
      <td>3.750000</td>
      <td>102.500000</td>
      <td>99.500000</td>
      <td>...</td>
      <td>20.000000</td>
      <td>9.500000</td>
      <td>46.775000</td>
      <td>1.750000</td>
      <td>2.250000</td>
      <td>1.250000</td>
      <td>4.500000</td>
      <td>70.450000</td>
      <td>6.250000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>小牛</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>38.000000</td>
      <td>1.500000</td>
      <td>7.500000</td>
      <td>3.000000</td>
      <td>4.000000</td>
      <td>120.000000</td>
      <td>103.500000</td>
      <td>...</td>
      <td>17.000000</td>
      <td>9.500000</td>
      <td>54.550000</td>
      <td>2.000000</td>
      <td>2.000000</td>
      <td>0.500000</td>
      <td>3.000000</td>
      <td>75.000000</td>
      <td>4.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>尼克斯</th>
      <td>2.000000</td>
      <td>0.750000</td>
      <td>25.000000</td>
      <td>38.500000</td>
      <td>0.500000</td>
      <td>5.500000</td>
      <td>3.250000</td>
      <td>3.000000</td>
      <td>88.000000</td>
      <td>98.250000</td>
      <td>...</td>
      <td>16.500000</td>
      <td>7.000000</td>
      <td>43.925000</td>
      <td>1.500000</td>
      <td>1.000000</td>
      <td>0.500000</td>
      <td>2.750000</td>
      <td>66.675000</td>
      <td>3.750000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>开拓者</th>
      <td>1.500000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>39.000000</td>
      <td>1.500000</td>
      <td>7.500000</td>
      <td>5.500000</td>
      <td>3.500000</td>
      <td>89.000000</td>
      <td>85.500000</td>
      <td>...</td>
      <td>17.500000</td>
      <td>9.000000</td>
      <td>45.100000</td>
      <td>2.500000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>100.000000</td>
      <td>2.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>快船</th>
      <td>2.500000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>36.500000</td>
      <td>3.000000</td>
      <td>8.000000</td>
      <td>3.500000</td>
      <td>4.000000</td>
      <td>92.500000</td>
      <td>91.500000</td>
      <td>...</td>
      <td>14.500000</td>
      <td>4.000000</td>
      <td>26.450000</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>0.500000</td>
      <td>1.000000</td>
      <td>25.000000</td>
      <td>2.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>掘金</th>
      <td>2.500000</td>
      <td>1.000000</td>
      <td>33.350000</td>
      <td>36.500000</td>
      <td>2.500000</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>2.000000</td>
      <td>104.000000</td>
      <td>96.000000</td>
      <td>...</td>
      <td>15.000000</td>
      <td>4.500000</td>
      <td>29.450000</td>
      <td>2.000000</td>
      <td>2.500000</td>
      <td>1.500000</td>
      <td>3.000000</td>
      <td>85.700000</td>
      <td>4.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>森林狼</th>
      <td>2.500000</td>
      <td>0.500000</td>
      <td>16.650000</td>
      <td>40.000000</td>
      <td>1.500000</td>
      <td>3.500000</td>
      <td>7.500000</td>
      <td>2.000000</td>
      <td>100.000000</td>
      <td>87.500000</td>
      <td>...</td>
      <td>18.500000</td>
      <td>7.000000</td>
      <td>37.700000</td>
      <td>0.500000</td>
      <td>3.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>62.500000</td>
      <td>3.500000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>步行者</th>
      <td>2.750000</td>
      <td>0.250000</td>
      <td>12.500000</td>
      <td>42.500000</td>
      <td>0.750000</td>
      <td>4.250000</td>
      <td>4.750000</td>
      <td>5.250000</td>
      <td>95.500000</td>
      <td>92.750000</td>
      <td>...</td>
      <td>19.000000</td>
      <td>9.750000</td>
      <td>51.600000</td>
      <td>1.500000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>4.750000</td>
      <td>72.475000</td>
      <td>6.250000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>活塞</th>
      <td>2.500000</td>
      <td>0.500000</td>
      <td>14.575000</td>
      <td>38.250000</td>
      <td>1.000000</td>
      <td>6.250000</td>
      <td>3.000000</td>
      <td>3.250000</td>
      <td>89.000000</td>
      <td>86.000000</td>
      <td>...</td>
      <td>16.250000</td>
      <td>5.000000</td>
      <td>29.700000</td>
      <td>0.750000</td>
      <td>3.000000</td>
      <td>0.750000</td>
      <td>3.500000</td>
      <td>80.425000</td>
      <td>4.500000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>湖人</th>
      <td>3.500000</td>
      <td>2.000000</td>
      <td>50.000000</td>
      <td>40.000000</td>
      <td>0.500000</td>
      <td>5.500000</td>
      <td>4.000000</td>
      <td>3.000000</td>
      <td>100.000000</td>
      <td>92.500000</td>
      <td>...</td>
      <td>23.000000</td>
      <td>9.000000</td>
      <td>38.100000</td>
      <td>0.500000</td>
      <td>1.500000</td>
      <td>0.500000</td>
      <td>4.000000</td>
      <td>80.000000</td>
      <td>5.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>火箭</th>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>25.000000</td>
      <td>37.500000</td>
      <



<b>limit_output extension: Maximum message size of 10000 exceeded with 34292 characters</b>


### 数据框中选取部分values

```python
df.head(3)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>球员姓名</th>
      <th>赛季</th>
      <th>胜负</th>
      <th>对手名称</th>
      <th>对手总得分</th>
      <th>己方总得分</th>
      <th>己方名称</th>
      <th>首发</th>
      <th>上场时间</th>
      <th>投篮命中率</th>
      <th>...</th>
      <th>罚球次数</th>
      <th>总篮板数</th>
      <th>前场篮板数</th>
      <th>后场篮板数</th>
      <th>助攻数</th>
      <th>抢断数</th>
      <th>盖帽数</th>
      <th>失误数</th>
      <th>犯规数</th>
      <th>得分</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>黄蜂</td>
      <td>115</td>
      <td>129</td>
      <td>湖人</td>
      <td>1</td>
      <td>32</td>
      <td>57.9</td>
      <td>...</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>9</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>27</td>
    </tr>
    <tr>
      <th>1</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>奇才</td>
      <td>106</td>
      <td>124</td>
      <td>湖人</td>
      <td>1</td>
      <td>34</td>
      <td>55.0</td>
      <td>...</td>
      <td>2</td>
      <td>7</td>
      <td>0</td>
      <td>7</td>
      <td>14</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>23</td>
    </tr>
    <tr>
      <th>2</th>
      <td>勒布朗-詹姆斯</td>
      <td>18-19</td>
      <td>胜</td>
      <td>国王</td>
      <td>106</td>
      <td>111</td>
      <td>湖人</td>
      <td>1</td>
      <td>35</td>
      <td>40.9</td>
      <td>...</td>
      <td>9</td>
      <td>11</td>
      <td>2</td>
      <td>9</td>
      <td>11</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>29</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 27 columns</p>
</div>

#### 生成透视表，使用部分数据

查看詹姆斯在不同的赛季下的胜负情况，以及总篮板数、助攻数、抢断数、犯规数四项数据：


```python
pd.pivot_table(df
               ,index=['赛季', '胜负']
               ,values=['总篮板数','助攻数', '抢断数', '犯规数']
              )
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>助攻数</th>
      <th>总篮板数</th>
      <th>抢断数</th>
      <th>犯规数</th>
    </tr>
    <tr>
      <th>赛季</th>
      <th>胜负</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">03-04</th>
      <th>胜</th>
      <td>6.121212</td>
      <td>5.787879</td>
      <td>1.818182</td>
      <td>1.848485</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.717391</td>
      <td>5.239130</td>
      <td>1.521739</td>
      <td>1.913043</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">04-05</th>
      <th>胜</th>
      <td>7.560976</td>
      <td>7.731707</td>
      <td>2.487805</td>
      <td>1.902439</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.846154</td>
      <td>6.948718</td>
      <td>1.923077</td>
      <td>1.743590</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">05-06</th>
      <th>胜</th>
      <td>6.957447</td>
      <td>7.276596</td>
      <td>1.829787</td>
      <td>2.297872</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.062500</td>
      <td>6.687500</td>
      <td>1.156250</td>
      <td>2.281250</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">06-07</th>
      <th>胜</th>
      <td>6.234043</td>
      <td>6.872340</td>
      <td>1.723404</td>
      <td>2.148936</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.709677</td>
      <td>6.548387</td>
      <td>1.419355</td>
      <td>2.258065</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">07-08</th>
      <th>胜</th>
      <td>7.755556</td>
      <td>8.644444</td>
      <td>2.111111</td>
      <td>2.200000</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.333333</td>
      <td>6.766667</td>
      <td>1.433333</td>
      <td>2.200000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">08-09</th>
      <th>胜</th>
      <td>7.348485</td>
      <td>7.696970</td>
      <td>1.712121</td>
      <td>1.636364</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.800000</td>
      <td>7.000000</td>
      <td>1.600000</td>
      <td>2.066667</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">09-10</th>
      <th>胜</th>
      <td>8.750000</td>
      <td>7.383333</td>
      <td>1.616667</td>
      <td>1.483333</td>
    </tr>
    <tr>
      <th>负</th>
      <td>7.875000</td>
      <td>6.937500</td>
      <td>1.750000</td>
      <td>1.875000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">10-11</th>
      <th>胜</th>
      <td>7.070175</td>
      <td>7.368421</td>
      <td>1.666667</td>
      <td>2.017544</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.863636</td>
      <td>7.727273</td>
      <td>1.318182</td>
      <td>2.181818</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">11-12</th>
      <th>胜</th>
      <td>6.422222</td>
      <td>7.888889</td>
      <td>1.955556</td>
      <td>1.622222</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.764706</td>
      <td>8.058824</td>
      <td>1.588235</td>
      <td>1.352941</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">12-13</th>
      <th>胜</th>
      <td>7.573770</td>
      <td>8.049180</td>
      <td>1.606557</td>
      <td>1.295082</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.933333</td>
      <td>7.933333</td>
      <td>2.066667</td>
      <td>2.066667</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">13-14</th>
      <th>胜</th>
      <td>6.365385</td>
      <td>7.038462</td>
      <td>1.692308</td>
      <td>1.423077</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.280000</td>
      <td>6.680000</td>
      <td>1.320000</td>
      <td>2.080000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">14-15</th>
      <th>胜</th>
      <td>8.000000</td>
      <td>6.340000</td>
      <td>1.700000</td>
      <td>1.860000</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.842105</td>
      <td>5.210526</td>
      <td>1.263158</td>
      <td>2.210526</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">15-16</th>
      <th>胜</th>
      <td>7.392857</td>
      <td>7.267857</td>
      <td>1.517857</td>
      <td>1.839286</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.000000</td>
      <td>7.900000</td>
      <td>0.950000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">16-17</th>
      <th>胜</th>
      <td>9.372549</td>
      <td>8.431373</td>
      <td>1.196078</td>
      <td>1.745098</td>
    </tr>
    <tr>
      <th>负</th>
      <td>7.304348</td>
      <td>9.086957</td>
      <td>1.347826</td>
      <td>1.956522</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">17-18</th>
      <th>胜</th>
      <td>10.062500</td>
      <td>9.104167</td>
      <td>1.458333</td>
      <td>1.666667</td>
    </tr>
    <tr>
      <th>负</th>
      <td>7.533333</td>
      <td>7.500000</td>
      <td>1.333333</td>
      <td>1.766667</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">18-19</th>
      <th>胜</th>
      <td>8.071429</td>
      <td>8.892857</td>
      <td>1.428571</td>
      <td>1.535714</td>
    </tr>
    <tr>
      <th>负</th>
      <td>8.423077</td>
      <td>7.923077</td>
      <td>1.230769</td>
      <td>1.846154</td>
    </tr>
  </tbody>
</table>

</div>

### Aggfunc

#### 参数详解

- 参数的含义：设置我们对数据聚合时使用的函数操作

- 不同的属性可以使用不同的参数，用`字典`形式

- 默认情况：求均值`mean`

- 多个参数使用`列表`形式，可以使用numpy的函数形式，例如：`np.sum、np.mean`


```python
pd.pivot_table(df
               ,index=['赛季', '胜负']
               ,values=['总篮板数','助攻数', '抢断数', '犯规数']
               ,aggfunc=[np.mean, np.sum, np.median]
              )
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead tr th {
        text-align: left;
    }
    
    .dataframe thead tr:last-of-type th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">mean</th>
      <th colspan="4" halign="left">sum</th>
      <th colspan="4" halign="left">median</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>助攻数</th>
      <th>总篮板数</th>
      <th>抢断数</th>
      <th>犯规数</th>
      <th>助攻数</th>
      <th>总篮板数</th>
      <th>抢断数</th>
      <th>犯规数</th>
      <th>助攻数</th>
      <th>总篮板数</th>
      <th>抢断数</th>
      <th>犯规数</th>
    </tr>
    <tr>
      <th>赛季</th>
      <th>胜负</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">03-04</th>
      <th>胜</th>
      <td>6.121212</td>
      <td>5.787879</td>
      <td>1.818182</td>
      <td>1.848485</td>
      <td>202</td>
      <td>191</td>
      <td>60</td>
      <td>61</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.717391</td>
      <td>5.239130</td>
      <td>1.521739</td>
      <td>1.913043</td>
      <td>263</td>
      <td>241</td>
      <td>70</td>
      <td>88</td>
      <td>6.0</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">04-05</th>
      <th>胜</th>
      <td>7.560976</td>
      <td>7.731707</td>
      <td>2.487805</td>
      <td>1.902439</td>
      <td>310</td>
      <td>317</td>
      <td>102</td>
      <td>78</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.846154</td>
      <td>6.948718</td>
      <td>1.923077</td>
      <td>1.743590</td>
      <td>267</td>
      <td>271</td>
      <td>75</td>
      <td>68</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">05-06</th>
      <th>胜</th>
      <td>6.957447</td>
      <td>7.276596</td>
      <td>1.829787</td>
      <td>2.297872</td>
      <td>327</td>
      <td>342</td>
      <td>86</td>
      <td>108</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.062500</td>
      <td>6.687500</td>
      <td>1.156250</td>
      <td>2.281250</td>
      <td>194</td>
      <td>214</td>
      <td>37</td>
      <td>73</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">06-07</th>
      <th>胜</th>
      <td>6.234043</td>
      <td>6.872340</td>
      <td>1.723404</td>
      <td>2.148936</td>
      <td>293</td>
      <td>323</td>
      <td>81</td>
      <td>101</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.709677</td>
      <td>6.548387</td>
      <td>1.419355</td>
      <td>2.258065</td>
      <td>177</td>
      <td>203</td>
      <td>44</td>
      <td>70</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">07-08</th>
      <th>胜</th>
      <td>7.755556</td>
      <td>8.644444</td>
      <td>2.111111</td>
      <td>2.200000</td>
      <td>349</td>
      <td>389</td>
      <td>95</td>
      <td>99</td>
      <td>8.0</td>
      <td>8.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.333333</td>
      <td>6.766667</td>
      <td>1.433333</td>
      <td>2.200000</td>
      <td>190</td>
      <td>203</td>
      <td>43</td>
      <td>66</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>1.5</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">08-09</th>
      <th>胜</th>
      <td>7.348485</td>
      <td>7.696970</td>
      <td>1.712121</td>
      <td>1.636364</td>
      <td>485</td>
      <td>508</td>
      <td>113</td>
      <td>108</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>1.5</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.800000</td>
      <td>7.000000</td>
      <td>1.600000</td>
      <td>2.066667</td>
      <td>102</td>
      <td>105</td>
      <td>24</td>
      <td>31</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">09-10</th>
      <th>胜</th>
      <td>8.750000</td>
      <td>7.383333</td>
      <td>1.616667</td>
      <td>1.483333</td>
      <td>525</td>
      <td>443</td>
      <td>97</td>
      <td>89</td>
      <td>9.0</td>
      <td>7.0</td>
      <td>1.5</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>负</th>
      <td>7.875000</td>
      <td>6.937500</td>
      <td>1.750000</td>
      <td>1.875000</td>
      <td>126</td>
      <td>111</td>
      <td>28</td>
      <td>30</td>
      <td>7.0</td>
      <td>6.5</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">10-11</th>
      <th>胜</th>
      <td>7.070175</td>
      <td>7.368421</td>
      <td>1.666667</td>
      <td>2.017544</td>
      <td>403</td>
      <td>420</td>
      <td>95</td>
      <td>115</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.863636</td>
      <td>7.727273</td>
      <td>1.318182</td>
      <td>2.181818</td>
      <td>151</td>
      <td>170</td>
      <td>29</td>
      <td>48</td>
      <td>6.5</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">11-12</th>
      <th>胜</th>
      <td>6.422222</td>
      <td>7.888889</td>
      <td>1.955556</td>
      <td>1.622222</td>
      <td>289</td>
      <td>355</td>
      <td>88</td>
      <td>73</td>
      <td>6.0</td>
      <td>8.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.764706</td>
      <td>8.058824</td>
      <td>1.588235</td>
      <td>1.352941</td>
      <td>98</td>
      <td>137</td>
      <td>27</td>
      <td>23</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">12-13</th>
      <th>胜</th>
      <td>7.573770</td>
      <td>8.049180</td>
      <td>1.606557</td>
      <td>1.295082</td>
      <td>462</td>
      <td>491</td>
      <td>98</td>
      <td>79</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>2.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.933333</td>
      <td>7.933333</td>
      <td>2.066667</td>
      <td>2.066667</td>
      <td>89</td>
      <td>119</td>
      <td>31</td>
      <td>31</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">13-14</th>
      <th>胜</th>
      <td>6.365385</td>
      <td>7.038462</td>
      <td>1.692308</td>
      <td>1.423077</td>
      <td>331</td>
      <td>366</td>
      <td>88</td>
      <td>74</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>6.280000</td>
      <td>6.680000</td>
      <td>1.320000</td>
      <td>2.080000</td>
      <td>157</td>
      <td>167</td>
      <td>33</td>
      <td>52</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">14-15</th>
      <th>胜</th>
      <td>8.000000</td>
      <td>6.340000</td>
      <td>1.700000</td>
      <td>1.860000</td>
      <td>400</td>
      <td>317</td>
      <td>85</td>
      <td>93</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.842105</td>
      <td>5.210526</td>
      <td>1.263158</td>
      <td>2.210526</td>
      <td>111</td>
      <td>99</td>
      <td>24</td>
      <td>42</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">15-16</th>
      <th>胜</th>
      <td>7.392857</td>
      <td>7.267857</td>
      <td>1.517857</td>
      <td>1.839286</td>
      <td>414</td>
      <td>407</td>
      <td>85</td>
      <td>103</td>
      <td>7.5</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>5.000000</td>
      <td>7.900000</td>
      <td>0.950000</td>
      <td>2.000000</td>
      <td>100</td>
      <td>158</td>
      <td>19</td>
      <td>40</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">16-17</th>
      <th>胜</th>
      <td>9.372549</td>
      <td>8.431373</td>
      <td>1.196078</td>
      <td>1.745098</td>
      <td>478</td>
      <td>430</td>
      <td>61</td>
      <td>89</td>
      <td>9.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>负</th>
      <td>7.304348</td>
      <td>9.086957</td>
      <td>1.347826</td>
      <td>1.956522</td>
      <td>168</td>
      <td>209</td>
      <td>31</td>
      <td>45</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">17-18</th>
      <th>胜</th>
      <td>10.062500</td>
      <td>9.104167</td>
      <td>1.458333</td>
      <td>1.666667</td>
      <td>483</td>
      <td>437</td>
      <td>70</td>
      <td>80</td>
      <td>10.0</td>
      <td>9.5</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>




<b>limit_output extension: Maximum message size of 10000 exceeded with 10892 characters</b>


#### 字典形式


```python
pd.pivot_table(df
               ,index=['赛季', '胜负']
               ,values=['总篮板数','助攻数', '抢断数', '犯规数']
               ,aggfunc={"总篮板数": np.mean    # 字典形式
                        ,'助攻数': np.sum}
              )
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>助攻数</th>
      <th>总篮板数</th>
    </tr>
    <tr>
      <th>赛季</th>
      <th>胜负</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">03-04</th>
      <th>胜</th>
      <td>202</td>
      <td>5.787879</td>
    </tr>
    <tr>
      <th>负</th>
      <td>263</td>
      <td>5.239130</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">04-05</th>
      <th>胜</th>
      <td>310</td>
      <td>7.731707</td>
    </tr>
    <tr>
      <th>负</th>
      <td>267</td>
      <td>6.948718</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">05-06</th>
      <th>胜</th>
      <td>327</td>
      <td>7.276596</td>
    </tr>
    <tr>
      <th>负</th>
      <td>194</td>
      <td>6.687500</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">06-07</th>
      <th>胜</th>
      <td>293</td>
      <td>6.872340</td>
    </tr>
    <tr>
      <th>负</th>
      <td>177</td>
      <td>6.548387</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">07-08</th>
      <th>胜</th>
      <td>349</td>
      <td>8.644444</td>
    </tr>
    <tr>
      <th>负</th>
      <td>190</td>
      <td>6.766667</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">08-09</th>
      <th>胜</th>
      <td>485</td>
      <td>7.696970</td>
    </tr>
    <tr>
      <th>负</th>
      <td>102</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">09-10</th>
      <th>胜</th>
      <td>525</td>
      <td>7.383333</td>
    </tr>
    <tr>
      <th>负</th>
      <td>126</td>
      <td>6.937500</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">10-11</th>
      <th>胜</th>
      <td>403</td>
      <td>7.368421</td>
    </tr>
    <tr>
      <th>负</th>
      <td>151</td>
      <td>7.727273</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">11-12</th>
      <th>胜</th>
      <td>289</td>
      <td>7.888889</td>
    </tr>
    <tr>
      <th>负</th>
      <td>98</td>
      <td>8.058824</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">12-13</th>
      <th>胜</th>
      <td>462</td>
      <td>8.049180</td>
    </tr>
    <tr>
      <th>负</th>
      <td>89</td>
      <td>7.933333</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">13-14</th>
      <th>胜</th>
      <td>331</td>
      <td>7.038462</td>
    </tr>
    <tr>
      <th>负</th>
      <td>157</td>
      <td>6.680000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">14-15</th>
      <th>胜</th>
      <td>400</td>
      <td>6.340000</td>
    </tr>
    <tr>
      <th>负</th>
      <td>111</td>
      <td>5.210526</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">15-16</th>
      <th>胜</th>
      <td>414</td>
      <td>7.267857</td>
    </tr>
    <tr>
      <th>负</th>
      <td>100</td>
      <td>7.900000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">16-17</th>
      <th>胜</th>
      <td>478</td>
      <td>8.431373</td>
    </tr>
    <tr>
      <th>负</th>
      <td>168</td>
      <td>9.086957</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">17-18</th>
      <th>胜</th>
      <td>483</td>
      <td>9.104167</td>
    </tr>
    <tr>
      <th>负</th>
      <td>226</td>
      <td>7.500000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">18-19</th>
      <th>胜</th>
      <td>226</td>
      <td>8.892857</td>
    </tr>
    <tr>
      <th>负</th>
      <td>219</td>
      <td>7.923077</td>
    </tr>
  </tbody>
</table>

</div>

### columns

columns类似index，可以设置成列层次化字段，非必须参数


```python
pd.pivot_table(df
               ,index=['赛季']
               ,columns=['胜负']   # 新增参数columns
               ,values=['总篮板数','助攻数', '抢断数', '犯规数']
               ,aggfunc=[np.mean, np.sum]
               ,fill_value=0   # 缺失值补0
#                ,margins=1
              )
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead tr th {
        text-align: left;
    }
    
    .dataframe thead tr:last-of-type th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="10" halign="left">mean</th>
      <th>...</th>
      <th colspan="10" halign="left">sum</th>
    </tr>
    <tr>
      <th></th>
      <th colspan="3" halign="left">助攻数</th>
      <th colspan="3" halign="left">总篮板数</th>
      <th colspan="3" halign="left">抢断数</th>
      <th>犯规数</th>
      <th>...</th>
      <th>助攻数</th>
      <th colspan="3" halign="left">总篮板数</th>
      <th colspan="3" halign="left">抢断数</th>
      <th colspan="3" halign="left">犯规数</th>
    </tr>
    <tr>
      <th>胜负</th>
      <th>胜</th>
      <th>负</th>
      <th>All</th>
      <th>胜</th>
      <th>负</th>
      <th>All</th>
      <th>胜</th>
      <th>负</th>
      <th>All</th>
      <th>胜</th>
      <th>...</th>
      <th>All</th>
      <th>胜</th>
      <th>负</th>
      <th>All</th>
      <th>胜</th>
      <th>负</th>
      <th>All</th>
      <th>胜</th>
      <th>负</th>
      <th>All</th>
    </tr>
    <tr>
      <th>赛季</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>03-04</th>
      <td>6.121212</td>
      <td>5.717391</td>
      <td>5.886076</td>
      <td>5.787879</td>
      <td>5.239130</td>
      <td>5.468354</td>
      <td>1.818182</td>
      <td>1.521739</td>
      <td>1.645570</td>
      <td>1.848485</td>
      <td>...</td>
      <td>465</td>
      <td>191</td>
      <td>241</td>
      <td>432</td>
      <td>60</td>
      <td>70</td>
      <td>130</td>
      <td>61</td>
      <td>88</td>
      <td>149</td>
    </tr>
    <tr>
      <th>04-05</th>
      <td>7.560976</td>
      <td>6.846154</td>
      <td>7.212500</td>
      <td>7.731707</td>
      <td>6.948718</td>
      <td>7.350000</td>
      <td>2.487805</td>
      <td>1.923077</td>
      <td>2.212500</td>
      <td>1.902439</td>
      <td>...</td>
      <td>577</td>
      <td>317</td>
      <td>271</td>
      <td>588</td>
      <td>102</td>
      <td>75</td>
      <td>177</td>
      <td>78</td>
      <td>68</td>
      <td>146</td>
    </tr>
    <tr>
      <th>05-06</th>
      <td>6.957447</td>
      <td>6.062500</td>
      <td>6.594937</td>
      <td>7.276596</td>
      <td>6.687500</td>
      <td>7.037975</td>
      <td>1.829787</td>
      <td>1.156250</td>
      <td>1.556962</td>
      <td>2.297872</td>
      <td>...</td>
      <td>521</td>
      <td>342</td>
      <td>214</td>
      <td>556</td>
      <td>86</td>
      <td>37</td>
      <td>123</td>
      <td>108</td>
      <td>73</td>
      <td>181</td>
    </tr>
    <tr>
      <th>06-07</th>
      <td>6.234043</td>
      <td>5.709677</td>
      <td>6.025641</td>
      <td>6.872340</td>
      <td>6.548387</td>
      <td>6.743590</td>
      <td>1.723404</td>
      <td>1.419355</td>
      <td>1.602564</td>
      <td>2.148936</td>
      <td>...</td>
      <td>470</td>
      <td>323</td>
      <td>203</td>
      <td>526</td>
      <td>81</td>
      <td>44</td>
      <td>125</td>
      <td>101</td>
      <td>70</td>
      <td>171</td>
    </tr>
    <tr>
      <th>07-08</th>
      <td>7.755556</td>
      <td>6.333333</td>
      <td>7.186667</td>
      <td>8.644444</td>
      <td>6.766667</td>
      <td>7.893333</td>
      <td>2.111111</td>
      <td>1.433333</td>
      <td>1.840000</td>
      <td>2.200000</td>
      <td>...</td>
      <td>539</td>
      <td>389</td>
      <td>203</td>
      <td>592</td>
      <td>95</td>
      <td>43</td>
      <td>138</td>
      <td>99</td>
      <td>66</td>
      <td>165</td>
    </tr>
    <tr>
      <th>08-09</th>
      <td>7.348485</td>
      <td>6.800000</td>
      <td>7.246914</td>
      <td>7.696970</td>
      <td>7.000000</td>
      <td>7.567901</td>
      <td>1.712121</td>
      <td>1.600000</td>
      <td>1.691358</td>
      <td>1.636364</td>
      <td>...</td>
      <td>587</td>
      <td>508</td>
      <td>105</td>
      <td>613</td>
      <td>113</td>
      <td>24</td>
      <td>137</td>
      <td>108</td>
      <td>31</td>
      <td>139</td>
    </tr>
    <tr>
      <th>09-10</th>
      <td>8.750000</td>
      <td>7.875000</td>
      <td>8.565789</td>
      <td>7.383333</td>
      <td>6.937500</td>
      <td>7.289474</td>
      <td>1.616667</td>
      <td>1.750000</td>
      <td>1.644737</td>
      <td>1.483333</td>
      <td>...</td>
      <td>651</td>
      <td>443</td>
      <td>111</td>
      <td>554</td>
      <td>97</td>
      <td>28</td>
      <td>125</td>
      <td>89</td>
      <td>30</td>
      <td>119</td>
    </tr>
    <tr>
      <th>10-11</th>
      <td>7.070175</td>
      <td>6.863636</td>
      <td>7.012658</td>
      <td>7.368421</td>
      <td>7.727273</td>
      <td>7.468354</td>
      <td>1.666667</td>
      <td>1.318182</td>
      <td>1.569620</td>
      <td>2.017544</td>
      <td>...</td>
      <td>554</td>
      <td>420</td>
      <td>170</td>
      <td>590</td>
      <td>95</td>
      <td>29</td>
      <td>124</td>
      <td>115</td>
      <td>48</td>
      <td>163</td>
    </tr>
    <tr>
      <th>11-12</th>
      <td>6.422222</td>
      <td>5.764706</td>
      <td>6.241935</td>
      <td>7.888889</td>
      <td>8.058824</td>
      <td>7.935484</td>
      <td>1.955556</td>
      <td>1.588235</td>
      <td>1.854839</td>
      <td>1.622222</td>
      <td>...</td>
      <td>387</td>
      <td>355</td>
      <td>137</td>
      <td>492</td>
      <td>88</td>
      <td>27</td>
      <td>115</td>
      <td>73</td>
      <td>23</td>
      <td>96</td>
    </tr>
    <tr>
      <th>12-13</th>
      <td>7.573770</td>
      <td>5.933333</td>
      <td>7.250000</td>
      <td>8.049180</td>
      <td>7.933333</td>
      <td>8.026316</td>
      <td>1.606557</td>
      <td>2.066667</td>
      <td>1.697368</td>
      <td>1.295082</td>
      <td>...</td>
      <td>551</td>
      <td>491</td>
      <td>119</td>
      <td>610</td>
      <td>98</td>
      <td>31</td>
      <td>129</td>
      <td>79</td>
      <td>31</td>
      <td>110</td>
    </tr>
    <tr>
      <th>13-14</th>
      <td>6.365385</td>
      <td>6.280000</td>
      <td>6.337662</td>
      <td>7.038462</td>
      <td>6.680000</td>
      <td>6.922078</td>
      <td>1.692308</td>
      <td>1.320000</td>
      <td>1.571429</td>
      <td>1.423077</td>
      <td>...</td>
      <td>488</td>
      <td>366</td>
      <td>167</td>
      <td>533</td>
      <td>88</td>
      <td>33</td>
      <td>121</td>
      <td>74</td>
      <td>52</td>
      <td>126</td>
    </tr>
    <tr>
      <th>14-15</th>
      <td>8.000000</td>
      <td>5.842105</td>
      <td>7.405797</td>
      <td>6.340000</td>
      <td>5.210526</td>
      <td>6.028986</td>
      <td>1.700000</td>
      <td>1.263158</td>
      <td>1.579710</td>
      <td>1.860000</td>
      <td>...</td>
      <td>511</td>
      <td>317</td>
      <td>99</td>
      <td>416</td>
      <td>85</td>
      <td>24</td>
      <td>109</td>
      <td>93</td>
      <td>42</td>
      <td>135</td>
    </tr>
    <tr>
      <th>15-16</th>
      <td>7.392857</td>
      <td>5.000000</td>
      <td>6.763158</td>
      <td>7.267857</td>
      <td>7.900000</td>
      <td>7.434211</td>
      <td>1.517857</td>
      <td>0.950000</td>
      <td>1.368421</td>
      <td>1.839286</td>
      <td>...</td>
      <td>514</td>
      <td>407</td>
      <td>158</td>
      <td>565</td>
      <td>85</td>
      <td>19</td>
      <td>104</td>
      <td>103</td>
      <td>40</td>
      <td>143</td>
    </tr>
    <tr>
      <th>16-17</th>
      <td>9.372549</td>
      <td>7.304348</td>
      <td>8.729730</td>
      <td>8.431373</td>
      <td>9.086957</td>
      <td>8.635135</td>
      <td>1.196078</td>
      <td>1.347826</td>
      <td>1.243243</td>
      <td>1.745098</td>
      <td>...</td>
      <td>646</td>
      <td>430</td>
      <td>209</td>
      <td>639</td>
      <td>61</td>
      <td>31</td>
      <td>92</td>
      <td>89</td>
      <td>45</td>
      <td>134</td>
    </tr>
    <tr>
      <th>17-18</th>
      <td>10.062500</td>
      <td>7.533333</td>
      <td>9.089744</td>
      <td>9.104167</td>
      <td>7.500000</td>
      <td>8.487179</td>
      <td>1.458333</td>
      <td>1.333333</td>
      <td>1.410256</td>
      <td>1.666667</td>
      <td>...</td>
      <td>709</td>
      <td>437</td>
      <td>225</td>
      <td>662</td>
      <td>70</td>
      <td>40</td>
      <td>110</td>
      <td>80</td>
      <td>53</td>
      <td>133</td>
    </tr>
    <tr>
      <th>18-19</th>
      <td>8.071429</td>
      <td>8.423077</td>
      <td>8.240741</td>
      <td>8.892857</td>
      <td>7.923077</td>
      <td>8.425926</td>
      <td>1.428571</td>
      <td>1.230769</td>
      <td>1.333333</td>
      <td>1.535714</td>
      <td>...</td>
      <td>445</td>
      <td>249</td>
      <td>206</td>
      <td>455</td>
      <td>40</td>
      <td>32</td>
      <td>72</td>
      <td>43</td>
      <td>48</td>
      <td>91</td>
    </tr>
    <tr>
      <th>All</th>
      <td>7.594663</td>
      <td>6.497537</td>
      <td>7.221291</td>
      <td>7.604828</td>
      <td>6.990148</td>
      <td>7.395641</td>
      <td>1.707751</td>
      <td>1.445813</td>
      <td>1.618609</td>
      <td>1.770013</td>
      <td>...</td>
      <td>8615</td>
      <td>5985</td>
      <td>2838</td>
      <td>8823</td>
      <td>1344</td>
      <td>587</td>
      <td>1931</td>
      <td>1393</td>
      <td>808</td>
      <td>2201</td>
    </tr>
  </tbody>
</table>
<p>17 rows × 24 columns</p>
</div>

```python
# pivot_table和groupby机制的同样效果
pd.pivot_table(df,index=[字段1],values=[字段2],aggfunc=[函数],fill_value=0)
df.groupby([字段1])[字段2].agg(函数).fillna(0)
```

