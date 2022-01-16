# Matplotlib
## [二：艺术画笔见乾坤][2]
### 一.概述
头文件
```
import numpy as np
import pandas as pd
import re
import matplotlib
import matplotlib.pyplot as plt
from matplotlib.lines import Line2D   
from matplotlib.patches import Circle, Wedge
from matplotlib.collections import PatchCollection
```
#### 1. matplotlib的三层api
matplotlib有三个层次的api：
**api：引用程序编程接口**

**matplotlib.backend_bases.FigureCanvas** 代表了**绘图区**，所有的图像都是在绘图区完成的
**matplotlib.backend_bases.Renderer** 代表了**渲染器**，可以近似理解为画笔，控制如何在 FigureCanvas 上画图。
**matplotlib.artist.Artist** 代表了具体的**图表组件**，即调用了Renderer的接口在Canvas上作图。

前两者**处理程序和计算机的底层交互**的事项，第三项Artist就是**具体的调用接口**来做出我们想要的图，比如图形、文本、线条的设定。

#### 2.Artist的分类
Artist
- primitives——基本要素:包含一些我们要在绘图区作图用到的标准图形对象——曲线Line2D，文字text，矩形Rectangle，图像image
- containers——容器:装基本要素的地方——图形figure、坐标系Axes和坐标轴Axis
<img src="assets/markdown-img-paste-20220114141113749.png" width = "400" height = "400" alt="图片名称" align=center />


### 二.基本元素——primitives
#### 1.2DLines
曲线绘制主要是通过**类matplotlib.lines.Line2D**来完成的。

**三种方法设置线的属性**
- 直接在plot()函数中设置
```
x = range(0,5)
y = [2,5,7,8,10]
plt.plot(x,y, linewidth=10); # 设置线的粗细参数为10
```
- 通过获得线对象，对线对象进行设置
```
x = range(0,5)
y = [2,5,7,8,10]
line, = plt.plot(x, y, '-') # 这里等号坐标的line,是一个列表解包的操作，目的是获取plt.plot返回列表中的Line2D对象
line.set_antialiased(False) # 关闭抗锯齿功能
```
- 获得线属性，使用setp()函数设置
```
x = range(0,5)
y = [2,5,7,8,10]
lines = plt.plot(x, y)
plt.setp(lines, color='r', linewidth=10)
```

**绘制lines方法**
- plot方法绘制
```
x = range(0,5)
y1 = [2,5,7,8,10]
y2= [3,6,8,9,11]

fig,ax= plt.subplots()
ax.plot(x,y1)
ax.plot(x,y2)
print(ax.lines); # 通过直接使用辅助方法画线，打印ax.lines后可以看到在matplotlib在底层创建了两个Line2D对象
```
- Line2D对象绘制
```
x = range(0,5)
y1 = [2,5,7,8,10]
y2= [3,6,8,9,11]
fig,ax= plt.subplots()
lines = [Line2D(x, y1), Line2D(x, y2,color='orange')]  # 显式创建Line2D对象
for line in lines:
    ax.add_line(line) # 使用add_line方法将创建的Line2D添加到子图中
ax.set_xlim(0,4)
ax.set_ylim(2, 11);
```

**errorbar绘制误差折线图**
通过**errorbar类**实现
参数
- x：需要绘制的line中点的在x轴上的取值
- y：需要绘制的line中点的在y轴上的取值
- yerr：指定y轴水平的误差
- xerr：指定x轴水平的误差
- fmt：指定折线图中某个点的颜色，形状，线条风格，例如‘co--’
- ecolor：指定error bar的颜色
- elinewidth：指定error bar的线条宽度
```
fig = plt.figure()
x = np.arange(10)
y = 2.5 * np.sin(x / 20 * np.pi)
yerr = np.linspace(0.05, 0.2, 10)
plt.errorbar(x, y + 3, yerr=yerr, label='both limits (default)');
```

#### 2.patches
**matplotlib.patches.Patch类**是二维图形类，是众多二维图形的父类
##### 矩形
1. hist-直方图
` matplotlib.pyplot.hist(x,bins=None,range=None, density=None, bottom=None, histtype='bar', align='mid', log=False, color=None, label=None, stacked=False, normed=None)`

参数：
- x: 数据集，最终的直方图将对数据集进行统计
- bins: 统计的区间分布
- range: tuple, 显示的区间，range在没有给出bins时生效
- density: bool，默认为false，显示的是**频数统计结果**，为True则显示频率统计结果。——True：y轴表现的概率0.0x表现形式
  Tips：
  $ 频率统计结果 = \frac{区间数目}{(总数*区间宽度)} $
- histtype: 可选{'bar', 'barstacked', 'step', 'stepfilled'}之一，默认为bar,step使用的是梯状，stepfilled则会对梯状内部进行填充
    - 'step'：空心
    - 'stepfilled'=='bar'
- align: 可选{'left', 'mid', 'right'}之一，默认为'mid'，控制柱状图的水平分布，left或者right，会有**部分空白区域**
    - left 最右边有空隙，靠左边
    - right 最左边有空隙，靠右边
- log: bool，默认False,即y坐标轴是否选择**指数刻度**
- stacked: bool，默认为False，是否为**堆积状图**

hist绘制直方图
```
x=np.random.randint(0,100,100) #生成[0-100)之间的100个数据,即 数据集
bins=np.arange(0,101,10) #设置连续的边界值，即直方图的分布区间[0,10),[10,20)...
plt.hist(x,bins,color='fuchsia',alpha=0.5)#alpha设置透明度，0为完全透明
plt.xlabel('scores')
plt.ylabel('count')
plt.xlim(0,100); #设置x轴分布范围
plt.show()
```
Rectangle矩形类绘制直方图

2. bar-柱状图
`matplotlib.pyplot.bar(left, height, alpha=1, width=0.8, color=, edgecolor=, label=, lw=3)`

参数：
- left：x轴的位置序列，一般采用range函数产生一个序列，但是有时候可以是字符串
- height：y轴的数值序列，也就是柱形图的高度——即需要展示的数据；
- alpha：透明度，值越小越透明
- width：为柱形图的宽度，一般这是为0.8即可
- color或facecolor：柱形图填充的颜色；
- edgecolor：图形边缘颜色
- label：解释每个图像代表的含义，这个参数是为legend()函数做铺垫的，表示该次bar的标签

hist绘制柱状图
```
# bar绘制柱状图
y = range(1,17)
plt.bar(np.arange(16), y, alpha=0.5, width=0.5, color='yellow', edgecolor='red', label='The First Bar', lw=3);
```
Rectangle矩形类绘制
```
fig = plt.figure()
ax1 = fig.add_subplot(111)

for i in range(1,17):
    rect =  plt.Rectangle((i+0.25,0),0.5,i)
    ax1.add_patch(rect)
ax1.set_xlim(0, 16)
ax1.set_ylim(0, 16);
```
——**add_subplot(xyz)**
参数：3个数字
表示x*y个子图构成大图，当前图是第z个图
Eg:
```
ax = fig.add_subplot(221)
ax = fig.add_subplot(222)
ax = fig.add_subplot(223)
ax = fig.add_subplot(224)
```
![](assets/markdown-img-paste-20220116180603734.png)

——**set_xlim,set_ylim**
设置轴的范围
##### Polygon-多边形
**matplotlib.patches.Polygon类**是多边形类
`matplotlib.pyplot.fill(*args, data=None, **kwargs)`
关于x、y和color的序列，其中color是可选的参数，每个多边形都是由其节点的x和y位置列表定义的，后面可以选择一个颜色说明符。您可以通过提供多个x、y、[颜色]组来绘制多个多边形。
```
x = np.linspace(0, 5 * np.pi, 1000)
y1 = np.sin(x)
y2 = np.sin(2 * x)
plt.fill(x, y1, color = "g", alpha = 0.3)
```

##### Wedge-契形
制作数据x的饼图，每个楔子的面积用$ \frac{x}{sum(x)} $表示。
`matplotlib.pyplot.pie(x, explode=None, labels=None, colors=None, autopct=None, pctdistance=0.6, shadow=False, labeldistance=1.1, startangle=0, radius=1, counterclock=True, wedgeprops=None, textprops=None, center=0, 0, frame=False, rotatelabels=False, *, normalize=None, data=None)`
参数:
- x：契型的形状，一维数组。
- explode：如果不是等于None，则是一个len(x)数组，它指定用于偏移每个楔形块的半径的分数。
- labels：用于指定每个契型块的标记，取值是列表或为None。
- colors：饼图循环使用的颜色序列。如果取值为None，将使用当前活动循环中的颜色。
- startangle：饼状图开始的绘制的角度。
```
labels = 'Frogs', 'Hogs', 'Dogs', 'Logs'
sizes = [15, 30, 45, 10]
explode = (0, 0.1, 0, 0)
fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%', shadow=True, startangle=90)
ax1.axis('equal'); # Equal 确保其画成圆形
```

wedge绘制饼图
```
fig = plt.figure(figsize=(5,5))
ax1 = fig.add_subplot(111)
theta1 = 0
sizes = [15, 30, 45, 10]
patches = []
patches += [
    Wedge((0.5, 0.5), .4, 0, 54),           
    Wedge((0.5, 0.5), .4, 54, 162),  
    Wedge((0.5, 0.5), .4, 162, 324),           
    Wedge((0.5, 0.5), .4, 324, 360),  
]
colors = 100 * np.random.rand(len(patches))
p = PatchCollection(patches, alpha=0.8)
p.set_array(colors)
ax1.add_collection(p);
```
#### collections
**collections类**是用来绘制一组对象的集合，collections有许多不同的子类，如RegularPolyCollection, CircleCollection, Pathcollection, 分别对应不同的集合子类型。
其中比较常用的就是散点图，属于PathCollection子类，**scatter方法**提供了该类的封装，根据x与y绘制不同大小或颜色标记的散点图.
`Axes.scatter(self, x, y, s=None, c=None, marker=None, cmap=None, norm=None, vmin=None, vmax=None, alpha=None, linewidths=None, verts=, edgecolors=None, *, plotnonfinite=False, data=None, **kwargs)`
参数：
- x：数据点x轴的位置
- y：数据点y轴的位置
- s：尺寸大小
- c：可以是单个颜色格式的字符串，也可以是一系列颜色
- marker: 标记的类型

用scatter绘制散点图：
```
x = [0,2,4,6,8,10]
y = [10]*len(x)
s = [20*2**n for n in range(len(x))]
plt.scatter(x,y,s=s) ;
```

#### images
**images**是image图像的类，其中最常用的imshow可以根据数组绘制成图像。
构造函数：
`class matplotlib.image.AxesImage(ax, cmap=None, norm=None, interpolation=None, origin=None, extent=None, filternorm=True, filterrad=4.0, resample=False, **kwargs)`

imshow根据数组绘制图像
`matplotlib.pyplot.imshow(X, cmap=None, norm=None, aspect=None, interpolation=None, alpha=None, vmin=None, vmax=None, origin=None, extent=None, shape=, filternorm=1, filterrad=4.0, imlim=, resample=None, url=None, *, data=None, **kwargs）`
使用imshow画图时首先需要传入一个数组，数组对应的是空间内的像素位置和像素点的值，interpolation参数可以设置不同的差值方法，具体效果如下。
```
methods = [None, 'none', 'nearest', 'bilinear', 'bicubic', 'spline16',
           'spline36', 'hanning', 'hamming', 'hermite', 'kaiser', 'quadric',
           'catrom', 'gaussian', 'bessel', 'mitchell', 'sinc', 'lanczos']


grid = np.random.rand(4, 4)

fig, axs = plt.subplots(nrows=3, ncols=6, figsize=(9, 6),
                        subplot_kw={'xticks': [], 'yticks': []})

for ax, interp_method in zip(axs.flat, methods):
    ax.imshow(grid, interpolation=interp_method, cmap='viridis')
    ax.set_title(str(interp_method))

plt.tight_layout();
```
### 对象容器-Object container
容器会包含一些primitives，并且容器还有它自身的属性。
#### Figure容器
matplotlib.figure.Figure是**Artist最顶层**的container对象容器，它包含了图表中的所有元素。
一张图表的背景就是在Figure.patch的一个矩形Rectangle。

当我们向图表添**Figure.add_subplot()或者Figure.add_axes()元素**时，这些都会被添加到**Figure.axes**列表中。
```
fig = plt.figure()
ax1 = fig.add_subplot(211) # 作一幅2*1的图，选择第1个子图
ax2 = fig.add_axes([0.1, 0.1, 0.7, 0.3]) # 位置参数，四个数分别代表了(left,bottom,width,height)
print(ax1)
print(fig.axes) # fig.axes 中包含了subplot和axes两个实例
```
由于Figure维持了current axes，通过**Figure.add_subplot()、Figure.add_axes()** 来添加元素，通过**Figure.delaxes()** 来删除元素。
可以迭代或者访问Figure.axes中的Axes，然后修改属性。
```
#添加网格线
fig = plt.figure()
ax1 = fig.add_subplot(211)

for ax in fig.axes:
    ax.grid(True)
```
Figure属性：
- Figure.patch属性：Figure的背景矩形
Figure.axes属性：一个Axes实例的列表（包括Subplot)
- Figure.images属性：一个FigureImages patch列表
- Figure.lines属性：一个Line2D实例的列表（很少使用）
- Figure.legends属性：一个Figure Legend实例列表（不同于Axes.legends)
- Figure.texts属性：一个Figure Text实例列表

#### Axes容器
**matplotlib.axes.Axes** 是matplotlib的核心。大量的用于绘图的Artist存放在它内部，并且它有许多辅助方法来创建和添加Artist给它自己，而且它也有许多赋值方法来访问和修改这些Artist。

和Figure容器类似，Axes包含了一个patch属性:
- 对于笛卡尔坐标系而言，它是一个Rectangle；
- 对于极坐标而言，它是一个Circle。这个patch属性决定了绘图区域的形状、背景和边框。

**Subplot**就是一个特殊的Axes，其实例是位于网格中某个区域的Subplot实例

不应该直接通过**Axes.lines和Axes.patches列表**来添加图表。
（因为当创建或添加一个对象到图表中时，Axes会做许多自动化的工作:
它会设置Artist中figure和axes的属性，同时默认Axes的转换；
它也会检视Artist中的数据，来更新数据结构，这样数据范围和呈现方式可以根据作图范围自动调整。）

另外Axes还包含两个最重要的**Artist container**：
- **ax.xaxis**：XAxis对象的实例，用于处理x轴tick以及label的绘制
- **ax.yaxis**：YAxis对象的实例，用于处理y轴tick以及label的绘制


Axes容器属性：
- artists: Artist实例列表
- patch: Axes所在的矩形实例
- collections: Collection实例
- images: Axes图像
- legends: Legend 实例
- lines: Line2D 实例
- patches: Patch 实例
- texts: Text 实例
- xaxis: matplotlib.axis.XAxis 实例
- yaxis: matplotlib.axis.YAxis 实例

#### Axis容器
**matplotlib.axis.Axis**实例处理tick line、grid line、tick label以及axis label的绘制，它包括**坐标轴上的刻度线、刻度label、坐标网格、坐标轴标题**。也可以独立的配置**y轴的左边刻度**以及**右边的刻度**，也可以独立地配置x轴的上边刻度以及下边的刻度。

刻度包括**主刻度**和**次刻度**，它们都是Tick刻度对象。



#### Tick容器
暂略



[2]: https://datawhalechina.github.io/fantastic-matplotlib/%E7%AC%AC%E4%BA%8C%E5%9B%9E%EF%BC%9A%E8%89%BA%E6%9C%AF%E7%94%BB%E7%AC%94%E8%A7%81%E4%B9%BE%E5%9D%A4/index.html#  "二：艺术画笔见乾坤"
