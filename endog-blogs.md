# 因果推断推文

**检索提示：**

1. 在 <https://www.lianxh.cn> 搜索框中输入关键词，如 `IV`，`因果推断`，`DID` 等。
2. 在 Stata 命令行中输入 `lianxh 因果推断` 即可。也可以使用 `lianxh DID, md2 nocat` 获取相关推文列表的 Markdown 格式。

&emsp;

## 1. 综合性：因果推断全景、综述与学习路线 {.unnumbered}

这类内容适合用来“搭框架”：先把常见研究设计 (IV / DID / RDD / SCM / Matching / Causal ML) 放在一张地图里，再决定你要走哪条识别路线。
**常见误区**：只记住工具和命令，但对识别假设与估计对象 (ATE / ATT / LATE) 没概念，导致“会跑但不会解释”。

* 丁闪闪, 2026, [Cai博士笔记（上）：因果推断核心方法和文献速览](https://www.lianxh.cn/details/1741.html).
* 丁闪闪, 2026, [Cai博士笔记（下）：因果推断核心方法和文献速览](https://www.lianxh.cn/details/1742.html).
* 胡文涛, 2021, [Stata：因果推断方法综述和Stata操作](https://www.lianxh.cn/details/795.html).
* 连享会, 2020, [Abadie新作：简明IV,DID,RDD教程和综述](https://www.lianxh.cn/details/433.html).
* 连享会, 2022, [NEW-面板数据因果推断：从入门到精通](https://www.lianxh.cn/details/1048.html).
* 连享会, 2020, [经典文献回顾：政策评价-因果推断的计量方法](https://www.lianxh.cn/details/428.html).
* 涂冰倩, 2021, [因果推断：哪本教材适合我？](https://www.lianxh.cn/details/832.html).
* 牛坤在, 2021, [因果推断新书在线读：Causal Inference-The Mixtape](https://www.lianxh.cn/details/704.html).
* 连享会, 2020, [Robins - Causal Inference: What if - 数据和程序](https://www.lianxh.cn/details/50.html).

---

## 2. 识别假设与因果图：DAG、后门法则与“能不能控制”的问题 {.unnumbered}

这类适合在研究设计最早期阅读：你还没跑回归，但已经在决定“该控制什么、不能控制什么”。
**常见误区**：把控制变量当作“越多越好”，忽视了坏控制 (bad controls)、碰撞点偏误 (collider bias)，以及控制会改变估计对象。

* 丁杨洋, 2025, [因果推断之后门法则详解](https://www.lianxh.cn/details/1676.html).
* 雷诺, 2023, [R语言：拆解因果推断中的后门法则](https://www.lianxh.cn/details/1323.html).
* 闫钊鹏, 2024, [因果推断必备：有向无环图DAG的解读和绘制-R 语言之ggdag包](https://www.lianxh.cn/details/1380.html).
* 倪克金, 2021, [趣读：相关性未必意味着因果关系](https://www.lianxh.cn/details/591.html).
* 刘潍嘉, 2024, [辛普森悖论及模拟分析](https://www.lianxh.cn/details/1406.html).
* 雷诺, 2023, [因果推断：傻傻分不清-独立与不相关等价吗？](https://www.lianxh.cn/details/1274.html).

---

## 3. 内生性与工具变量：IV / 2SLS / Shock-IV / Shift-Share / 弱工具变量 {.unnumbered}

IV 适合处理“解释变量有内生性”的场景：遗漏变量、反向因果、测量误差、自选择等。
**常见误区**：只盯第一阶段显著性，忽视排他性约束与 LATE 解释；以及弱工具变量导致偏误和错误推断。

### 3.1 IV 入门与识别直觉 {.unnumbered}

* 杨柳, 连玉君, 2020, [Stata: 工具变量法 (IV) 也不难呀！](https://www.lianxh.cn/details/31.html).
* 万储诚, 2025, [IV：形形色色的IV](https://www.lianxh.cn/details/1583.html).
* 秦范, 2021, [IV在哪里？奇思妙想的工具变量](https://www.lianxh.cn/details/619.html).
* 尚佳雪, 2022, [论文推介：IV-天气是好的工具变量吗？](https://www.lianxh.cn/details/1136.html).
* 雷诺, 2024, [IV的标准动作：工具变量法实用指南](https://www.lianxh.cn/details/1351.html).
* 徐云娇, 甘徐沁, 2020, [IV-工具变量法：第一阶段系数符号确定时的小样本无偏估计](https://www.lianxh.cn/details/481.html).
* 徐云娇, 2020, [工具变量-IV：排他性约束及经典文献解读](https://www.lianxh.cn/details/241.html).
* 张子楠, 2021, [IV估计中的外部有效性(External Validity)](https://www.lianxh.cn/details/634.html).

### 3.2 弱工具变量、多工具变量与检验 {.unnumbered}

* 苗妙, 2021, [twostepweakiv：弱工具变量有多弱？](https://www.lianxh.cn/details/524.html).
* 李萌, 2024, [weakivtest2：允许异方差和序列相关的弱工具变量检验方法](https://www.lianxh.cn/details/1504.html).
* 卫明昊, 2022, [Stata：工具变量的秩检验-bootrantest](https://www.lianxh.cn/details/815.html).
* 贺旭, 冀云阳, 2020, [Sargan+Hansen：过度识别检验及Stata实现](https://www.lianxh.cn/details/258.html).
* 王如玉, 2021, [多个(弱)工具变量如何应对-IV-mivreg？](https://www.lianxh.cn/details/521.html).
* 吴梦萱, 2024, [Stata-IV：csa2sls - 诸多工具变量的应对方法](https://www.lianxh.cn/details/1346.html).
* 康瑜欣, 2022, [Stata：高度共线性情况下的IV估计-pariv](https://www.lianxh.cn/details/1117.html).
* 董洁妙, 2022, [Stata：IV估计新方法-ivreg2m](https://www.lianxh.cn/details/1023.html).
* 蔡金威, 2022, [Stata：IV和OLS估计系数差异分解-ivolsdec](https://www.lianxh.cn/details/1129.html).
* 连玉君, 2026, [IV 和 2SLS 估计中的 R2 为负怎么办？](https://www.lianxh.cn/details/1734.html). 

### 3.3 Shift-Share IV、Shock-IV 与其他“结构型”IV {.unnumbered}

* 徐婷, 2022, [使用Shock-IV的文献汇总](https://www.lianxh.cn/details/1041.html).
* 韩杰, 2022, [工具变量：顶刊中的Shock-IV整理](https://www.lianxh.cn/details/972.html).
* 韩杰, 2022, [工具变量：Shock-IV中预处理平衡的必要性](https://www.lianxh.cn/details/964.html).
* 甘徐沁, 2021, [Stata：最牛IV-Shift Share IV 实操-ssaggregate](https://www.lianxh.cn/details/811.html).
* 秦范, 2022, [Stata论文复现：份额移动法工具变量(Shift-Share IV)](https://www.lianxh.cn/details/912.html).
* 黎佳迅, 2024, [再中心化IV：Shift-Share IV的新变种](https://www.lianxh.cn/details/1398.html).
* 张瑞, 2023, [工具变量：与朱熹书院距离作为IV](https://www.lianxh.cn/details/1124.html).
* 高娜娜, 2020, [IV：可以用内生变量的滞后项做工具变量吗？](https://www.lianxh.cn/details/297.html).

### 3.4 内生性来源与“非标准”处理 {.unnumbered}

* 吴煜铭, 郑浩文, 2021, [内生性！内生性！解决方法大集合](https://www.lianxh.cn/details/579.html).
* 郭佳佳, 2023, [内生性之应对（上）：原理篇--遗漏变量-反向因果-测量误差-自选择](https://www.lianxh.cn/details/1264.html).
* 郭佳佳, 2023, [内生性之应对（下）：方法篇--遗漏变量-反向因果-测量误差-自选择](https://www.lianxh.cn/details/1266.html). 
* 茅陈斌, 2021, [反向因果：来源、特征及解决方法](https://www.lianxh.cn/details/522.html).
* 王舒瑶, 2022, [第三种内生性：衡量偏误(测量误差)如何解决？-eivreg-sem](https://www.lianxh.cn/details/1066.html).
* 陈贤孟, 2020, [第三种内生性：衡量偏误(测量误差)如何检验-dgmtest？](https://www.lianxh.cn/details/289.html).
* 江鑫, 2021, [Stata：无需工具变量的IV估计-kinkyreg-](https://www.lianxh.cn/details/740.html). 
* 雷诺, 2024, [IV-控制函数法-内生性和 Hausman 检验的原理解读](https://www.lianxh.cn/details/1331.html).
* 雷诺, 2024, [控制函数法：Bootstrap标准误的获取](https://www.lianxh.cn/details/1328.html).

---

## 4. 局部识别设计：RDD / RKD / 聚束 (Bunching) {.unnumbered}

RDD 适合处理“阈值规则”或“准则触发”的政策冲击：断点附近可视作局部随机。
**常见误区**：把 RDD 当作“自动因果”，却不做操纵检验、带宽敏感性、函数形式稳健性；以及把控制变量当作救命稻草，反而引入偏误。

### 4.1 RDD 入门与 Stata 实现 {.unnumbered}

* 崔颖, 2020, [RDD：断点回归的非参数估计及Stata实现](https://www.lianxh.cn/details/159.html).
* 张子楠, 2020, [Stata：断点回归RDD简明教程](https://www.lianxh.cn/details/134.html).
* 连享会, 2020, [Stata: 两本断点回归分析 (RDD) 易懂教程](https://www.lianxh.cn/details/66.html).
* 连享会, 2020, [Stata：断点回归分析-(RDD)-文献和命令](https://www.lianxh.cn/details/271.html).
* 连享会, 2020, [Stata：断点回归分析-RDD-文献和命令](https://www.lianxh.cn/details/391.html).

### 4.2 多断点、多分配变量、离散 running variable {.unnumbered}

* 亢延锟, 2020, [RDD 最新进展：多断点 RDD、多分配变量 RDD](https://www.lianxh.cn/details/40.html).
* 连玉君, 2020, [Stata 新命令：多断点 RDD 分析 - rdmc](https://www.lianxh.cn/details/26.html).
* 林育莉, 2021, [RDD断点回归：多个断点多个分配变量如何处理](https://www.lianxh.cn/details/665.html).
* 邱紫烨, 2021, [RDD：离散变量可以作为断点回归的分配变量吗？](https://www.lianxh.cn/details/565.html).
* 高净鹤, 2022, [Stata：RDD与RKD的最优模型选择-pzms](https://www.lianxh.cn/details/1079.html).
* 黄思佳, 2023, [Stata：pzms-RDD和RKD的最优模型选择](https://www.lianxh.cn/details/1194.html).

### 4.3 操纵检验、平滑性检验与“能不能加控制变量” {.unnumbered}

* 李鑫, 2020, [Stata: 断点回归 (RDD) 中的平滑性检验](https://www.lianxh.cn/details/118.html).
* 黄天泽, 2022, [rddensity-RDD中的平滑性检验和操纵检验](https://www.lianxh.cn/details/869.html).
* 谭睿鹏, 2021, [RDD：断点回归可以加入控制变量吗？](https://www.lianxh.cn/details/517.html).
* 连享会, 2020, [Stata：RDD-中可以加入控制变量](https://www.lianxh.cn/details/371.html).
* 李琼琼, 2020, [断点回归RDD：样本少时如何做？](https://www.lianxh.cn/details/427.html).

### 4.4 RDD 的扩展组合 {.unnumbered}

* 张超, 2021, [Stata：RDD-DID-断点回归与倍分法完美结合](https://www.lianxh.cn/details/763.html).
* 张子楠, 2021, [当PSM遇上RDD：rddsga命令详解](https://www.lianxh.cn/details/661.html).
* 郑伊达, 2021, [倒U型+RDD：利用断点回归检验 U 形关系](https://www.lianxh.cn/details/837.html).

### 4.5 聚束效应 (Bunching) {.unnumbered}

* 赵月川, 2025, [Stata新命令-rfbunch：聚束效应的估计与实现](https://www.lianxh.cn/details/1619.html). 

---

## 5. 双重差分 DID 与事件研究 Event Study {.unnumbered}

DID 适合“政策评估 + 时间维度清晰”的场景，尤其是面板数据里常见的制度冲击。
**常见误区**：平行趋势只做一张图就草草收工；交错处理时点下 TWFE 估计量会混合不同对比并产生偏误；标准误与序列相关、空间相关也经常被忽略。

### 5.1 DID 入门与基础写法 {.unnumbered}

* 王昆仑, 2019, [倍分法DID详解 (一)：传统 DID](https://www.lianxh.cn/details/73.html).
* 王昆仑, 2019, [倍分法DID详解 (二)：多时点 DID (渐进DID)](https://www.lianxh.cn/details/72.html).
* 王昆仑, 2019, [倍分法DID详解 (三)：多时点 DID (渐进DID) 的进一步分析](https://www.lianxh.cn/details/9.html).
* 袁洛琪, 2021, [Stata：DID入门教程](https://www.lianxh.cn/details/321.html).
* 张伟广, 2020, [Stata：双重差分的固定效应模型-(DID)](https://www.lianxh.cn/details/381.html).
* 张远远, 2019, [Stata：多期倍分法 (DID) 详解及其图示](https://www.lianxh.cn/details/76.html).

### 5.2 交错 DID、偏误与稳健估计量 {.unnumbered}

* 胡文涛, 2021, [倍分法：交错DID与Stata操作](https://www.lianxh.cn/details/638.html).
* 李胜胜, 2021, [DID陷阱解析-L111](https://www.lianxh.cn/details/606.html).
* 苗妙, 2022, [DID的陷阱和注意事项](https://www.lianxh.cn/details/142.html). 
* 彭甲超, 2022, [Stata：高效实现面板回归控制法-rcm](https://www.lianxh.cn/details/885.html).
* 张子楠, 2022, [DID偏误问题：两时期DID的双重稳健估计量(上)-drdid](https://www.lianxh.cn/details/1024.html).
* 张子楠, 2022, [DID偏误问题：多时期DID的双重稳健估计量(下)-csdid](https://www.lianxh.cn/details/1027.html).
* 彭晴, 2022, [DID新进展：异质性多期DID估计的新方法-csdid](https://www.lianxh.cn/details/1071.html).
* 杜静玄, 2022, [Stata：事件研究法的稳健有效估计量-did_imputation](https://www.lianxh.cn/details/853.html).
* 胡文涛, 2021, [Stata倍分法新趋势：did2s-两阶段双重差分模型](https://www.lianxh.cn/details/679.html).
* 曹昊煜, 2021, [Fuzzy DID：模糊倍分法](https://www.lianxh.cn/details/536.html).
* 张超, 2021, [Stata倍分法：全国一刀切的DID](https://www.lianxh.cn/details/649.html).

### 5.3 平行趋势检验、安慰剂与诊断 {.unnumbered}

* 侯新烁, 2019, [多期DID：平行趋势检验图示](https://www.lianxh.cn/details/112.html).
* 朱学贵, 2020, [多期DID之安慰剂检验、平行趋势检验](https://www.lianxh.cn/details/259.html).
* 李闯, 2023, [多时点DID保姆级教程(上)-平行趋势检验](https://www.lianxh.cn/details/1201.html).
* 李闯, 2023, [多时点DID保姆级教程(下)-安慰剂检验](https://www.lianxh.cn/details/1163.html).
* 郭楚玉, 2022, [DID-倍分法：事前趋势检验的局限性和诊断](https://www.lianxh.cn/details/1103.html). 
* 王烨文, 2024, [Stata：平行趋势敏感性检验-honestdid](https://www.lianxh.cn/details/1467.html).
* 赵文琦, 2025, [平行趋势不显著≠平行趋势成立：pretrends 带你看清 DID 风险](https://www.lianxh.cn/details/1704.html).
* 陈波, 2022, [Stata：一行代码绘制平行趋势图-eventdd](https://www.lianxh.cn/details/927.html).

### 5.4 DID 的扩展：空间相关、溢出、连续处理、队列 DID {.unnumbered}

* 伊凌雪, 2020, [倍分法(DID)的标准误：不能忽略空间相关性](https://www.lianxh.cn/details/231.html).
* 雷诺, 2024, [DID：考虑空间相关性的倍分法-B550](https://www.lianxh.cn/details/1441.html).
* 袁子晴, 2021, [考虑溢出效应的倍分法：spillover-robust DID](https://www.lianxh.cn/details/499.html).
* 巴宁, 2022, [Stata：空间双重差分模型(Spatial DID)-xsmle](https://www.lianxh.cn/details/840.html).
* 秦利宾, 2021, [队列DID：以知识青年“上山下乡”为例-T401](https://www.lianxh.cn/details/639.html).
* 庞小冬, 2023, [组内队列DID：高知女性愿意生更多孩子吗？-within cohort DID](https://www.lianxh.cn/details/1293.html).
* 李萌, 2025, [论文推介：包含连续处理变量的 DID](https://www.lianxh.cn/details/1535.html).
* 李梦玉, 2025, [unitdid：以个体事件反应为因变量的 DID 估计方法](https://www.lianxh.cn/details/1708.html).

### 5.5 DID 经典论文与应用复现 {.unnumbered}

* 安贤娟, 2020, [Big Bad Banks：多期 DID 经典论文介绍](https://www.lianxh.cn/details/165.html).
* 高君, 2025, [Big-Bad-Bank并不稳健：DID元老级文献的脆弱](https://www.lianxh.cn/details/1548.html).
* 邹恬华, 2021, [多期DID文献解读：含铅汽油与死亡率和社会成本-L113](https://www.lianxh.cn/details/622.html).
* 金钊, 2023, [AEJ论文推介：DID-安慰剂检验-机制分析-中国增值税改革对企业投资和生产率的影响](https://www.lianxh.cn/details/1254.html). 
* 刘依云, 2023, [论文复现：土豆对人口与城市化的贡献-连续DID应用](https://www.lianxh.cn/details/1190.html).
* 杨云帆, 2023, [论文复现：多期DID应用之地方选举的兴衰](https://www.lianxh.cn/details/1321.html).
* 杨云帆, 2024, [地方选举的兴衰——交叠DID的应用](https://www.lianxh.cn/details/1517.html).
* 刘梦蝶, 2024, [DID大餐：49 篇 QJE 论文汇总（2018-2022）](https://www.lianxh.cn/details/1363.html).

---

## 6. 合成控制与反事实面板：SCM / SDID / ASCM / RCM / 交互固定效应 {.unnumbered}

SCM 适合“少数处理组 + 长期面板 + 明确冲击时点”的政策评估，强调事前拟合与可解释性权重。
**常见误区**：把 SCM 当作“画图神器”，但不做安慰剂、预测区间、供体池敏感性；以及忽视时间变化的混杂和动态因子结构。

### 6.1 SCM 入门与实现 {.unnumbered}

* 何庆红, 2020, [合成控制法 (Synthetic Control Method) 及 Stata实现](https://www.lianxh.cn/details/119.html).
* 何庆红, 2020, [Synth_Runner命令：合成控制法高效实现](https://www.lianxh.cn/details/205.html).
* 连享会, 2020, [Stata：合成控制法程序分享](https://www.lianxh.cn/details/203.html).
* 李晶晶, 2022, [合成控制法简介](https://www.lianxh.cn/details/910.html).
* 陈少廷, 2022, [Stata：合成控制法介绍-synth2](https://www.lianxh.cn/details/1133.html).

### 6.2 纠偏、预测区间与稳健推断 {.unnumbered}

* 伍站锋, 2022, [Stata：纠偏合成控制法介绍-allsynth](https://www.lianxh.cn/details/854.html).
* 高净鹤, 2022, [Stata：合成控制法的预测区间-scpi](https://www.lianxh.cn/details/1096.html).
* 马英杰, 2025, [论文复现：合成控制法需要做哪些假设检验？](https://www.lianxh.cn/details/1570.html).
* 陈勇吏, 2020, [Stata：合成控制法-synth-命令无法加载-plugin-的解决办法](https://www.lianxh.cn/details/204.html).

### 6.3 机器学习增强 SCM 与 Lasso-SCM {.unnumbered}

* 郭盼亭, 2023, [Stata：基于Lasso的合成控制法-scul](https://www.lianxh.cn/details/1167.html). 
* 浦进博, 2024, [合成控制法最新进展！机器学习+SCM！-qcm](https://www.lianxh.cn/details/1515.html).
* 浦进博, 2024, [Stata：非参数合成控制法命令—npsynth](https://www.lianxh.cn/details/1458.html).

### 6.4 SDID 与“合成 + DID”融合 {.unnumbered}

* 陈卓然, 2022, [Stata+R：合成DID原理及实现-sdid](https://www.lianxh.cn/details/970.html).
* 周瑾, 2024, [sdid_event命令：合成DID事件研究法](https://www.lianxh.cn/details/1482.html).

### 6.5 面板回归控制法 RCM 与交互固定效应 IFE {.unnumbered}

* 彭甲超, 2022, [Stata：高效实现面板回归控制法-rcm](https://www.lianxh.cn/details/885.html).
* 王晓娟, 甘徐沁, 2021, [regife：面板交互固定效应模型-Interactive Fixed Effect](https://www.lianxh.cn/details/42.html).
* 任缘, 2024, [xtnumfac：估计面板数据中共同因子的个数](https://www.lianxh.cn/details/1438.html).
* 周翔宇, 2022, [主成分分析-交互固定效应基础：协方差矩阵的几何意义](https://www.lianxh.cn/details/845.html).

---

## 7. 匹配与加权：PSM / IPW / RA / IPWRA / 熵平衡与高维倾向得分 {.unnumbered}

匹配与加权适合“处理组与对照组可比性较差，但可观测协变量较丰富”的场景，本质是用数据预处理降低模型依赖。
**常见误区**：只报一个匹配后回归结果，却不报告平衡性检验；以及把 PSM 当作“消除内生性”的万能解法。

### 7.1 PSM 入门、误区与实践建议 {.unnumbered}

* 占云, 2020, [Stata+PSM：倾向得分匹配分析简介](https://www.lianxh.cn/details/173.html).
* 丁海, 2020, [Stata：psestimate-倾向得分匹配(PSM)中协变量的筛选](https://www.lianxh.cn/details/370.html).
* 连享会, 2020, [Stata：psestimate-倾向得分匹配(PSM)中匹配变量的筛选](https://www.lianxh.cn/details/230.html).
* 秦利宾, 2021, [Stata：PSM-倾向得分匹配分析的误区](https://www.lianxh.cn/details/178.html).
* 连享会, 2020, [伍德里奇先生的问题：PSM-分析中的配对——小蝌蚪找妈妈](https://www.lianxh.cn/details/285.html).
* 连玉君, 2024, [执行PSM匹配后观察值会减少吗？](https://www.lianxh.cn/details/1418.html). 
* 肖志文, 2025, [匹配使用指南：通过数据预处理降低模型依赖](https://www.lianxh.cn/details/1546.html). 
* 徐云娇, 2020, [Stata-从匹配到回归：精确匹配、模糊匹配和PSM](https://www.lianxh.cn/details/216.html).

### 7.2 加权与双重稳健：IPW / RA / IPWRA / 熵平衡 {.unnumbered}

* 段义学, 2023, [因果推断中的 RA, IPW, IPWRA](https://www.lianxh.cn/details/1268.html). 
* 李增杰, 2024, [R语言：IPW深度解读-离散变量和连续变量情形](https://www.lianxh.cn/details/1532.html).
* 王文韬, 2021, [ebalance：基于熵平衡法的协变量平衡性检验](https://www.lianxh.cn/details/618.html).

### 7.3 高维倾向得分与扩展 {.unnumbered}

* 陈佳慧, 2021, [Stata：高维倾向得分法-hdps](https://www.lianxh.cn/details/820.html). 
* 展一帆, 2021, [面板PSM+DID如何做匹配？](https://www.lianxh.cn/details/592.html).

---

## 8. 固定效应、控制变量与模型设定：FE / 控制变量选择 / 交互项 / 面板设定细节 {.unnumbered}

这一类非常“日常”，适合写作阶段反复翻看：你在决定回归式里到底该放哪些控制、固定效应到什么颗粒度。
**常见误区**：控制变量的内生性、过度控制、以及交互项与固定效应叠加引发的解释混乱。

### 8.1 固定效应模型：能做因果吗？颗粒度怎么选？ {.unnumbered}

* 何屹, 2021, [用FE-固定效应模型能做因果推断吗？](https://www.lianxh.cn/details/544.html).
* 胡雨霄, 2020, [reghdfe：多维面板固定效应估计](https://www.lianxh.cn/details/156.html). 
* 刘佳宁, 2022, [Stata：固定效应的颗粒度选择：实践与陷阱](https://www.lianxh.cn/details/1093.html).
* 郭思媛, 2024, [FE vs POLS：聊聊固定效应-优点和缺点](https://www.lianxh.cn/details/1381.html). 
* 韩杰, 2022, [固定效应还是随机效应？](https://www.lianxh.cn/details/947.html).
* 董洁妙, 2023, [JF论文推介：FE和RE如何选择？](https://www.lianxh.cn/details/1303.html). 
* 展一帆, 2020, [Stata：非对称固定效应模型](https://www.lianxh.cn/details/238.html).
* 关欣, 2022, [Stata：如何处理固定效应模型中的单期数据-xtfesing](https://www.lianxh.cn/details/913.html).
* 肖志文, 2024, [孰优孰劣：固定效应模型与混合效应模型对比-FE-ME](https://www.lianxh.cn/details/1334.html). 

### 8.2 控制变量：Good Controls / Bad Controls 与组合筛选 {.unnumbered}

* 付一帆, 2022, [Stata：控制变量与核心解释变量地位对等吗？](https://www.lianxh.cn/details/1019.html).
* 刘琦, 2020, [不用太关心控制变量，真的！](https://www.lianxh.cn/details/222.html).
* 秦利宾, 2021, [控制变量！控制变量！](https://www.lianxh.cn/details/713.html).
* 张雪娇, 2022, [控制变量如何选？大牛们的10条建议](https://www.lianxh.cn/details/1011.html).
* 张雪娇, 2022, [控制变量越多越好吗？](https://www.lianxh.cn/details/1025.html).
* 左祥太, 2022, [Stata：控制变量组合的筛选-tuples](https://www.lianxh.cn/details/987.html).
* 汪京, 2024, [控制变量：对所有可能的控制变量组合进行测试](https://www.lianxh.cn/details/1516.html). 
* 连玉君, 张静瑶, 2020, [加入控制变量后结果悲催了！](https://www.lianxh.cn/details/135.html).

### 8.3 交互项、调节效应与“交乘项要不要加” {.unnumbered}

* 伊凌雪, 2021, [调节效应是否需要考虑对控制变量交乘？](https://www.lianxh.cn/details/789.html).
* 修博文, 2024, [交乘项困惑：交互模型中的控制变量如何选择？](https://www.lianxh.cn/details/1497.html).
* 崔娜, 2020, [Stata：内生变量和它的交乘项](https://www.lianxh.cn/details/186.html).
* 崔娜, 2020, [Stata：内生变量的交乘项如何处理？](https://www.lianxh.cn/details/377.html).
* 崔娜, 2021, [内生变量的交乘项如何处理？](https://www.lianxh.cn/details/500.html).
* 连小白, 2025, [交互项要不要加？理论解释与实操建议](https://www.lianxh.cn/details/1707.html).

---

## 9. 机制识别、异质性与因果分解：CATE、异质性处理效应、中介效应 {.unnumbered}

当你的论文不满足于“有显著效应”，而是要回答“对谁有效、通过什么机制、在什么条件下更强”，这一类就非常关键。
**常见误区**：异质性做成“分组回归堆表格”，但缺乏可解释的分层逻辑；中介分析忽视识别假设，容易把相关当机制。

### 9.1 异质性处理效应与分解视角 {.unnumbered}

* 严安冬, 2024, [Stata：异质性DID代码详解](https://www.lianxh.cn/details/1399.html).
* 梁淑珍, 2022, [Stata：异质性稳健DID估计量方法汇总](https://www.lianxh.cn/details/823.html). 
* 杜静玄, 2022, [DID最新进展：异质性处理条件下的双向固定效应DID估计量 (TWFEDD)](https://www.lianxh.cn/details/846.html).
* 李雪, 2023, [Stata：当处理效应为异质性的时候OLS估计值的解释-hettreatreg](https://www.lianxh.cn/details/1280.html).
* 林旭姿, 2022, [Stata因果推断：hettreatreg-用OLS估计异质性处理效应](https://www.lianxh.cn/details/902.html).
* 赵俊, 2025, [cate：政策学习与条件平均处理效应-Stata19新功能](https://www.lianxh.cn/details/1657.html).
* 张逸林, 2025, [异质性分析的新视角：政策效应分解](https://www.lianxh.cn/details/1684.html).

### 9.2 中介效应与因果中介分析 {.unnumbered}

* 曹琳君, 2021, [Stata：因果中介分析大比拼-T323](https://www.lianxh.cn/details/584.html).
* 曹昊煜, 2022, [Stata：一般化的因果中介分析](https://www.lianxh.cn/details/1046.html).
* 关欣, 2022, [Stata：基于IV的因果中介分析-ivmediate](https://www.lianxh.cn/details/914.html).
* 李昭宏, 2024, [rwrmed：基于回归残差法的因果中介分析](https://www.lianxh.cn/details/1422.html).
* 余坚, 2023, [中介效应：有序因果中介分析的半参数估计A-理论](https://www.lianxh.cn/details/1177.html).
* 余坚, 2023, [中介效应：有序因果中介分析的半参数估计B-实操](https://www.lianxh.cn/details/1179.html).

---

## 10. 稳健性、敏感性分析与“可信度革命”之后的实证写作 {.unnumbered}

这类内容解决的是“审稿人最关心的第二层问题”：即使你做了识别设计，结论对遗漏变量/模型设定/样本选择是否敏感？
**常见误区**：稳健性检验做成“堆结果”，却没有说明每个检验对应的威胁是什么、能排除什么。

### 10.1 敏感性分析与稳健性检验 {.unnumbered}

* 吴思锐, 2019, [Stata新命令：konfound - 因果推断的稳健性检验](https://www.lianxh.cn/details/6.html).
* 李胜胜, 2021, [因果推断：未测量混杂因素的敏感性分析-T249](https://www.lianxh.cn/details/632.html).
* 李适源, 2022, [Stata：敏感性分析-rcr](https://www.lianxh.cn/details/877.html).
* 陈卓然, 2022, [因果推断：混杂因素敏感性分析理论(上)](https://www.lianxh.cn/details/1031.html).
* 陈卓然, 2022, [因果推断：混杂因素敏感性分析实操(下)-tesensitivity](https://www.lianxh.cn/details/1032.html).
* 陈卓然, 2022, [敏感性分析A-理论基础：控制变量内生时的系数敏感性分析-regsensitivity](https://www.lianxh.cn/details/1007.html).
* 陈卓然, 2022, [敏感性分析B-Stata实操：控制变量内生时的系数敏感性分析-regsensitivity](https://www.lianxh.cn/details/1010.html).
* 邹恬华, 2021, [遗漏变量？敏感性分析！新命令sensemakr](https://www.lianxh.cn/details/621.html).
* 李沣航, 2024, [敏感性分析在社会学研究中的应用](https://www.lianxh.cn/details/1472.html).

### 10.2 经典图形与结果呈现 (写作友好) {.unnumbered}

* 陈卓然, 2022, [论文中因果推断的经典图形](https://www.lianxh.cn/details/994.html).
* 陈卓然, 2022, [一组动图读懂因果推断](https://www.lianxh.cn/details/1098.html).
* 曹琳君, 2021, [如何解释和展示你的实证结果（上）](https://www.lianxh.cn/details/493.html).
* 曹琳君, 2021, [如何解释和展示你的实证结果（下）](https://www.lianxh.cn/details/494.html).

### 10.3 “可信度革命”与研究设计反思 {.unnumbered}

* 雷诺, 2025, [经济学实证研究：可信度革命后去向何方？](https://www.lianxh.cn/details/1714.html).
* 贾寰宇, 2025, [实证研究：你知道你在估计什么吗？](https://www.lianxh.cn/details/1597.html).
* 罗丹, 贾寰宇, 2025, [不要“拍脑袋”建模：如何有效连接统计证据与理论解释](https://www.lianxh.cn/details/1590.html).
* 郑宇, 徐文曦, 2024, [顶刊编辑的投稿建议：定量研究要注意什么？](https://www.lianxh.cn/details/1530.html).

---

## 11. (附) 因果机器学习、DML、AI 助手与自动化工作流 {.unnumbered}

这一类更偏“新工具箱”：当你希望在高维控制、非线性、异质性处理效应上走得更远，DML / EconML 会更有用。
**常见误区**：把预测能力当作识别能力；或者把复杂模型当作黑箱，缺乏可解释的估计对象与稳健性验证。 

* 李金桐, 2023, [因果推断：双重机器学习-ddml](https://www.lianxh.cn/details/1221.html).
* 李俊奇, 2025, [Python-EconML包：快速上手动态双重机器学习](https://www.lianxh.cn/details/1577.html).
* 李长生, 2025, [EconML：因果机器学习的实现流程](https://www.lianxh.cn/details/1635.html).
* 王烨文, 2025, [新书免费读：CausalMLBook-因果机器学习](https://www.lianxh.cn/details/1602.html).
* 赵俊, 2025, [静态面板数据下的双重机器学习模型（上）—— 理论基础](https://www.lianxh.cn/details/1664.html).
* 赵俊, 2025, [静态面板数据下的双重机器学习模型（下）——R代码实操](https://www.lianxh.cn/details/1665.html).
* 李梦玉, 2025, [DML-CER：使用双重机器学习克服面板数据中的不可观测异质性](https://www.lianxh.cn/details/1687.html).
* 连小白, 2025, [自动化因果推断助手：Causal-Copilot 简介](https://www.lianxh.cn/details/1643.html).
* 吴茜, 2025, [我们需要因果 AI：Judea Pearl 聊 AI 的未来](https://www.lianxh.cn/details/1655.html).
* 张弛, 2025, [找不到IV？如何借助大语言模型寻找工具变量](https://www.lianxh.cn/details/1575.html).
