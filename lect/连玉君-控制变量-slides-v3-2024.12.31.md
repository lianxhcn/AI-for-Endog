---
marp: true
size: 16:9
paginate: true  

footer: '[lianxh.cn](https://www.lianxh.cn)'
---

<style>
/*一级标题局中*/
section.lead h1 {
  text-align: center; /*其他参数：left, right*/
  /*使用方法：
  <!-- _class: lead -->
  */
}
section {
  font-size: 20px; 
}
h1 {
  color: blackyellow;
}
h2 {
  color: green;
}
h3 {
  color: darkblue;
}
h4 {
  color: brown;
}

section::after {
  content: attr(data-marpit-pagination) '/' attr(data-marpit-pagination-total); 
}
header,
footer {
  position: absolute;
  left: 50px;
  right: 50px;
  height: 25px;
}
</style>

<!--顶部文字-->
连享会

<br>

<!--封面图片-->
![bg right:50% w:400 brightness:. sepia:50%](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220722114227.png) 

<!--幻灯片标题-->
# 因果推断中的控制变量 

<br>
<br>

- 好的控制变量
- 坏的控制变量
- 中性的控制变量
- 后门准则
- 应用


<br>
<br>

<!--作者信息-->
[连玉君](https://www.lianxh.cn) (中山大学)
arlionn@163.com

<br>






--- - --

# A. 好的控制变量

## A1. 共同原因

当 $Z$ 作为共同原因或者共同原因的派生变量时，控制 $Z$ 可以阻断虚假的因果路径。

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/好的控制与坏的控制_曹昊煜_Fig04.png)

- 模型 1 中 $Z$ 是共同原因，因此必须控制在模型中。
- 模型 2 或模型 3 中，$Z$ 并不是传统意义上的混淆因素，
  但控制 $Z$ 可以切断来自不可观测因素的混淆。

--- - --
$$
X = Z + u \quad \Rightarrow \quad Z = X - u
$$
$$
Y = Z + X + e  \quad \Rightarrow \quad Y = 2X+ e - u
$$
```stata
*-共同原因  X <-- Z --> Y

  clear
  set seed 135
  set obs 30 
  gen Z = _n
  gen X = 1*Z + rnormal()
  gen Y = 1*X + 1*Z + 0.1*rnormal()
  
  eststo m1: reg Y X
  eststo m2: reg Y Z
  eststo m3: reg Y X Z

  esttab m1 m2 m3 , nogap compress
-------------------------------------------------
                 (1)          (2)          (3)   
                   Y            Y            Y   
-------------------------------------------------
X              1.985***                  1.008***
             (79.98)                   (69.57)   
Z                           1.995***     0.990***
                          (78.20)      (68.02)   
_cons          0.309       0.0109       0.0245   
              (0.70)       (0.02)       (0.71)   
-------------------------------------------------
N                 30           30           30   
-------------------------------------------------
```
--- - --

## A2. 带有中介的共同原因

如果模型中同时存在共同原因和中介关系，那么同样必须阻断后门路径。

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/好的控制与坏的控制_曹昊煜_Fig05.png)

以上三个模型中同时包含了中介关系和共同原因
- 以模型 4 为例，其后门路径为 $X \leftarrow Z \rightarrow M \rightarrow Y$。
- 在模型 5 和模型 6 中，$Z$ 是共同原因 $U$ 的派生变量，因此同样可以阻断后门路径。

--- - --

# B. 坏的控制

## B1. M 偏误 (共同结果)

- 该模型中，变量 $Z$ 同时与处理变量和结果变量相关，
因此其被称为 “预处理” 变量。
- 尽管在传统的计量经济学中认为 $Z$ 是一个好的控制，
但实际上可能会打开一条后门路径 $X \leftarrow U_1 \rightarrow Z \leftarrow U_2 \rightarrow Y$。

- 这种坏的控制称为 **M 偏误**。

![bg right:34% w:350](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/好的控制与坏的控制_曹昊煜_Fig06.png)

--- - --

$$
Z = 0.5X + Y + e，corr(X,Y)=0
$$
$$
Y = -0.5X + Z - e
$$


```stata
  clear
  set seed 135
  set obs 30 
  gen X = rnormal()
  gen Y = rnormal()
  gen Z = -0.5*X + Y + 0.2*rnormal()

.   esttab m1 m2 m3 m4, nogap r2 compress

---------------------------------------------------
          (1)       (2)         (3)          (4)   
            Y         Y           X            Y   
---------------------------------------------------
X      0.0840                              0.491***
       (0.58)                            (12.76)   
Z                 0.722***   -0.572**      1.003***
                 (6.93)     (-2.91)      (21.95)   
_cons  0.0234   -0.0495     -0.0223      -0.0386   
       (0.14)   (-0.49)     (-0.12)      (-0.99)   
---------------------------------------------------
N          30        30          30           30   
R-sq    0.012     0.632       0.232        0.948   
---------------------------------------------------
t statistics in parentheses
* p<0.05, ** p<0.01, *** p<0.001

```

--- - --

## B2. 阻断正确路径

在因果推断中，我们需要兼顾两件事情：
- 要剔除所有可疑的路径
- 尽量避免阻断正确的因果路径

下面两个模型显示了阻断因果路径的坏控制：

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/好的控制与坏的控制_曹昊煜_Fig08.png)

在这两个模型中，$Z$ 分别作为中介变量和中介变量的派生变量，
因此在模型中加入 $X$ 之后，会完全和部分阻断正确的因果路径，
导致不一致的估计。


![bg right:30% w:200](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220701221040.png)



--- - --

## B3. 打开混淆路径

对具有中介变量的模型稍加改动。

假设存在不可观测的因素 $U$ 作为 $Z$ 和 $Y$ 的共同原因。

此时路径 $X\rightarrow Z \leftarrow U \rightarrow Y$ 被 $Z$ 这一共同结果阻断，加入 $Z$ 之后反而会打开该路径。

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/好的控制与坏的控制_曹昊煜_Fig09.png)



--- - --

# C. 中性的控制

## C1. 可能提高精度的情形

在很多情形下，加入某些控制变量是无害的，但也无法提供更多因果信息。

例如，在以下模型中，$Z$ 并没有混淆因果关系，也没有阻断可疑的因果路径，因此 $Z$ 是一个中性的控制。

然而，加入 $Z$ 之后，因果关系估计的标准误会下降。

因此，$Z$ 能够改善 ACE 的估计精度。

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/好的控制与坏的控制_曹昊煜_Fig12.png)

--- - --

#### **Q.** 如果控制变量只与 $\small Y$ 相关，而与 $\small X$ 无关会怎样？
  * **A.** 标准误变小 ( *t* 值变大 ), 系数估计值不受影响
  * Note: 这种情况其实很少发生

![w:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS_bad_control_y_corr_01.png)

> 延伸阅读：Schjoedt, Bird, Carsrud, Brännback ([2014](https://doi.org/10.4337/9780857935052.00013), [PDF](http://sci-hub.ren/10.4337/9780857935052.00013))


--- - --

## C2. 可能降低精度的情形

与第一种情形相反，在右侧的模型中，
虽然控制 $Z$ 不会影响从 $X$ 到 $Y$ 的因果关系，
但此时会放大 ACE 的估计方差，降低估计的精度。

可见，
- 控制 $X$ 的父变量会损害估计精度
- 而控制 $Y$ 的父变量则会提高估计精度

![bg right:30% w:300](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/好的控制与坏的控制_曹昊煜_Fig13.png)

> **注意**：
该模型与偏误放大情形非常类似，
唯一的区别在于该模型中不存在与 $X$ 和 $Y$ 同时相关的不可观测因素。
--- - --

<!-- backgroundColor: white -->
#### **Q.** 如果控制变量只与 $\small X$ 相关，但与 $\small Y$ 无关会怎样？

  * **A.** 标准误变大 ( *t* 值变小 ), 系数估计值不受影响 ($\small Z$ 是冗余变量)
  
![w:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS_bad_control_x_corr_01.png)

> 延伸阅读：Schjoedt, Bird, Carsrud, Brännback ([2014](https://doi.org/10.4337/9780857935052.00013), [PDF](http://sci-hub.ren/10.4337/9780857935052.00013))

--- - --

## C3. 可能缓解选择偏误的情形

与传统经济学不同，并非所有 “处理后” 变量都是坏的控制。

在以下两个模型中，$Z$ 的加入并未打开混淆路径。

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/好的控制与坏的控制_曹昊煜_Fig14.png)

在这两个模型中，加入 $Z$ 都会降低 $X$ 的方差，因而损害估计的精度。

但在右边的模型中，控制 $Z$ 可以缓解关于 $W$ 的选择偏误。

--- - --

## C4. 偏误放大

另一种关于 “预处理” 的控制是加入影响处理变量的因素。

在这一情形下，不但无法分离出真实的因果效应，还会放大本身存在的偏误。

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/好的控制与坏的控制_曹昊煜_Fig07.png)



--- - --

# C. 理论基础

## C1. 后门路径 (Back door path)

<!-- Backdoor paths are defined as any sequence of arrows connecting the treatment and outcome variable (irrespective of their orientation) that remains if arrows emitted by the treatment are deleted from the graph (Pearl, 2000) -->

> 定义：**后门路径**：在连接处理变量 ($X$) 和结果变量 ($Y$) 的任何箭头序列 (无论其方向如何) 中，如果删除从 $X$ 发出的箭头，某个序列依然存在，则称其为「后门路径」 (Pearl，2000)。

> 用途：切断所有后门路径。我们可以通过在回归模型中加入适当的控制变量来切断所有后门路径，以便识别 $X \rightarrow Y$ 的因果关系。

- **Source:** Hünermund, P., & Louw, B. (2025). On the Nuisance of Control Variables in Causal Regression Analysis. Organizational Research Methods, 28(1), 138–151. [Link](https://doi.org/10.1177/10944281231219274), [PDF](https://journals.sagepub.com/doi/pdf/10.1177/10944281231219274), [Google](<https://scholar.google.com/scholar?q=On the Nuisance of Control Variables in Causal Regression Analysis>).

--- - --

## C2. 后门准则

因果图揭示了何种 $\mathbf{Z}$ 的设定会阻断正确的因果路径，我们需要做的是选择 $\mathbf{Z}$，以保证：

- 阻断所有虚假的路径；
- 避免阻断或部分阻断真实的因果路径；
- 避免打开其他虚假的路径。


--- - --
## C3. 三种因果关系
在一般的因果图中，需要理解三种重要的因果关系：

- 中介 (Chains)：中介指的是路径 $X \rightarrow Z \rightarrow Y$，即 $X$ 对 $Y$ 的因果影响是通过 $Z$ 实现的。在方程中控制 $Z$ 会阻断这一联系；
- 共同原因 (Forks)：共同原因指的是路径 $X \leftarrow Z \rightarrow Y$，即 $Z$ 同时影响 $X$ 和 $Y$。因此二者间存在非因果路径，在方程中控制 $Z$ 会阻断这一联系；
- 共同结果 (Coliders)：共同结果指的是路径 $X \rightarrow Z \leftarrow Y$，这一路径本身是关闭的，但如果我们在方程中控制了 $Z$，则会打开这一非因果路径。

需要注意的是，控制某一变量的派生变量也视为部分控制了该因素。现在我们可以判断当以 $\mathbf{Z}$ 为条件时，路径 $p$ 是否被阻断：

- 当路径是中介或共同原因时，$\mathbf{Z}$ 中会纳入中间节点能够阻断路径 $p$；
- 当路径是共同结果时，$\mathbf{Z}$ 中既不包含中间节点，也不包含其结果，则能够阻断路径 $p$。

--- - --
## C4. 简例

![w:700](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230725151414.png)

> Source: Control of Confounding and Reporting of Results in Causal Inference Studies: Guidance for Authors from Editors of Respiratory, Sleep, and Critical Care Journals. [-PDF-](https://www.atsjournals.org/doi/pdf/10.1513/AnnalsATS.201808-564PS)    
> Kohler, Ulrich, Sawert, Tim, & Class, Fabian (2022). Replication material to "Control variable selection in applied quantitative sociology: A critical review". [-Link-](https://doi.org/10.7802/2479)




--- - --

# D. 控制变量的选择与研究目标有关

$$
Y_{i}=\beta_{0}+\beta_{1} X_{i}+\varepsilon_{i}
$$

<!-- 单变量的回归分析不太可能得出处理 $X$ 对结果 $Y$ 的无偏估计。由于 $X$ 和 $\varepsilon$ 之间相关性所产生的内生性问题被称为遗漏变量偏误 (OVB)，通过从误差项 ($\varepsilon$) 提取 $Z$ 构造多元回归可以缓解 OVB。但鉴于会计研究中的大多数处理方法都是自选择或非随机分配的，因此研究人员必须准确的指定适当的 $Z$，才可以正确识别出 $X$ 对 $Y$ 的因果影响。 -->

构建因果模型时，可以借助因果图来识别遗漏变量的来源、理解因果关系，从而设定合理的模型。
我们用以下因果图来示例变量之间的因果关系：

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/控制变量越多越好吗_张雪娇_Fig01.png)

- 如果关注 D &rarr; E 的影响，则 C 作为混杂因素，必须要控制。
- 如果关注 C &rarr; E 的影响，则 D 不应该被控制，因为 D 是 C 和 E 之间关系的中介。
  - Q：此时，A 和 B 是否是好的控制变量？

实际上，回归不能告诉我们是 C 导致 E 还是 E 导致 C，回归只是在给定条件下对相关性的估计，只有通过理论分析才能论证因果关系。

--- - --

## 例子：工资的性别差异
假设我们想了解工资中性别差距的程度：
> $\small wage_{it} = a + \beta Female_{it} + u_{it}$

**Q：** 如何选择控制变量以克服遗漏变量偏误？

**可能的控制变量：**
  - 工作经验 ($\small Exp$)、教育水平 ($\small Edu$)、职业、失业率 ($\small Unemp$)、住江景房、...

**Q：** 哪些是 Bad Controls？ 

--- - --

## 例子：工资的性别差异 (Cont.)

> $\small wage_{it} = a + \beta Female_{it} + u_{it}$
- $\small Exp$, $\small Edu$
  - Good Controls：它们与性别相关，也会影响收入水平。
  - Bad Controls：如果我们认为性别导致教育或经验的变化，则二者便是 Bad Control。
- 控制您所在地区的失业率 ($\small Unemp$) 可能是 Bad control。
  - $\small Unemp$ 肯定会影响工资，但与性别无关，除非某些地区有更多/更少的女性。
  - 控制 $\lambda_t$ (`i.year`) 是更好的选择，有助于控制宏观经济因素导致的年度差异
- 控制职业 (`i.occupation`) 是一种 Bad Control，因为它在很大程度上直击受性别影响，即 $\small Female \rightarrow Occupation \rightarrow Wage$


--- - --

# 识别好和坏的控制变量

Angrist 和 Pischke (2009) 提供的**经验法则**：

- “好” 的控制变量是指在确定处理变量 $X$ 时就已经固定的变量
-  “坏” 的控制变量则是那些本身就是结果变量的变量。



## 几个例子

- 铲球次数 (Source: [Hastie](https://hastie.su.domains/lectures.htm), [2021](https://hastie.su.domains/MOOC-Slides/linear_regression.pdf))
  - $\small Y$: 一名足球运动员在一个赛季中的铲球次数；$\small W$ 和 $\small H$ 是他的体重和身高。 
  - 回归结果为 $\small \widehat{Y}=b_{0}+0.5 W-0.1 H$。如何解释 $\small \hat{\beta}_{2}=-0.1<0$ ?
  
- 若研究啤酒税 ($\small Tax$) 对交通死亡人数 ($\small Death$) 的影响，如下模型设定可行吗？[-Lk-](https://www.uio.no/studier/emner/sv/oekonomi/ECON4150/v15/lectures/lec-7-v2.pdf)
$$\small
Death=\beta_{0}+\beta_{1} {Tax}+\beta_{2} {BeerSales}+\ldots 
$$

- 若研究气温 (Temp) 对暴力冲突 (Conflict) 的影响，如下模型是否可行？
$$\small
Conflict = \beta_{0}+\beta_{1} {Temp}+\beta_{2} {Income}+\ldots 
$$


--- - --
### Bad control：模拟分析 A
- Literaturn on temprature, income and conflict
  - Dell et al. ([2012](https://doi.org/10.1257/mac.4.3.66), [PDF](http://sci-hub.ren/10.1257/mac.4.3.66)): $\small Temp \stackrel{(-)}{\longrightarrow} Income$
  - Miguel et al. ([2004](https://doi.org/10.1086/421174), [PDF](http://sci-hub.ren/10.1086/421174), [Codes](https://doi.org/10.7910/DVN/27324), [cite](https://xs2.dailyheadlines.cc/scholar?cites=7490175262554688472&as_sdt=2005&sciodt=0,5&hl=zh-CN)) $\small Income \stackrel{(-)}{\longrightarrow} Conflict$
  - $\small \underbrace{Temp}_{X} \stackrel{(-)}{\longrightarrow} \underbrace{Income}_{W} \stackrel{(-)}{\longrightarrow} \underbrace{Conflict}_{Y} \quad \text{or} \quad X {\longrightarrow} W {\longrightarrow} Y$
- DGP1: $\small X {\longrightarrow} W {\longrightarrow} Y$
  - $\small temp \sim N(0,1)$, $e_1\sim N(0,1)$, $e_2\sim N(0,1)$
  - $\small income = \beta_1\,temp + e_1 \quad (\beta_1 =-0.5)$
  - $\small conflict = \beta_2\,income + e_2 \quad (\beta_2 = -1.0)$
  - Total effect: $\small \partial Y/\partial X =\beta_1\times\beta_2 = -1.0\times (-0.5) = +0.5$
--- - --

```stata
clear
set seed 123
set obs 1000
gen temp = rnormal()  //temperature variable
gen  e_1 = rnormal()  //noise 1
gen  e_2 = rnormal()  //noise 2, uncorrelated with e_1
gen   income = -0.5*temp   + e_1  
gen conflict = -1.0*income + e_2  

eststo m1: reg conflict temp
eststo m2: reg conflict income
eststo m3: reg conflict income temp
esttab m1 m2 m3, nogap r2

conflict        (1)             (2)             (3)   
------------------------------------------------------
temp          0.522***                       0.0263   
            (11.34)                          (0.71)   
income                       -0.997***       -0.986***
                           (-34.33)        (-30.50)   
_cons       -0.0613         -0.0406         -0.0413   
            (-1.37)         (-1.26)         (-1.28)   
------------------------------------------------------ 
R-sq          0.114           0.542           0.542   
```

--- - --

- Dell, M., B. F. Jones, B. A. Olken, **2012**, Temperature shocks and economic growth: Evidence from the last half century, **American Economic Journal-Macroeconomics**, 4 (3): 66-95. [-Link-](https://doi.org/10.1257/mac.4.3.66), [-PDF-](https://sci-hub.ren/10.1257/mac.4.3.66), [PDF2](https://www.aeaweb.org/aej/mac/app/2010-0092_app.pdf), [Replication](http://doi.org/10.3886/E114251V1), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=6508137188310466158&as_sdt=2005&sciodt=0,5&hl=zh-CN)
- Miguel, E., S. Satyanath, E. Sergenti, **2004**, Economic shocks and civil conflict: An instrumental variables approach, **Journal of Political Economy**, 112 (4): 725-753. [-Link-](https://doi.org/10.1086/421174), [-PDF-](https://sci-hub.ren/10.1086/421174), [-Slides-](https://www.uio.no/studier/emner/sv/oekonomi/ECON4918/h14/miguel_etal_slides.pdf), [Replication](https://doi.org/10.7910/DVN/27324), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=7490175262554688472&as_sdt=2005&sciodt=0,5&hl=zh-CN)
  - Miguel, E., S. Satyanath, **2011**, Re-examining economic shocks and civil conflict, **American Economic Journal: Applied Economics**, 3 (4): 228-232. [-Link-](https://doi.org/10.1257/app.3.4.228), [-PDF-](https://sci-hub.ren/10.1257/app.3.4.228), [Replication](https://doi.org/10.3886/E113802V1), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=10440418962698472574&as_sdt=2005&sciodt=0,5&hl=zh-CN)





--- - --

### Bad control？ 模拟分析 B

- DGP2: $\small Y {\longleftarrow}X {\longrightarrow} W {\longrightarrow} Y$
  - $\small conflict = -1.0\,income + {\color{blue}{0.4\,temp}} + e_2$，
  - $\small income = -0.5\,temp + e_1$
- Note: 总效应 = $\small \underbrace{-0.5\times(-1.0)}_{\text{直接效应}} + \underbrace{0.4}_{中介效应} = 0.9$
```stata
conflict2             (1)             (2)             (3)    
------------------------------------------------------------
temp                0.922***                        0.426***
                  (20.03)                         (11.55)   
income                             -1.151***       -0.986***
                                 (-37.25)        (-30.50)   
------------------------------------------------------------  
R-sq                0.287           0.582           0.631 

. reg income temp // regfit
                     income =  0.02 - 0.50*temp 
                              (0.03) (0.03)
```


--- - --

### 进一步讨论
- Income 能否作为控制变量？
  - *... variables such as income both affect and are affected by civil conflict, including them directly as regressorsis problematic* (Miguel et al., [2004](https://doi.org/10.1086/421174), [PDF](http://sci-hub.ren/10.1086/421174); Burke et al., [2010](https://www.pnas.org/doi/epdf/10.1073/pnas.1005748107))
- temperature 是否会直接影响 conflict ？
  - Hsiang, S., **2016**, Climate econometrics, **Annual Review of Resource Economics, Vol 8**, 8: 43-75. [-Link-](https://doi.org/10.1146/annurev-resource-100815-095343), [-PDF-](https://sci-hub.ren/10.1146/annurev-resource-100815-095343)
  - Burke, M., S. M. Hsiang, E. Miguel, **2015**, Climate and conflict, **Annual Review of Economics**, 7 (1): 577-617. [-Link-](https://doi.org/10.1146/annurev-economics-080614-115430), [-PDF-](https://sci-hub.ren/10.1146/annurev-economics-080614-115430), [PDF2](https://web.archive.org/web/20170808011530id_/http://web.stanford.edu/~mburke/papers/Burke%20Hsiang%20Miguel%202015.pdf), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=7668610584263248172&as_sdt=2005&sciodt=0,5&hl=zh-CN)


--- - --

# 后门准则应用：不同的控制方案

![w:700](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230812011328.png)


<!-- 
--- - --

To illustrate this phenomenon quantitatively, we parameterize the causal graph in **Figure 1a** in the following way:
$$
\begin{aligned}
& z_1 \leftarrow u+\varepsilon_1, \\
& z_2 \leftarrow u+\varepsilon_2, \\
& x \leftarrow z_1+\varepsilon_3, \\
& y \leftarrow x+z_2+\varepsilon_4,
\end{aligned}
$$ -->


<!-- This is different in **Figure 1c**. Here the two backdoor paths are $X \leftarrow Z_1 \rightarrow Y$ and $X \leftrightarrow--\rightarrow Z_1 \rightarrow Y$. When we simulate data according to:
$$
\begin{aligned}
& z_1 \leftarrow u+\varepsilon_1, \\
& x \leftarrow z_1+u+\varepsilon_2, \\
& y \leftarrow x+z_1+\varepsilon_3,
\end{aligned}
$$ -->

--- - --

![20230812011128](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230812011128.png)


--- - --

# FAQs

## 是否需要展示和讨论控制变量的经济含义？

- [不用太关心控制变量的系数！](https://www.lianxh.cn/news/77d0128e722e7.html)

- 虽然控制变量对于因果关系的识别至关重要，但其本身通常不具有结构性解释。即使是有效的控制变量，也常常会与其他未观察到 (或不能观测到) 的因素（unobserved factors）关联，从因果推断的角度来看，这使得它们的边际效应无法解释 (Westreich 和 Greenland，2013； Keele等，2020)。
- 因此，在因果识别和因果推断中，可以不必 (也很难) 讨论控制变量的系数含义。


--- - --

## 控制变量可以是内生变量吗？

> Yes!

假设真实的因果模型如下图所示：

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/控制变量到底控制了什么02_付一帆_Fig01.png)

--- - --

在如下回归模型中： 
$$y=\beta_0+\beta_xx+\beta_zz+\varepsilon$$ 

- $\beta_x$ 可以作为因果解释：给定母亲的上学年限相同，平均每多上一年学能增加工资 $\beta_x$。这是因为母亲上学年限 $z$ 作为控制变量之后，$x$ 和 $\varepsilon$ 就不再相关，$E(x\varepsilon|z) = 0$，$\beta_x$ 无偏；
- $\beta_z$ 只能作为相关解释：不能说母亲每多上一年学，就能增加子女的工资 $\beta_z$，只能说母亲的教育水平和子女的工资是正相关的。这是由于母亲上学年限在 $\varepsilon$ 里面导致 $z$ 与 $\varepsilon$ 相关，$E(z\varepsilon|x) \neq 0$，$\beta_z$ 有偏。

- **传统视角下的疑问**：控制变量 $z$ 与扰动项 $\varepsilon$ 相关。这显然不符合 OLS 的基本假设：所有的解释变量都与随机误差项不相关。否则，它们的估计值都不一致。

为何在因果框架下可以放松这一假定，而允许控制变量 $z$ 和扰动项 $\epsilon$ 相关呢？



--- - --

### 启示
- 在因果推断中，我们关注的是 $X$ 的系数，此时可以「牺牲」控制变量 $Z$ 系数的无偏性，这意味着：
  - 不必做过多讨论控制变量的系数，因为多数情况下，控制变量的系数估计值是有偏的。
  - 要借助理论分析和因果图 (DAG) 来辅助模型设定，说清楚上述设定思路
- 在因果框架下，我们通常只对回归方程中的一个核心解释变量感兴趣，特别希望得到对其系数的一致估计，并将其解释为核心变量对被解释变量的因果效应。
- 对于方程中的其他变量并无太大兴趣，之所以把它们放入回归方程，只是为了 “控制” 那些对被解释变量有影响的遗漏因素来避免 “遗漏变量偏差”。即使对控制变量系数估计不一致，我们也可以接受。

> 详见：[Stata：控制变量与核心解释变量地位对等吗？](https://www.lianxh.cn/news/4eb3cb3d50dcd.html)。给出了上述处理方法的理论依据和证明过程。




--- - --

# IV 估计中的控制变量


--- - --

### 工具变量需要满足的条件：

- 相关性：工具变量与内生解释变量相关。
$$
\operatorname{Cov}(Z, D) \neq 0
$$
- 外生性或独立性：工具变量与扰动项不相关。
$$
\operatorname{Cov}(Z, \varepsilon)=0
$$
- &#x1F34E; **排斥性约束**：工具变量只通过 $X$ 或其他变量影响 $Y$，但不直接影响 $Y$。
  - 换言之，$Z$ 不直接出现在结构方程右边。
- 教育回报率例子中的 $Z$ 有哪些可能的选择?

![bg right:45% h:190](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220719213926.png)

--- - --

## IV 中的控制变量 I：控制混淆因素

<br>

此时，$X$ 是一个混淆变量，必须加以控制

例：
- $Y$: Income
- $A$：Education
- $Z$：Distance from College
- $X$: Family income, Father's Education, ... 


![bg right:50% w:500](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230813021926.png)




--- - --

## IV 中的控制变量 II: 阻断其它路径
### 哪个说法正确 ?

<br>

> A. $\small Z$ 不能与 $\small Y$ 相关，即 $\small \operatorname{corr}(Z,Y)=0$ 
  
<br>

> B. $\small Z$ 可以与 $\small Y$ 相关

- 但要满足 $\small Z \longrightarrow D \longrightarrow Y$，而非 $\small {\color{blue}{Y \longleftarrow}} Z \longrightarrow D$  
- 甚至可以 $\small Z \longrightarrow D \longrightarrow Y$，且 $\small Z \longrightarrow W \longrightarrow Y$
  - 这时候就需要考虑控制变量的作用了 (next page)

--- - --

### IV 中控制变量的作用

- 排斥性约束要求工具变量 $Z$ 只通过内生解释变量 $D$ 影响 $Y$ ，那么在 包含控制变量的工具变量回归中， $Z$ 可不可以通过控制变量 $X$ 影响 $Y$ 呢?

  - **答：当然可以**。但这个问题应该反过来理解，正是因为担心工具变量有除了 $D$ 之外影响 $Y$ 的渠道，故而把这些潜在的渠道尽量控制起来，这些被控制起来的渠道就成了控制变量。这正是排斥性约束检验的主要思路。
  - 例子：Card (1995) 教育回报论文中，加入 South, ASMA 等控制变量就是这个目的。 

![bg right:35% h:190](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220719213149.png)

--- - --
## Card (1995) 教育回报率
- Outcome Eq: &emsp; $Income = \beta_1Educ + \beta_2 X  + e$
- First Eq: &emsp; $Educ = \theta_0 + \theta_1 near4$


```stata
----------------------------------------------------------------------------------
                 (1)         (2)         (3)         (4)         (5)         (6)  
                OLS1         IV1   OLS_argu1        OLS2         IV2   OLS_argu2  
----------------------------------------------------------------------------------
ed76           0.074**     0.132**     0.074**     0.085**     0.221**     0.084**
             (20.32)      (2.73)     (20.12)     (23.05)      (5.44)     (22.55)  
nearc4                                 0.020                               0.063**
                                      (1.27)                              (4.11)  
black         -0.190**    -0.131**    -0.190**                                    
            (-10.88)     (-2.54)    (-10.88)                                      
reg76r        -0.125**    -0.105**    -0.122**    -0.196**    -0.100**    -0.184**
             (-8.13)     (-4.58)     (-7.87)    (-13.16)     (-2.98)    (-12.19)  
smsa76r        0.161**     0.131**     0.155**                                    
             (10.64)      (4.41)      (9.73)                                      
exp            0.084**     0.107**     0.083**     0.085**     0.143**     0.085**
             (12.42)      (5.09)     (12.40)     (12.29)      (7.46)     (12.23)  
exp2          -0.224**    -0.228**    -0.224**    -0.230**    -0.239**    -0.229**
             (-7.04)     (-6.59)     (-7.05)     (-6.96)     (-5.55)     (-6.95)  
_cons          4.734**     3.753**     4.729**     4.677**     2.330**     4.653**
             (67.47)      (4.59)     (67.50)     (65.13)      (3.32)     (65.08)  
----------------------------------------------------------------------------------
N               3010        3010        3010        3010        3010        3010  
r2             0.291       0.225       0.291       0.241      -0.134       0.245  
----------------------------------------------------------------------------------
t statistics in parentheses
** p<0.05
```

--- - --
<!-- backgroundColor: #FFFFF9 -->

### 附：Stata codes 

> Data Source: Hansen B E . 2021. **Econometrics**. Princeton University Press. [Data and Contents](https://www.ssc.wisc.edu/~bhansen/econometrics/), [PDF](https://www.ssc.wisc.edu/~bhansen/econometrics/Econometrics.pdf)


```stata
  use "Card1995_edu.dta", replace  // Hansen, 2021 Book

* New Variables 
  gen exp  = age76 - ed76 - 6
  gen exp2 = (exp^2)/100
  gen age2 = (age^2)/100

* Drop observations with missing wage
  drop if lwage76==.

*-参考：Table 12.1 (Hansen 2021, book, modified)
  // SMSA: Standard Metropolitan Statistical Area
  //       (美国行政区划单位) 大城市及其郊区
   global cx1 "black reg76r smsa76r exp exp2"  // [a] common controls
   global cx2 "      reg76r         exp exp2"  // [c] black, smsa 是 cofounder ? 
   global IV  "nearc4"         

*-Note: 在 [c] 设定下，不再满足【排他性】假设  

```


--- - --
<!-- backgroundColor: #FFFFF9 -->

### 附：Stata codes (cont.)

```stata
 *-(1) OLS 
   reg lwage76 ed76 $cx1   , robust
   est store OLS1
   reg lwage76 ed76 $cx2   , robust
   est store OLS2
   
 *-(4) 2SLS
   ivreg2 lwage76 $cx1 (ed76=$IV), robust   
   est store IV1  
   ivreg2 lwage76 $cx2 (ed76=$IV), robust   
   est store IV2    

 *-(5) OLS + argument
   reg lwage76 ed76 $cx1 $IV, robust
   est store OLS_argu1
   reg lwage76 ed76 $cx2 $IV, robust
   est store OLS_argu2
   
 *---- 对比结果 ----
  local m "OLS1 IV1 OLS_argu1 OLS2 IV2 OLS_argu2"
  esttab `m' `s', mtitle(`m') nogap compress ///
         b(%6.3f) order(ed76 near*)          ///
		 s(N r2, fmt(0 3)) star(** 0.05)
```

--- - --

<!-- --- - --

> Source: Mellon, Jonathan, Rain, Rain, Go Away: 195 Potential Exclusion-Restriction Violations for Studies Using Weather as an Instrumental Variable, **2023**. [-PDF-](https://osf.io/preprints/socarxiv/9qj4f/download)

![w:600](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230725125927.png)
> Figure 2: Causal web of rainfall from 192 papers. Nodes and ties sized proportional to the number of appearances in the literature. Colors: weather (green), instrumented variable (orange), and outcome (purple) -->


<!-- --- - --

Reviewing criteria

To explain our reviewing criteria, we first consider a researcher interested in the total effect of son's first occupation on son's occupation given the status attainment model (Fig. 4). In this case it is necessary to control for son's education and father's occupation, which translates to the regression model

$$
\mathrm{Oc}=\alpha+\beta 1 \mathrm{stOc}+\gamma_1 \mathrm{ED}+\gamma_2 \mathrm{FaOc}+\epsilon,
$$

with 1 stOc being the exposure, $\beta$ being the estimate of the total effect of the exposure, and ED and FaOc being the adjustment set $A$.

--- - --

Observe that the research designs discussed so far implied three different regression equations. None of the three examples implied running a regression of son's occupation on the entire set of covariates. In fact, in the full regression

$$
\mathrm{Oc}=\beta_0+\beta_1 \mathrm{FaEd}+\beta_2 \mathrm{FaOc}+\beta_3 \mathrm{Ed}+\beta_4 1 \mathrm{stOc}+\epsilon
$$

only the coefficient of son's first occupation can be interpreted as an estimate of the total effect. For all other covariates, the full model blocks causal paths by conditioning on descendants. The estimated effects of these variables thus cannot be interpreted as estimates of the total effect on occupation.

![20230725144834](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230725144834.png) -->


<!-- - [很清楚](http://www.statslab.cam.ac.uk/~qz280/talk/ssrmp-2020/slides.pdf) -->



## 动态面板中的 Controls


### 模型设定：
$$
  y_{it} = {\color{red}{\gamma}} y_{i,t-1} + x_{it}'\beta + \alpha_i + \varepsilon_{it} \quad (1)
$$

### 基本假设：

- **H1: 无序列相关** $\text{Corr}(\varepsilon_{it}, \varepsilon_{it-1})=0$
- **H2: 外生性** &ensp; &ensp;  $E(\varepsilon_{it}\ |\ y_{it-1}, x_{it}, \alpha_i)=0$

### 估计：IV/GMM

$$
  \Delta y_{it} = {\gamma} {\color{red}{\Delta y_{i,t-1}}} + \Delta x_{it}'\beta  + \Delta\varepsilon_{it} \quad (2) 
$$
- $IV_1 = y_{it-2}$ &emsp; &rarr; &emsp;  $E(y_{it-2}\cdot\Delta\varepsilon_{it})=0$ 
- $IV_2 = y_{it-3}$  &emsp; &rarr; &emsp; $E(y_{it-3}\cdot\Delta\varepsilon_{it})=0$ 
$\cdots$ (好多！)

--- - --

### 序列相关检验 [[AB91]](https://www.jianguoyun.com/p/Df3hiZYQtKiFCBjb4dgC)

- 水平方程：&emsp; $y_{it} = {\gamma} y_{i,t-1} + x_{it}'\beta + \alpha_i + \varepsilon_{it} \quad (1)$ 
- 差分方程：&emsp; $\Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta  + \Delta\varepsilon_{it} \quad (2)$
- **H1: 无序列相关** $\text{Corr}(\varepsilon_{it}, \varepsilon_{it-1})=0$

$$\Delta \varepsilon_{it}=\varepsilon_{it}-\varepsilon_{it-1}$$

$$\Delta \varepsilon_{it-1}=\varepsilon_{it-1}-\varepsilon_{it-2}$$

$$\Delta \varepsilon_{it-2}=\varepsilon_{it-2}-\varepsilon_{it-3}$$

$\rho_1 = \text{Corr}(\Delta \varepsilon_{it}, \Delta \varepsilon_{it-1}) \rightarrow m_1$ 

$\rho_2 = \text{Corr}(\Delta \varepsilon_{it}, \Delta \varepsilon_{it-2}) \rightarrow m_1$

<br>
<br>

--- - --


### 应用指南：Sargan 和序列相关检验为何无法通过 ？

Sargan 检验的目的：在于检验工具变量的合理性，简言之，是工具变量与差分后的干扰项是否存在统计上显著的相关性。相当于做如下回归：

$\Delta\varepsilon_{it} = \beta_1 + y_{it-2}\beta_2 + y_{it-3}\beta_3 + \cdots + v_{it}$

然后看 $y_{it-s}\ (s \geq 2)$ 的系数是否联合显著，如果显著，那就悲剧了。

可以看出，我们可以从 $\Delta\varepsilon_{it}$ 和 $y_{it-s}$ 两个角度入手。
- 让 $\Delta\varepsilon_{it}$ 干净一点
- 选择谁做 IVs ？—— $s$ 的选择

- **建议：** &#x1F34E; 
  - 尽量加入 `i.year` ($\lambda_t$), 甚至 `i.industry#i.year` ($\lambda_{jt}$)
  - 使用 `maxldep(#)` 选项控制冗余 IV 问题 (Weak IVs)

--- - --

## 断点回归中的 Controls

- 若样本量很大，可以设定一个很窄的窗口，此时，无需加控制变量
- 若样本量较小，就需要设定一个较大的窗口，此时，需要加入控制变量 (通常是驱动变量的高阶项或交互项)，以捕捉非线性特征，从而降低模型误设偏误 
- Calonico S, Cattaneo M D, Farrell M H, et al. Regression discontinuity designs using covariates[J]. Review of Economics and Statistics, 2019, 101(3): 442-451. [-PDF-](https://arxiv.org/pdf/1809.03904.pdf)

--- - --

> 参数法：二次函数

![w:900](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Fig_x2.png)

--- - --

> 非参数方法：局部多项式回归，Stata: `rdrobust`

![w:900](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Fig_lpoly.png)

--- - --

## SCM 中的 Controls
- 是否需要？ **YES**
- 如何设定？ 要兼顾 **样本外预测能力**，配合时间安慰剂检验
  - 可以使用 **Lasso** 筛选变量
  - 也可以使用回归控制法
  - 核心思想：采用交叉验证法筛选变量


--- - --

### 合成控制法：基本思想

Abadie et al. ([2010](https://web.stanford.edu/~jhain/Paper/JASA2010.pdf), JASA) 假设结果变量由因子模型所确定:
$$y_{j t}=\delta_{t}+\theta_{t} Z_{i}+\lambda_{t} \mu_{i}+\varepsilon_{i j}$$

- $Z_{i}$ 是一个 $(r \times 1)$ 阶可观测的预测变量：人均收入，啤酒消费量，人口结构等
  - 太多：过拟合
  - 太少：欠拟合

![w:700](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Abadie-2010-Fig01-02.png)




--- - --

### 过拟合问题图示

![w:800](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Abadie10-Fig09-place-time.png)

--- - --

### 使用 Lasso / RCM 的效果

![w:800](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/RCM-pred_pboTime1985-good.png)

--- - --

```stata
use "smoking_wide.dta", clear

*-Lasso
lasso linear cig0 cig1-cig39 lny1-lny39  ///
          if year<=1984, select(cv, fold(10)) 

*-回归控制法
rcm cig $xx, trunit(3) trperiod(1989) ///
             method(lasso)  ///
             criterion(cv) fold(10)   ///  // 10-fold CV  
             placebo(period(1985))
```

--- - --


# 总结

## 控制变量的选择 —— 好的和坏的

- 好的：
  - 控制混淆因素
  - 阻断后门路径
  - 可以有多种模型设定方式
- 坏的
  - $X$ 的结果变量 (中介变量)
  - $X$ 和 $Y$ 的共同结果变量

--- - --

## 控制变量的选择 —— 不要过度控制
> Source: Schjoedt, Bird, Carsrud, Brännback ([2014](https://doi.org/10.4337/9780857935052.00013), [PDF](http://sci-hub.ren/10.4337/9780857935052.00013))
- 在理论和实践上对 y 的影响不明确的变量：尽量不要加入
  - Carlson, K. D., J. Wu ([2012](https://doi.org/10.1177/1094428111428817), [PDF](http://sci-hub.ren/10.1177/1094428111428817)): 'When in doubt, leave them out' (p. 413)
- 控制变量主要用于排除混杂因素
- 避免包含多个控制变量以涵盖所有可能性。这会导致 II 类错误（例如，在实际上有效果时得出没有效果的结论）
- 加入过度冗余变量的代价：降低统计检验力；降低研究结果的普适性


--- - --


# 实操建议

![Curado_2024_Control_variable_use_Fig3](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Curado_2024_Control_variable_use_Fig3.png)

> Source: 


# 建议：控制变量选择工作流程

> Source: [Mändli](https://doi.org/10.1177/10944281231221703) and Rönkkö ([2023](https://journals.sagepub.com/doi/epdf/10.1177/10944281231221703))


1. **初步筛选**  
   - 根据既有理论、类似研究中的控制变量以及个人直觉，列出一份潜在控制变量的候选清单。

2. **考虑潜在的内生性**  
   - 分析每个备选控制变量是否存在内生性，分析内生的来源（[Guide & Ketokivi, 2015](https://journals.sagepub.com/doi/full/10.1177/10944281231221703)）。  
   - 两种常见的内生性控制变量情形是：
     - 控制中介变量；
     - 控制因变量的结果变量（[Cinelli et al., 2022](https://journals.sagepub.com/doi/full/10.1177/10944281231221703)）。

3. **剔除内生控制变量，保留其余部分**  
   - 删除内生性控制变量，并保留其他变量。虽然优化控制变量集合（例如，识别最小调整集 [Knüppel & Stang, 2010](https://journals.sagepub.com/doi/full/10.1177/10944281231221703)）是可行的，但超出了本指南的讨论范围。  
   - 对于与研究变量**无相关性**的变量，可以为简化模型而排除。

4. **记录变量筛选过程**  
   - 记录第 1 步中生成的变量清单，并详细说明变量分类结果（包括：纳入、因内生性排除、不相关性排除），呈现于文章的附录中。


--- - --

### 主要文献
- Cinelli, C., A. Forney, J. Pearl, **2022**, A crash course in good and bad controls, **Sociological Methods & Research**, 53(3), 1071–1104. [-Link-](https://doi.org/10.1177/00491241221099552), [-PDF-](https://sci-hub.ren/10.1177/00491241221099552), [Replication-R-Codes](https://www.kaggle.com/code/carloscinelli/crash-course-in-good-and-bad-controls-linear-r), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=2703035661213314347&as_sdt=2005&sciodt=0,5&hl=zh-CN)
  - 中文解读，[A-理论部分：控制变量！控制变量！Good-Controls-Bad-Controls](https://www.lianxh.cn/news/c548e33fbb649.html)
  - Stata 模拟：[B-Stata模拟：控制变量！控制变量！Good-Controls-Bad-Controls](https://www.lianxh.cn/news/a54246bf82745.html)
- Whited, R. L., Q. T. Swanquist, J. E. Shipman, J. R. Moon, Jr., **2022**, Out of control: The (over) use of controls in accounting research, **The Accounting Review**, 97 (3): 395-413. [-Link-](https://doi.org/10.2308/tar-2019-0637), [-PDF-](https://sci-hub.ren/10.2308/tar-2019-0637), [PDF2](https://file.lianxh.cn/PDFTW/Whited_Swanquist_Shipman2021.pdf), [推文](https://www.lianxh.cn/news/5156229fb5147.html)
- Wysocki, A. C., K. M. Lawson, M. Rhemtulla, **2022**, Statistical control requires causal justification, **Advances in Methods and Practices in Psychological Science**, 5 (2): 25152459221095823. [-Link-](https://doi.org/10.1177/25152459221095823), [-PDF-](https://sci-hub.ren/10.1177/25152459221095823), [PDF2](https://journals.sagepub.com/doi/pdf/10.1177/25152459221095823), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=18418809503165176479&as_sdt=2005&sciodt=0,5&hl=zh-CN)

--- - --

- Becker, T. E., G. Atinc, J. A. Breaugh, K. D. Carlson, J. R. Edwards,P. E. Spector, **2016**, Statistical control in correlational studies: 10 essential recommendations for organizational researchers, **Journal of Organizational Behavior**, 37 (2): 157-167. [-Link-](https://doi.org/10.1002/job.2053), [-PDF-](https://sci-hub.ren/10.1002/job.2053), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=1892263279870015392&as_sdt=2005&sciodt=0,5&hl=zh-CN&scioq=%22Control+Variables+in+Management+Research%22)
- Carlson, K. D., J. Wu, **2012**, The illusion of statistical control: Control variable practice in management research, **Organizational Research Methods**, 15 (3): 413-435. [-Link-](https://doi.org/10.1177/1094428111428817), [-PDF-](https://sci-hub.ren/10.1177/1094428111428817), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=13034321191658128822&as_sdt=2005&sciodt=0,5&hl=zh-CN)
- Loh, Wen Wei, and Dongning Ren. **2021**. “Data-driven Covariate Selection for Confounding Adjustment by Focusing on the Stability of the Effect Estimator.” PsyArXiv. September 27. [-PDF-](https://psyarxiv.com/zkdqa/)
- Hünermund, P., Louw, B., & Caspi, I. (**2023**). Double machine learning and automated confounder selection: A cautionary tale. Journal of Causal Inference, 11(1). [Link](https://doi.org/10.1515/jci-2022-0078), [PDF](https://www.degruyter.com/document/doi/10.1515/jci-2022-0078/pdf), [Google](<https://scholar.google.com/scholar?q=Double machine learning and automated confounder selection: A cautionary tale>).
- Mändli, F., & Rönkkö,, M. (**2023**). To Omit or to Include? Integrating the Frugal and Prolific Perspectives on Control Variable Use. Organizational Research Methods, 28(1), 114–137. [Link](https://doi.org/10.1177/10944281231221703), [PDF](https://journals.sagepub.com/doi/epdf/10.1177/10944281231221703), [Google](<https://scholar.google.com/scholar?q=To Omit or to Include? Integrating the Frugal and Prolific Perspectives on Control Variable Use>).


--- - --

## 相关推文

- 专题：[内生性-因果推断](https://www.lianxh.cn/blogs/19.html)
  - [A-理论部分：控制变量！控制变量！Good-Controls-Bad-Controls](https://www.lianxh.cn/news/c548e33fbb649.html)
  - [B-Stata模拟：控制变量！控制变量！Good-Controls-Bad-Controls](https://www.lianxh.cn/news/a54246bf82745.html)
  - [敏感性分析B-Stata实操：控制变量内生时的系数敏感性分析-regsensitivity](https://www.lianxh.cn/news/b96bf11d5d81a.html)
  - [敏感性分析A-理论基础：控制变量内生时的系数敏感性分析-regsensitivity](https://www.lianxh.cn/news/212015484d17c.html)
- 专题：[论文写作](https://www.lianxh.cn/blogs/31.html)
  - [控制变量如何选？大牛们的10条建议](https://www.lianxh.cn/news/e1324fad69f21.html)

--- - --

- 专题：[回归分析](https://www.lianxh.cn/blogs/32.html)
  - [控制变量越多越好吗？](https://www.lianxh.cn/news/7baa597b33b8d.html)
  - [Stata：控制变量与核心解释变量地位对等吗？](https://www.lianxh.cn/news/4eb3cb3d50dcd.html)
  - [调节效应是否需要考虑对控制变量交乘？](https://www.lianxh.cn/news/cd35b684bea20.html)
  - [控制变量！控制变量！](https://www.lianxh.cn/news/5156229fb5147.html)
  - [不用太关心控制变量，真的！](https://www.lianxh.cn/news/77d0128e722e7.html)
  - [加入控制变量后结果悲催了！](https://www.lianxh.cn/news/046d284fd51b2.html)


--- - --

- 专题：[断点回归RDD](https://www.lianxh.cn/blogs/40.html)
  - [RDD：断点回归可以加入控制变量吗？](https://www.lianxh.cn/news/39b87ee213889.html)
- 专题：[内生性-因果推断](https://www.lianxh.cn/blogs/19.html)
  - [因果推断：双重机器学习-ddml](https://www.lianxh.cn/news/5529578569a81.html)
  - [Stata：控制变量组合的筛选-tuples](https://www.lianxh.cn/news/839f31d03cdf4.html)
  - [Lasso一下：再多的控制变量和工具变量我也不怕-T217](https://www.lianxh.cn/news/8ba33ee7cc1d4.html)
  - [Stata：双重机器学习-多维聚类标准误的估计方法-crhdreg](https://www.lianxh.cn/news/7519c2f054479.html)
  - [DDML：从随机实验到双重机器学习](https://www.lianxh.cn/news/119e3e7a064d7.html)
  - [Stata新命令-pdslasso：众多控制变量和工具变量如何挑选？](https://www.lianxh.cn/news/ccada6e4c41df.html)
  - [DID偏误问题：多时期DID的双重稳健估计量(下)-csdid](https://www.lianxh.cn/news/762e878e7063b.html)
  - [DID偏误问题：两时期DID的双重稳健估计量(上)-drdid](https://www.lianxh.cn/news/e6ef033e13c3e.html)

--- - --

<center>

# 谢&emsp; 谢

<center>