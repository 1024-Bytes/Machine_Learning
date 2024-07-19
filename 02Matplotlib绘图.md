# Matplotlib绘图

## （一）绘图程序举例

*导入Matplotlib包中的Pyplot模块，并以as别名的形式简化引入包的名称*

```python
from matplotlib import pyplot as plt
```

*使用Numpy提供的函数`arrange()`创建一组数据来绘制图像*

```python
# 引入numpy包
import numpy as np
# 获得-50到50之间的ndarray对象
x = np.arrange(-50, 51)
```

*上述所得x的值作用到x轴上，而该值对应的平方值，也就是y值，使用以下方式获取（列表不可以用这种方式获取）*

```python
y = x**2
```

*使用plt的`plot()`函数对x，y进行绘制*

```python
# plot()绘制线性图表
plt.plot(x, y)
# show()展示图表
plt.show()
```

## （二）Matplotlib的基本方法

### **1.设置图表名称plt.title()**

```python
import numpy as np
# x轴获得-50到50之间的ndarray对象
x = np.arrange(-50, 51)
# y轴的值是x轴的平方
y = x**2
# 设置图名
plt.title("y = x^2")
# 绘制图形
plt.plot(x, y)
# 展示图形
plt.show()
```

*注：默认情况下标题不能写成中文*

**修改字体配置plt.rcParams["font.sans-serif"]**

字体说明：

| 中文字体 | 说明     |
| :------: | -------- |
|  SimHei  | 中文黑体 |
|  Kaiti   | 中文楷体 |
|   LiSu   | 中文隶书 |
| FangSong | 中文仿宋 |
| YouYuan  | 中文幼圆 |
|  STSong  | 华文宋体 |

临时设置

```python
# 设置字体样式以正常显示中文标签
plt.rcParams['font.sans-serif']=['SimHei']
# 默认是使用Unicode负号，设置正常显示字符
plt.rcParams['axes.unicode_minus']=False
```

### 2.x轴和y轴名称

```python
import numpy as np
# x轴获得-50到50之间的ndarray对象
x = np.arrange(-50, 51)
# y轴的值是x轴的平方
y = x**2
# 设置图名
plt.title("y = x^2")
# 设置x轴名称
plt.xlabel("x轴")
# 设置y轴名称
plt.ylabel("y轴")
# 绘制图形
plt.plot(x, y)
# 展示图形
plt.show()
```

**修改字体参数**

参数说明

|   参数    |     说明     |
| :-------: | :----------: |
| fontsize  | 设置文字大小 |
| linewidth | 设置线条宽度 |

临时设置

```python
# 设置标题
plt.title("y = x^2", fontsize=16)
# 设置x轴名称
plt.xlabel("x轴", fontsize=12)
# 设置y轴名称
plt.ylabel("y轴", fontsize=12)
# 绘制图形，设置线条宽度
plt.plot(x, y, linewidth=5)
```

### 3.绘制多条曲线

```python
import numpy as np
# x轴获得-50到50之间的ndarray对象
x = np.arrange(-50, 51)
# y1的值是x的平方
y1 = x**2
# y2的值等于x
y2 = x
# 设置图名
plt.title("y1 = x^2  y2 = x")
# 设置x轴名称
plt.xlabel("x轴")
# 设置y轴名称
plt.ylabel("y轴")
# 绘制图形1
plt.plot(x, y1)
# 绘制图形2
plt.plot(x, y2)
# 展示图形
plt.show()
```

### 4.设置x轴和y轴刻度（特别是字符串格式需自行设置）

`matplotlib.pylot.xticks(ticks=None, labels=None, **kwargs)`

**ticks**：此参数是xtick位置的列表和一个可选参数，如果将空列表作为参数传入，则它将删除所有xticks。

**labels**：此参数包含放置在给定刻度线位置的标签，是一个可选参数。

** **kwargs**：此参数是文本属性，用于控制标签的外观，包括

**rotation**：旋转角度，如：`rotation=45`

**color**：颜色，如：`color="red"`

***延伸***：`matplotlib.pylot.yticks(ticks=None, labels=None, **kwargs)`

```python
import numpy as np
# 获得时间列表
time = ["2011", "2012", "2013", "2015", "2016", "2017", "2018", "2019", "2020", "2021", "2022", "2023"]
labels = [1, 2, 3, 4, 5, 6]
# 将列表统一转化为标准格式
times = ['日期%s' % i for i in time]
# 根据时间列表随机出销量
sales = np.random.randint(500, 2000, size=len(times))
# x轴刻度（红色）显示部分时间（被标签列表内容替代）并倾斜45度，或按照某个规则展示
plt.xticks(range(0, len(times), 2), labels, rotation=45, color='red') # range函数表示从0开始步长为2显示times列表
# 绘制图形
plt.plot(times, sales)
# 展示图形
plt.show()
```

### 5.图表显示

**Pycharm：**`plt.show()`

**jupyter：**显示图形操作菜单，使用`matplotlib`中的魔术方法— `%matplotlib notebook`；

​                  回归原图形展示方式— `%matplotlib inline`

### 6.图例展示

```python
import numpy as np
# 获得时间列表
time = ["2011", "2012", "2013", "2015", "2016", "2017", "2018", "2019", "2020", "2021", "2022", "2023"]
# 将列表统一转化为标准格式
times = ['日期%s' % i for i in time]
# 根据时间列表随机出收入
income = np.random.randint(500, 2000, size=len(times))
# 根据时间列表随机出支出
expenses = np.random.randint(300, 1500, size=len(times))
# x轴刻度（红色）显示部分时间（被标签列表内容替代）并倾斜45度，或按照某个规则展示
plt.xticks(range(1, len(times), 2), rotation=45, color='red') # range函数表示从0开始步长为2显示times列表
# 绘制图形，为图例设置label参数
plt.plot(times, income, label='收入')
plt.plot(times, expenses, label='支出')
# 显示图例
plt.legend(loc="best")
# 展示图形
plt.show()
```

**设置图例位置参数**

loc代表了图例在整个坐标轴平面中的位置：

1.默认是“best”，图例自动被放在坐标图内数据图表最少的位置；

2.loc='xxx'，有数值和字符串两种表述（如下表）

| 位置字符串（上下/中/左右） | 位置数值 |         备注         |
| :------------------------: | :------: | :------------------: |
|           "best"           |    0     | 自动寻找最合适的位置 |
|       "upper right"        |    1     |        右上角        |
|        "upper left"        |    2     |        左上角        |
|        "lower left"        |    3     |        左下角        |
|       "lower right"        |    4     |        右下角        |
|          "right"           |    5     |       右侧中间       |
|       "center left"        |    6     |       左侧中间       |
|       "center right"       |    7     |       右侧中间       |
|       "lower center"       |    8     |      中间最下面      |
|       "upper center"       |    9     |      中间最上面      |
|          "center"          |    10    |         中间         |

### 7.显示数据值标签

`plt.text(x,y, string, fontsize=15, verticalalignment='top', horizontalalignment='right')`

**x,y：**对应坐标轴上的值

**string：**说明文字

**fontsize：**字体大小

**verticalalignment：**(va)垂直对齐方式，参数：['center'|'top'|'bottom'|'baseline']

**horizontalalignment：**(ha)水平对齐方式，参数：['center'|'right'|'left']

```python
import numpy as np
# 获得时间列表(注意其中的参数类型为数值型或者字符型)
time = ["2011", "2012", "2013", "2015", "2016", "2017", "2018", "2019", "2020", "2021", "2022", "2023"]
# 将列表统一转化为标准格式
times = ['日期%s' % i for i in time]
# 根据时间列表随机出收入
income = np.random.randint(500, 2000, size=len(times))
# 根据时间列表随机出支出
expenses = np.random.randint(300, 1500, size=len(times))
# x轴刻度（红色）显示部分时间（被标签列表内容替代）并倾斜45度，或按照某个规则展示
plt.xticks(range(1, len(times), 2), rotation=45, color='red') # range函数表示从0开始步长为2显示times列表
# 绘制图形，为图例设置label参数
plt.plot(times, income, label='收入')
plt.plot(times, expenses, label='支出')
# 显示图例
plt.legend(loc="best")
for x,y in zip(times, income):
    plt.text(x, y, '%s万'%y)
for a,b in zip(times, expenses):
    plt.text(a, b, b)
# 展示图形
plt.show()
```

###  8.绘制网格

**显示网格：**`plt.grid(True, linestyle = "--", color = "gray", linewidth = "0.5", axis = 'x')`

**linestyle：**线型

**color：**颜色

**linewidth：**宽度

**axis：**x，y，both，显示x/y/两者的格网

```python
x = np.linspace(-np.pi, np.pi, 256, endpoint = True)
c, s = np.cos(x), np.sin(x)
plt.plot(x, c)
plt.plot(x, s)
plt.grid(True, linestyle = "--", color = "gray", linewidth = "0.5", axis = 'both')
```

### 9.关于坐标轴的操作

**获取、移动坐标轴位置以及修改坐标轴属性：**

**获取坐标轴：**`ax = plt.gca()`

**选取坐标轴：**`ax.spines['top'|'bottom'|'left'|'right']`

**设置坐标轴颜色：**`ax.spines['xxx'].set_color('none')`

**设置坐标轴位置：**`ax.spines['xxx'].set_position('xxx', xxx)`

**设置坐标区间：**`plt.ylim(xx, xx)`

```python
x = np.arrange(-50, 51)
y = x**2
# 获取当前坐标轴
ax = plt.gca()
# 通过坐标轴spines，确定top，bottom，left，right(分别表示上下左右)
# 通过颜色设置隐藏右侧和上侧线条
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
# 移动下侧坐标轴至指定位置
# 在这里，position位置参数有三种，data、outward(向外)、axes(0.0~1.0之间的值，表示在整个轴的比例)
ax.spines['bottom'].set_position('data', 0.0)
# 移动y轴至x=0.0处或x轴0.5比例处
ax.spines['left'].set_position('data', 0.0) #ax.spines['left'].set_position('axes', 0.5)
# 设置坐标区间(0, 2500)或(0, y的最大值)
plt.ylim(0, 2500) #plt.ylim(0, y.max())
plt.plot(x, y)
```

### 10.线条样式设置

传入x，y，通过plot画图，并设置折线颜色、透明度、折线样式、折线宽度、标记点大小、标记点颜色、标记点边宽、网格

`plt.plot(x, y, color = 'red', alpha = 0.3, linestyle = '-', linewidth = 5, marker = 'x', markeredgecolor = 'r', markersize = '20', markeredgewidtth = 10)`

**color：**可以使用颜色的16进制，也可以使用线条颜色的英文，还可以使用缩写（参考网址：[http://tools.jb51.net/color/jPicker]）

| 字符 | 颜色 | 英文全称 |
| :--: | :--: | :------: |
| 'b'  | 蓝色 |   blue   |
| 'g'  | 绿色 |  green   |
| 'r'  | 红色 |   red    |
| 'c'  | 青色 |   cyan   |
| 'm'  | 品红 | magenta  |
| 'y'  | 黄色 |  yellow  |
| 'k'  | 黑色 |  black   |
| 'w'  | 白色 |  white   |

**alpha：**0~1，透明度

**linestyle：**折线样式

| 字符 |  描述  |
| :--: | :----: |
| '-'  |  实线  |
| '--' |  虚线  |
| '-.' | 点划线 |
| ';'  |  虚线  |

**linewidth：**线条宽度

**marker：**线条样点标记

| 标记符号 |    描述    |
| :------: | :--------: |
|   '.'    |   点标记   |
|   'o'    |  圆圈标记  |
|   'x'    |  'X'标记   |
|   'D'    |  钻石标记  |
|   'H'    |  六角标记  |
|   's'    | 正方形标记 |
|   '+'    |  加号标记  |

