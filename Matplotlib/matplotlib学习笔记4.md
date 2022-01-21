# Matplotlib
## [三.布局格式定方圆][4]
摘自Datawhale Matplotlib提供文章
```
import matplotlib
import matplotlib.pyplot as plt
import numpy as np
import matplotlib.dates as mdates
import datetime
```

### 一.Figure和Axes上的文本
#### 1.文本API示例
|pyplot API|OO API|description
|:--|:----|:--
|text|text|	在**子图**axes的任意位置添加**文本**|
|annotate|annotate|在子图axes的任意位置添加**注解**，包含指向性的箭头|
|xlabel|set_xlabel|为子图axes添加**x轴标签**|
|ylabel|set_ylabel|为子图axes添加**y轴标签**|
|title|set_title|为**子图**axes添加**标题**|
|figtext|figtext|在**画布**figure的任意位置添加**文本**|
|suptitle|suptitle|为**画布**figure添加**标题**|

**Eg:**
```
fig = plt.figure()
ax = fig.add_subplot()


# 分别为figure和ax设置标题，注意两者的位置是不同的
fig.suptitle('bold figure suptitle', fontsize=14, fontweight='bold')
ax.set_title('axes title')

# 设置x和y轴标签
ax.set_xlabel('xlabel')
ax.set_ylabel('ylabel')

# 设置x和y轴显示范围均为0到10
ax.axis([0, 10, 0, 10])

# 在子图上添加文本
ax.text(3, 8, 'boxed italics text in data coords', style='italic',
        bbox={'facecolor': 'red', 'alpha': 0.5, 'pad': 10})

# 在画布上添加文本，一般在子图上添加文本是更常见的操作，这种方法很少用
fig.text(0.4,0.8,'This is text for figure')

ax.plot([2], [1], 'o')
# 添加注解
ax.annotate('annotate', xy=(2, 1), xytext=(3, 4),arrowprops=dict(facecolor='black', shrink=0.05));
```
Tips:
- suptitle和text是对画布操作，所以前面加fig.--;
对子图操作的前面家的是ax。（对应子图名称）
![](https://s2.loli.net/2022/01/20/P9FEN47TsB8MzCb.png)

#### 2.text - 子图上的文本
调用方式：
`Axes.text(x, y, s, fontdict=None, **kwargs)`
-  x,y：文本出现的位置，默认当前坐标下的**坐标**
- s：文本内容
- fontdict：可选参数，覆盖默认的文本属性
- **kwargs：关键字参数，也可以用来传入文本样式参数

fontdict  **kwargs参数：都用于调整呈现的文本样式，且最终效果一样
```
fig = plt.figure(figsize=(10,3))
axes = fig.subplots(1,2)

# 使用关键字参数修改文本样式
axes[0].text(0.3, 0.8, 'modify by **kwargs', style='italic',
        bbox={'facecolor': 'red', 'alpha': 0.5, 'pad': 10});

# 使用fontdict参数修改文本样式
font = {'bbox':{'facecolor': 'red', 'alpha': 0.5, 'pad': 10}, 'style':'italic'}
axes[1].text(0.3, 0.8, 'modify by fontdict', fontdict=font);
```
**bbox**属性：
简单的说就是在用不同的矩形框将文字框起来，并用一系列属性来定义矩形框的。
![](https://s2.loli.net/2022/01/21/tBR4pj65VgezAvJ.png)
[matplotlib支持的所有样式参数][41]
常用参数：
- **alpha**：float or None 透明度——越接近0，越透明
- **bbox**：用来设置text周围的box外框
- **color or c**：字体的颜色
- **fontfamily or family**：{FONTNAME, 'serif', 'sans-serif', 'cursive', 'fantasy', 'monospace'} 字体的类型
- **fontsize or size**：float or {'xx-small', 'x-small', 'small', 'medium', 'large', 'x-large', 'xx-large'} 字体大小
- **fontstyle or style**：{'normal', 'italic', 'oblique'} 字体的样式是否倾斜等
- **fontweight or weight**：{a numeric value in range 0-1000, 'ultralight', 'light', 'normal', 'regular', 'book', 'medium', 'roman', 'semibold', 'demibold', 'demi', 'bold', 'heavy', 'extra bold', 'black'} 文本粗细
- **horizontalalignment or ha**：{'center', 'right', 'left'} 选择文本左对齐右对齐还是居中对齐
- **linespacing**：float (multiple of font size) 文本间距
- **rotation**：float or {'vertical', 'horizontal'} 指text逆时针旋转的角度，“horizontal”等于0，“vertical”等于90
- **verticalalignment or va**：{'center', 'top', 'bottom', 'baseline', 'center_baseline'} 文本在垂直角度的对齐方式

#### 3.xlabel和ylabel - 子图的x，y轴标签
调用方式：
`Axes.set_xlabel(xlabel, fontdict=None, labelpad=None, *, loc=None, **kwargs)`
- **labelpad**：标签和坐标轴的距离，默认为4
- **loc**：标签位置，可选的值为'left', 'center', 'right'之一，默认为居中
-
labelpad和loc参数的使用效果对比
```
fig = plt.figure(figsize=(10,3))
axes = fig.subplots(1,2)、
#labelpad
axes[0].set_xlabel('xlabel',labelpad=20,loc='left')
#loc
axes[1].set_xlabel('xlabel', position=(0.2, _), horizontalalignment='left');
```
Tips：
- loc参数仅能提供**粗略**的位置调；
- 如果想要更**精确**的设置标签的位置，可以使用**position + orizontalalignment参数**来定位
    - position由一个元组过程，第一个元素0.2表示**x轴标签在x轴的位置**，第二个元素对于xlabel其实是无意义的，随便填一个数都可以
    - horizontalalignment='left'表示**左对齐**
![](https://s2.loli.net/2022/01/21/sEVon1XmiHzQGyC.png)

#### 4.title和suptitle - 子图和画布的标题
title调用方式：
`Axes.set_title(label, fontdict=None, loc=None, pad=None, *, y=None, **kwargs)`
- **label**：子图标签的内容
- **pad**：标题偏离图表顶部的距离，默认为6
- **y**：title所在子图垂向的位置，默认值为1，即位于子图的顶部

suptitle调用方式：
`figure.suptitle(t, y, **kwargs)`
- **t**：画布的标题内容

```
# 观察pad参数的使用效果
fig = plt.figure(figsize=(10,3))
fig.suptitle('This is figure title',y=1.2) # 通过参数y设置高度
axes = fig.subplots(1,2)
axes[0].set_title('This is title',pad=15)
axes[1].set_title('This is title',pad=6);
```

#### 5.annotate - 子图的注解
调用方式：
`Axes.annotate(text, xy, *args, **kwargs)`
- text：注解的内容
- xy：注解箭头指向的坐标
- xycoords：定义xy参数的坐标系
- xytext：注解文字的坐标
- textcoords：定义xytext参数的坐标系
- arrowprops：定义指向箭头的样式

[annotate官网详细介绍][42]
```
fig = plt.figure()
ax = fig.add_subplot()
ax.annotate("",
            xy=(0.2, 0.2), xycoords='data',
            xytext=(0.8, 0.8), textcoords='data',
            arrowprops=dict(arrowstyle="->", connectionstyle="arc3,rad=0.2")
            );
```
![](https://s2.loli.net/2022/01/21/4sVFoh5p8zbYKZM.png)

#### 6.字体的属性设置
字体设置有**全局字体设置**和**自定义局部字体设置**两种方法
[中文字体的英文名称][43]
- 全局字体更改
```
plt.rcParams['font.sans-serif'] = ['SimSun']    # 指定默认字体为新宋体。
plt.rcParams['axes.unicode_minus'] = False      # 解决保存图像时 负号'-' 显示为方块和报错的问题。
```
- 局部字体更改
```
#局部字体的修改方法1
x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
plt.plot(x, label='小示例图标签')

# 直接用字体的名字
plt.xlabel('x 轴名称参数', fontproperties='Microsoft YaHei', fontsize=16)         # 设置x轴名称，采用微软雅黑字体
plt.ylabel('y 轴名称参数', fontproperties='Microsoft YaHei', fontsize=14)         # 设置Y轴名称
plt.title('坐标系的标题',  fontproperties='Microsoft YaHei', fontsize=20)         # 设置坐标系标题的字体
plt.legend(loc='lower right', prop={"family": 'Microsoft YaHei'}, fontsize=10) ;   # 小示例图的字体设置
```
![](https://s2.loli.net/2022/01/21/NnumExoFXRDGLAz.png)

### 二.Tick上的文本
tick（刻度）和ticklabel（刻度标签）也是可视化中经常需要操作的步骤。

#### 1.简单模式
- 使用axis的**set_ticks方法**手动设置**标签位置**；
- 使用axis的**set_ticklabels方法**手动设置**标签格式**。

#### 2.Tick Locators and Formatter

### 三.legend（图例）
相关术语：
- legend entry（图例条目)：
  每个图例由一个或多个legend entries组成。一个entry包含一个key和其对应的label。
- legend key（图例键)：
  每个legend label左面的colored/patterned marker（彩色/图案标记）
- legend label（图例标签)：
  描述由key来表示的handle的文本
- legend handle（图例句柄)：
  用于在图例中生成适当图例条目的原始对象

两个legend entry在右侧方框中；
两个legend key，分别是一个蓝色和一个黄色的legend key；
两个legend label，一个名为‘Line up’和一个名为‘Line Down’的legend label
![](https://s2.loli.net/2022/01/21/meqjzQRop78srOi.png)

OO模式和pyplot模式两种方式，都是使用legend()即可调用。

- 在使用legend方法时，我们可以手动传入两个变量，**句柄和标签**，用以指定条目中的特定绘图对象和显示的标签值。
- 更简单的操作是不传入任何参数，此时matplotlib会**自动寻找合适的图例条目**。
```
fig, ax = plt.subplots()
line_up, = ax.plot([1, 2, 3], label='Line 2')
line_down, = ax.plot([3, 2, 1], label='Line 1')
ax.legend(handles = [line_up, line_down], labels = ['Line Up', 'Line Down']);
```

#### 设置图例位置
loc参数接收一个**字符串**或**数字**表示图例出现的位置

|Location String|Location Code|
|-|-|
|'best'|0   |
|'upper right'|1   |
|'upper left'|2   |
|'lower left'|3   |
|'lower right'|4   |
|'right'|5   |
|'center left'|6   |
|'center right'|7   |
|'lower center'|8   |
|'upper center'|9   |
|'center'   |10   |
```
fig,axes = plt.subplots(1,4,figsize=(10,4))
for i in range(4):
    axes[i].plot([0.5],[0.5])
    axes[i].legend(labels='a',loc=i) #loc不同对应的图中legend不同位置
fig.tight_layout()
```
![](https://s2.loli.net/2022/01/21/zjUkF1tNM4ySsqQ.png)

#### 设置图例边框及背景
```
fig = plt.figure(figsize=(10,3))
axes = fig.subplots(1,3)
for i, ax in enumerate(axes):
    ax.plot([1,2,3],label=f'ax {i}')
axes[0].legend(frameon=False) #去掉图例边框
axes[1].legend(edgecolor='blue') #设置图例边框颜色
axes[2].legend(facecolor='gray'); #设置图例背景颜色,若无边框,参数无效
```
![](https://s2.loli.net/2022/01/21/GkPWK8EslyoDNzR.png)

#### 设置图例标题
(title=' ')
```
fig,ax =plt.subplots()
ax.plot([1,2,3],label='label')
ax.legend(title='legend title');
```
![](https://s2.loli.net/2022/01/21/eSMYzg8jpGLhRD2.png)

[4]:https://datawhalechina.github.io/fantastic-matplotlib/%E7%AC%AC%E5%9B%9B%E5%9B%9E%EF%BC%9A%E6%96%87%E5%AD%97%E5%9B%BE%E4%BE%8B%E5%B0%BD%E7%9C%89%E7%9B%AE/index.html
[41]:https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.text.html#matplotlib.axes.Axes.text
[42]:https://matplotlib.org/stable/tutorials/text/annotations.html#plotting-guide-annotation
[43]:https://www.cnblogs.com/chendc/p/9298832.html
