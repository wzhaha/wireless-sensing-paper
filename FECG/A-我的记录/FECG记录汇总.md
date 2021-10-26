# FECG记录汇总

[TOC]

## 常用方法

### 匹配滤波法

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh2rm0smxj615s0d0ab402.jpg" alt="image-20211016131314446" style="zoom:50%;" />

+ 首先从混合信号中获取母体心电信号的QRS波群
+ 使用相关算法获得母体信号的模版，并对模版不断调整
+ 从腹部信号中减去MECG模版，从而抵消母体信号
+ 最后使用波形识别等算法得到FECG

优缺点：

当混合信号中胎儿波形较大时比较有效。

但是当胎儿信号很微弱时，使用匹配滤波法去除母体信号后，得到的胎儿心电信号会被淹没在噪声中，容易造成漏检和误检。

### 自适应噪声抵消技术

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh2wzy2kkj61g40iuabt02.jpg" alt="image-20211016131824133" style="zoom: 40%;" />

自适应噪声抵消技术的基本原理是将从母体胸部采集到的母体心电信号作为参考输入，把它送入到自适应滤波器中，按照最优准则不断调节滤波器的参数，以适应噪声和信号随时间变化的统计特性，最后从孕妇腹部采集的混合心电信号中抵消母体心电信号的成分，实现最优滤波，得到胎儿心电信号。

优缺点：

结构简单、易于实现、计算量小、收敛快。

不适用于非平稳性较强的胎儿心电信号，在MECG和FECG重叠的部分会无法提取。

并且需要使用胸部电极，而且胸部电极与腹部电极还有不同步问题会带来误差。

### 小波分析法

该方法是利用小波系数模极大值，检测腹部混合信号上的奇异点，然后将腹部混合信号和MEC相同的奇异点进行抵销，最后利用小波重构得到胎儿心电信号。

优缺点：

准确性高。

但是小波变换中的小波基以及小波系数的选择对最后的结果影响很大。

### 神经网络

Reaz等在2004年提出了一种利用==自适应线性神经网络滤波==的方法提取FECG，利用神经网络进行训练以消除腹部混合信号中的母体成分，==该方法比传统的滤波方法效果好，提取出的FECG的质量比较高,并且避免了将所需要的信号成分去除==。

KhaledAssaleh等利用多项式神经网络从腹部混合信号中非线性映射出MECG，然后再将其从腹部混合信号中减去，从而可以得到FECG。随后又在相同的条件下将==自适应模糊神经推理系统==应用在FECG的提取中，并且获得了较为清晰的FECG。

优缺点：

基于人工神经网络的FECG提取方法==需要导联数少，且提取性能较好，在胎儿心电与母体心电重合时仍然能够很好的提取出胎儿心电信号。==

但==不足的是它需要学习训练==，这个过程一般要花费较多的时间，而且训练的样本要具有一定的代表性，实施起来有一定困难，不适用于临床。

### 独立分量分析

ICA系列

监督式ICA

ICA+小波变换

改进ICA：使用新的优化方法来替代牛顿迭代法

## 相关论文解读

### 伴随宫缩波的胎儿心电信号提取 李朝兰 北京工业大学 硕士论文

本课题以腹部检测法为主要检测方式，直接从腹部混合信号中同时获得胎儿心电信号和宫缩信号,而不需要用不同的采集手段分别获取两种信号。本课题==研究对象是通过腹部采集法直接获得包含宫缩信号的腹部混合信号,研究目标是研究一种有效的提取胎儿心电信号和宫缩信号的方法,这样可以提取出胎儿心电信号获得胎心率，然后利用宫缩信号判断出宫缩期间==，对比检测子宫收缩时胎儿心率的变化情况。.

#### 使用数据库

PhysioNet 中 Abdominal and Direct Fetal 即 ADFECGDB

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh49z0kiqj61dw0m844q02.jpg" alt="image-20211016140531844" style="zoom: 33%;" />

上图为数据库中的一组数据的一个通道数据，红色圈出的是子宫收缩时产生的爆发波的表现。

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh4d2jwemj61b00litau02.jpg" alt="image-20211016140827376" style="zoom:33%;" />

上图是宫缩时的腹部混合信号，可以看到混合信号随着宫缩信号表现出上下波动的形式。	

#### 本文内容

+ ICA在胎儿心电信号提取中的应用

+ 结合自相关分析和ICA的方法提取胎儿心电信号

+ 胎儿心电监测系统

#### 应用独立分量分析提取胎儿心电信号

##### 孕妇腹部混合信号的构成

+ 胎儿心电信号：正常频率范围为120-160次/分钟，形状与成人心电信号一致

+ 宫缩信号：子宫收缩信号表现为峰电信号比较频繁的爆发波。主要频率成分集中在0-5Hz，正常频率是每十分钟会小于或者是等于五次

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh4n12r4ij615e0b8acl02.jpg" alt="image-20211016141804627" style="zoom:33%;" />

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh4mh4wiyj60xw0d274x02.jpg" alt="image-20211016141732223" style="zoom: 33%;" />

+ 母体心电信号

+ 工频干扰：工频干扰主要是由室内动力设备或室内电压引起的，由50Hz及其谐波分量组成。

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh4ooo260j610s0kignr02.jpg" alt="image-20211016141939552" style="zoom:33%;" />

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh4ovf5jwj610c0l2jsp02.jpg" alt="image-20211016141951335" style="zoom:33%;" />

+ 基线漂移：基线漂移主要是由母亲的呼吸或者电极移动所引起的，其频率比较低--般在0.15~0.3Hz之间，在信号上可以看到心电信号波形随之上下波动，使心电信号的基线随着时间上下起伏，但这种起伏在信号上表现为- -个比较缓慢的变化。

##### 独立变量分析

![image-20211016142803373](https://tva1.sinaimg.cn/large/008i3skNgy1gvh4xeyq0qj617608e74l02.jpg)

ICA需要满足的假设：

+ 源信号s各分量之间相互统计独立（统计独立：一些随机现象经过大量观察,在它们出现的结果之间不呈现显著联系,因此认为这些随机现象的规律性相互独立,称为统计独立性。）
+ 观测信号数量大于等于源信号数量
+ 最多一个信号源是高斯信号，其余信号被要求是具有高斯分布
+ 混合矩阵A需要满秩，即非奇异矩阵

ICA的不确定性：

+ 幅度不确定
+ 符号（正负）不确定
+ 顺序不确定

结果：

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh5kafg66j61cw0l0jx502.jpg" alt="image-20211016145001338" style="zoom:33%;" />

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh5khyv63j619o0jcaey02.jpg" alt="image-20211016145014449" style="zoom:33%;" />

上述两个图是FastICA对ADFECGDB中r01数据的分析结果，分离出的信号中第一个通道是母亲信号，第二个是胎儿信号，第三个对应宫缩信号。

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh5wlri4gj61hm0m077y02.jpg" alt="image-20211016150150958" style="zoom:33%;" />

FastICA在该数据集上的效果有母体和胎儿混合情况。

但是r01是最好的一组数据，没有什么参考意义。-_- 并且没有后续如何从分量中判断哪个是胎儿哪个是母亲的算法。

#### 结合自相关与独立分量分析提取胎儿心电信号

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh61cqmiuj61hi0duq4302.jpg" alt="image-20211016150626408" style="zoom:40%;" />

根据实际情况，宫缩信号的形状在检测过程中并不受到太多的关注，我们不需要知道宫缩信号的具体波形，只是==着重于知道哪段时间产生了宫缩==,得到一个大致的宫缩曲线即可。鉴于此，在信号提取过程中，根据宫缩信号的频谱特性，它的频率很低，同心电信号相比而言，其频谱基本.上不会与之重叠，因此可以==直接利用一个低通滤波器将宫缩信号的大致曲线直接提取出来，不需要将宫缩信号作为一个独源再加一个高通滤波器，将宫缩信号去除，可以得到混合的心电信号，然后再利用分离算法从混合心电信号中将胎儿心电信号提取出来==。

ICA在应用中的问题：

当观测信号的值不变时，只改变其先后顺序，得到的解混矩阵是相对稳定的，也就是说在这一模型中没有考虑信号时间上的关系，对于观测信号的先后关系没有做过多的考虑。

==从单个周期混合信号中提取出来的胎儿心电信号效果比较好，母体成分干扰很小，当增加一个周期时，母体成分的干扰增加了，但相比较混合信号而言，母体成分还是得到了一定的抑制，继续增加周期，发现随着周期数目的增多，母体成分的干扰越来越明显==。可以得出这样一个结论，当处理的混合信号的样本容量值增加时，分离效果不一定会更好。因此在对这样的腹部混合信号进行分离时，可以考虑采用片段截取信号的方式来进行。

可以通过自相关函数得到原始时域信号的一个准周期，然后利用这个周期对混合信号进行截取，从而使得混合信号中的时间相关性减弱。

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh77bjdmpj60pk0ygtae02.jpg" alt="image-20211016154643888" style="zoom:33%;" />

最终步骤：

(1) 选取混合信号的某一组，截取部分信号进行自相关分析，确定长度L,提取长度为L的混合信号;

(2)对混合信号数据进行中心化使其变为零均值的量;.

(3)对数据进行白化，得到白化后数据z;

(4)选择需要估计的独立分量的个数m, m≤n;

(5)随机选择初始化向量w

(6)更新迭代w

(7)对w进行正交化:

(8)对wp+1进行标准化

(9)如果Wp+1没有收敛，返回步骤6，继续计算;

(10)得到解混矩阵W，将其带入到ICA模型中分离混合信号。



本文提出的改进算法简单概括就是：使用一个较短的自相关较弱的时间片段信号去计算fastica的解混矩阵，然后将该矩阵应用在长序列上。宫缩曲线的获取是直接低通滤波。



实际效果：

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh84z0t7rj60ss0gqtb502.jpg" alt="image-20211016161907088" style="zoom:50%;" />

上图是直接对r01数据进行分离的结果，共5min，步长6s，可以看到在宫缩部分完全分离失败，整体成功率只有32%。

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvh87i7mv2j60s40g0q5c02.jpg" alt="image-20211016162133250" style="zoom: 50%;" />

上图是使用本文提供的改进算法（使用自相关系数求出的准周期对数据划分之后先求出解混矩阵，然后与所有数据相乘）对r01数据进行分离的结果，共5min，步长6s，可以看到在宫缩部分完全分离失败，整体成功率只有52%。



### Cancellation of the Maternal and Extraction of the Fetal ECG in Noninvasive Recordings

#### 简介

本文提出一种消除母体ECG的方法，包括==母体QRS检测==，==心率相关的P-QRS-T间期选择==，通过互相关、重叠间期、计算P-QRS-T间期的平均信号来==定位该间期内的基准点以实现最佳匹配==，以及==从整个fECG记录中连续减去平均信号==。合成所有4个通道的组合导联，并对其进行胎儿QRS检测。

#### 方法

+ Maternal QRS detection：采用Christov自适应QRS检测方法检测母体QRS。从所有四根导线中选择QRSs幅值最高的两根导线作为算法的输入。（仅观察到少数错误检测，将胎儿标记为母体QRS波。）
+ Fiducial points' location（基准点定位）：获得平均母亲心率，并根据其计算P-QRS-T间期的长度。通过互相关获得连续P-QRS-T间期最佳匹配的基准点。
+ Mean P-QRS-T interval：考虑基准点，计算P-QRS-T间期的平均母体信号
+ Subtraction of the mean signal：从整个ECG记录中减去平均母体信号（使用截止频率为4 Hz（高于诊断心电图机建议的0.64 Hz的截止频率）的漂移抑制。使用了高通递归滤波器。）
+ Fetal QRS detection：使用Christov自适应方法的两种改进去检测胎儿QRS
  + -假设心率调整为每分钟120-180次；
  + -进行QRS检测的“==复合导联==”是由挑战数据库提供的所有四条tECG导联合成的。



### Non-invasive FECG extraction from a set of abdominal sensors( Joachim Behar ) 

#### 简介

首先通过级联低通和高通滤波器对心电图进行预处理，以消除高频和基线漂移。如果需要，可使用Notch滤波器(带阻滤波器的一种，其阻带很窄，因此也称点阻滤波器)消除50Hz的电源干扰。然后对信号进行归一化处理，然后应用各种源分离技术取消母体心电图。这些技术包括：模板法、主/独立分量分析、扩展卡尔曼滤波和这些方法的子集组合（融合法）。使用Pan Tompkins QRS检测器对所有残差进行FQRS检测，并选择具有最平滑FHR时间序列的通道。

#### 方法

![image-20211020160621306](https://tva1.sinaimg.cn/large/008i3skNgy1gvlu8z0yg0j60vc08m75502.jpg)

FECG extraction block diagram

+ 对四个ABD通道进行预处理，通过去除基线漂移和更高频率分量，如果需要进行陷波滤波去除工频干扰
+ 在每个预滤波通道上进行MQRS检测
+ 采用源分离算法分离ABD信号，以提取FECG
+ 对含有FECG的滤波后残余信号进行FQRS检测
+ 选择在剩余通道上检测到的FQRS时间序列之一
+ RR时间序列处理

使用Pan Tompkins QRS检测器分别检测MQR和FQR，其不应期周期分别为250ms和150ms（即QRS peak所占的期间）。

从单个剩余通道提取的FQRS时间序列的选择基于平滑指数（SMI），SMI定义为瞬时心率变异绝对值优于30bpm的发生次数。选择SMI最低的通道。

Source separation：

使用的方法：

template subtraction (TS)

Kalman filtering (KF)

Echo State Neural Network

principal component analysis (PCA)

independent component analysis (ICA)

blind source separation (BSS)



在这项工作中，这些方法分为四类:

(i) TS is performed in the time domain

(ii) applying a BSS technique directly on the ABD channels, eg: ICA

(iii) performing TS and applying BSS on the residuals, denoted TS-ICA or ICA-TS

(iv) moving to the source domain using BSS and performing TS in that domain with an eventual final BSS step (as in [5], denoted ICA-TS-ICA).

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvluny8im0j60iy07uq3g02.jpg" alt="image-20211020162047685" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvluq769a6j60o40n042402.jpg" alt="image-20211020162257208" style="zoom:50%;" />

### Noninvasive Fetal QRS Detection Using Echo State Network





## 数据库

### DaISy

### ADFECGDB

### Challenge 2013

Set b：

-25例患者要么没有心电图（图4），要么母亲的QRS非常低-小于0。1毫伏（图5）；

-5例信噪比极低（图6）；

-胎儿QRS可以在几次记录中直观地观察到；

-在28种情况下，4条引线是通过复制粘贴一条相同的引线获得的（见图5），这减少了从4条独立引线获得的信息；

-至少有5例记录了振幅标度的剧烈变化，这注定无法使用振幅标准。





## 实现方法及其效果

### physionet2013 匹配滤波法A by arunadeogire 

步骤：

+ 基线漂移消除
  + [c,l] = wavedec(x,n,wname)。使用小波wname返回n级一维信号x的小波分解。输出分解结构由小波分解向量c和簿记向量l组成，簿记向量l按级别包含系数的数量。
  + XD=wden（C，L，__;）返回信号X的去噪版本XD，直接从X的小波分解结构[C，L]中获得，[C，L]是wavedec的输出。其中阈值选择规则使用minimax阈值。阈值重缩放的类型使用sln，即使用基于第一级的噪声估计值重新缩放
  + 使用smooth得到原始信号的趋势
  + 使用降噪后的数据XD减去数据趋势，得到基线漂移后的数据
+ Mother QRS Detection：本文使用基于最大值搜索的peak detection
  + 根据成年人心率指定一个窗口大小，选定窗口大小内的最大值作为peak。
  + 然后将peak中相邻两个间隔小于对应每分钟60次心跳的fake peak去掉，作为最终的peak。
+ Mother QRS Cancellation
  + 模版计算
    + 根据Mother QRS Detection得到的R波peak，取模版长度为0.7$\times$fs=700个点，对应频率为85次心跳/分钟
    + 取R波前面0.25$\times$fs为P波开始，R波后面0.45$\times$fs为T波的结束点
    + 指定一个循环迭代次数r，根据之前的到的peak，可以的到r个模版，计算均值得到最终模版
  + MECG cancellation
    + 将上述模版分成3段：0-0.2$\times$fs为PR间期，0.2$\times$fs-0.3$\times$fs为QRS间期，0.3$\times$fs-0.7$\times$fs为RT间期
    + 然后根据模版和原始信号计算三段间期的一个峰值比例，得到三个系数
    + 将3个系数分别乘模版中的三个间期，得到母体信号
    + 将母体信号从原始信号中减去得到去除母体信号的残差信号
+ 残差信号预处理
  + 对残差信号同样使用小波变换降噪
+ Fetal QRS Detection by PeakDetection Algorithm
  + 同样使用上述的基于最大值搜索的peak detection来得到胎儿的QRS peak
  + 由于每一个通道都可以得到一个peak list
  + 取数量最多的peak list作为结果

实验效果：

下述结果是在当ground truth的peak与precicated的peak距离小于100个点(即0.1$\times$fs，认为是在QRS期间位置)时认为是正确的。

5min时长的数据运行时间为17秒。

| ADFECGDB数据 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ---- | ------------- | -------- | ------- |
| R01  | 0.86496         | 0.85151   | 0.85801 |
| R04  | 0.80723         | 0.80112   | 0.80379 |
| R07  | 0.81917         | 0.80658   | 0.81255 |
| R08  | 0.90876         | 0.90703   | 0.9076  |
| R10  | 0.89636         | 0.86404   | 0.87618 |
| 平均值 | 0.8593   | 0.8461 | 0.8516 |

下述结果是在Challenge2013数据上的效果，实验发现原始数据的标签有一些漏标的，所以导致准确率并不是那么好。

| Challenge2013数据   | 提取成功率 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ------------------- | ---------- | --------------- | --------- | ------- |
| set a数据集（75条） | 99.1111%   | 0.72704         | 0.75927   | 0.74129 |



### physionet2013 匹配滤波法B by arunadeogire 

步骤：

+ 基线漂移消除 同A
+ Mother QRS Detection 同A
+ Mother QRS Cancellation 同A
+ ==残差信号预处理==
  + 使用smooth得到残差信号的趋势
  + 使用残差减去数据趋势
+ ==PCA==
  + 对预处理后的数据进行通过Matlab函数pca进行PCA，得到经过主成分分析变换后的数据
+ PeakDetection 同A
  + 取PCA后结果中的第一行数据进行peak detect，只有一个结果，作为最终结果

实验效果：

下述结果是在当ground truth的peak与precicated的peak距离小于100个点(即0.1$\times$fs，认为是在QRS期间位置)时认为是正确的。

5min时长的数据运行时间为13秒。

| ADFECGDB数据 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ---- | ------------- | -------- | ------- |
| R01  | 0.95804  | 0.96192 | 0.95974 |
| R04  | 0.94124  | 0.94899 | 0.94481 |
| R07  | 0.9618   | 0.96351 | 0.9625 |
| R08  | 0.90876         | 0.90703   | 0.9076  |
| R10  | 0.8968 | 0.88827 | 0.88871 |
| 平均值 | 0.90109 | 0.9179 | 0.90849 |

讨论：与方法A相比，增加了PCA之后，精度提高了很多，说明从多通道的残差信号中提取主成分有利于胎儿QRS peak的检测。

| Challenge2013数据   | 提取成功率 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ------------------- | ---------- | --------------- | --------- | ------- |
| set a数据集（75条） | 99.1111%   | 0.66368         | 0.73377   | 0.69386 |

Challenge2013结果比较差，原因是因为Challenge2013中胎儿的peak相比ADFECGDB更小，因此去除母体信号之后和噪音混合在一起，会出现难以分离的情情况。



### physionet2013 匹配滤波法C by arunadeogire 

步骤：

+ 基线漂移消除 同A
+ ==PCA of Mother ECG==
  + 对原始信号做PCA（之后的操作都是只有一个通道的数据）
+ Mother QRS Detection 同A
+ Mother QRS Cancellation 同A
+ 残差信号预处理 同B
+ PeakDetection 同A

实验效果：

下述结果是在当ground truth的peak与precicated的peak距离小于100个点(即0.1$\times$fs，认为是在QRS期间位置)时认为是正确的。

5min时长的数据运行时间为5秒。

| ADFECGDB数据 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ---- | ------------- | -------- | ------- |
| R01  | 0.91244 | 0.92429 | 0.91787 |
| R04  | 0.93504 | 0.9439 | 0.93924 |
| R07  | 0.96188 | 0.95905 | 0.96026 |
| R08  | 0.75784  | 0.81042 | 0.78201 |
| R10  | 0.89183 | 0.87964 | 0.88186 |
| 平均值 | 0.8918 | 0.9035 | 0.8962 |

| Challenge2013数据   | 提取成功率 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ------------------- | ---------- | --------------- | --------- | ------- |
| set a数据集（75条） | 99.1111%   | 0.64518         | 0.71617   | 0.67589 |

讨论：精度比A高，比B低，说明PCA整体来说是有用的，但是提前对比较明显的母体信号使用了PCA，导致了包含胎儿的信号只有一个通道，因此与方法B相比精度有所降低。



### physionet2013 匹配滤波法 by Joachim Behar

步骤：

+ 数据预处理
  + remove NaN值
  + ==基于抽取和插值的心电信号等值线滤波（基线漂移消除）by Jakub Kuzilek==
    + `y = decimate(x,r,'fir')` uses a finite impulse response (FIR) filter designed using the window method with a Hamming window. The filter has an order of 30.
    + 对抽取后的信号做中值滤波
    + 对滤波后的数据做插值使得与原始数据长度相同
    + 原始数据减去插值数据得到去除基线漂移后的数据
  + ==通过一个窗口大小为50的移动平均滤波器得到局部趋势，然后原始信号减去局部趋势==
+ MQRS PeakDetection
  + 计算每个通道的样本峰度（Matlab function：kurtosis），然后升序排列
  + 然后使用峰度小的两个通道的数据进行QRS detection。QRS detection based on Christov algorithm
    + 预处理
      + 去均值
      + remove grid noise 使用递归移动平均过滤器过滤数据
      + remove muscle noise 使用递归移动平均过滤器过滤数据
      + 使用以下等式将ecg转换为复杂导联：Y[i] = 1/L * sum(abs(Xj[i+1]-Xj[i-1]))
      + remove transforming noise 使用递归移动平均过滤器过滤数据
    + 后续。。。。。。。。
+ MECG cancellation
  + 取==模版长度为0.7$\times$fs=700个==点，取R波前面0.25$\times$fs为P波开始，R波后面0.45$\times$fs为T波的结束点。指定一个循环迭代次数r，根据之前的到的peak，可以的到r个模版，计算均值得到最终模版
  + 将上述模版分成3段：0-0.2$\times$fs为PR间期，0.2$\times$fs-0.3$\times$fs为QRS间期，0.3$\times$fs-0.7$\times$fs为RT间期
  + 然后根据模版和原始信号计算三段间期的一个峰值比例，得到三个系数。
  + 将3个系数分别乘模版中的三个间期，得到母体信号
  + 将母体信号从原始信号中减去得到去除母体信号的残差信号
+ FQRS detection
  + 使用之前基于最大值的PeakDetection算法去检测peak
  + correctFQRS
  + 得到一堆FQRS
  + 找到其中fqrs peak index差值的标准差最小的一组作为最终结果



实验效果：

下述结果是在当ground truth的peak与precicated的peak距离小于100个点(即0.1$\times$fs，认为是在QRS期间位置)时认为是正确的。

5min时长的数据运行时间为30秒。

| ADFECGDB数据 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ---- | ------------- | -------- | ------- |
| R01  | 0.99392  | 0.97972 | 0.98651 |
| R04  | 0.94538 | 0.93583 | 0.94036 |
| R07  | 0.96498  | 0.9633 | 0.96405 |
| R08  | 0.95702  | 0.94123 | 0.94881 |
| R10  | 0.94334  | 0.89644 | 0.91569 |
| 平均值 | 0.9609 | 0.9433 | 0.9511 |

1min的数据运行时间为12秒左右，运行6次，每次处理10秒的数据。

| Challenge2013数据   | 提取成功率 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ------------------- | ---------- | --------------- | --------- | ------- |
| set a数据集（75条） | 99.3333%   | 0.72851         | 0.74064   | 0.73216 |

与之前的几个方法比，本算法的优势在于：

+ 数据预处理的结果相比其他算法，基线漂移去除的效果更好
+ MQRS PeakDetection时，选择了两个峰度比较小的通道去进行peak选择
+ 在MECG cancellation，与之前的算法不同，这里在求比例时，求了一个原始信号peak与模版的最佳匹配位置，相比之前加了一个偏移（具体带来的好处需要继续探索）
+ FQRS detection：使用之前基于最大值的PeakDetection算法与基于熵估计和峰值检测的FQRS检测算法去检测结果，并进行了peak correct，得到一堆FQRS，找到其中fqrs peak index差值的标准差最小的一组作为最终结果



### physionet2013 匹配滤波法 by Maurizio Varanini(winner of challenge 2013)

步骤：

+ Artifact canceling 伪影消除
  + 中值滤波
  + 去除数据原始趋势
  + 找出大于阈值的异常点，将附近点作为其值
+ detrending 基线漂移去除
  + 使用一个一阶的5Hz低通滤波器去estimated baseline
  + 从原始信号中减去estimated baseline从而达到去除基线漂移的效果
+ Power line interference removal by notch filtering 工频干扰去除
  + notch filtering去除50Hz和60Hz的工频干扰
+ ==Independent Component Analysis for mother ecg separation==
  + FastICA: 先使用双曲余弦的ICA算法，如果失败，使用峰度的ICA算法
+ Signal interpolation 信号插值
  + 对ICA的结果做4倍的插值
+ ==Channel selection and Mother QRS detection==
  + 通过某种方法选择一个最好的通道（在不同大小的窗口内求数据的最大值，然后求一个比例）
  + 在选择的通道上进行QRS peak的提取
    +  基于阈值，结合成人的RR间期，判断出每一个QRS间期的入点，peak，出点
+ Mother QRS cancelling (使用奇异值分解获得的PQRST近似进行“母亲”ECG消除。)
  + 根据QRS 得到的RR间期设置==动态模版==的长度
  + 在每一个通道上，取peak附近一定长度的点，拿到所有peak附近的母亲心跳模版之后，做SVD分解然后取主要的成分再变换回去作为每一个peak附近的母亲心跳模版
  + 从原始信号中减去模版
  + 与之前的方法不同：==1. 动态模版 2. 不同通道，不同peak处的模版都保持了各自的性质，而不是统一使用一个模版==
+ ==Source separation by ICA on residual signals==
  + FastICA: 先使用双曲余弦的ICA算法，如果失败，使用峰度的ICA算法
+ Channel selection and Fetal QRS detection
  + 采用与Mother QRS detection类似的方法得到一个peak的结果
  + 根据上述结果计算RR间期
  + 根据RR间期重新使用另一种方法对数据从前向后和从后向前计算peak，将结果融合在一起
  + 根据RR间期等参数计算一个权重从多个通道中选择出最好的一个通道，将其结果作为最终结果



实验效果：

5min时长的数据运行时间为5.8秒。

| ADFECGDB数据 | 提取成功率 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ---- | ------------- | -------- | ------- | ------- |
| R01  | 96.6667% | 0.93454 | 0.91016 | 0.92026 |
| R04  | 93.3333% | 0.99514 | 0.99345 | 0.99428 |
| R07  | 90% | 0.99479 | 0.99479 | 0.99479 |
| R08  | 100% | 0.92199  | 0.90055 | 0.90935 |
| R10  | 96.6667% | 0.92062 | 0.90498 | 0.90959 |
| 平均值 | 95.333% | 0.95342         | 0.94079   | 0.94565 |



Matlab版本：1min的数据运行时间为1.3秒左右，运行6次，每次处理10秒的数据。

Python版本：1min的数据运行时间为2.5秒左右，运行6次，每次处理10秒的数据。

| Challenge2013数据               | 提取成功率 | R峰提取灵敏度Se | 精确率PPV | F1      |
| ------------------------------- | ---------- | --------------- | --------- | ------- |
| Matlab版本：set a数据集（75条） | 96.2222%   | 0.93906         | 0.93438   | 0.93599 |
| Python版本：set a数据集（75条） | 98.0%      | 0.9438          | 0.93742   | 0.93906 |



## 与需求的差异

胎心提取：

1. Daisy数据库测试
2. FECGSYN工具箱模拟数据要求有胎儿心跳波形，这个使用之前比较简单的FastICA+autocorr阈值判断就可以达到很好的效果。这个卡看是否可以直接使用目前的算法中的一部分。

其他需求：

1. 。。。。。。
