# Matplotlib
## [一.Matplotlib初相识][1]
### 认识matplotlib

Matplotlib是一个Python 2D绘图库，能够以多种硬拷贝格式和跨平台的交互式环境生成出版物质量的图形，用来绘制各种静态，动态，交互式的图表。

### 绘图例子
Matplotlib的图像是画在figure上的（一个窗口）
每一个figure又包含了若干个axes（一个可以指定坐标系的子区域）
```
fig, ax = plt.subplots()  # 创建一个包含一个axes的figure
ax.plot([1, 2, 3, 4], [1, 4, 2, 3]);  # 绘制图像
plt.show() #展示图像
```

### Figure的组成
一个完整的matplotlib图像通常会包括以下四个层级
- Figure：顶层级，用来容纳所有绘图元素
- Axes：matplotlib宇宙的核心，容纳了大量元素用来构造一幅幅子图，一个figure可以由一个或多个子图组成
- Axis：axes的下属层级，用于处理所有和坐标轴，网格有关的元素
- Tick：axis的下属层级，用来处理所有和刻度有关的元素
每个层级也被称为容器（container）
<br>
**在matplotlib的世界中，我们将通过各种命令方法来操纵图像中的每一个部分，从而达到数据可视化的最终效果，一副完整的图像实际上是各类子元素的集合。**
<br>
### 两种绘图接口
- 显式创建figure和axes，在上面调用绘图方法，也被称为OO模式（object-oriented style)
- 依赖pyplot自动创建figure和axes，并绘图


1. 创建figure和axes（OO模式）
```
x = np.linspace(0, 2, 100)

fig, ax = plt.subplots()  
ax.plot(x, x, label='linear')  
ax.plot(x, x**2, label='quadratic')  
ax.plot(x, x**3, label='cubic')  

ax.set_xlabel('x label')     //x轴标签
ax.set_ylabel('y label')     //y轴标签
ax.set_title("Simple Plot")  //图像标题
ax.legend()                  //图像上角的图框，用来展示每条曲线的名称以及对应的颜色(图例)
plt.show()
```
2. 依赖pyplot自动创建figure和axes
```
x = np.linspace(0, 2, 100)

plt.plot(x, x, label='linear')
plt.plot(x, x**2, label='quadratic')  
plt.plot(x, x**3, label='cubic')

plt.xlabel('x label')
plt.ylabel('y label')
plt.title("Simple Plot")
plt.legend()
plt.show()
```

二者对比
- OO模板：
由于先创建figure和axes，所以我们可以设置多个axes，即同时创建多个图像，而且创建后在上面调用绘图函数的方法，使该模式拥有更强的兼容性和可扩展性，但这也意味着会相对写法更麻烦；

- pyplot模板：
调用方便，直接就可调用进行绘图，但每次也只能花一个图像。


### 通用绘图模板
有了绘图模板，我们可以在模板框架中填充所需语句模块。只要了解每个模块在做什么，即可快速完成图表的制作。
- OO模式模板
```
# step1 准备数据
x = np.linspace(0, 2, 100)
y = x**2

# step2 设置绘图样式
mpl.rc('lines', linewidth=4, linestyle='-.')

# step3 定义布局
fig, ax = plt.subplots()  

# step4 绘制图像
ax.plot(x, y, label='linear')  

# step5 添加标签，文字和图例
ax.set_xlabel('x label')
ax.set_ylabel('y label')
ax.set_title("Simple Plot")  
ax.legend() ;
```
- pyplot模板
```
# step1 准备数据
x = np.linspace(0, 2, 100)
y = x**2

# step2 设置绘图样式
mpl.rc('lines', linewidth=4, linestyle='-.')

# step3 定义布局

# step4 绘制图像
plt.plot(x, y, label='linear')  

# step5 添加标签，文字和图例
plt.xlabel('x label')
plt.ylabel('y label')
plt.title("Simple Plot")  
plt.legend() ;
```
[1]: https://datawhalechina.github.io/fantastic-matplotlib/%E7%AC%AC%E4%B8%80%E5%9B%9E%EF%BC%9AMatplotlib%E5%88%9D%E7%9B%B8%E8%AF%86/index.html#id2  "一.Matplotlib初相识"
