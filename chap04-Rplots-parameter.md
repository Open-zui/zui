## 基本图形参数

- 高级函数作出的图形包含默认的参数设置, 若此无法满足要求, 则可对已有图形进行修饰: 修改参数值或通过低级函数添加新的元素.

- 图形参数可通过函数 `par()` 或者具体作图函数(如 `plot()` 或 `lines()`)的参数进行修改, 二者的区别在于, 前者的设置持续起作用, 后者的设置起临时作用. 同时应注意, 有些参数只能通过函数 `par()` 设置 (粗体表示). 这里介绍常用的参数以及一些可添加元素的低级函数.

### 符号与线条

参数      | 描述
--------- | -------
pch       | 数据点的形状 (取值 0-25，21-25 可填充颜色), 参见图 2.11
cex       | 数据符号或文本的大小, 表示相对于默认大小的缩放倍数
lty       | 线条类型 (取值 1-6), 参见图 2.12
lwd       | 线条宽度, 表示相对于默认大小的缩放倍数
ljoin     | 线条连接处的类型 (取值 0-2)
lend      | 线条末端的类型 (取值 0-2)


##### 图 2.11
```{r}
png(file = "pic-Rplot-21.png")
par(cex = 1.2, mar = c(0.5, 1, 0.5, 1))
plot(x = 6, y = 5, xlim = c(0.8, 6.2), ylim = c(0, 6), pch = 25, ann = FALSE, axes = FALSE)

pch <- 0
for(x1 in 1:5){
  pch = pch
  for(y1 in 5:1){
    points(x1, y1, pch = pch)
    text(x1, y1, pch, pos = 2)
    pch = pch + 1
  }
}
text(6, 5, 25, 2)
box(lwd = 0.5)
```

##### 图 2.12

```{r}
png(file = "pic-Rplot-22.png")
par(lwd = 2, mar = c(0.5, 1, 0.5, 1))
plot(x = 1:5, y = rep(1, 5), type = "l", xlim = c(0.8, 5), ylim = c(0.5, 6.8), lty = 1, ann = FALSE, axes = FALSE)

lty <- 2
for(i in 2:6){
  lty = lty
  lines(x = 1:5, y = rep(i, 5), lty = lty)
  text(0.8, i, i)
  lty = lty + 1
}
text(0.8, 1, 1)
box(lwd = 0.5)
```

- 用以添加符号或线条的低级函数, 效果参见图 2.13

函数        | 描述                        | 用法
----------- | --------------------------- | ---------
points()    | 在坐标点 (x, y)处绘制符号   | points(x, y, type, ...)
lines()     | 在坐标点 (x, y)之间绘制线条 | lines(x, y, type, ...)
abline()    | 通过斜率和截距绘制一条直线  | abline(a, b, h, v, ...)
segments()  | 在两个坐标之间绘制线段      | segments(x1, y1, x2, y2, ...)
arrows()    | 绘制箭头线                  | arrows(x1, y1, x2, y2, length, angle, code, ...)
xspline()   | 绘制光滑曲线                | xspline(x, y, shape, open, ...)
grid()      | 绘制网格线                  | grid(nx, ny, ...)

##### 图 2.13

```{r}
png(file = "pic-Rplot-23.png")
set.seed(1234)
par(mar = c(3, 5, 2, 4), bty = "l")
plot(1:10, type = "n", xlim = c(0, 12), ylim = c(0, 12), xlab = "", ylab = "", xaxs = "i", yaxs = "i")
points(2:6, c(10, 8, 10, 8, 10), pch = 21, col = rainbow(5), cex = 15)
lines(0:12, runif(13, 0.0, 3.0), lty = 2, lwd = 1.5)
abline(a = 0, b = 1, col = "darkgray")
segments(0, 6, 6, 6, lty = 2, col = "black")
segments(6, 0, 6, 6, lty = 2, col = "black")
arrows(9, 8, 9, 3, length = 0.3, code = 3, col = "red")
grid()
box()
```


### 颜色

参数      | 描述
--------- | -------
col       | 符号和线条的颜色
col.axis  | 坐标轴刻度文字的颜色
col.lab   | 坐标轴标签的颜色
col.main  | 主标题颜色
col.sub   | 副标题颜色
fg        | 前景色
bg        | 背景色

- 颜色设置可通过直接指定颜色名称, 如 "red"

- 或通过函数 `rgb()`, `hsv()`, `hcl()`, `gray()` 等生成系列颜色, 例子参见图 2.14 左上和右上

- 或通过颜色集合函数 `rainbow()`, `heat.colors()`, `terrain.colors()`, `topo.colors`, `cm.colors`生成渐变色，例子参见图 2.14 左下和右下

##### 图 2.14

```{r}
png(file = "pic-Rplot-24.png")
par(mfrow = c(2, 2), mar = c(0.5, 0.1, 0.5, 0.1))
barplot(rep(1, 40), col = rgb(1, seq(0, 1, length = 40), 0.5), axes = FALSE,ann = FALSE)
barplot(rep(1, 40), col = gray(seq(0, 1, 0.02)), axes = FALSE,ann = FALSE)
barplot(rep(1, 40), col = rainbow(40, s = 0.8, v = 1, start = 0), axes = FALSE,ann = FALSE)
barplot(rep(1, 40), col = cm.colors(40), axes = FALSE,ann = FALSE)
```


### 文本

参数         | 描述
------------ | -------
**ps**       | 文本尺寸, 指定字体的绝对大小
cex          | 指定字体相对于默认大小的缩放倍数, 文本的最终大小为 ps*cex
cex.axis     | 坐标轴刻度文字的缩放倍数
cex.lab      | 坐标轴标签的缩放倍数
cex.main     | 主标题的缩放倍数
cex.sub      | 副标题的缩放倍数
family       | 文本字体族, 标准的取值有serif(衬线)、sans(无衬线) 和 mono(等宽), 参见图 2.15
font         | 字体样式, 1 = 常规, 2 = 粗体, 3 = 斜体, 4 = 粗斜体, 5 = 符号字体(以 Adobe 符号编码表示), 参见图 2.15
font.axis    | 坐标轴刻度文字的字体样式
font.lab     | 坐标轴标签 (名称) 的字体样式
font.main    | 标题的字体样式
font.sub     | 副标题的字体样式
adj          | 文本水平或垂直移动, 取值 0-1, 默认 c(0.5, 0.5)
srt          | 文本旋转角度, 起点为 x 轴正方向
**lheight**  | 多行文本的间隔, 默认为 1

##### 图 2.15

```{r}
png(file = "pic-Rplot-25.png")
par(mar = c(0.4, 0.8, 0.4, 0.8))
plot(-1, 0, xlim = c(0.5, 4.5), ylim =  c(3, 9), ann = FALSE, axes = FALSE)

text(0.8, 8, paste("family = \"serif\"", "font = 1", sep = "\n"), family = "serif", font = 1, cex = 0.8)
text(0.8, 6, paste("family = \"sans\"", "font = 1", sep = "\n"), family = "sans", font = 1, cex = 0.8)
text(0.8, 4, paste("family = \"mono\"", "font = 1", sep = "\n"), family = "mono", font = 1, cex = 0.8)

text(1.6, 8, paste("family = \"serif\"", "font = 2", sep = "\n"), family = "serif", font = 2, cex = 0.8)
text(1.6, 6, paste("family = \"sans\"", "font = 2", sep = "\n"), family = "sans", font = 2, cex = 0.8)
text(1.6, 4, paste("family = \"mono\"", "font = 2", sep = "\n"), family = "mono", font = 2, cex = 0.8)

text(2.4, 8, paste("family = \"serif\"", "font = 3", sep = "\n"), family = "serif", font = 3, cex = 0.8)
text(2.4, 6, paste("family = \"sans\"", "font = 3", sep = "\n"), family = "sans", font = 3, cex = 0.8)
text(2.4, 4, paste("family = \"mono\"", "font = 3", sep = "\n"), family = "mono", font = 3, cex = 0.8)

text(3.2, 8, paste("family = \"serif\"", "font = 4", sep = "\n"), family = "serif", font = 4, cex = 0.8)
text(3.2, 6, paste("family = \"sans\"", "font = 4", sep = "\n"), family = "sans", font = 4, cex = 0.8)
text(3.2, 4, paste("family = \"mono\"", "font = 4", sep = "\n"), family = "mono", font = 4, cex = 0.8)

text(4, 8, paste("family = \"serif\"", "font = 5", sep = "\n"), family = "serif", font = 5, cex = 0.8)
text(4, 6, paste("family = \"sans\"", "font = 5", sep = "\n"), family = "sans", font = 5, cex = 0.8)
text(4, 4, paste("family = \"mono\"", "font = 5", sep = "\n"), family = "mono", font = 5, cex = 0.8)
box(lwd = 0.5)
```

- 用以添加文本的低级函数, 效果参见图 2.16

函数        | 描述                        | 用法
----------- | --------------------------- | ---------
title()     | 添加标题及坐标轴标签        | title(main, sub, xlab, ylab, ...)
text()      | 在坐标点 (x, y) 处添加文本  | text(x, y, labels, ...)
mtext()     | 在边界添加文本              | mtext(text, side, line, outer, at, ...)
legend()    | 添加图例                    | legend(x, y, legend, fill, angle, density, ...)

##### 图 2.16

```{r}
png(file = "pic-Rplot-26.png")
plot(1:10, type = "n", xlim = c(0, 12), ylim = c(0, 12), xlab = "", ylab = "")
title(main = "main-title", sub = "sub-title", xlab = "xlab", ylab = "ylab")
segments(c(2, 6), c(10, 2), c(6, 10), c(2, 10), lty = c(2, 6), col = c("red", "blue"), lwd = 2)
legend(10.5, 11, c("Red", "Blue"), lty = c(2, 6), col = c("Red", "Blue"), cex = 0.8, bty = "n")
mtext("mtext(side = 4)", side = 4, line = 0.8, outer = FALSE, las = 0)
```


### 坐标轴

参数           | 描述
-------------- | -------
lab            | 坐标轴刻度线数目, 形式为 c(x, y, len)
las            | 坐标轴刻度标签样式, 0 = 平行于坐标轴, 1 = 总是水平, 2 = 垂直于坐标轴, 3 = 总是竖直, 参见图 2.17
mgp            | 默认为 c(3, 1, 0), 三个数字分别表示坐标轴标题、刻度线标签、坐标轴线偏离绘图区域的文本行数, 参见图 2.17
tcl            | 坐标轴刻度线的长度, 取值为一文本行高的缩放比, 正值表明刻度线向内画, 负值表明刻度线向外画, 参见图 2.17
tck            | 坐标轴刻度线的长度, 默认为 NA(不使用)
xaxs, yaxs     | 坐标轴范围的计算方式, 默认 "r", 表示坐标轴范围比原始数据范围大; 还可取 "i", 表示两个范围一致, 参见图 2.17
xaxt, yaxt     | 坐标轴类型, 默认 "s", 表示需要绘制坐标轴; "n"则表示不需要绘制坐标轴, 但会留下框架线
xaxp, yaxp     | 坐标轴刻度的个数
**xlog, ylog** | 坐标是否取对数, 默认 FALSE
**usr**        | 坐标轴尺度范围, c(x1, x2, y1, y2), 分别表示x轴的左右极限和y轴的下上极限

##### 图 2.17

```{r}
png(file = "pic-Rplot-27.png")
par(mfrow = c(2, 2), lheight = 2, bty = "l")

plot(-1, -1, xlim = c(0, 5), ylim = c(0,5), xlab = "xlab", ylab = "ylab", xaxt = "n", las = 0, mgp = c(3, 1, 0), tcl = 0.2, xaxs = "r")
text(2.5, 3, paste("las = 0, mgp = c(3, 1, 0)", "tcl = 0.2, xaxs = \"r\"", sep = "\n"))
axis(1, 0:5, 0:5, las = 0, col = "red", tcl = 0.2, mgp = c(3, 1, 0))

plot(-1, -1, xlim = c(0, 5), ylim = c(0,5), xlab = "xlab", ylab = "ylab", xaxt = "n", las = 1, mgp = c(1.5, 1, 0), tcl = 0.6, xaxs = "r")
text(2.5, 3, paste("las = 1, mgp = c(1.5, 1, 0)", "tcl = 0.6, xaxs = \"r\"", sep = "\n"))
axis(1, 0:5, 0:5, las = 1, col = "red", tcl = 0.6, mgp = c(1.5, 1, 0))

plot(-1, -1, xlim = c(0, 5), ylim = c(0,5), xlab = "xlab", ylab = "ylab", xaxt = "n", las = 2, mgp = c(3, 2, 0), tcl = -0.2, xaxs = "i")
text(2.5, 3, paste("las = 2, mgp = c(3, 2, 0)", "tcl = -0.2, xaxs = \"i\"", sep = "\n"))
axis(1, 0:5, 0:5, las = 2, col = "red", tcl = -0.2, mgp = c(3, 2, 0))

plot(-1, -1, xlim = c(0, 5), ylim = c(0,5), xlab = "xlab", ylab = "ylab", xaxt = "n", las = 3, mgp = c(3, 2, 0.6), tcl = -0.6, xaxs = "i")
text(2.5, 3, paste("las = 3, mgp = c(3, 2, 0.6)", "tcl = -0.6, xaxs = \"i\"", sep = "\n"))
axis(1, 0:5, 0:5, las = 2, col = "red", mgp = c(3, 1.4, 0.6), tcl = -0.6)
```

- 用以添加坐标轴的低级函数 `axis()` 的主要参数如下:

参数      | 描述
----------| -------
side      | 整数, 表示在图形的哪侧绘制坐标轴 (1 = 下, 2 = 左, 3 = 上, 4 = 右)
at        | 数值型向量, 指明需要绘制刻度线的位置
labels    | 字符型向量, 表示置于刻度线旁边的文字标签
pos       | 坐标轴线绘制位置的坐标 (即与另一条坐标轴交点的值)
tck       | 刻度线的长度, 以相对于绘图区域大小的分数表示 (负值表示在图形外侧, 正值表示在图形内侧, 0 表示禁用刻度, 1 表示绘制网格线). 默认值为 0.01

### 绘图区域

- R 语言的绘图设备分为三个区域: 绘图区域 (Plot Region)、图像区域 (Figure Region)、设备区域 (Device Region), 对应着两个边界: 外边界 (Outer Margin)、图像边界 (Figure Margin), 具体参见图 2.18.

- 最里面的灰色区域为*绘图区域*; 虚线以内的区域为内部区域，当只有一个图像时就是*图像区域*, 当有多个图像时, 内部区域对应着多个图像区域的总和; 浅灰色区域 (图像区域除去绘图区域) 为*图像边界*; 深灰色区域为*外边界*, 默认不显示; 整个图形设备内的区域则为*设备区域*. 可通过参数控制这些区域大小.

##### 图 2.18
       
```{r}
png(file = "pic-Rplot-28.png")
par(mar = c(0.5, 0.5, 0.5, 0.5))
plot(x = -1, y = -1, xlim = c(1,14), ylim = c(1, 14), ann = FALSE, axes = FALSE )

rect(1, 1, 14, 14, col = "darkgray")
rect(2, 2.5, 12.5, 12, col = "lightgray", lty = 2)
rect(3, 4, 11, 10, col = "gray")

text(c(7, 7, 7), c(7, 11, 13), c("Plot Region", "Figure Region", "Outer Margin"), cex = 1.5, family ="serif", font = 2 )
```

参数          | 描述
------------- | -------
**oma**       | 外边界大小, 默认 c(0, 0, 0, 0), 四个数分别表示下、左、上、右边界大小, 单位为文本行
**mar**       | 图像边界大小, 默认 c(5, 4, 4, 2) + 0.1, 四个数分别表示下、左、上、右边界大小, 单位为文本行
**mai**       | 图像边界大小, 顺序为下、左、上、右, 单位为英尺
**mex**       | 图像边界大小, 默认为 1, 表示一个文本行的高度, 文本行高度由边界文本尺寸* mex 决定, 此参数会影响 mgp 参数
**fin**       | 图像区域大小, c(width, height), 单位为英尺
**pin**       | 绘图区域大小, c(width, height), 单位为英尺
**pty**       | 绘图区域形状, 默认 "m", 表示占据全部可用空间, "s" 表示绘图区域为正方形
xpd           | 超出边界的剪切方式, 取值 FALSE (默认), 表示剪切超出绘图区域部分; TRUE, 表示剪切超出图像区域部分; NA, 表示剪切超出设备区域部分
**din**       | 图形设备尺寸, 单位为英尺, 只可查询不可修改



