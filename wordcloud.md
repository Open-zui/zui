## R 制作词云

步骤如下：
- 获取数据(以 *政府工作报告* 为例)
- 分词( **jiebaR** 分词包)
- 统计词频
- 画词云( **wordcolud2** 词云包)

需安装的 R 包如下：**jiebaRD**, **jiebaR**, **Rcpp**, **wordcloud2**。

### 分词
结巴分词( **jiebaR**)，是一款高效的 R 语言中文分词包，底层使用 C++ 语言，通过 Rcpp 调用。

载入分词包：
```
library(jiebaR)
```
#### 案例引入

- 调用分词器 `worker()` 并命名。
```{r}
fc <- worker()
```

- 分词语法有如下三种，效果相同，以“R 语言是门出色的编程语言”为例：
```{r}
fc["R语言是门出色的编程语言"]
fc <= "R语言是门出色的编程语言"
segment("R语言是门出色的编程语言", fc)
```

- 若需要对两句话分词，并要求返回各自的分词结果，需调用 `bylines = TRUE` 参数，举例如下：
```{r}
fc <- worker(bylines = TRUE)
fc[c("R语言是门出色的编程语言", "今天是2019年某月某日")]
```
- `worker()` 分词器还有如下参数，可按需要调用：
```
worker(type = "mix", dict = DICTPATH, hmm = HMMPATH, user = USERPATH, idf = IDFPATH, stop_word = STOPPATH, write = T, 
       qmax = 20, topn = 5, encoding = "UTF-8", detect = T, symbol = F, lines = 1e+05, output = NULL, bylines = F, user_weight = "max")
```
#### 配置词典

对于分词结果好坏的关键因素是词典，jiebaR默认有配置标准的词典。对于我们的使用来说，不同行业或不同的文字类型，最好用专门的分词词典。在jiebaR中通过`show_dictpath()`函数可以查看默认的标准词典

```{r}
show_dictpath() # 查看字典路径
```

```{r}
dir(show_dictpath()) # 查看目录
```

```
jieba.dict.utf8, 系统词典文件，最大概率法，utf8编码

hmm_model.utf8, 系统词典文件，隐式马尔科夫模型，utf8编码

user.dict.utf8, 用户词典文件，utf8编码的

stop_words.utf8，停止词文件，utf8编码的

idf.utf8，IDF语料库，utf8编码的
```

```{r}
scan(file = "C:/Users/john/Documents/R/win-library/3.5/jiebaRD/dict/jieba.dict/jieba.dict.utf8", what = character(), nlines = 20 ,sep = "\n", encoding = "utf-8", fileEncoding = "utf-8") # 系统字典
```



```{r}
fc <- worker(user = "C:\\Users\\john\\Documents\\R\\win-library\\3.5\\jiebaRD\\dict\\user2.txt", bylines = TRUE) # 自定义字典

fc[c("R语言是一门出色的编程语言", "今天是2019年5月21日")]
```


### 停止词过滤

停止词就是分词过程中，我们不需要作为结果的词，比如*的，地，得，我，你，他*。这些词因为出现频率太高，影响分词结果，所以需要过滤掉。

```{r}
fc <- worker(user = "C:\\Users\\john\\Documents\\R\\win-library\\3.5\\jiebaRD\\dict\\user.txt", 
            stop_word = 'C:\\Users\\john\\Documents\\R\\win-library\\3.5\\jiebaRD\\dict\\stop_word.txt', 
            bylines = TRUE)

fc[c("R语言是一门出色的编程语言", "今天是2019年5月21日")]
```

### 文件分词

```{r}
gwr <- readLines("gwr.csv") 

fc <- worker()

word0 <- fc[gwr]

fc["./gwr.csv"]

fc <- worker(stop_word = "C:\\Users\\john\\Documents\\R\\win-library\\3.5\\jiebaRD\\dict\\stop_words.utf8")

word <- fc[gwr]

fc["./gwr.csv"]
```

## 统计词频

```{r}
library(dplyr)
count <- freq(word) %>%
  arrange(desc(freq))

head(count, 30)
```

## 绘制词云

```{r}
library(wordcloud2)
```

```{r}
wordcloud2(data, size = 1, minSize = 0, gridSize =  0,  
    fontFamily = NULL, fontWeight = 'normal',  
    color = 'random-dark', backgroundColor = "white",  
    minRotation = -pi/4, maxRotation = pi/4, rotateRatio = 0.4,  
    shape = 'circle', ellipticity = 0.65, widgetsize = NULL) 
```

```
data：词云生成数据，包含具体词语以及频率；

size：字体大小，默认为1，一般来说该值越小，生成的形状轮廓越明显；

fontFamily：字体，如‘微软雅黑’；

fontWeight：字体粗细，包含‘normal’，‘bold’以及‘600’；；

color：字体颜色，可以选择‘random-dark’以及‘random-light’，其实就是颜色色系；

backgroundColor：背景颜色，支持R语言中的常用颜色，如‘gray’，‘blcak’，但是还支持不了更加具体的颜                  色选择，如‘gray20’；

minRontatin与maxRontatin：字体旋转角度范围的最小值以及最大值，选定后，字体会在该范围内随机旋转；

rotationRation：字体旋转比例，如设定为1，则全部词语都会发生旋转；

shape：词云形状选择，默认是‘circle’，即圆形。还可以选择‘cardioid’（苹果形或心形），‘star’（星形），‘diamond’（钻石），‘triangle-forward’（三角形），‘triangle’（三角形），‘pentagon’（五边形）
```

```{r}
wordcloud2(count, size = 0.1, shape = "star", fontFamily = "宋体", color = "random-light", backgroundColor = "black")
```


```{r}
picture <- system.file("examples/picture.jpg", package = "wordcloud2") # 图片放在wordclou2的sample中, C:\Users\john\Documents\R\win-library\3.5\wordcloud2\examples

wordcloud2(count, size = 0.5, figPath = picture, fontFamily = "楷体", color = "black")
```
