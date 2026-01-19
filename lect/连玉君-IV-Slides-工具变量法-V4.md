---
### ------------------- 幻灯片还是普通网页
marp: true
#marp: false

### ------------------- 幻灯片尺寸，宽版：4:3
size: 16:9
#size: 4:3

### ------------------- 是否显示页码
paginate: true  
#paginate: false

### ------------------- 页眉 (备选的用 '#' 注释掉)
# header: lianxh.cn
#header: '[连享会](https://www.lianxh.cn)'
#header: '[lianxh.cn](https://www.lianxh.cn/news/46917f1076104.html)'

### ------------------- 页脚 (备选的用 '#' 注释掉)
#footer: 'lianx.cn Marp 模板'
footer: '连享会&nbsp;|&nbsp;[lianxh.cn](https://www.lianxh.cn)&nbsp;|&nbsp;[Bilibili](https://space.bilibili.com/546535876)'
#footer: '![20230202003318](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230202003318.png)'
---

<!-- 
Notes: 
1. 选中一段文本，按快捷键 'Ctrl+/' 可以将其注释掉；再次操作可以解除 
2. header, footer 后面的文本需要用单引号引起来，否则会有语法错误
3. '#size: 16:9' 不能写为 'size:16:9'，即 'size:' 后要有一个空格
-->



<!-- Global style: 设置标题字号、颜色 -->
<!-- Global style: 正文字号、颜色 -->
<style>
/*一级标题局中*/
section.lead h1 {
  text-align: center; /*其他参数：left, right*/
  /*使用方法：
  <!-- _class: lead -->
  */
}
section {
  font-size: 24px; 
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
  /*font-size: 28px; */
}
/* 右下角添加页码 */
section::after {
  /*font-weight: bold; */
  /*text-shadow: 1px 1px 0 #fff; */
/*  content: 'Page ' attr(data-marpit-pagination) ' / ' attr(data-marpit-pagination-total); */
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
[lianxh.cn](https://www.lianxh.cn/news/46917f1076104.html) 

<br>

<!--封面图片-->
![bg right:50% w:400 brightness:. sepia:50%](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220722114227.png) 

<!--幻灯片标题-->
# 工具变量法 (IV)


<br>

<br>

<!--作者信息-->
[连玉君](https://www.lianxh.cn) (中山大学)
arlionn@163.com

<br>

---
### 参考资料
- [Qu Zhaopeng, 2021, Lecture 5: Instrumental Variable](https://byelenin.github.io/MicroEconometrics/Slides/2021/GradMetrics_2021_Lec5.pdf)，写的很清楚，slides &#x1F34E; 
- Rubinstein 2016, Slides, [Instrumental Variables](https://yonarubinstein.files.wordpress.com/2016/07/instrumental-variables.pdf)
- [Causal Inference: The Mixtape - 7 Instrumental Variables](https://mixtape.scunning.com/instrumental-variables.html#the-problem-of-weak-instruments) 重点参考
  - [Baylor-Causal-Inference-03-IV-Slides](https://github.com/scunning1975/Baylor-Causal-Inference/blob/main/Slides/03-IV.pdf)
- [applied-methods-phd](https://github.com/paulgp/applied-methods-phd/tree/main/lectures) 
- Cunningham, Scott. "Popular IV Designs". Causal Inference: The Mixtape, New Haven: Yale University Press, 2021, pp. 359-384. [-Link-](https://doi.org/10.12987/9780300255881-031) 
- 参考 https://donskerclass.github.io/EconometricsII/MultivariateIV.html
- Weak IV: [-Link-](http://www3.grips.ac.jp/~yamanota/Lecture%20Note%208%20to%2010%202SLS%20&%20others.pdf)
- Pull, P., 2014, Slides, [Probits,Logits,and 2SLS](https://ocw.mit.edu/courses/14-387-applied-econometrics-mostly-harmless-big-data-fall-2014/resources/mit14_387f14_recitation2/)

--- - --

### Instrumental variables

<br>

> Source: [Baylor-Causal-Inference-03-IV-Slides](https://github.com/scunning1975/Baylor-Causal-Inference/blob/main/Slides/03-IV.pdf)
<!-- - If treatment is tied to an unobservable, then conditioning strategies, even RDD, are invalid
- Instrumental variables offers some hope at recovering the causal effect of $D$ on $Y$
- The best instruments come from deep knowledge of institutional details (Angrist and Krueger 1991)
- Certain types of natural experiments can be the source of such opportunities and may be useful -->

- 如果 Treat 变量与干扰项相关相关 (如存在遗漏变量问题)，则即使是 RDD（断点回归设计）等方法也无法识别因果效应。
- 工具变量为识别 $D$ 对 $Y$ 的因果效应提供了一些可行的方法。
- 最好的工具变量来自对制度细节的深入了解（Angrist 和 Krueger，1991）。
- 某些类型的自然实验可以提供这样的机会。


--- - --

### 何时使用 IV ？
<!-- When is IV used?
Instrumental variables methods are typically used to address the following kinds of problems encountered in naive regressions
1. Omitted variable bias
2. Classical measurement error
3. Simultaneity (eg supply and demand)
4. Reverse causality
5. Randomized control trials with noncompliance
6. Fuzzy RDD -->

工具变量方法通常用于如下场景：
1. 遗漏变量偏误
2. 测量误差 / 衡量偏误
3. 同时性（例如供给和需求）
4. 逆因果关系
5. 非随机控制试验中的不服从性
6. 模糊断点回归设计（Fuzzy RDD）
--- - --

![bg left:32% w:330](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-01.png)

### 遗漏变量偏误
- **真实模型：** $\small Y=X_1 {\color{red}{\beta_1}} + X_2 \beta_2 + {\color{blue}{\varepsilon}}$
- &#x2753; 如果不控制 $x_2$，即 
  - `reg Y X1` $\small\iff$ $\small Y = X_1\theta_1 + \underbrace{{\color{red}{u}}}_{X_2\beta_2+{\color{blue}{\varepsilon}}}$
  - $\widehat{\theta}_1 \neq \beta_1$
- 解决方法：
  - 加入控制变量 `reg y X1 i.industry i.firm` 
  - IV: $Z$
    - $\small\operatorname{Corr}(X_1, Z) \neq 0\ \  (\operatorname{Corr}(X_1, Z)\to 1)$ 
    - $\small \operatorname{Corr}({\color{red}{u}}, Z)=0\ {\color{red}{|}}\  \operatorname{Corr}(X_2, Z)=0$

--- - --

### 遗漏变量偏误分析
> Source: [Omitted Variable Bias: The Simple Case](http://hedibert.org/wp-content/uploads/2016/09/Bias-omittedvariable.pdf)
  - **真实模型：** $(1) \ \ \quad \small Y=X_1 {\color{red}{\beta_1}} + X_2 \beta_2 + {\color{blue}{\varepsilon}}$
  - **真实模型：** $(2) \quad \small X_2 = \rho X_1 + v, \qquad \rho=\operatorname{Corr}\left(X_{1}, X_{2}\right)$

将 (2) 带入 (1)，可得：
- $(3) \quad \small Y=X_1 \underbrace{({\color{red}{\beta_1}}+\rho \beta_2)}_{\gamma} +  \underbrace{({\color{blue}{\varepsilon}}+v\beta_2)}_{\tilde{\varepsilon}} = X_1\gamma + \tilde{\varepsilon}$

<br>

$$
\begin{array}{r|cc}
\hline  \gamma & \rho>0 & \rho<0 \\
\hline \beta_{2}>0 & \text { Positive bias } & \text { Negative bias } \\
\beta_{2}<0 & \text { Negative bias } & \text { Positive bias } \\
\hline
\end{array}
$$

- 偏误程度分析: Wilms, et al. ([2021](https://doi.org/10.1016/j.metip.2021.100075), [PDF](http://sci-hub.ren/10.1016/j.metip.2021.100075))
--- - --

#### **Example 1:** 教育回报率

<br>

$\small\qquad \text{M0: }
\text { wage }=\beta_{0}+\beta_{1} \text { educ }+ {\color{red}{\beta_{2} \text { abil }}}+u$
&emsp; &emsp; &emsp; &emsp; &emsp; &emsp;$\small \ \ \quad\quad\quad \text { educ } = \gamma + \rho \text { abil }$
$\small\qquad \text{M1: }
\text { wage }=\theta_{0} +\ \theta_{1} \text { educ }+v,
$

- $\small\beta_{2}>0$：More ability $\Rightarrow$ higher productivity $\small\Rightarrow$ higher wages
- $\small\rho>0$：$\small\textbf{educ}$ and $\small\textbf{abil}$ are positively correlated. 
- **Consequence:** OLS estimates $\small\widehat{\theta}_1 > \widehat{\beta}_1$,  
  - Note: $\small{\theta}_1 = \beta_1 + \rho \beta_2$

--- - --
<!-- backgroundColor: #e6e6fa -->

### 补充：遗漏变量偏误相关文献

> Source: [-Link-](https://bost.people.uic.edu/uploads/8/2/4/6/82466680/lectures.pdf), p.13
- 遗漏变量偏误：If the variation in $X$ is partly caused by factors that also cause $Y$, this is an **omitted variable** problem.
- 互为因果：If the variation in $\mathrm{X}$ is partly caused by $\mathrm{Y}$, this is **simultaneity**.

<br>

- Wilms, R., E. Mäthner, L. Winnen, R. Lanwehr, **2021**, Omitted variable bias: A threat to estimating causal relationships, **Methods in Psychology**, 5: 100075. [-Link-](https://doi.org/https://doi.org/10.1016/j.metip.2021.100075), [-PDF-](https://sci-hub.ren/https://doi.org/10.1016/j.metip.2021.100075)
- 专题：[回归分析](https://www.lianxh.cn/blogs/32.html)
  - [遗漏变量？敏感性分析！新命令sensemakr-T310](https://www.lianxh.cn/news/deaf1de29c610.html)
- 专题：[内生性-因果推断](https://www.lianxh.cn/blogs/19.html)
  - [Selection Ratio：帮你解决头疼的遗漏变量偏误](https://www.lianxh.cn/news/520d9c77b7b43.html)

--- - --
<!-- backgroundColor: #FFFFF9 -->

## IV 估计：基本思想
<!--![bg left:35% w:350](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220629081608.png)-->

![bg left:35% w:350](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220706115501.png)

<font color=black size=5>

- There is a mediated pathway from $Z$ to $Y$ via $D$. 
- When $Z$ varies, $D$ varies, which causes $Y$ to change. 
- But, even though $Y$ is varying when $Z$ varies, notice that $Y$ is only varying because $D$ has varied. 
- We call this as the "**only through**" assumption. That is, $Z$ affects $Y$ "only through" $D$.

</font> 




--- - --
<!-- backgroundColor: #FFFFF9 -->
![bg left:50% w:500](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/IV_2SLS_YangLiu_cartoon_01.png)

![w:500](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/IV_2SLS_YangLiu_cartoon_02.png)

--- - --

![w:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/IV_2SLS_YangLiu_cartoon_03.png)

<br>


--- - --
### Dirty Weak IV 

<br>

![bg left:46% w:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-IV-01-corr.png)

![w:450](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-IV-01-weak.png)

<br>

<br>

--- - --
### Bad IVs

![bg left:46% w:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-IV-02-bad.png)

![w:450](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-IV-02-nocorr.png)

<br>


  

--- - --

### 结构方程与简约方程

在施加一系列参数假设后，我们定义如下的方程组:
- 结构方程: $\ \ \ \quad (1)\ \  y_{i}=\alpha+\beta x_{i}+\varepsilon_{i}$
- 第一阶段方程: $(2)\ \ x_{i}=\pi_{0}+\pi_{1} z_{i}+v_{i}$

两个方程中的误差项 $\varepsilon_{i}$ 和 $v_{i}$ 可能是相关的，致使 $corr(x,\varepsilon)\neq 0$。可能的原因：
- **混杂因素：** 存在同时影响 $y_{i}$ 和 $x_{i}$ 且无法观测的遗漏变量；
- $x_{i}$ 的 **测量误差** / **衡量偏误**;
- **互为因果**：$y_{i}$ 反过来影响 $x_{i}$。



--- - --

**结构方程**: $\ \ \ \quad (1)\ \  y_{i}=\alpha+\beta x_{i}+\varepsilon_{i}$

**第一阶段方程**: $(2)\ \ x_{i}=\pi_{0}+\pi_{1} z_{i}+v_{i}$

将 (2) 带入 (1)，可得：

**简约式**: (3) 
  $$
  \begin{aligned}
   y_{i} &=\left(\alpha_{0}+\beta \pi_{0}\right)+\left(\beta \pi_{1}\right) z_{i}+\left(\beta v_{i}+\varepsilon_{i}\right) \\
   &=\gamma_{0}+\gamma_{1} z_{i}+\left(\beta v_{i}+\varepsilon_{i}\right)
  \end{aligned}
  $$
因此 ( **LATE**, Local Average Treatment Effect )，
$$\beta= \frac{\gamma_{1}}{\pi_{1}} =\frac{\widehat{\operatorname{Cov}}(y, z)}{\widehat{\operatorname{Cov}}(x, z)}$$

![bg left:35% w:350](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220629081608.png)

--- - --

### IV 估计的假设条件 / 识别条件

- **结构方程**: $\ \ \ \quad (1)\ \  y_{i}=\alpha+\beta x_{i}+\varepsilon_{i}$
- **第一阶段方程**: $(2)\ \ x_{i}=\pi_{0}+\pi_{1} z_{i}+v_{i}$
- **简约式**: $\quad\quad\ \  (3)\ \ y_{i} =\gamma_{0}+\gamma_{1} z_{i}+\left(\beta v_{i}+\varepsilon_{i}\right)$
<br>

获得 $\beta$ 一致估计的假设条件:
- 假设 1 (**相关性**)：$\pi_{1} \neq 0$ ，即要求工具变量可以预测处理变量
- 假设 2 (**外生性 / 排他性**)：$z_{i} \perp \varepsilon_{i}$ 。由于 $\varepsilon_{i}$ 和 $v_{i}$ 可能相关，这意味着 $z_{i} \perp v_{i}$
  - 一是给定外生协变量的 情况下，工具变量是随机或准随机的
  - 二是除了处理变量，工具变量不存在影响结果变量的其他途径
   即，$\small Z \longrightarrow X \longrightarrow Y$

<br>

--- - --
### 2SLS 估计

**第一阶段：** $(1)\ \ x_{i}=\pi_{0}+\pi_{1} z_{i}+v_{i} {\color{blue}{\ \Longrightarrow\ }} \widehat{x} = z\widehat{\pi}_1 =  z \underline{(z'z)^{-1}z'x} \quad ({\color{red}{1a}})$

**第二阶段：** $(2)\ \  y_{i}=\alpha+\beta \widehat{x}_{i}+\varepsilon_{i} {\color{blue}{\ \Longrightarrow\ }} \widehat{\beta}_{OLS}=(\widehat{x}'\widehat{x})^{-1}\widehat{x}'y\quad ({\color{red}{2a}})$ 

用 (${\color{red}{1a}}$) 中的 $\widehat{x}$ 带入 (${\color{red}{2a}}$)，可得 2SLS 估计量：

$$
\beta_{2 S L S}=\left(x^{\prime} P_{z} x\right)^{-1} x^{\prime} P_{z} y
$$
其中，$P_{z}=z\left(z^{\prime} z\right)^{-1} z^{\prime}$。如果模型恰好识别 (工具变量个数等于内生变量个数)，就有:
$$
\hat{\beta}_{2 S L S}=\hat{\beta}_{I V}=\left(z^{\prime} x\right)^{-1} z^{\prime} y
$$
若 $x_{i}$ 和 $z_{i}$ 中都只包含 **一个变量**，则 $2\mathrm{SLS}$ 估计量也可以写成 **Wald 估计量**:
$$
\hat{\beta}_{2 S L S}=\hat{\beta}_{I V}=\hat{\beta}_{W a l d}=\frac{\hat{\gamma}_{1}}{\hat{\pi}_{1}}=\frac{\widehat{\operatorname{Cov}}(y, z)}{\widehat{\operatorname{Cov}}(x, z)}
$$


--- - --

## 特别说明：(self-reading)

假设随机误差项 $u$ 是独立同分布的，大样本条件下 2SLS 估计量的 VCE 的一致估计量为:
$$
\operatorname{Var}\left[\hat{\beta}_{\text {2SLS }}\right]=\hat{\sigma}^{2}\left\{\mathbf{X}^{\prime} \mathbf{Z}\left(\mathbf{Z}^{\prime} \mathbf{Z}\right)^{-1} \mathbf{Z}^{\prime} \mathbf{X}\right\}^{-1}=\hat{\sigma}^{2}\left(\mathbf{X}^{\prime} \mathbf{P}_{Z} \mathbf{X}\right)^{-1}
$$
上式中，
$$
\hat{\sigma}^{2}=\frac{\widehat{\mathbf{u}}^{\prime} \widehat{\mathbf{u}}}{N}
$$
其中， $\widehat{\mathbf{u}}$ 为 $2 S L S$ 的残差，由初始的 $\mathbf{X} ， \mathbf{y}$ 和 2SLS 的估计系数计算得到:
$$
\widehat{\mathbf{u}}=\mathbf{y}-\mathbf{X} \widehat{\boldsymbol{\beta}}_{2 S L S}
$$


--- - --

需要特别说明的是，
- 尽管在思路上，2SLS 法是按照两个阶段回归的顺序进行的，
- 但是我们不能按照这两个阶段的顺序进行手动计算，因为如果这样做的话，第二个阶段的回归将得到错误的残差
$$\widehat{\mathbf{u}}_{i}=\mathbf{y}_{i}-\widehat{\mathbf{X}} \widehat{\boldsymbol{\beta}}_{2 \mathrm{SLS}}$$ 
而正确的残差是 
$$\widehat{\mathbf{u}}_{i}=\mathbf{y}_{i}-\mathbf{X} \widehat{\boldsymbol{\beta}}_{2 \mathrm{SLS}}$$

- **解决办法：** 使用 Stata 的 2SLS 法的命令 `ivreg 2sls` 或 `ivreg2`, `ivreghdfe` 将避免上述错误。
- 完成 2SLS 估计之后，使用 `predict`` 命令将得到正确的残差值。



--- - --

![bg left:45% w:500](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/IV-2SLS-venn-03-weak-irre.png)

- $\small Z_1$: weak IV
* $\small Z_2$: good IV
* $\small Z_3$: ${\color{red}{?}}$
  * $\small Z_3$ 虽然与 $\small X$ 相关，但却无法通过 $X$ 对 $Y$ 产生影响，即
   $\small Z \longrightarrow X \longrightarrow{\!\!\!\!\!\!\!\!\!{/}}$ $Y$
  * &#x2753;  
    $$
     \hat{\beta}_{2 S L S}=\frac{\hat{\gamma}_{1}}{\hat{\pi}_{1}}=\frac{\widehat{\operatorname{Cov}}(y, z)}{\widehat{\operatorname{Cov}}(x, z)}
    $$ 

<!--  
--- - --

### 例子：教育回报率

- `Edu_dad, Edu_mum`
- `near4`
- `birthq`

&#x1F34E;  绘制我的纸质笔记本上的几个 DAG 图形，说明 NearC 可能存在的问题，以及解决方法

--- - --

### 两阶段最小二乘法 (2SLS)

<br>

![w:450](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/IV-2SLS-venn-01.png)

-->


--- - --

## IV-2SLS：另一个角度


![w:600](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/IV_2SLS_omit_z1_z2.png)

--- - --

### 工具变量需要满足的条件：

<!-- 江艇课件 -->
- 相关性：工具变量与内生解释变量相关。
$$
\operatorname{Cov}(Z, D) \neq 0
$$
- 外生性或独立性：工具变量与扰动项不相关。
$$
\operatorname{Cov}(Z, \varepsilon)=0
$$
- 排斥性约束：工具变量只通过 $X$ 或其他变量影响 $Y$，但不直接影响 $Y$。
  - 换言之，$Z$ 不直接出现在结构方程右边。
- 教育回报率例子中的 $Z$ 有哪些可能的选择?

![bg right:45% h:190](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220719213926.png)

--- - --


### Which one is Right ?

<br>

- $\small Z$ 不能与 $\small Y$ 相关，即 $\small \operatorname{corr}(Z,Y)=0$ 
  
<br>

- $\small Z$ 可以与 $\small Y$ 相关，但要满足 $\small Z \longrightarrow X \longrightarrow Y$，
  - 而不是 $\small \qquad\qquad\ {\color{blue}{Y \longleftarrow}} Z \longrightarrow X$  
- 甚至可以是 $\small Z \longrightarrow X \longrightarrow Y$ 且 $\small Z \longrightarrow W \longrightarrow Y$
  - 这时候就需要考虑控制变量的作用了 (next page)

--- - --

### 控制变量的作用 (Source: 江艇课件)

<!-- 江艇课件 -->

- 排斥性约束要求工具变量 $Z$ 只通过内生解释变量 $D$ 影响 $Y$ ，那么在 包含控制变量的工具变量回归中， $Z$ 可不可以通过控制变量 $X$ 影响 $Y$ 呢?

  - 答：当然可以。但这个问题应该反过来理解，正是因为担心工具变 量有除了 $D$ 之外影响 $Y$ 的渠道，故而把这些潜在的渠道尽量控制起 来，这些被控制起来的渠道就成了控制变量。这正是排斥性约束检验的主要思路。
  - 例子：Card (1995) 教育回报论文中，加入 South, ASMA 等控制变量就是这个目的，把这个例子加进来。 

<center>

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220719213149.png)



<!-- 江艇课件 -->
<!-- ### 识别策略 IV：工具变量的证伪检验
- 当排斥性约束满足时, 约简式关系 $=$ 第一阶段关系 $\times$ 因果效应
- 当排斥性约束不满足时, 
  - 约简式关系 $=$ 第一阶段关系 $\times$ 因果效应 $+Z$ 对 $Y$ 的直接效应
- 因此，当第一阶段关系不存在, 但约简式关系却在统计上显著时，很可能说明工具变量有影响被解释变量的直接渠道。因此，可以构造“理论上不存在第一阶段关系”的样本，检验约简式关系是否也不存在，若确实如此，则有理由相信排斥性约束得到满足。

<center>

![h:160](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220719213705.png) -->


<!-- 
--- - --

### 识别策略 V：工具变量疑似内生
- 排斥性约束不满足并不总是坏事。
$$
\begin{gathered}
Y=\beta_{0}+\beta_{1} D+\gamma Z+\varepsilon \\
D=\pi_{0}+\pi_{1} Z+v \\
\hat{\beta}_{1}^{I V}=\frac{\operatorname{Cov}(Y, Z)}{\operatorname{Cov}(D, Z)} \rightarrow_{\mathrm{p}} \beta_{1}+\gamma \frac{\operatorname{Var}(Z)}{\operatorname{Cov}(D, Z)}=\beta_{1}+\frac{\gamma}{\pi_{1}}
\end{gathered}
$$
- 若 $\hat{\beta}_{1}^{I V}$ 与 $\frac{\gamma}{\pi_{1}}$ 同号, 则 $\left|\hat{\beta}_{1}^{I V}\right|>\left|\beta_{1}\right|, \hat{\beta}_{1}^{I V}$ 高估了真实的因果效应;
- 反之, 若 $\hat{\beta}_{1}^{O L S}$ 与 $\frac{\gamma}{\pi_{1}}$ 异号, 则 $\left|\hat{\beta}_{1}^{I V}\right|<\left|\beta_{1}\right|, \hat{\beta}_{1}^{I V}$ 低估了真实的因果效应。
- 再看 Dale and Krueger (1991)。
- 在本文中, $\beta_{1}<0, \pi_{1}<0$, 此时什么样的 $\gamma$ 会对估计结果产生威 胁?


--- - --

- 注意：不能通过用 $Y$ 与 $D$ 和 $Z$ 回归，进而看 $Z$ 的系数估计是否显著来判断排斥性约束是否满足。
- 工具变量方法存在一个基本权衡：工具变量强度 (strength) vs. 排斥性约束的违背程度。
- Conley et al.(2012，REStat) 提出了工具变量排斥性约束不满足时的处理办法。
  
  - 如果我们知道关于 $\gamma$ 取值的信息, 可以对原结构方程进行改造,

$$
Y^{*} \equiv(Y-\gamma Z)=\beta_{0}+\beta_{1} D+\varepsilon
$$

此时，用 $Z$ 作为 $D$ 的工具变量可以得到对 $\beta_{1}$ 的一致估计。
$$
\hat{\beta}_{1}^{I V}(\gamma)=\frac{\operatorname{Cov}\left(Y^{*}, Z\right)}{\operatorname{Cov}(D, Z)}
$$


--- - --

- 等价地, 我们可以考察若要将估计结果完全归因于排斥性约束的违 反, $\gamma$ 必须达到多大。在本文中，对于给定的 $\gamma$, 计算 $\hat{\beta}_{1}^{I V}$ 的 $(1-\alpha)$ 置信区间，
$$
\left.C I(1-\alpha, \gamma)=\left[\hat{\beta}_{1}^{I V}(\gamma) \pm t_{\alpha / 2} \text { s.e. } \widehat{\left(\hat{\beta}_{1}^{I V}\right.}(\gamma)\right)\right]
$$
选取初始 $\gamma$ ，若置信区间不包含零，则意味着 $\hat{\beta}_{1}^{I V}$ 在 $\alpha$ 水平下显 著，继续增大 $\gamma$, 直到置信区间首次包含零（上界等于零）。由此得 到的 $\gamma$ 的临界值如果过大，则认为估计结果对于工具变量的疑似 内生性稳健。


Applying Conley, Hansen, and Rossi (2008), we can show that the positive estimate of $\gamma$ reported in column 4 is not above the value of $\gamma$ necessary to lose confidence in the finding of a negative impact of the slave trade on trust. For the 90 percent confidence interval for $\beta$ to include zero, $\gamma$ must be larger than $56 \times 10^{-6}$. This is over eight times greater than the estimate of $7 \times 10^{-6}$ from column 4 of Table 4. Therefore, even allowing for plausible amounts of imperfect exogeneity, we are still able to confirm a negative effect of the slave trade on trust. ${ }^{30}$ 


--- - --
<!-- backgroundColor: white -->

<!-- > 相关推导：自行阅读 

## Matrix IV
Setup:
- $n \times 1$ vector $Y, n \times r$ "endogenous" matrix $X_{1}$
- $n \times s$ matrix of "controls" $X_{2}, n \times t$ matrix of "instruments" $Z_{1}$

$$\small
\begin{array}{r}
X_{1}=Z_{1} \pi_{1}+X_{2} \pi_{2}+v \\
Y=X_{1} \beta_{1}+X_{2} \beta_{2}+\varepsilon
\end{array}
$$

--- - --

Terminology:
- (1) the first stage; (2) the second stage.
- Plugging (1) into (2) gives the reduced form:

$$\small
\begin{aligned}
y &=\left(Z \pi_{1}+X_{2} \pi_{2}+v\right) \beta_{1}+X_{2} \beta_{2}+\varepsilon \\
&=Z_{1}\left(\pi_{1} \beta_{1}\right)+X_{2}\left(\pi_{2} \beta_{1}+\beta_{2}\right)+\left(v \beta_{1}+\varepsilon\right)
\end{aligned}
$$

- Model is identified if $t \geq r$ (just-identified if $t=r$ )
- Exclusion restriction: $E\left[Z^{\prime} \varepsilon\right]=0$ (weak), $E[\varepsilon \mid Z]=0$ (strong)


--- - --

## Matrix-y IV (cont.)
$$\small
\begin{array}{r}
X_{1}=Z_{1} \pi_{1}+X_{2} \pi_{2}+v \\
Y=X_{1} \beta_{1}+X_{2} \beta_{2}+\varepsilon
\end{array}
$$
Define:
$$\small
\begin{array}{ll}
X \equiv\left[\begin{array}{ll}
X_{1} & X_{2}
\end{array}\right], \quad n \times(r+s) \\
Z \equiv\left[\begin{array}{ll}
Z_{1} & X_{2}
\end{array}\right], \quad n \times(t+s)
\end{array}
$$
Also define:
$$\small
P_{Z} \equiv Z\left(Z^{\prime} Z\right)^{-1} Z^{\prime}, P_{2} \equiv X_{2}\left(X_{2}^{\prime} X_{2}\right)^{-1} X_{2}, M_{2} \equiv I-P_{2}
$$
What's $P_{Z} Z=$ ? $P_{Z} X=? \quad M_{2} X_{2}=$ ? $P_{Z} P_{Z}=$ ? $P_{Z}^{\prime}=$ ?


--- - --

## 2SLS is an IV Estimator
IV Estimator:
$$\small
\begin{aligned}
\widehat{\beta^{I V}} & \equiv\left(W^{\prime} X\right)^{-1} W^{\prime} Y \\
W & \equiv Z A
\end{aligned}
$$
where $A=(t+s) \times(r+s)$ is some (possibly random) matrix.
Note that when we' re just-identified $(t=r) A$ is (probably) invertible, so
$$\small
\begin{aligned}
\widehat{\beta^{I V}} & \equiv\left(A^{\prime} Z^{\prime} X\right)^{-1} A^{\prime} Z^{\prime} Y \\
&=\left(Z^{\prime} X\right)^{-1} A^{\prime-1} A^{\prime} Z^{\prime} Y=\left(Z^{\prime} X\right)^{-1} Z^{\prime} Y
\end{aligned}
$$
$\Longrightarrow$ all IV estimators are (numerically) equivalent when just-id
Two-Stage Least Squares sets $\small A \equiv\left(Z^{\prime} Z\right)^{-1} Z^{\prime} X$. What's W?


--- - --

## 2SLS is a second-stage WLS/OLS regression
Two-Stage Least Squares is
$$\small
\begin{aligned}
\widehat{\beta^{2 S L S}} &=\left(\left(Z\left(Z^{\prime} Z\right)^{-1} Z^{\prime} X\right)^{\prime} X\right)^{-1}\left(Z\left(Z^{\prime} Z\right)^{-1} Z^{\prime} X\right)^{\prime} Y \\
&=\left(\left(P_{Z} X\right)^{\prime} X\right)^{-1}\left(P_{Z} X\right)^{\prime} Y \\
&=\left(X^{\prime} P_{Z} X\right)^{-1} X^{\prime} P_{Z} Y \qquad\qquad\quad (3) \\
&=\left(\left(P_{Z} X\right)^{\prime} P_{Z} X\right)^{-1}\left(P_{Z} X\right)^{\prime} Y \qquad (4)
\end{aligned}
$$
- (some kinda) Weighted Least Squares, by (3). What are the weights doing?
- (some kinda) Ordinary Least Squares, by (4). What are the regressors?


--- - --

## Just- "reduced-form over first-stage"
$\widehat{\beta^{2 S L S}}$ is OLS of $Y$ on $P_{Z} X$
When $r=1$ (one endogenous regressor), $\widehat{\beta_{1}^{2 S L S}}$ is bivariate OLS of $Y$ on $M_{2} P_{Z} X$
$$\small
\widehat{\beta_{1}^{2 S L S}} \stackrel{p}{\rightarrow} \frac{\operatorname{Cov}\left(y_{i}, \hat{x}_{1 i}^{*}\right)}{\operatorname{Var}\left(\hat{x}_{1 i}^{*}\right)}=\frac{\operatorname{Cov}\left(y_{i}, \hat{x}_{1 i}^{*}\right)}{\operatorname{Cov}\left(x_{1 i}^{*}, \hat{x}_{1 i}^{*}\right)}
$$
When $t=r$ (just-identified),
$$\small
\begin{aligned}
\operatorname{Var}\left(\hat{x}_{1 i}^{*}\right) &=\pi_{1}^{2} \operatorname{Var}\left(Z_{1 i}^{*}\right) \\
\operatorname{Cov}\left(y_{i}, \hat{x}_{1 i}^{*}\right) &=\operatorname{Cov}\left(\left(\pi_{1} \beta_{1}\right) Z_{1}+X_{2}\left(\pi_{2} \beta_{1}+\beta_{2}\right)+\left(v \beta_{1}+\varepsilon\right), \pi Z_{i}^{*}\right) \\
&=\pi_{1}^{2} \beta \operatorname{Var}\left(Z_{1 i}^{*}\right)
\end{aligned}
$$
so that
$$\small
\widehat{\beta_{1}^{2 S L L}} \stackrel{p}{\rightarrow} \frac{\pi_{1}^{2} \beta \sigma_{Z^{*}}^{2}}{\pi_{1}^{2} \sigma_{Z^{*}}^{2}}=\underbrace{\overbrace{\pi_{1} \beta}^{R F}}_{F S}=\beta
$$
-->


<!-- 

## 2SLS

> [link](https://mixtape.scunning.com/instrumental-variables.html#heterogeneous-treatment-effects)

 -->


<!-- ### Two-stage least squares -->
<!--https://github.com/scunning1975/Baylor-Causal-Inference/blob/main/Slides/03-IV.pdf-->
<!-- Recall
$$
S_{i}=\gamma+\rho Z_{i}+\zeta_{i}
$$
Then
$$
\widehat{S}=\widehat{\gamma}+\widehat{\rho} Z
$$
Then
$$
\widehat{\delta}_{2 s l s}=\frac{\operatorname{Cov}(\widehat{\rho} Z, Y)}{\operatorname{Var}(\widehat{\rho} Z)}=\frac{\operatorname{Cov}(\widehat{S}, Y)}{\operatorname{Var}(\widehat{S})}
$$ -->


--- - --

<!-- ### Proof.
We will show that $\widehat{\delta} \operatorname{Cov}(Y, Z)=\operatorname{Cov}(\widehat{S}, Y)$. I will leave it to you to show that $\operatorname{Var}(\widehat{\delta} Z)=\operatorname{Var}(\widehat{S})$
$$
\begin{aligned}
\operatorname{Cov}(\widehat{S}, Y) &=E[\widehat{S} Y]-E[\widehat{S}] E[Y] \\
&=E(Y[\widehat{\rho}+\widehat{\delta} Z])-E(Y) E(\widehat{\rho}+\widehat{\delta} Z) \\
&=\widehat{\rho} E(Y)+\widehat{\delta} E(Y Z)-\widehat{\rho} E(Y)-\widehat{\delta} E(Y) E(Z) \\
&=\widehat{\delta}[E(Y Z)-E(Y) E(Z)] \\
\operatorname{Cov}(\widehat{S}, Y) &=\widehat{\delta} \operatorname{Cov}(Y, Z)
\end{aligned}
$$

> Source: [Baylor-Causal-Inference-03-IV](https://github.com/scunning1975/Baylor-Causal-Inference/blob/main/Slides/03-IV.pdf) -->



### Stata 实操：2SLS

<!-- backgroundColor: #FFFFF9 -->
- [Baylor-Causal-Inference/Automation/card2.do](https://github.com/scunning1975/Baylor-Causal-Inference/blob/main/Automation/card2.do)
  
```stata
use https://github.com/scunning1975/mixtape/raw/master/card.dta, clear

* OLS estimate of schooling (educ) on log wages
reg lwage  educ  exper black south married smsa

* 2SLS estimate of schooling (educ) on log wages 
*      using "college in the county" as an instrument for schooling
ivregress 2sls lwage (educ=nearc4) exper black south married smsa, first 

* First stage regression of schooling (educ) on 
* all covariates and the college and the county variable
reg educ nearc4 exper black south married smsa

* F test on the excludability of college in the county 
*   from the first stage regression.
test nearc4
```
具体解释参见 [scunning - book](https://mixtape.scunning.com/07-instrumental_variables#heterogeneous-treatment-effects)




--- - --

<!-- backgroundColor: #C1CDCD -->

# 弱工具变量 / Many IVs


--- - --

<!-- backgroundColor: white -->
![bg left:35% w:350](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220629081608.png)


### IV 估计的假设条件 / 识别条件

- **结构方程**: $\ \ \ \quad (1)\ \  Y = \alpha+\beta D+\varepsilon$
- **第一阶段方程**: $(2)\ \ D = b + \pi Z + v$
- **简约式**: $\quad\quad\ \  (3)\ \ Y =c + \gamma Z + \left(\beta v+\varepsilon\right)$

<br>

$$\hat{\gamma} = \hat{\pi}\times \hat{\beta} \Longrightarrow \hat{\beta} = \frac{\hat{\gamma}}{\hat{\pi}}$$ 
<br>

> 如果我们可以准确衡量 $\hat{\pi}$，那么即使它的数值很小，也不影响对 $\hat{\beta}$ 的估计。然而，事实并非如此。

--- - --
<!-- backgroundColor: #FFFFF9 -->

- 弱 IV：因为 IV 系数本质上是个比值，当 $\operatorname{Cov}(Z, X) \approx 0$ ，弱 IV 就是一个 “分母为 0 ” 的问题。
- 弱工具变量会导致如下问题:
  - 即使排他性假设成立，也会导致 2SLS 估计结果非常不准确 (SE 很大);
  - 样本量不足以支持假设检验的需求， $t$ 统计量并不服从 $t$ 分布。
  
- 经验法则：
  - 为了克服这个问题, 传统上要求第一阶段的 $F$ 值大于 10 ，且研究者需要将其明确地报告出来。
  - 近期，也有学者认为 $F$ 值应该更高。
  - &#x1F34E; $F$ 值的临界值要根据样本数来确定，Y2005 的文章中有提供。具体参见 Hansen2021，Chap12 中的两个实例 Sec 12.41. 另见，[Causal Inference: The Mixtape - 7 Instrumental Variables](https://mixtape.scunning.com/instrumental-variables.html#the-problem-of-weak-instruments)


--- - --
### 弱工具变量应对

- IVs + IVs 的交乘项 
  - [多个(弱)工具变量如何应对-IV-mivreg？](https://www.lianxh.cn/news/807b616b11aae.html)，提供了 MORZ.dta 的范例
  - Angrist 1991，`i.Qbir#.i.year  i.Qbir#i.State`，详见 Hansen2021, Sec 12.41 中的说明
    - 此时，Yo 2005 的 F 检验可能无法通过，但减少 IV 个数后就好了
    - 启发：用 Lasso IV 或 MA 方法或许可以解决这个问题
  - Lasso 讲义中介绍 Lasso IV 时提供的两个例子，Acemogulu, AER



--- - --
<!-- backgroundColor: #C1CDCD -->

## 弱工具变量案例 —— Angrist and Krueger (1991)


--- - --
<!-- backgroundColor: #FFFFF9 -->
### 教育回报中的工具变量：出生季度

<br>


![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220601153719.png)

> Source: [Causal Inference: The Mixtape - 7 Instrumental Variables](https://mixtape.scunning.com/instrumental-variables.html#the-problem-of-weak-instruments)

--- - --

![bg left:60% w:750](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220707223747.png)

> First stage relationship between quarter of birth and schooling.  
>    
> Source: Angrist and Krueger (1991).


--- - --

![bg left:55% w:720](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220707224129.png)

> Reduced form visualization of the relationship between quarter of birth and log weekly earnings.  
>    
> Source: Angrist and Krueger (1991).

Figure 7.6 shows the reduced-form relationship between quarter of birth and log weekly earnings. You have to squint a little bit, but you can see the pattern—all along the top of the jagged path are 3s and 4s, and all along the bottom of the jagged path are 1s and 2s. Not always, but it’s correlated.

--- - --


Poi (2006) 对 Angrist and Krueger (1991) 存在的 Weak IV 问题进行了较为细致的介绍。

- Poi, B. P., **2006**, Jackknife Instrumental Variables Estimation in Stata, **Stata Journal**, 6(3): 364–376. [-PDF-](https://journals.sagepub.com/doi/pdf/10.1177/1536867X0600600305), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=1180073605559606482&as_sdt=2005&sciodt=0,5&hl=zh-CN) 
- Regressions of education on Angrist and Krueger’s seasonal dummy instruments resulted in $R^2$ values as low as 0.0001, suggesting that the bias of 2SLS was severe.
- The 2SLS estimator is biased toward the ordinary leastsquares (OLS) estimator, and that bias becomes more severe as the correlation between the endogenous regressor and the instruments approach zero. (Bound, Jaeger, and Baker (1995))
  - 实操建议：要同时呈现 OLS 和不同 IV 设定下的 2SLS 的结果，进行对比
  - 证据：Angrist and Krueger (1991) 的 IV 估计结果与 OLS 结果非常接近，0.08 v.s 0.07；而 Card 1995 使用 NearC 估计的结果是 $\widehat{\beta}=0.13$
    - 参见 Hansen2021，chp12 中的介绍


--- - --

### 弱工具变量检验
- 第一阶段回归  
  - The first-stage $R^{2}$ is also often used to gauge the validity of instruments. Although there is no universally accepted test for or against weak instruments, the first-stage $F$ statistic and $R^{2}$ both have the benefit of intuitive appeal. (Poi, [2006](https://journals.sagepub.com/doi/pdf/10.1177/1536867X0600600305), SJ)
- F test
  - Stock, Wright, and Yogo (2002) indicate that the first-stage $F$ statistic typically must **exceed 10** for inference based on the 2SLS estimator to be reliable. 
  - That paper also includes an alternative to the $F$ statistic to use when the number of endogenous regressors is greater than one, though the alternative is not implemented in jive.

<!--  
- 参见 Latex 讲义对应部分的参考文献
- 主要介绍 Hansen2021 书中的两个例子即可
  - Angrist 1991 例子中，通过减少 IV 的个数，仅保留 `i.Qbirth` 可以保证 F>10  
-->

--- - --

- Anatolyev S, Skolkova A., 2019, Many instruments: Implementation in Stata, **Stata Journal**, 19(4): 849–866. [-PDF-](https://sci-hub.ren/10.1177/1536867X19893627), [-PDF2-](https://journals.sagepub.com/doi/pdf/10.1177/1536867X19893627)
- Anna Mikusheva,  Brian P. Poi, **2006**, Tests and Confidence Sets with Correct Size when Instruments are Potentially Weak, **Stata Journal**, 6(3): 335–347. [-PDF-](https://journals.sagepub.com/doi/pdf/10.1177/1536867X0600600303)
- Carolin E. Pflueger,  Su Wang, 2015, A Robust Test for Weak Instruments in Stata, **Stata Journal**, 15(1): 216–225. [-PDF-](https://journals.sagepub.com/doi/pdf/10.1177/1536867X1501500113)
- Keith Finlay,  Leandro M. Magnusson, **2009**, Implementing Weak-Instrument Robust Tests for a General Class of Instrumental-Variables Models, **Stata Journal**, 9(3): 398–421. [-PDF-](https://journals.sagepub.com/doi/pdf/10.1177/1536867X0900900304)
- Keith Finlay, Leandro M. Magnusson, **2009**, Implementing Weak-Instrument Robust Tests for a General Class of Instrumental-Variables Models, **Stata Journal**, 9(3): 398–421. [-PDF-](https://journals.sagepub.com/doi/pdf/10.1177/1536867X0900900304)


--- - --
### 弱工具变量检验 

> `help estat firststage`，[**[R]** estat firststage](https://www.stata.com/manuals/rivregresspostestimation.pdf) 

When instruments that are only weakly correlated with the endogenous regressors,  
- the usual 2SLS, GMM, and LIML estimators are biased toward the OLS estimator, 
- inference based on the standard errors reported by, for example, `ivregress` can be severely misleading. 

For more information on the theory behind instrumental-variables estimation with weak instruments, see 
- Nelson and Startz (1990); Staiger and Stock (1997); Hahn and Hausman (2003); 
- survey: Stock, Wright, and Yogo (2002); and Angrist and Pischke (2009, chap. 4).


--- - --

- [IV：工具变量不满足外生性怎么办？](https://www.lianxh.cn/news/c66c145337ffa.html)
- [twostepweakiv：弱工具变量有多弱？](https://www.lianxh.cn/news/2e36ddefa9f76.html)
- [多个(弱)工具变量如何应对-IV-mivreg？](https://www.lianxh.cn/news/807b616b11aae.html)

<!-- --- - --
### LIML estimator
> Source: > `help estat firststage`，[**[R]** estat firststage](https://www.stata.com/manuals/rivregresspostestimation.pdf) 

- When the instruments are only weakly correlated with the endogenous regressors, some Monte Carlo evidence suggests that the **LIML estimator** performs better than the 2SLS and GMM estimators; 
  - see, for example, Poi (2006) and Stock, Wright, and Yogo (2002) (and the papers cited therein). 
- On the other hand, the LIML estimator often results in confidence intervals that are somewhat larger than those from the 2SLS estimator. -->

--- - --

### 加入更多的工具变量？
> Source: > `help estat firststage`，[**[R]** estat firststage](https://www.stata.com/manuals/rivregresspostestimation.pdf) 

Moreover, using more instruments is not a solution, 
- because the biases of instrumental-variables estimators increase with the number of instruments. See Hahn and Hausman (2003).


--- - --

## Many IVs

> Poi, B. P., **2006**, Jackknife Instrumental Variables Estimation in Stata, **Stata Journal**, 6(3): 364–376. [-PDF-](https://journals.sagepub.com/doi/pdf/10.1177/1536867X0600600305), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=1180073605559606482&as_sdt=2005&sciodt=0,5&hl=zh-CN)

Even when the instruments are relevant in the sense that they are sufficiently correlated with the endogenous variable, the 2SLS estimator still exhibits a bias that increases as more instruments are used. Researchers often use many instruments under the presumption that doing so will make up for the instruments' being weak. However, that logic is faulty, because the bias of the 2SLS estimator increases with the number of instruments; and, to the extent that the instruments are correlated with one another, using more of them may not aid in identification.

- 解决办法：Lasso IV。Stata 命令：`help poivregress`, `xpoivregress`, `xpoivregress`


--- - --
<!-- backgroundColor: #C1CDCD -->
## 内生性检验和过度识别检验

- 专题：[面板数据](https://www.lianxh.cn/blogs/20.html)
  - [Sargan+Hansen：过度识别检验及Stata实现](https://www.lianxh.cn/news/67e04ad54052c.html)
- 专题：[IV-GMM](https://www.lianxh.cn/blogs/38.html)
  - [IV专题- 内生性检验与过度识别检验](https://www.lianxh.cn/news/cc3f92710dd7a.html)


--- - --
<!-- backgroundColor: #C1CDCD -->

## IV 估计：排他性约束


参见 
- [工具变量-IV：排他性约束及经典文献解读](https://www.lianxh.cn/news/d4e7734f4d10d.html)



--- - --

<!-- backgroundColor: #C1CDCD -->

# IV 实例：股市波动与金融改革

> Bonfiglioli, A., R. Crinò, G. Gancia, **2022**, Economic uncertainty and structural reforms: Evidence from stock market volatility, **Quantitative Economics**, 13 (2): 467-504. [-Link-](https://doi.org/10.3982/qe1551), [-PDF-](https://aisberg.unibg.it/retrieve/14ae8c09-f105-44b8-bb6a-8bfc42e4ace7/1621-7532-1-SP.pdf), [PDF2](https://onlinelibrary.wiley.com/doi/pdf/10.3982/QE1551), [Appendix](https://www.econometricsociety.org/publications/quantitative-economics/2022/05/01/Economic-uncertainty-and-structural-reforms-Evidence-from-stock-market-volatility/supp/1621-7534-1-SP.pdf), [Replication](https://www.econometricsociety.org/publications/quantitative-economics/2022/05/01/Economic-uncertainty-and-structural-reforms-Evidence-from-stock-market-volatility/supp/QE1551_code_and_data.zip) 
>
> 本地-复现文档：【**../B2_IV/Bonfiglioli-2022-QE-IV**】

--- - --
<!-- backgroundColor: #FFFFF9 -->

## 1. 简介


- 经济的不确定性是否促进了结构性改革的实施？我们使用关于六个主要领域改革的最详尽的跨国小组数据集来回答这个问题，并用股市波动来衡量经济不确定性。
- 为了确定因果关系，作者采用 IV 估计。基本的识别策略是利用其它国家股市与本国的相关性来定义外生的工具变量。IV = SUM_j [第 j 国股市波动 X 第 j 国与本国的经济依赖程度]。
- 模型设定中包含了大量控制变量，如：政治变量、经济变量、危机指标和一系列国家、改革和时间固定效应，以及适应异质趋势和同期冲击的各种方法。
- 在所有设定中，我们发现股市波动对改革的实施有着积极而显著的影响。总的来说，在以高度不确定性为特征的市场动荡期间，一些原本不会实施的改革政策可能会被采用。

--- - --


## 2. 模型设定和 IV 识别策略

### 2.1 OLS - outcome equation

We study how **economic volatility** affects the implementation of structural reforms:

$$
ref_{s, c, t}=\eta_{s, c}+\eta_{s, t}+\beta_1 l i b_{s, c, t-1}+\beta_2 \operatorname{vol}_{c, t-1}+\boldsymbol{\beta}_3 \mathbf{X}_{s, c, t-1}+\beta_{4, c} t + \boldsymbol{\epsilon}_{s, c, t} \quad (1)
$$

where,
- $ref_{s, c, t} \equiv l i b_{s, c, t}-l i b_{s, c, t-1}$
  - the change in the **liberalization index (lib)** for sector $s$ and country $c$ over year $t$, 
- $vol_{c, t-1}$ is the **stock market volatility** of country $c$ at time $t-1$. 

--- - --

**Control variables**:
- $\eta_{s, c}$, country-sector fixed effects, , to absorb time-invariant determinants of reforms in each sector of a given country.
- $\eta_{s, t}$, sector-year fixed effects, to account for aggregate trends and global shocks that could induce reforms within a given sector across all countries. 
- $\beta_{4, c} t$, country-specific linear trends, and 
- $\mathbf{X}_{s, c, t-1}$, a rich set of covariates, to absorb other time-varying determinants of reforms potentially correlated with volatility. 
- $l i b_{s, c, t-1}$, we control for the **start-of-period level of the liberalization index**, to account for the fact that reforms may proceed at different pace across countries and sectors, depending on their initial level of regulation. 
  - For instance, it is often argued that the benefits of reforms are greater when starting from a higher level of regulation, which would imply $\beta_1<0$. 
  - Including $l i b_{s, c, t-1}$ **also helps** comparability with the empirical literature on both structural and fiscal reforms, where this term is standard. 

--- - --

## 2.2 内生性来源：反向因果

Indeed, the political debate over the design and approval of a reform could influence volatility in the years prior to the adoption of the reform itself. The bias generated by this **reverse causality** could go either way. 

**B1. 预期效应**：

> 讨论改革方案 &rarr; 经济和政治不确定性 (&uarr;) &rarr; 股市波动 (&uarr;) &rArr; $\beta_2$ 被高估
- (1) the discussion over a reform could raise economic and political uncertainty, implying an upward bias in the OLS estimate of $\beta_2$. 

**B2. 交易效应**：

> 近期即将实施的改革 &rarr; 交易量 (&darr;) &rarr; 股市波动 (&darr;) &rArr; $\beta_2$ 被低估
- (2) the prospect of a reform in the near future may contribute to calming down the stock market before the reform is actually implemented, implying a downward bias in the coefficient $\beta_2$ estimated by OLS.

--- - --

## 2.3 构造 IV：类似于 share shock 的构造方法

**基本思路**：用其它国家的 $\operatorname{vol}_{jt}$ 的加权平均值构造工具变量。原因如下：
- 各国之间的股市波动具有传导效应
- 两国之间的 (经济、政治) 关联越密切，传导效应越强

$$
\operatorname{vol}\_\operatorname{shock}_{c, t-1}=\sum_{j \in \Omega_{-c}} \frac{\operatorname{vol}_{j, t-1}}{N_{-c}} \times \ln \operatorname{Int}_{c, j} \quad (2)
$$

写成如下形式更容易理解：

$$
\operatorname{vol}\_\operatorname{shock}_{c, t-1}=\sum_{j \in \Omega_{-c}} {\operatorname{vol}_{j, t-1}} \times w_{c,j}, \quad w_{c,j} = \frac{\ln \operatorname{Int}_{c, j}}{{N_{-c}}} \quad (2a)
$$
- $\operatorname{vol}_{j, t-1}$ is the stock market volatility of country $j \neq c$ in year $t-1$, 
- $\Omega_{-c}$ and $N_{-c}$ are the set and the number of countries excluding $c$, 
- $Int_{c, j}$ is a measure of **economic integration** between countries $c$ and $j$. 
  - $Int_{c, j}^A$ = inverse of their geographical distance. 可以视为完全外生变量
  - $Int_{c, j}^B$ = predetermined component of **bilateral trade** due to geographical


--- - --

## 辩护：这是个好的工具变量

<br>

- This instrument is meant to isolate the differential variation that foreign volatility shocks induce on the volatility of each country $c$, depending on how interconnected it is with the origins of these shocks. 
- The instrument accommodates both the absolute integration of each country and relative differences in integration across countries. 
- To keep the composition of the instrument constant, we include in the set $\Omega_{-c}$ all countries with available data on stock market volatility for all years.
- 接下来，作者花了大量的篇幅来讨论该工具变量的合理性，包括：相关性和外生性。参见原文 pp. 480-481

--- - --

## 3. 结果： OLS 结果 + 不同的控制变量

$$
ref_{s, c, t}=\eta_{s, c}+\eta_{s, t}+\beta_1 l i b_{s, c, t-1}+\beta_2 \operatorname{vol}_{c, t-1}+\boldsymbol{\beta}_3 \mathbf{X}_{s, c, t-1}+\beta_{4, c} t + \boldsymbol{\epsilon}_{s, c, t} \quad (1)
$$

![h:450](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230713163321.png)

--- - --

#### Figure 3: 图示 OLS 估计系数：初步排除反向因果关系
$$
ref_{s, c, t}^\tau=\eta_{s, c}+\eta_{s, t}+\beta_1 l i b_{s, c, t-\tau}+\beta_2 \operatorname{vol}_{c, t-\tau}+\boldsymbol{\beta}_3 \mathbf{X}_{s, c, t-\tau}+\beta_{4, c} t+\epsilon_{s, c, t} \quad (3)
$$

- $\operatorname{ref}_{s, c, t}^\tau \equiv l i b_{s, c, t}-l i b_{s, c, t-\tau}$ and $\tau \in\{3,5\}$.

![h:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230714000245.png)



--- - --

> Figure 3. Timing of the relationship between volatility and reforms. 

Note: 
- Each coefficient is obtained by separately estimating the specification in column (7) of Table 3 using a different lag or lead of volatility, as indicated on the horizontal axis. 
- All regressions are estimated using OLS. 
- The confidence intervals are based on standard errors corrected for clustering at the country level and refer to the 95 per cent significance level.

--- - --

### 3.2 2SLS 估计结果

$$
ref_{s, c, t}=\eta_{s, c}+\eta_{s, t}+\beta_1 l i b_{s, c, t-1}+\beta_2 \operatorname{vol}_{c, t-1}+\boldsymbol{\beta}_3 \mathbf{X}_{s, c, t-1}+\beta_{4, c} t + \boldsymbol{\epsilon}_{s, c, t} \quad (1)
$$

$$
\operatorname{vol}\_\operatorname{shock}_{c, t-1}=\sum_{j \in \Omega_{-c}} \frac{\operatorname{vol}_{j, t-1}}{N_{-c}} \times \ln \operatorname{Int}_{c, j} \quad (2)
$$

```stata
* Column 1                     x = f(z)
reghdfe x iv  $CXX

* Column 2                     y = f(z)
reghdfe y iv  $CXX 

* Column 3                 ivreg y (x=z)
reghdfe y (x = iv) $CXX, stages(first reduced) 
```

--- - --

![Bonﬁglioli, et al., 2022, Quantitative Economics 13, Table 4 h:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230714041037.png)

> Note: The Kleibergen–Paap F -statistic is equal to 82.9, and thus exceeds the value of 10 normally considered as a rule-of-thumb threshold for instrument relevance.



--- - --

<!-- backgroundColor: pink -->

# 自读：典型 IV

> 后续内容不讲，留给大家自行研读

--- - --
<!-- backgroundColor: #FFFFF9 -->
## 典型 IV：降雨量 (经济增长)

- 由于内生性和遗漏变量偏差，估计经济状况对国内冲突可能性的影响颇具挑战。
- 我们使用 **降雨量变化** 作为 1981-99 年间 41 个非洲国家经济增长的 **工具变量**。 
- 增长与国内冲突密切相关：5 个百分点的负增长冲击会使次年发生冲突的可能性增加一半。 我们试图排除降雨可能影响冲突的其他渠道。 令人惊讶的是，在更富裕、更民主或种族更多样化的国家，增长冲击对冲突的影响并没有显着差异。
 
- Miguel, E., S. Satyanath, E. Sergenti, **2004**, Economic shocks and civil conflict: An instrumental variables approach, **Journal of Political Economy**, 112 (4): 725-753. [-Link-](https://doi.org/10.1086/421174), [-PDF-](https://sci-hub.ren/10.1086/421174), [-Slides-](https://www.uio.no/studier/emner/sv/oekonomi/ECON4918/h14/miguel_etal_slides.pdf), [Replication](https://doi.org/10.7910/DVN/27324), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=7490175262554688472&as_sdt=2005&sciodt=0,5&hl=zh-CN)
  - Miguel, E., S. Satyanath, **2011**, Re-examining economic shocks and civil conflict, **AEJ: AE**, 3 (4): 228-232. [-Link-](https://doi.org/10.1257/app.3.4.228), [-PDF-](https://sci-hub.ren/10.1257/app.3.4.228), [Replication](https://doi.org/10.3886/E113802V1), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=10440418962698472574&as_sdt=2005&sciodt=0,5&hl=zh-CN)



--- - --

### Potential Violations of the Exclusion Restriction 
> Miguel, et al., JPE, ([2004](https://doi.org/10.1086/421174), [PDF](http://sci-hub.ren/10.1086/421174))
- **Exclusion restriction:** Weather shocks should affect civil conflict only through economic growth.
- Economic channels other than per capita economic growth per se (i.e., income inequality or rural poverty rates) may be key underlying causes of civil conflict in the aftermath of adverse rainfall shocks
- High rainfall might directly affect civil conflict independently of economic conditions. For instance, floods may destroy the road network and thus make it more costly for government troops to contain rebel groups.
- Not a serious threat to estimation strategy, since higher rainfall levels are associated with significantly less conflict in the reduced-form regressions. Thus, the estimates would be lower bounds on the true impact of economic growth on civil conflict.

--- - --

![h:550](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220703193926.png)

> **Source:** Miguel and Satyanath, **2011**, **AEJ: AE**. [-Link-](https://doi.org/10.1257/app.3.4.228), [-PDF-](https://sci-hub.ren/10.1257/app.3.4.228), [Replication](https://doi.org/10.3886/E113802V1), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=10440418962698472574&as_sdt=2005&sciodt=0,5&hl=zh-CN)

--- - --

![h:550](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220703194254.png)

> **Source:** Miguel and Satyanath, **2011**, **AEJ: AE**. [-Link-](https://doi.org/10.1257/app.3.4.228), [-PDF-](https://sci-hub.ren/10.1257/app.3.4.228), [Replication](https://doi.org/10.3886/E113802V1), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=10440418962698472574&as_sdt=2005&sciodt=0,5&hl=zh-CN)

--- - --
### Miguel and Satyanath, **2011**, **AEJ: AE**. [Replication](https://doi.org/10.3886/E113802V1)
```stata
* https://doi.org/10.3886/E113802V1
use "AEJApp_2011-0102_Stata-data-file.dta", clear 

*-Panel A: IV
ivreg2 any_prio_mss (gdp_g gdp_g_l = gpcp_g gpcp_g_l) ///
       Iccode* Iccyear* if orig_mss, cluster(ccode) first

ivreg2 any_prio_mss (gdp_g gdp_g_l = lgpcp lgpcp_l lgpcp_l2) ///
       Iccode* Iccyear* if orig_mss, cluster(ccode) first

*-Panel B: first stage

reg gdp_g gpcp_g gpcp_g_l Iccode* Iccyear* if orig_mss, cluster(ccode) 

reg gdp_g lgpcp lgpcp_l lgpcp_l2 Iccode* Iccyear* if orig_mss, cluster(ccode)

reg penn_gdp1_new gpcp_g_phil gpcp_g_l_phil Iccode* Iccyear* ///
    if year>=2000 & year<=2008, cluster(ccode)

*-Panel C: reduced form
reg any_prio_mss gpcp_g gpcp_g_l Iccode* Iccyear* if orig_mss, cluster(ccode) 

reg any_prio_mss lgpcp lgpcp_l lgpcp_l2 Iccode* Iccyear* ///
    if orig_mss, cluster(ccode)
test lgpcp_l + lgpcp_l2 = 0
```

--- - --

### 典型 IV：科举考试与人力资本
> Chen, T., Kung, J. K. S., \& Ma, C., **2020**. Long live Keju! The persistent effects of China's civil examination system. **The Economic Journal**, 130(631), 2030-2064. [Link](https://doi.org/10.1093/ej/ueaa043), [PDF](http://sci-hub.ren/10.1093/ej/ueaa043)， [Replication](https://academic.oup.com/ej/article/130/631/2030/5819954?login=true#supplementary-data).  
- 推文: [工具变量法（IV）的Stata操作](https://mp.weixin.qq.com/s/BJwV2u-UnkpqR8xqrLz33w), &emsp; [推文2](https://mp.weixin.qq.com/s?__biz=MzU4ODU3NjM2MA==&mid=2247485276&idx=1&sn=d721466b56849b98d214fb4aad906137&chksm=fddbe45bcaac6d4d6605c2364c067753fc3341392f886be5910d1cc500a50f0701a5098678b4&token=565610391&lang=zh_CN&scene=21#wechat_redirect)
- 详见：对此案例进行了比较细致的介绍 [github - MicroEconometrics](https://byelenin.github.io/MicroEconometrics/Slides/2021/GradMetrics_2021_Lec5.pdf)




--- - --

### 工具变量的合理性：相关性-图示

![w:650](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/IV_Chen_2020_AEJ_Keju_Fig01.png)

> **Fig 1.** Correlation between Historical Success in China's Civil Exam (Keju) and Contemporary Human Capital Outcomes. Source: Chen,et al. ([2020](https://doi.org/10.1093/ej/ueaa043), [PDF](https://oup.silverchair-cdn.com/oup/backfile/Content_public/Journal/ej/130/631/10.1093_ej_ueaa043/2/ueaa043_online_appendix.pdf?Expires=1660037647&Signature=K1~YvP1irTP7Zw6DMGEcDxYS8wTMXLQIihleSFPINWSvypXUh-lNv43Dk8VyPt8~RjTmadmCP8ia~hHvPT3fETU~ZlnNyYxDazQlBbL1imEamkCfm0~J7sii6qOFb2druYnwGN-jX3BCkYFoyDVgrDeoX-KvauBtREkCQUfPwtHSYnt11dlQPSs1rKe~J~SC54gCr9rbzgz2I-dAZmo779HC2guIlTpduoLt38zn6mFbGkIP2vBOvOweDp-WEsWc6N4ra1clQ-sy6Tr~lAz090dN2LQQUKdtYMMYxr-G3GKyFbX-hclinNdLB9p5CiewIl0DbB6p63jEcg6XFLKqgg__&Key-Pair-Id=APKAIE5G5CRDK6RD3PGA), [Rep](https://academic.oup.com/ej/article/130/631/2030/5819954?login=true#supplementary-data)). 


--- - --

## 发表复现类论文：AER 论文中的 IV 不外生

- Buliskeria, N., J. Baxa, **2022**, Do rural banks matter that much? Burgess and pande (2005) reconsidered, **Journal of Applied Econometrics**, 37 (6): 1266-1274. [-Link-](https://doi.org/https://doi.org/10.1002/jae.2922), [-PDF-](https://sci-hub.ren/https://doi.org/10.1002/jae.2922), [-PDF2-](https://onlinelibrary.wiley.com/doi/epdf/10.1002/jae.2922?saml_referrer), [Replication](http://qed.econ.queensu.ca/jae/datasets/buliskeria001/), [-Appendix-](https://onlinelibrary.wiley.com/action/downloadSupplement?doi=10.1002%2Fjae.2922&file=jae2922-sup-0001-OnlineAppendix_merged+%281%29.pdf)
  - We replicate Burgess and Pande's (2005) analysis of the effect of India's state-led bank expansion on poverty. The authors instrument rural bank branch expansion by its trend reversal explained by the 1977 licensing rule and find that the bank expansion decreased poverty. However, the authors do not consider other licensing rule amendments and concurrent policies. Thus, their instrument is not necessarily exogenous to poverty. We show that the significant effect of bank expansion on poverty disappears after summarizing the trend reversal with more breaks linked to the bank licensing policy.
  - 复现对象：Burgess, R., & Pande, R. (2005). Do rural banks matter? Evidence from the Indian social banking experiment. **American Economic Review**, 95(3): 780-795. [-Link-](https://doi.org/10.1257/0002828054201242Claessens)


--- - --
<!-- backgroundColor: #C1CDCD -->

## Judge fixed effects (Judge IV)


--- - --
<!-- backgroundColor: #FFFFF9 -->
### 参考资料和重现论文
- Hou, Y., P. Wang, **2020**, Unpolluted decisions: Air quality and judicial outcomes in china, **Economics Letters**, 194: 109369. [-Link-](https://doi.org/10.1016/j.econlet.2020.109369), [-PDF-](https://sci-hub.ren/10.1016/j.econlet.2020.109369), [PDF2](https://ars.els-cdn.com/content/image/1-s2.0-S0165176520302342-mmc1.pdf), [Replication](https://data.mendeley.com/public-files/datasets/xzsppxw7rs/files/d7dc8a79-1e33-443d-8615-d7656b96b72b/file_downloaded)
  
- Frandsen, B. R., L. J. Lefgren, E. C. Leslie, **2019**, Judging judge fixed effects. [-PDF-](https://www.nber.org/system/files/working_papers/w25528/w25528.pdf)
- Norris, S., M. Pecenco, J. Weaver, **2021**, The effects of parental and sibling incarceration: Evidence from OHIO, **American Economic Review**, 111 (9): 2926-2963. [-Link-](https://doi.org/10.1257/aer.20190415), [-PDF-](https://sci-hub.ren/10.1257/aer.20190415), [Appendix](https://www.aeaweb.org/doi/10.1257/aer.20190415.appx), [Replication](https://doi.org/10.3886/E132762V2)



### Stata 实操
- `testjfe` 有范例数据和 dofile
--- - --

<!-- backgroundColor: #FFFFF9 -->
### 7.8.2 Judge fixed effects
A second IV design that has become extremely popular in recent years is the “judge fixed effects” design. You may also hear it called the “leniency design,” but because the applications so often involve judges, it seems the former name has stuck. A search on Google Scholar for the term yields over 70 hits, with over 50 since 2018 alone.

- [judge_fe.do](https://github.com/scunning1975/mixtape/blob/master/Do/judge_fe.do)

--- - --

- Frandsen, Lefgren, Leslie, **2019**, Judging judge fixed effects. [-PDF-](https://www.nber.org/system/files/working_papers/w25528/w25528.pdf)

```stata
use https://github.com/scunning1975/mixtape/raw/master/judge_fe.dta, clear
*-Judge 
global judge_pre judge_pre_1 judge_pre_2 judge_pre_3 judge_pre_4 /*
     */judge_pre_5 judge_pre_6 judge_pre_7 judge_pre_8
*-Controls     
global demo black age male white 
global off      fel mis sum F1 F2 F3 F M1 M2 M3 M 
global  prior priorCases priorWI5 prior_felChar /*
     */ prior_guilt onePrior threePriors
global control2  day day2 day3  bailDate t1 t2 t3 t4 t5 t6

* Naive OLS
* minimum controls
reg guilt jail3 $control2, robust
* maximum controls
reg guilt jail3 possess robbery DUI1st drugSell aggAss ///
    $demo $prior $off  $control2 , robust
```

--- - --

```stata
* First stage
reg jail3 $judge_pre $control2, robust
reg jail3 possess robbery DUI1st drugSell aggAss /// 
     $demo $prior $off  $control2 $judge_pre, robust

** Instrumental variables estimation
* 2sls main results
* minimum controls
ivregress 2sls guilt (jail3= $judge_pre) $control2, robust first
* maximum controls
ivregress 2sls guilt (jail3= $judge_pre) ///
    possess robbery DUI1st drugSell aggAss ///
    $demo $prior $off $control2 , robust first
```

--- - --

```stata
* JIVE main results
* jive can be installed using: 
  . net from https://www.stata-journal.com/software/sj6-3/
  . net install st0108

* minimum controls
jive guilt (jail3= $judge_pre) $control2, robust

* maximum controls
jive guilt (jail3= $judge_pre) possess robbery DUI1st ///
     drugSell aggAss $demo $prior $off $control2 , robust
```



--- - --


## IV causal interpretation
> Mogstad, M., A. Torgovitsky, C. R. Walters, **2021**, The causal interpretation of two-stage least squares with multiple instrumental variables, **American Economic Review**, 111 (11): 3663-3698. [-Link-](https://doi.org/10.1257/aer.20190221), [-PDF-](https://sci-hub.ren/10.1257/aer.20190221), [PDF2](https://eml.berkeley.edu/~crwalters/papers/multiple_Z.pdf), [Replication](https://doi.org/10.3886/E135041V1)
- Our analysis requires STATA version 13 or later with the packages `ivreg2`, `ranktest`, and `mivcausal` installed. The `mivcausal` package is included with our replication archive. The appendix simulations require **R** (we used version 3.6.2) with the gurobi, devtools, slam, and tidyverse packages, as well as the Gurobi software program and a full Latex distribution.

<!-- 
--- - --

## Salvaging falsified IV
Masten, M. A., A. Poirier, **2021**, Salvaging falsified instrumental variable models, **Econometrica**, 89 (3): 1449-1469. [-Link-](https://doi.org/10.3982/ecta17969), [-PDF2-](https://mattmasten.github.io/assets/pdf/ECTA17969.pdf), [-PDF2-](https://sci-hub.ren/10.3982/ecta17969), [Appendix](https://www.econometricsociety.org/sites/default/files/ecta200301-sup-0001-onlineappendix.pdf), [Replication](https://www.econometricsociety.org/sites/default/files/17969_Data_and_Programs.zip), [-Github-](), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=8881060716512990621&as_sdt=2005&sciodt=0,5&hl=zh-CN) 

- `ivfas.ado`

--- - --

### Foreshadowing the questions you need to be asking 
1. Is our instrument highly correlated with the treatment? With the outcome? Can you test that?
2. Are there random elements within the treatment? Why do you think that?
3. Is the instrument exogenous? Why do you think that?
4. Could the instrument affect outcomes directly? Why do you think that?
5. Could the instrument be associated with anything that causes the outcome even if it doesn't directly? Why do you think that? -->


--- - --
<!-- backgroundColor: #C1CDCD -->

## Some Practical Guides by Angrist and Pischke(2012) 
> Source: [Qu, 2021, IV](https://byelenin.github.io/MicroEconometrics/Slides/2021/GradMetrics_2021_Lec5.pdf)

### IV 使用指南
- Lal, A., M. Lockhart, Y. Xu, Z. Zu, **2023**, How much should we trust instrumental variable estimates in political science? Practical advice based on over 60 replicated studies, **arXiv preprint arXiv:2303.11399**. [PDF-正文](https://arxiv.org/pdf/2303.11399.pdf), [PDF-正文+附录](https://yiqingxu.org/papers/english/2021_iv/LLXZ.pdf), [-cited-](https://scholar.google.com/scholar?cites=7574964935013963040&as_sdt=2005&sciodt=0,5&hl=zh-CN)


--- - --
<!-- backgroundColor: #FFFFF9 -->
### Practical Guides
#### 1. **Check IV relevance**
  - Always report the first stage and think about whether it makes sense(Signs and magnitudes)
  - Always report the F-statistic on the excluded instruments. The bigger, the better. Don't forget the rule of thumb. $\small (F>10)$

#### 2. **Check exclusion restriction**
  - The exclusion restriction cannot be tested directly, but it can be falsified
  - Run and examine the reduced form(regression of dependent variable on instruments) and look at the coefficients, t-statistics and F-statistics for excluded instruments.

--- - --
<!-- backgroundColor: #FFFFF9 -->
### Practical Guides
#### 2. **Check exclusion restriction(cont')**
  - Because the reduced form is proportional to the casual effect of interest and is unbiased (OLS), so we should see the causal relation in the reduced form. If you can't see the causal relation in the reduced form, it's probably not there
  - **Falsification test:** Test the reduced form effect of $Z_i$ on $Y_i$ in situations where it is impossible or extremely unlikely that $Z_i$ could affect $X_i$. Because $Z_i$ can't affect $X_i$, then the exclusion restriction implies that this falsification test should have 0 effect.

--- - --

#### 3. Provide a substantive explanation for observed difference between 2SLS and OLS

- How bid is the difference? What does this tell you?
- Is the coefficient bigger when theory of endogeneity suggests it should be smaller? If so, why?
- Measurement Error or heterogeneous effects?

#### 4. If you have multiple instruments, report over-identification tests.
- Pick your best single instrument and report just-identified estimates using this one only because just-identified IV is relatively unlikely to be subject to a weak bias.
- Worry if it is substantially different from what you get using multiple instruments.
- Check over-identified 2SLS estimates with LIML. LIML is less than precise than 2SIS but also less biased. If the results come out similar, be happy. If not, worry, and try to find stronger instruments.

