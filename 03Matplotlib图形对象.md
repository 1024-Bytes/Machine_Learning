#  图形对象的创建及其属性

##  1.创建图形对象

在`Matplotlib`中，面向对象编程的核心思想是创建图形对象（figure object）。通过图形对象来调用其他的方法和属性，处理多个画布。在这个过程中，`pyplot`负责生成图形对象，并通过该对象来添加一个或多个`axes`对象（即绘图区域）。

`Matplotlib`提供了`matplotlib.figure`图形类模块，它包含了创建图形对象的方法。通过调用`pyplot`模块中`figure()`来实例化`figure`对象。

`figure object`(画布)>>`axes`(绘图区域)>>`pyplot`(图形对象)

**创建画布方法：**

`plt.figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True, **kwargs)`

**num：**图形编号或名称，数字为编号，字符串为名称

**figsize：**指定figure的宽和高，单位为英寸

**dpi：**指定绘画对象的分辨率，即每英寸多少个像素，缺省值为72

**facecolor：**背景颜色

**edgecolor：**边框颜色

**frameon：**是否显示边框

```python
from matplotlib import pyplot as plt
x = np.arange(0, 50)
y = x**2
fig = plt.figure('f1', figsize=(4,2), dpi=100, facecolor='gray')
plt.plot(x, y)
```

**注：不可以对画布对象直接进行绘制操作，需对区域（子图）进行绘制操作。**

##  2.绘制多子图

`figure`是绘制对象（空白画布），一个`figure`对象可以包含多个`Axes`子图，一个`Axes`是一个绘图区域，`Axes`默认为1，绘图都是在`figure`上的`Axes`上绘画。

### 绘制子图方法一：`add_axes()`——添加区域

`Matplotlib`定义了一个`axes`类（轴域类），这类对象称为`axes`对象（即轴域对象），它指定了一个有数值范围限制的绘图区域。在一个给定的画布（`figure`）中可以包含多个`axes`对象，但是同一个`axes`对象只能在一个画布中使用。

**2D绘图区域（`axes`）包含两个轴（`axis`）对象**

**方法：不同轴域范围有重叠时，最后添加的轴域在最上面的图层**

`add_axes(rect)`—生成一个`axes`轴域对象，位置由参数`rect`决定

**rect：**位置参数，接受一个由4个元素组成的浮点数列表，形如`[left, bottom, width, height]`，它表示添加到画布中的矩形区域的左下角坐标(x, y)，以及宽度和高度。

```python
fig = plt.figure(figure=(4, 2), facecolor='g')
# ax1从画布起始位置绘制，宽高和画布一致
ax1 = fig.add_axes([0, 0, 1, 1])
# ax2从画布20%的位置开始绘制，宽高是画布的50%
ax2 = fig.add_axes([0.2, 0.2, 0.5, 0.5])
ax1.plot([1, 2, 3, 4, 6], [2, 3, 5, 8, 9])
ax2.plot([1, 2, 3, 4, 6], [2, 3, 5, 8, 9])
ax3 = fig.add_axes([0.0, 0.5, 0.5, 0.5])
ax3.plot([1, 2, 3, 4, 6], [2, 3, 5, 8, 9])
# 在当前区域(ax3)绘制
plt.plot([1, 3, 5, 6], [2, 4, 7, 8])
```

*注：列表中每个元素的值是画布宽度和高度的分数，画布的宽、高分别为1个单位*

```python
fig = plt.figure()
ax = plt.gca()
ax.plot()
```

### 绘制子图方法二：`subplot()`——均等划分画布，创建一个包含子图区域的画布（返回区域对象）

**方法（返回区域对象）：**

`ax = plt.subplot(nrows, ncols, index, *args, **kwargs)`

**nrows：**行

**ncols：**列

**index：**位置

**kwargs：**title/xlabel/ylabel等……

**注：也可以直接将几个值写到一起，如`subplot(233)`，nrows与ncols表示要划分几行几列的子区域（nrows*ncols表示子图数量），index的初始值为1，用来选定具体的某个子区域。例如：`subplot(233)`表示在当前画布创建一个两行三列的绘图区域（如下图所示），同时选择在第三个位置绘制子图。**

|  1   |  2   |  3   |
| :--: | :--: | :--: |
|  4   |  5   |  6   |

```python
# 默认画布分割为2行1列，当前所在第一个区域
plt.subplot(211)
# x可省略，默认[0, ……, N-1]递增，N为y轴元素的个数
plt.plot(range(50, 70), marker="o")
plt.grid()
# 默认画布分割为2行1列，当前所在第二个区域
plt.subplot(212)
plt.plot(np.arange(12)*2)
```
**注：新建的子图与现有的子图重叠，那么重叠部分的子图将会被自动删除，它们不可以共享绘图区域**

```python
plt.plot([1, 2, 3])
plt.subplot(211)
plt.plot(range(50,70))
plt.subplot(212)
plt.plot(np.arange(12)**2)
```
**若不想覆盖之前的图，需要先创建画布，如下所示：**

```python
plt.plot([1, 2, 3, 4])
fig = plt.figure(figsize=(4,2))
fig.add_subplot(111)
plt.plot(range(20))
fig.add_subplot(221)
plt.plot(range(12))
```

**设置子图的属性：**

(1)通过关键词赋值参数设置属性

```python
# 创建一个子图，选择2行1列的顶部图
#-------第一区域-------
plt.subplot(211, title='pic1', xlabel='x axis')
# x可省略，默认[0, 1……, N-1]递增
plt.plot(range(50, 70))
#-------第二区域-------
plt.subplot(212, title='pic2', xlabel='x axis')
plt.plot(np.arange(12)**2)
# 紧凑布局（防止子图标题重叠）
plt.tight_layout()
```

(2)使用pyplot模块中的方法设置属性

```python
# 创建一个子图，选择2行1列的顶部图
#-------第一区域-------
plt.subplot(211)
# 使用图形对象：
plt.title('ax1')
# x可省略，默认[0, 1……, N-1]递增
plt.plot(range(50, 70))
#-------第二区域-------
plt.subplot(212)
plt.title('ax2')
plt.plot(np.arange(12)**2)
# 紧凑布局（防止子图标题重叠）
plt.tight_layout()
```

(3)使用返回的区域对象设置：

```python
# 创建一个子图，选择2行1列的顶部图
#-------返回第一区域-------
ax1 = plt.subplot(211)
#-------返回第二区域-------
ax2 = plt.subplot(212)
# 使用第一区域对象：
ax1.set_title('ax1')
# x可省略，默认[0, 1……, N-1]递增
plt.plot(range(50, 70))
# 使用第二区域对象：
ax2.set_title('ax2')
# x可省略，默认[0, 1……, N-1]递增
plt.plot(np.arange(12)**2)
# 紧凑布局（防止子图标题重叠）
plt.tight_layout()
```



### 绘制子图方法三：`subplots()`——创建一个包含子图区域的画布和一个figure图形对象（返回图形对象和区域对象）

`matplotlib.pyplot`模块提供了一个`subplots()`函数，使用方法和`subplot()`函数类似。其不同之处在于subplots()既创建了一个包含子图区域的画布，又创建了一个`figure`图形对象，而`subplot()`只是创建一个包含子图区域的画布。

**方法（返回图形对象和区域对象）：**

`fig, ax = plt.subplots(nrows, ncols)`

**nrows：**子图行数

**ncols：**子图列数

*函数返回值是一个元组，包括一个图形对象和所有的`axes`对象。其中`axes`对象数量等于`nrows*ncols`，且每个`axes`对象均可通过索引值访问（从0开始），如下所示为2行2列：*

| (0,0) | (0,1) |
| :---: | :---: |
| (1,0) | (1,1) |

```python
# 引入模块
import matplotlib.pyplot as plt
import numpy as np


#创建2行2列的子图，返回图形对象（画布），所有子图的坐标轴
fig, axes = plt.subplots(2,2)
# 第一个区域ax1
ax1 = axes[0][0]
# x轴
x = np.arange(1,5)
# 绘制平方函数
ax1.plot(x, x*x)
ax1.set_title('square')
# 绘制平方根图像
axes[0][1].plot(x, np.sqrt(x))
axes[0][1].set_title('square root')
# 绘制指数函数
axes[1][0].plot(x, np.exp(x))
axes[1][0].set_title('exp')
# 绘制对数函数
axes[1][1].plot(x, np.log10(x))
axes[1][1].set_title('log')
# 处理标题遮挡
plt.tight_layou()
```

### `subplots`与`subplot`实例

```python
fig, axes = plt.subplots(1, 2)
# 设置画布的高和宽，注意：只为英寸，默认分辨率为72
fig.set_figheight(3) #实际高度为73*3像素
fig.set_figwidth(8) #实际宽度为73*8像素
# 设置背景
fig.set_facecolor('gray')
# 分别定义x, y
x = np.arange(-50, 51)
y = x**2
#-----绘制图形1-----
axes[0].plot(x, y)
#-----绘制图形2-----
# 不需要右侧和上侧线条，则可以设置为无色
axes[1].spines['right'].set_color('none')
axes[1].spines['top'].set_color('none')
#下轴移动到指定位置
axes[1].spines['bottom'].set_position('data', 0.0)
axes[1].plot(x, y)
```

```python
plt.subplot(121, facecolor='r') #绘制1行2列的第1个子图
plt.subplot(222, facecolor='g') #绘制2行2列的第2个子图
plt.subplot(224, facecolor='b') #绘制2行2列的第4个子图
```

```python
plt.subplot(321, facecolor='r') #绘制3行2列的第1个子图
plt.subplot(322, facecolor='r') #绘制3行2列的第2个子图
plt.subplot(323, facecolor='r') #绘制3行2列的第3个子图
plt.subplot(324, facecolor='r') #绘制3行2列的第4个子图
plt.subplot(313, facecolor='b') #绘制3行1列的第3个子图
```

# 常见图形类别

## 1.柱状图的绘制

柱状图可以垂直绘制，也可以水平绘制，其高度与所表示的数值成正比关系。

**方法：**

`matplotlib.pyplot.bar(x, height, width:float = 0.8, bottom = None, *, align:str = 'center', data = None, **kwargs)`

**x：**x坐标，数据类型为`float`类型，一般为`np.arange()`生成的固定步长列表

**height：**柱状图的高度，即y坐标，数值类型为`float`类型，一般为包含生成柱状图的所有y值

**width：**柱状图的宽度，取值在0~1之间，默认值为0.8

**bottom：**柱状图的起始位置，即y轴的起始坐标，默认值为`None`

**align：**柱状图的中心位置，'center'，'lege'边缘，默认值为'center'

**color：**柱状图颜色，默认为蓝色

**alpha：**透明度，取值在0~1之间，默认值为1

**label：**标签，设置后需调用`plt.legend()`生成

**edgecolor：**边框颜色(ec)

**linewidth：**边框宽度，浮点数或类数组，默认为`None(lw)`

**tick_label：**柱子的刻度标签，字符串或字符串列表，默认值为`None`

**linestyle：**线条样式(ls)

### 基本柱状图的绘制

```python
import 	matplotlib.pyplot as plt

# x轴数据[0, 1, 2, 3, 4]
x = range(5)
# y轴数据
data = [5, 20, 15, 25, 10]
# 设置图形标题
plt.title('基本柱状图')
# 绘制网格
plt.grid(ls='--', alpha=0.5)
# bar绘制图形，x表示x坐标，data表示柱状图的高度，facecolor不可以设置多个颜色，而color可以设置(color=['r', 'g', 'b'])
plt.bar(x, data) #plt.bar(x, data, bottom=[10, 20, 5, 0, 10], facecolor='green', ec='r', ls='--', lw=2)
```

### 同位置多柱状图的绘制

同一x轴位置绘制多个柱状图，主要通过调整柱状图的宽度和每个柱状图x轴的起始位置

```python
# 国家
countries = ['挪威', '德国', '中国', '美国', '瑞典']
# 金牌个数
gold_medal = [16, 12, 9, 8, 8]
# 银牌个数
silver_medal = [8, 10, 4, 10, 5]
# 铜牌个数
bronze_medal = [13, 5, 2, 7, 5]
# x轴坐标
x = np.arange(len(countries))
# 设置图形的宽度
width = 0.2
#======确定x起始位置======
# 金牌起始位置
gold_x = x
# 银牌的起始位置
silver_x = x + width
# 铜牌的起始位置
bronze_x = x + 2*width
#======绘制图形======
# 金牌图形
plt.bar(gold_x, gold_medal, width=width, color='gold', label='金牌')
# 银牌图形
plt.bar(silver_x, silver_medal, width=width, color='silver', label='银牌')
# 铜牌图形
plt.bar(bronze_x, bronze_medal, width=width, color='saddlebrown', label='铜牌')
#======修改x轴坐标======
# x标签位置居中
plt.xticks(x+width, labels=countries)
#======显示高度文本======
for i in range(len(countries)):
    # 金牌的文本设置
    plt.text(gold_x[i], gold_medal[i], gold_medal[i], va='bottom', ha='center')
    # 银牌的文本设置
    plt.text(silver_x[i], silver_medal[i], silver_medal[i], va='bottom', ha='center')
    # 铜牌的文本设置
    plt.text(bronze_x[i], bronze_medal[i], bronze_medal[i], va='bottom', ha='center')
# 显示图例
plt.legend(loc="upper right")
```

### 堆叠柱状图的绘制

所谓堆叠柱状图就是将不同数组的柱状图堆叠在一起，堆叠后的柱状图高度显示了两者相加的结果值

```python
# 国家
countries = ['挪威', '德国', '中国', '美国', '瑞典']
# 金牌个数
gold_medal = [16, 12, 9, 8, 8]
# 银牌个数
silver_medal = [8, 10, 4, 10, 5]
# 铜牌个数
bronze_medal = [13, 5, 2, 7, 5]
# 设置图形的宽度
width = 0.3
#======绘制图形======
# 金牌图形
plt.bar(countries, gold_medal, color='gold', label='金牌', bottom=silver_medal + bronze_medal, width=width)
# 银牌图形
plt.bar(countries, silver_medal, color='silver', label='银牌', bottom=bronze_medal, width=width)
# 铜牌图形
plt.bar(countries, bronze_medal, color='#A0522D', label='铜牌', width=width)
# 设置坐标轴
plt.ylabel('奖牌数')
#======显示高度文本======
for i in range(len(countries)):
    max_y = bronze_medal[i]+silver_medal[i]+gold_medal[i]
    plt.text(countries[i], max_y, max_y, va='bottom', ha='center')
# 显示图例
plt.legend(loc="upper right")
```

### 水平柱状图的绘制

调用`Matplotlib`的`barh()`函数可以生成水平柱状图，`barh()`使用方法与`bar()`类似，区别为需传入y轴数据参数，`width`参数表示柱高，`height`表示柱宽。

**方法：**

`plt.barh(y, width, height=0.8, left, *, align='center', **kwargs)`

```python
countries = ['挪威', '德国', '中国', '美国', '瑞典']
# 金牌个数
gold_medal = [16, 12, 9, 8, 8]
# y轴为国家，宽度为奖牌数
plt.barh(countries, width=gold_medal)
```

**绘制堆叠图：**

```python
# 涉及到计算，数据转成numpy数组
movie = ['新蝙蝠侠', '狙击手', '奇迹笨小孩']
# 第一天
real_day1 = np.array([4053, 2548, 1543])
# 第二天
real_day2 = np.array([7840, 4013, 2421])
# 第三天
real_day3 = np.array([8080, 3673, 1342])
#======确定左侧距离======
left_day2 = real_day1
left_day3 = real_day1 + real_day2
# 设置线条高度
height = 0.2
# 绘制图形
plt.barh(movie, real_day1, height=height)
plt.barh(movie, real_day2, left=left_day2, height=height)
plt.barh(movie, real_day3, left=left_day3, height=height)
# 设置数值文本：计算宽度值
sum_data = real_day1 + real_day2 + real_day3
for i in range(len(movie)):
    plt.text(sum_data[i], movie[i], sum_data[i], va='center', ha='left')
```

**绘制同位置多柱状图：**

```python
# 涉及到计算，数据转成numpy数组
movie = ['新蝙蝠侠', '狙击手', '奇迹笨小孩']
# 第一天
real_day1 = np.array([4053, 2548, 1543])
# 第二天
real_day2 = np.array([7840, 4013, 2421])
# 第三天
real_day3 = np.array([8080, 3673, 1342])
#======y轴转换为数值型======
num_y = np.arange(len(movie))
# 设置线条高度
height = 0.2
#======计算每个图形高度的起始位置======
movie1_start_y = num_y
movie2_start_y = num_y + height
movie3_start_y = num_y + 2*height
#======绘制图形======
plt.barh(movie1_start_y, real_day1, height=height)
plt.barh(movie2_start_y, real_day2, height=height)
plt.barh(movie3_start_y, real_day3, height=height)
#======替换y轴数据======
plt.yticks(num_y + height, movie)
# 设置数值文本：计算宽度值
for i in range(len(movie)):
    plt.text(real_day1[i], movie1_start_y[i], real_day1[i], va='center', ha='left')
    plt.text(real_day2[i], movie2_start_y[i], real_day2[i], va='center', ha='left')
    plt.text(real_day3[i], movie3_start_y[i], real_day3[i], va='center', ha='left')
plt.xlim(0, 9000)
```

### 直方图的绘制

直方图，又称质量分布图，由一系列高度不等的纵向线段来表示数据概率分布的情况。直方图的横轴表示数据类型，纵轴表示分布情况。

**方法：**

`plt.hist(x, bins=None, range=None, density=None, weights=None, cumulative=False, bottom=None, histtype='bar', align='mid', orientation='vertical', rwidth=None, log=False, color=None, label=None, stacked=False, normed=None, *, data=None, **kwargs)`

**x：**直方图所用数据，必须为一维数组；多维数组先进行扁平化再作图；此为必选参数。

**bins：**直方图分组数，默认为10。

**weights：**与x形状相同的权重数组，将x中的每个元素乘以对应权重值再计数；如果normed或density取值为`True`，则会对权重进行归一化处理。这个参数可用于绘制已合并的数据的直方图。

**density：**布尔，可选。如果"True"，返回元组的第一个元素将会将计数标准化以形成一个概率密度，也就是说，直方图下的面积（或积分）总和为1。这是通过将计数除以数字的数量来实现的观察乘以箱子的宽度而不是除以总数数量的观察。如果叠加也是“真实”的，那么柱状图被规范化为1。（替代normed）

**bottom：**数组，标量值或`None`；每个柱子底部相对于y=0的位置。如果是标量值，则每个柱子相对于y=0向上/向下的偏移量相同。如果是数组，则根据数组元素取值移动对应的柱子；即直方图上下偏移距离。

**histtype：**{'bar', 'barstacked', 'step', 'stepfilled'}；'bar'是传统条形直方图；'barstacked'是堆叠的条形直方图；'step'是未填充的条形直方图，只有外边框；'stepfilled'是有填充的直方图；当histtype取值为'step'或'stepfilled'，rwidth设置失效，即不能指定柱子间的间隔，默认连接在一起。

**align：**{'left', 'mid', 'right'}；'left'：柱子的中心位于bins的左边缘；'mid'：柱子位于bins左右边缘之间；'right'：柱子的中心位于bins的右边缘。

**color：**具体颜色，数组（元素为颜色）或`None`。

**label：**字符串（序列）或`None`；有多个数据集时，用label参数做标注区分。

**normed：**是否将得到的直方图向量归一化，即显示占比，默认为0，不归一化；不推荐使用，建议改用density参数。

**edgecolor：**直方图边框颜色。

**alpha：**透明度。

```python
# 使用numpy随机生成200个随机数据
x_value = np.random.randint(140, 180, 300)
# 绘制直方图，第一个参数是数据
plt.hist(x_value, bins=10, edgecolor='white')
plt.title("数据统计")
plt.xlabel("身高")
plt.ylabel("比率")
#num为直方图的值（数组或数组列表），bins为各个bin的区间范围（数组），patches为每个bin里面包含的数据（列表）
num, bins, patches = plt.hist(x_value, bin=10, edgecolor='white')
```

**添加折线直方图：使用`plot()`方法**

```python
# 使用numpy随机生成200个随机数据
x_value = np.random.randint(140, 180, 300)
# 创建一个画布
fig, ax = plt.subplots()
# 绘制直方图
num, bins_limit, patches = ax.hist(x_value, bins=10, edgecolor='white')
# num返回的个数是10，bins_limit返回的个数为11，需要截取
print(bins_limit[:10])
# 曲线图
ax.plot(bins_limit[:10], num, '--') #ax.plot(bins_limit, np.append(num, num[-1]), '--', marker='o')
```

**不等距分组：**

```python
fig, ax = plt.subplots()
x = np.random.normal(100, 20, 100)
bins = [50, 60, 70, 90, 100, 110, 140, 150]
ax.hist(x, bins, color='g', edgecolor='white')
ax.set_title('不等距分组')
plt.show()
```

**多类型直方图：**

```python
# 指定分组个数
n_bins = 10
fig, ax = plt.subplots(figsize=(8,5))
# 分别生成10000，5000，2000个值
x_multi = [np.random.randn(n) for n in [10000, 5000, 2000]]
# 实际绘图代码与单类型直方图差异不大，只是增加了一个图例项
# 在ax.hist函数中先指定图例label名称
ax.hist(x_multi, n_bins, histtype='bar', label=list('ABC'))
ax.set_title('多类型直方图')
# 通过ax.legend函数来添加图例
ax.legend()
```

**堆叠直方图：**

```python
x_value = np.random.randint(140, 180, 200)
x2_value = np.random.randint(140, 180, 200)
# 设置stacked=True，允许数据覆盖；设置stacked=False，不允许数据覆盖
plt.hist([x_value, x2_value], bins=10, stacked=True)
```

## 2.饼状图的绘制

饼状图显示数据系列中各项目占项目总和的百分比。

**方法：**

`pyplot.pie(x, explode=None, labels=None, color=None, autopct=None)`

**x：**数组序列，数组元素对应扇形区域的数量大小。

**labels：**列表字符串序列，为每个扇形区域备注一个标签名字。

**colors：**为每个扇形区域设置颜色，默认按照颜色周期自动设置。

**autopct：**格式化字符串“fmt%pct”，使用百分比的格式设置每个扇形区的标签，并将其放置在扇形区内。

**pctdistance：**设置百分比标签与圆心的距离。

**labeldistance：**设置各扇形标签（图例）与圆心的距离。

**explode：**指定饼图某些部分的突出显示，即呈现爆炸式。

**shadow：**是否添加饼图的阴影效果。

```python
# 设置大小
plt.rcParams['figure.figsize'] = (5, 5)
# 定义饼的标签
labels = ['娱乐', '育儿', '饮食', '房贷', '交通', '其它']
# 每个标签所占的数量
x = [200, 500, 1200, 7000, 200, 900]
# 绘制饼图
plt.pie(x, labels=labels)
```

**百分比显示percentage：**

```python
# 定义饼的标签
labels = ['娱乐', '育儿', '饮食', '房贷', '交通', '其它']
# 每个标签所占的数量
x = [200, 500, 1200, 7000, 200, 900]
plt.title('饼图示例-8月份家庭支出')
# %.2f%%显示百分比，保留2位小数
plt.pie(x, labels=labels, autopct='%.2f%%')
```

**饼状图的分离：**

```python
# 定义饼的标签
labels = ['娱乐', '育儿', '饮食', '房贷', '交通', '其它']
# 每个标签所占的数量
x = [200, 500, 1200, 7000, 200, 900]
# 饼图分离
explode = (0.03, 0.05, 0.06, 0.04, 0.08, 0.1)
# 设置阴影效果
plt.pie(x, labels=labels, autopct='%3.2f%%', explode=explode)
```

**设置饼状图百分比和文本距离中心位置：**

```python
# 定义饼的标签
labels = ['娱乐', '育儿', '饮食', '房贷', '交通', '其它']
# 每个标签所占的数量
x = [200, 500, 1200, 7000, 200, 900]
# 饼图分离
explode = (0.03, 0.05, 0.06, 0.04, 0.08, 0.1)
# 设置阴影效果
plt.pie(x, labels=labels, autopct='%3.2f%%', explode=explode, labeldistance=1, pctdistance=1)
# 显示图例
plt.legend()
```

## 3.散点图的绘制

数据的相关关系主要分为：正相关（两个变量同时增长）、负相关（一个变量值增加另一个变量值下降）、不相关、线性相关、指数相关等，那些离点集群较远的点叫做离群点或异常点。

**方法：**

`matplotlib.pyplot.scatter(x, y, s=None, c=None, marker=None, cmap=None, norm=None, vmin=None, vmax=None, alpha=None, linewidths=None, edgecolors=None, plotnonfinite=False, data=None, *kwargs)`

**x, y：**散点的坐标。

**s：**散点的面积。

**c：**散点的颜色（默认值为蓝色，'b'，其余颜色同`plt.plot()`）。

**marker：**散点样式（默认值为实心圆，'o'，其余样式同`plt.plot()`）。

**alpha：**散点透明度（[0, 1]之间的数，0表示完全透明，1表示完全不透明）。

**linewidths：**散点的边缘线宽。

**edgecolors：**散点的边缘颜色。

**cmap：**Colormap，默认None，标量或者是一个colormap的名字，只有c是一个浮点数数组的时候才使用。

```python
# x轴数据
x = np.array([1, 2, 3, 4, 5, 6, 7, 8])
# y轴数据
y = np.array([1, 4, 9, 16, 7, 11, 23, 18])
# 绘制散点图和折线图
plt.scatter(x, y)
plt.plot(x, y)
```

**设置图标大小：**

```python
# x轴数据
x = np.array([1, 2, 3, 4, 5, 6, 7, 8])
# y轴数据
y = np.array([1, 4, 9, 16, 7, 11, 23, 18])
# 生成一个(0, 1)之间的随机浮点数或N维浮点数组
s = (20 * np.random.rand(8))**2
plt.scatter(x, y, s)
```

**自定义点的颜色和透明度：**

```python
# x轴数据
x = np.random.rand(50)
# y轴数据
y = np.random.rand(50)
# 生成一个(0, 1)之间的随机浮点数或N维浮点数组
s = (10 * np.random.rand(50))**2
# 颜色可以使用一组数字序列
# 如只需使用3种颜色
# colors = np.resize(np.array([1, 2, 3]), 100)
# 颜色随机
colors = np.random.rand(50)
plt.scatter(x, y, s, c=colors, alpha=0.5)
```

**可以选择不同的颜色条--配合cmap参数，Matplotlib模块提供了很多可用的颜色条Colormap，颜色条就像一个颜色列表，其中每种颜色都有一个范围从0到100的值**

cmap主要分四大类：连续化色图（低饱和到高饱和）、两端发散的色图（中间浅色，正负为不同颜色）、离散化色图（离散的颜色组合）、其他色图

```python
# x轴数据
x = np.arange(1, 101)
y = np.arange(1, 101)
# 颜色随机
colors = np.arange(1, 101)
plt.scatter(x, y, c=colors, cmap='viridis')
```

# 保存图片

**方法：**

`savefig(fname, dpi=None, facecolor='w', edgecolor='w', orientation='portrait', papertype=None, format=None, transparent=False, bbox_inches=None, pad_inches=0.1, frameon=None, metadata=None)`

**fname：**（字符串或者仿路径或仿文件）如果格式已设置，这将决定输出的格式并将文件按fname来保存。如果格式没有设置，在fname有扩展名的情况下推断按此保存，没有扩展名将按照默认格式存储为"png"格式，并将适当的扩展名添加在fname后面。

**dpi：**分辨率，每英寸的点数。

**facecolor：**图形表面颜色。默认为"auto"，使用当前图形的表面颜色。

**edgecolor：**图形边缘颜色。默认为"auto"，使用当前图形的边缘颜色。

**format：**文件格式，比如"png"、"pdf"、"svg"等（字符串），未设置的行为将被记录在fname中。

**transparent：**用于将图片背景设置为透明。图形也是是透明，除非通过关键字参数指定了表面颜色和边缘。

**注：**

1.第一个参数是保存的路径。

2.如果路径中包含未创建的文件夹，会报错，需要手动或者使用os模块创建

3.必须在调用`plt.show()`之前保存，否则将保存的是空白图形

```python
import os
# x轴数据
x_axis = [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
# y轴数据
y_axis = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
# 绘制图形
plt.hist(x_axis, y_axis)
# x和y轴标签
plt.xlabel("X")
plt.ylabel("Y")
# 使用os模块判断目录是否存在
if not os.path.exists("my"):
    # 使用os模块创建文件夹
    os.mkdir("my")
plt.savefig("my/my_show.png")
plt.show()
```