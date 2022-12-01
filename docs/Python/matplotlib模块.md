## 简介

matplotlib 是一个用于创建出版质量图表的桌面绘图包（主要是 2D）。

在 Jupyter notebook 进行交互式绘图：`%matplotlib notebook`
通常，它以这样的方式引入：`import matplotlib.pyplot as plt`

## Figure 和 Subplot

matplotlib 的图像都位于 Figure 对象中。使用 plt.figure 创建一个新的 Figure:

`fig = plt.figure()`

不能在空的 Figure 上面画图，你需要通过 `add_subplot` 创建一个或多个 subplot 才行：

```python
ax1 = fig.add_subplot(2, 2, 1)
ax2 = fig.add_subplot(2, 2, 2)
ax3 = fig.add_subplot(2, 2, 3)

# 紧接着调用 plot 画图, k-- 表示绘制黑色虚线图。
plt.plot(np.random.randn(50).cumsum(), 'k--')
```

plt 的绘图会画在 ax3 上，matplotlib 会在最后一个用过的 subplot 上进行绘制（如果没有则自动新建一个）。然后你可以在 ax1 和 ax2 上面画图：

```python
ax1.hist(np.random.randn(100), bins=20, color='k', alpha=0.3)
ax2.scatter(np.arange(30), np.arange(30) + 3 * np.random.randn(30))
```

结果展示：

![](https://charmy-1256878123.cos.ap-nanjing.myqcloud.com/imgs/20221122231716.png)

不过，matplotlib 有一个更为方便的方法 `plt.subplots`，它可以创建一个新的 Figure，并返回一个含有已创建的 subplot 对象的 NumPy 数组

```python
In [2]: import matplotlib.pyplot as plt

In [3]: fig, axes = plt.subplots(2, 3)

In [4]: axes
Out[4]: 
array([[<AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>],
       [<AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>]], dtype=object)
```

你可以通过下标索引来指定 axes： `axes[0, 1]`。另外，你可以通过 sharex 和 sharey 指定它们是否具有相同的 X 轴或者 Y 轴。

|    参数    |                                  说明                                  |
| :--------: | :--------------------------------------------------------------------: |
|   nrows    |                             subplot 的行数                             |
|   ncols    |                             subplot 的列数                             |
|   sharex   | 所有 subplots 应该使用相同的 X 轴刻度（调节 xlim 将会影响所有 subplot) |
|   sharey   | 所有 subplots 应该使用相同的 Y 轴刻度（调节 ylim 将会影响所有 subplot) |
| subplot_kw |                               创建各 subplot 的关键字字典                               |
|  **fig_kw  | 创建 figure 时的其他关键字，例如 plt.subplots(2, 2, figsize=(8,6)) |


## 调整

### subplot 周围的间距

使用 Figure 的 subplots_adjust 方法可以轻而易举地修改间距，此外，它也是一个顶级函数：

```python
subplots_adjust(left=None, bottom=None, right=None, top=None, wspace=None, hspace=None)
```

其中，wspace 和 hspace 控制宽度和高度的百分比，可以用作 subplot 之间的间距。

例如，在绘图之后，调用 `plt.subplot_adjust(wspace = 0, hspace = 0)` 会使得所有子图都紧挨着。在此之前你必须手动处于刻度位置和刻度标签，不然会重叠在一起。

### 颜色，标记和线型

matplotlib 的 plot 函数接受一组 X 和 Y 坐标，还可以接受一个表示颜色和线型的字符串缩写。例如 `ax.plot(x, y, 'g--')` 可以绘制绿色虚线。

当然，你也可以分别指定：

`ax.plot(x, y, linestyle='--', color='g')`。

再例如：

- `plt.plot(randn(30).cumsum(), 'ko--')` 
- `plt.plot(randn(30).cumsum(), color='k', linestyle='dashed', marker='o')` 

上面两种不同的操作有着同样的效果。

颜色对应关系

| 简称 |  颜色   |
| :--: | :-----: |
|  b   |  blue   |
|  c   |  cyan   |
|  g   |  green  |
|  k   |  black  |
|  m   | magenta |
|  r   |   red   |
|  w   |  white  |
|  y   |  yello  |

![](https://charmy-1256878123.cos.ap-nanjing.myqcloud.com/imgs/20221123191905.png)

附上 RGB 代码转换网站：http://www.sioe.cn/yingyong/yanse-rgb-16/

控制线性

| 简称 |    线性    |
| :--: | :--------: |
|  -   |    实线    |
|  --  |    短线    |
|  -.  | 短点相间线 |
|  :   |    虚线    |

控制标记：

1. `.`, `,`, `o`, `v`, `^`, `<`, `>`
2. `1`, `2`, `3`, `4`
3. `s`, `p`, `*`, `h`, `H`, `d`, `|`, `_`, `+`, `x`

### 设置标题、轴标签、刻度以及刻度标签

要改变 x 轴刻度，可以使用 `set_xticks` 和 `set_xticklabels`。

```python
ticks = ax.set_xticks([0, 250, 500, 700, 1000])
labels = ax.set_xticklabels(['one', 'two', 'three', 'four', 'five'], rotation = 30, fonsize = 'small')
```

其中，rotation 选项设定 $x$ 刻度倾斜 30 度。

然后，设置图的标题以及 X 轴的名称：

```python
ax.set_title('My first matplotlib plot')
ax.set_xlabel('Stages')
```
Y 轴阈值类似，除此之外，你可以使用集合方法来批量设定：

```python
props = {
    "title" : "My first matplotlib plot"
    "xlabel" : "Stages"
}
ax.set(**props)
```

### 添加图例

添加图例的方式有很多，最简单的是直接在创建 subplot 的时候传入 label 参数：

```python
ax.plot(randn(1000).cumsum(), 'k', label = 'one')
```

然后，调用 ax.legend() 或者 plt.legend() 来自动创建图例：

```python
ax.legend(loc = 'best')
```

legend 方法有几个位置参数，例如：
- `upper left`, `upper center`, `upper right`
- `center left`, `center`, `center right`
- `lower left`, `lower center`, `lower right`

## 注解以及在 subplot 上绘图

除了标准的绘图，你可能还希望在一些子集上绘制一些子集的注解，可能是文本、箭头或其他图形等。

注解和文字可以通过 text, arrow, annotate 函数进行添加。text 可以将文本绘制在图表的指定坐标 $(x, y)$，还可以加上一些自定义格式：

```python
ax.text(x, y, 'Hello workld!', family='monospace', fontsize=10)
```


!!! example 

    下面是加箭头注解的一个例子，使用最近的普尔 500 指数价格（来自Yahoo!Fiance）绘制一张曲线图，并标注出一些重要时间。

    ```python
    from datetime import datetime 
    fig = plt.figure() 
    ax = fig.add_subplot(1, 1, 1) 
    data = pd.read_csv('examples/spx.csv', index_col=0, parse_dates=True) 
    spx = data['SPX'] 
    spx.plot(ax=ax, style='k-') 
    crisis_data = [ (datetime(2007, 10, 11), 'Peak of bull market'), 
                    (datetime(2008, 3, 12), 'Bear Stearns Fails'), 
                    (datetime(2008, 9, 15), 'Lehman Bankruptcy') ] 

    for date, label in crisis_data: 
        ax.annotate(label, xy=(date, spx.asof(date) + 75), 
                    xytext=(date, spx.asof(date) + 225), 
                    arrowprops=dict(facecolor='black', headwidth=4, width=2, headlength=4), 
                    horizontalalignment='left', verticalalignment='top') 

    # Zoom in on 2007-2010 
    ax.set_xlim(['1/1/2007', '1/1/2011']) 
    ax.set_ylim([600, 1800]) 

    ax.set_title('Important dates in the 2008-2009 financial crisis')
    ```

    ![](https://charmy-1256878123.cos.ap-nanjing.myqcloud.com/imgs/20221123125550.png)

图形的绘制要麻烦一些，一般也不常用。需要创建一个块 shp，然后通过 `ax.add_patch(shp)` 将其添加到 subplot 中。

## 将图表保存到文件

利用 plt.savefig 可以将当前图表保存到文件。该方法相当于 Figure 对象的实例方法 savefig。

```python
plt.savefig('figpath.svg')
```

文件的类型是通过文件名后缀推测出来的。最常用的两个参数是 dpi 和 bbox_inches，分别控制每英寸点数（即分辨率）和图片边缘空白。

```python
plt.savefig('figpath.png', dpi = 400, bbox_inches = 'tight')
```

当然，他还可以写入到任何文件型的对象，比如 BytesIO：

```python
from io import BytesIO
buffer = BytesIO()
plt.savefig(buffer)
plot_data = buffer.getvalue()
```

关于 savefig 的其他参数：

|         参数         |                                说明                                |
| :------------------: | :----------------------------------------------------------------: |
|        fname         |                          文件名或文件对象                          |
|         dpi          |                        图像分辨率，默认 100                        |
| facecolor, edgecolor |                   图像背景色，默认 `w` （白色）                    |
|        format        |                          显示设置文件格式                          |
|     bbox_inches      | 图标需要保存的部分，如果是 `tight`，则将尝试剪除图表周围的空白部分 |

## matplotlib 配置

几乎所有默认行为都可以通过一组全局参数进行自定义，它们可以管理图像大小，supblot 边距、配色方案、字体大小、网格类型等。一种方法是使用 rc 方法。

例如，要将全局图像默认大小设置为 10*10，你可以执行：

```python
plt.rc('figure', figsize=(10,10))
```

rc 的第一个参数是希望自定义的对象，例如 `figure, axes, xtick, ytick, grid, legend` 等。其后跟上一系列的关键字参数。将这些选项写成参数会更方便：

```python
font_options = {
    "family" : "monospace", # 如需中文，改为 SimHei
    "weight" : "bold",
    "size"   : "small"
}
plt.rc("font", **font_options)
```

!!! tip 
    关于设置中文显示，可以参考 [python设置 matplotlib 正确显示中文的四种方式](https://www.w3cschool.cn/article/62227500.html#:~:text=%E4%BA%8C%E3%80%81%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%201%201.%20%E6%96%B9%E5%BC%8F%E4%B8%80%20from%20matplotlib.font_manager%20import%20FontProperties,%E6%96%B9%E5%BC%8F%E4%B8%89%20%E6%94%B9%E5%8F%98%E5%85%A8%E5%B1%80%E7%9A%84%E5%AD%97%E4%BD%93%20...%204%204.%20%E6%96%B9%E5%BC%8F%E5%9B%9B%20%E5%90%8C%E6%A0%B7%E4%B9%9F%E6%98%AF%E5%85%A8%E5%B1%80%E6%94%B9%E5%8F%98%E5%AD%97%E4%BD%93%E7%9A%84%E6%96%B9%E6%B3%95%20)

    救急方案：
    ```python
    font = {'family' : 'SimHei',
        'weight' : 'bold',
        'size'   : '16'}
    plt.rc('font', **font)               # 步骤一（设置字体的更多属性）
    plt.rc('axes', unicode_minus=False)  # 步骤二（解决坐标轴负数的负号显示问题）
    ```

    如果没有对应字体，需要去下载。

通过 `matplotlib.get_configdir()` 可以找到用户默认配置所处的位置，通常是在用户家目录下的隐藏文件中。在这里修改配置将对该用户全局生效。通过 `matplotlib.matplotlib_fname()` 可以找到当前使用的配置文件。

比较推荐的方法是，针对同一项目建议一份配置文件。在项目目录下创建 `matplotlibrc` 即可。


### 风格

查看内置风格：

```python
In [1]:  print(plt.style.available)
['Solarize_Light2', '_classic_test_patch', 'bmh', 'classic', 'dark_background', 'fast', 'fivethirtyeight', 'ggplot', 'grayscale', 'seaborn', 'seaborn-bright', 'seaborn-colorblind', 'seaborn-dark', 'seaborn-dark-palette', 'seaborn-darkgrid', 'seaborn-deep', 'seaborn-muted', 'seaborn-notebook', 'seaborn-paper', 'seaborn-pastel', 'seaborn-poster', 'seaborn-talk', 'seaborn-ticks', 'seaborn-white', 'seaborn-whitegrid', 'tableau-colorblind10']
```

这里展示了每种风格的样子：[Style sheets reference](https://matplotlib.org/stable/gallery/style_sheets/style_sheets_reference.html)

每种风格的配置代码都在 `xx\Lib\site-packages\matplotlib\mpl-data\stylelib` 下，可以自己模仿进行配置。

如果要使用 ggplot 风格：

```python
plt.style.use('ggplot')
```



## 参考 

- https://matplotlib.org/
- https://zhuanlan.zhihu.com/p/137677737
- https://www.runoob.com/w3cnote/matplotlib-tutorial.html