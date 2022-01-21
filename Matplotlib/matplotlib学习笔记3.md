# Matplotlib
## [三.布局格式定方圆][3]
摘自Datawhale Matplotlib提供文章

```
plt.rcParams['font.sans-serif'] = ['SimHei']   #用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False   #用来正常显示负号
```
### 一. 子图
#### 1. 使用plt.subplots绘制均匀状态下的子图

##### subplots
**subplots**是基于OO模式的写法，显式创建一个或多个axes对象，然后在对应的子图对象上进行绘图操作。

`fig, axs = plt.subplots(2, 5, figsize=(10, 4), sharex=True, sharey=True)`
- 第一个数字为行，第二个为列，不传入时默认值都为1
- figsize 参数可以指定整个画布的大小
- sharex 和 sharey 分别表示是否共享横轴和纵轴刻度
    - True：每行列只有外围图形有横纵轴刻度
    - False：每个子图都有横纵轴刻度

**tight_layout() 函数**可以调整子图的相对大小使字符不会重叠
```
fig, axs = plt.subplots(2, 5, figsize=(10, 4), sharex=True, sharey=True) #创建了2*5个子图，通过二维数组形式进行调用
fig.suptitle('样例1', size=20)
for i in range(2):
    for j in range(5):
        axs[i][j].scatter(np.random.randn(10), np.random.randn(10))
        axs[i][j].set_title('第%d行，第%d列'%(i+1,j+1))
        axs[i][j].set_xlim(-5,5)
        axs[i][j].set_ylim(-5,5)
        if i==1: axs[i][j].set_xlabel('横坐标')
        if j==0: axs[i][j].set_ylabel('纵坐标')
fig.tight_layout()
```

###### subplot
使用**基于pyplot模式的subplot**，每次在指定位置新建一个子图，并且之后的绘图操作都会指向当前子图。
（本质上subplot也是Figure.add_subplot的一种封装）
subplot(x,y,z)：一共x行，y列，当前是第z个图
```
plt.figure()
# 子图1
plt.subplot(2,2,1)
plt.plot([1,2], 'r')
# 子图2
plt.subplot(2,2,2)
plt.plot([1,2], 'b')
#子图3
plt.subplot(224)
plt.plot([1,2], 'g');
```
plt.subplot(224): **当三位数都小于10时，可以省略中间的逗号，这行命令等价于plt.subplot(2,2,4)**

###### projection
通过projection方法创建极坐标系下的图表
 **projection = 'polar'** 指定为 **极坐标**
```
N = 150
r = 2 * np.random.rand(N)
theta = 2 * np.pi * np.random.rand(N)
area = 200 * r**2
colors = theta

plt.subplot(projection='polar')
plt.scatter(theta, r, c=colors, s=area, cmap='hsv', alpha=0.75);
```
![markdown-img-paste-20220118170301569.png](https://s2.loli.net/2022/01/18/hcl2UAPtOCGzjyY.png)

##### GridSpec
使用 GridSpec 绘制非均匀子图
非均匀：
1. 比例大小不同但没有跨行或跨列
2. 图为跨列或跨行状态

**add_gridspec()**：指定**相对**宽度比例 *width_ratios* 和**相对**高度比例 *height_ratios*
（即在当前画布下，自适应每个子图的比例）
```
fig = plt.figure(figsize=(10, 4))
spec = fig.add_gridspec(nrows=2, ncols=5, width_ratios=[1,2,3,4,5], height_ratios=[1,3])
#2*5的图
fig.suptitle('样例2', size=20)
for i in range(2):
    for j in range(5):
        ax = fig.add_subplot(spec[i, j])
        ax.scatter(np.random.randn(10), np.random.randn(10))
        ax.set_title('第%d行，第%d列'%(i+1,j+1))
        if i==1: ax.set_xlabel('横坐标')
        if j==0: ax.set_ylabel('纵坐标')
fig.tight_layout()
```
![](https://s2.loli.net/2022/01/18/YlTN5XuGhs6A2SH.png)

**spec[i, j]** 选择画图上对应的子图
**spec[a:b , c:d]** 实现子图的合并，达到跨图的功能——合并[a,b)行，[c,d)列的子图为一个大图。
```
# sub1
ax = fig.add_subplot(spec[0, :3])
ax.scatter(np.random.randn(10), np.random.randn(10))
# sub3
ax = fig.add_subplot(spec[:, 5])
ax.scatter(np.random.randn(10), np.random.randn(10))
# sub5
ax = fig.add_subplot(spec[1, 1:5])
ax.scatter(np.random.randn(10), np.random.randn(10))
```
![](https://s2.loli.net/2022/01/18/uIrFypanGW7vQAZ.png)



### 子图上的方法
1. 直线的画法：
    - 水平直线 **axhline()**:
          axhline(y=0, xmin=0, xmax=1, **kwargs)
          y：浮点型，水平线在坐标中点y的位置
          xmin：取值[0,1]——0为图的左边界，1为图的右边界
          ymax: 取值[0,1]——同上
          **kwargs：可视化其他关键参数
    - 垂直直线 **axvline()**
          同上
    - 任意方向直线 **axline()**
          绘制出无限长的直线
          axline(xy1, xy2=None, *, slope=None, **kwargs)
          xy1:直线经过的点坐标，浮点数二元组
          xy2:同上，默认None
            （**xy2和slope同时只能有一个值有效。两者既不能同时为None，也不能同时为非空值**）
          slope：直线的斜率。浮点数，默认值为None。
    ```
    fig, ax = plt.subplots(figsize=(4,3))
    ax.axhline(0.5,0.2,0.8)
    ax.axvline(0.5,0.2,0.8)
    ax.axline([0.3,0.3],[0.7,0.7]);
    ```
    ![](https://s2.loli.net/2022/01/18/3c4uR7QYZL1n2Fz.png)

2. 添加灰色网格
    **grid()** 添加灰色网格 True添加
    ```
    fig, ax = plt.subplots(figsize=(4,3))
    ax.grid(True)
    ```
    ![](https://s2.loli.net/2022/01/18/hiuatpCn17k3TzJ.png)
3. 设置坐标轴规度
    **set_xscale** 或 **set_yscale**设置对应坐标轴为对数坐标
    set_xscale('log')
    ```
    fig, axs = plt.subplots(1, 2, figsize=(10, 4))
    for j in range(2):
        axs[j].plot(list('abcd'), [10**i for i in range(4)])
        if j==0:
            axs[j].set_yscale('log')
        else:
            pass
    fig.tight_layout()
    ```
    ![](https://s2.loli.net/2022/01/18/xB64LqTY37abRsd.png)

[3]:https://datawhalechina.github.io/fantastic-matplotlib/%E7%AC%AC%E4%B8%89%E5%9B%9E%EF%BC%9A%E5%B8%83%E5%B1%80%E6%A0%BC%E5%BC%8F%E5%AE%9A%E6%96%B9%E5%9C%86/index.html "布局格式定方圆"
