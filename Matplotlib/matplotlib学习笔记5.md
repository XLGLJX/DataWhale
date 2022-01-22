# Matplotlib
## [五.样式色彩秀芳华][5]
*摘自Datawhale Matplotlib提供文章*

绘图样式和颜色是丰富可视化图表的重要手段。
### 一.matplotlib的绘图样式（style）
#### 1.matplotlib预先定义样式
在python脚本的最开始输入想使用style的名称调用
```
plt.style.use('default')
plt.plot([1,2,3,4],[2,3,4,5]);
```
![](https://s2.loli.net/2022/01/22/9rBwZymNnEpzWkc.png)
```
plt.style.use('ggplot')
plt.plot([1,2,3,4],[2,3,4,5]);
```
![](https://s2.loli.net/2022/01/22/2thau8v7dxVYjre.png)
matplotlib内置的可使用样式
```
print(plt.style.available)
```
['Solarize_Light2', '_classic_test_patch', 'bmh', 'classic', 'dark_background', 'fast', 'fivethirtyeight', 'ggplot', 'grayscale', 'seaborn', 'seaborn-bright', 'seaborn-colorblind', 'seaborn-dark', 'seaborn-dark-palette', 'seaborn-darkgrid', 'seaborn-deep', 'seaborn-muted', 'seaborn-notebook', 'seaborn-paper', 'seaborn-pastel', 'seaborn-poster', 'seaborn-talk', 'seaborn-ticks', 'seaborn-white', 'seaborn-whitegrid', 'tableau-colorblind10']

#### 2.用户自定义stylesheet
在任意路径下创建一个后缀名为mplstyle的样式清单，编辑文件添加以下样式内容
>axes.titlesize : 24
axes.labelsize : 20
lines.linewidth : 3
lines.markersize : 10
xtick.labelsize : 16
ytick.labelsize : 16

style引用自定义的stylesheet
```
plt.style.use('file/presentation.mplstyle')
plt.plot([1,2,3,4],[2,3,4,5]);
```
![](https://s2.loli.net/2022/01/22/qpWOT1ZhXQVKxzi.png)
**Tips**:matplotlib支持混合样式的引用，在引用时输入一个样式列表，若是几个样式中涉及到同一个参数，右边的样式表会覆盖左边的值。
```
plt.style.use('dark_background')
plt.plot([1,2,3,4],[2,3,4,5]);
```
![](https://s2.loli.net/2022/01/22/MB1Y7dcvURnZQT8.png)
```
plt.style.use(['dark_background', 'file/presentation.mplstyle'])
plt.plot([1,2,3,4],[2,3,4,5]);
```
![](https://s2.loli.net/2022/01/22/ai5pwvrPnUGmkh3.png)


#### 3.设置rcparams
我们还可以通过修改默认rc设置的方式改变样式，所有rc设置都保存在一个叫做 matplotlib.rcParams的变量中。
`plt.plot([1,2,3,4],[2,3,4,5])`
`plt.style.use('default') # 默认样式`
![markdown-img-paste-20220122174646503.png](https://s2.loli.net/2022/01/22/9rBwZymNnEpzWkc.png)
```
mpl.rcParams['lines.linewidth'] = 2
mpl.rcParams['lines.linestyle'] = '--'
```
![](https://s2.loli.net/2022/01/22/413bJjZqYwlaLms.png)
便捷修改方式：
`mpl.rc('lines', linewidth=4, linestyle='-.')`
![](https://s2.loli.net/2022/01/22/8ryCjEcf3zPJSqD.png)


### 二.matplotlib的色彩设置（color）
从可视化编码的角度对颜色进行分析，可以将颜色分为**色相**、**亮度和饱和度**三个视觉通道。
- **色相**： 没有明显的顺序性、一般不用来表达数据量的高低，而是用来表达数据列的类别。
- **明度和饱和度**： 在视觉上很容易区分出优先级的高低、被用作表达顺序或者表达数据量视觉通道。

`plt.style.use('default')`
#### 1.RGB或RGBA
**color=(red, green, blue, alpha)**
颜色用[0,1]之间的浮点数表示，其中alpha透明度可省略
```
plt.plot([1,2,3],[4,5,6],color=(0.1, 0.2, 0.5))
plt.plot([4,5,6],[1,2,3],color=(0.1, 0.2, 0.5, 0.5));
```
![](https://s2.loli.net/2022/01/22/uIMR5lAcfrZWpC1.png)

#### 2.HEX RGB 或 RGBA
用十六进制颜色码表示，最后两位表示透明度，可省略
```
plt.plot([1,2,3],[4,5,6],color='#0f0f0f')
plt.plot([4,5,6],[1,2,3],color='#0f0f0f80');
```
![](https://s2.loli.net/2022/01/22/eLb8J7jHd3wKMAa.png)

#### 3.灰度色戒
**灰度色阶**:只有一个位于[0,1]的值
```
plt.plot([1,2,3],[4,5,6],color='0.5');
```
![](https://s2.loli.net/2022/01/22/Zp7DshatMHSIein.png)

#### 4.单字符基本颜色
matplotlib有八个基本颜色:
|'b'|'g'|'r'|'c'|'m'|'y'|'k'|'w'|
|-|-|-|-|-|-|-|-|
|blue|green|red|cyan|magenta|yellow|black|white|
```
plt.plot([1,2,3],[4,5,6],color='m');
```
![](https://s2.loli.net/2022/01/22/FrE5imWQBLynM2d.png)

#### 5.颜色名称
```
plt.plot([1,2,3],[4,5,6],color='tan');
```
![](https://s2.loli.net/2022/01/22/g8XkZNe65cAnUMb.png)

matplotlib提供了颜色对照表，可供查询颜色对应的名称:
![](https://s2.loli.net/2022/01/22/9s2Y3JOy8MifUFa.png)
![](https://s2.loli.net/2022/01/22/qtyxbGN3CzncP9S.png)

#### 6.使用colormap设置一组颜色
有些图表支持使用colormap的方式配置一组颜色，从而在可视化中通过色彩的变化表达更多信息。

colormap共有五种类型:
- **顺序（Sequential）**：通常使用**单一色调**，**逐渐改变**亮度和颜色渐渐增加，用于表示**有顺序**的信息
- **发散（Diverging）**：改变两种不同颜色的亮度和饱和度，这些颜色在中间以不饱和的颜色相遇;当绘制的信息具有**关键中间值**（例如地形）或**数据偏离零**时，应使用此值。
- **循环（Cyclic）**：改变两种不同颜色的亮度，在中间和开始/结束时以不饱和的颜色相遇。用于在端点处环绕的值，例如相角，风向或一天中的时间。
- **定性（Qualitative）**：常是杂色，用来表示**没有排序或关系**的信息。
- **杂色（Miscellaneous）**：一些在特定场景使用的**杂色组合**，如彩虹，海洋，地形等。

```
x = np.random.randn(50)
y = np.random.randn(50)
plt.scatter(x,y,c=x,cmap='RdPu');
```
![](https://s2.loli.net/2022/01/22/SFTKjWec61ZY4km.png)

[官网中五种colormap的字符串表示和颜色图的对应关系][51]

[5]:https://datawhalechina.github.io/fantastic-matplotlib/%E7%AC%AC%E4%BA%94%E5%9B%9E%EF%BC%9A%E6%A0%B7%E5%BC%8F%E8%89%B2%E5%BD%A9%E7%A7%80%E8%8A%B3%E5%8D%8E/index.html
[51]:https://matplotlib.org/stable/tutorials/colors/colormaps.html
