---
layout: page
permalink: /blogs/sceua/index.html
title: sceua
---






## 下载地址


您可以直接复制本文源代码，按照说明构建项目运行程序。


如果您愿意支持本人的研究，也可以通过点击此链接[优化算法 SCEUA算法C++实现 附Python、MATLAB、Fortran代码](https://download.csdn.net/download/weixin_43012724/72372688)下载已经构建完成的项目文件和参考代码等，并附有使用说明，感谢支持与信任！


**如有问题，欢迎交流探讨！** 邮箱：lujiabo@hhu.edu.cn 卢家波  
 来信请说明博客标题及链接，谢谢。


## 编程思路


参考段青云教授在92年发布的Fortran源码和其他作者贡献的Python、MATLAB、C++等[代码](https://blog.csdn.net/weixin_43012724/article/details/121401083)，用C++以OOP方式写一个SCE-UA的C++版本，供研究使用。


整体思路是自上而下，将SCE-UA算法设计成一个类，开放优化接口，封装算法细节。


### 成员变量


在设计时，应该首先考虑哪些全局变量是算法要用到的，将其作为SCEUA类的成员变量。我总结了一下，大概可以分为以下几类：待优化参数信息、SCE-UA算法控制参数、收敛检查参数、标志变量、优化结果和收敛判据。


* 待优化参数信息是指每个参数的参数名、初始值、下限、上限；
* SCE-UA算法控制参数是指种群规模、复形个数、子复形点数、循环次数等；
* 收敛检查参数包括最大试验次数、函数值最低变化率、参数收敛的标准；
* 标志变量是指示是否采取某些行动的布尔值；
* 优化结果用于存储每次洗牌循环的最佳点及其函数值；
* 收敛判据用于存储每次洗牌循环的优化目标试验次数、函数值变化率和参数范围的归一化几何平均值。


### 成员函数


私有成员函数主要用于实现SCE-UA算法细节，分为`scein()`、`sceua()`、`sceout()`三大部分，分别实现算法的输入、处理和输出，即IPO(Input, Process, Output)。


* `sceua()`函数实现了初始化、生成样本、样本点排序、划分复形群体、复形演化、复形洗牌、收敛判断这七步。
* 三个小工具为`getpnt()`、`sort()`、`chkcst()`分别实现在可行区域内生成一个新点、按函数值的升序对一组点数组和对应的函数值数组进行排序、检查试验点是否满足所有约束功能。
* 目标函数用`functn()`函数定义，输入变量为问题维数和参数数组。


公有成员函数是`Optimize()`，开放优化接口供外部调用。构造函数为成员变量设置了初始值。


### 函数关系




#mermaid-svg-EKf2JR7VQIY8vZG0 {font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}#mermaid-svg-EKf2JR7VQIY8vZG0 .error-icon{fill:#552222;}#mermaid-svg-EKf2JR7VQIY8vZG0 .error-text{fill:#552222;stroke:#552222;}#mermaid-svg-EKf2JR7VQIY8vZG0 .edge-thickness-normal{stroke-width:2px;}#mermaid-svg-EKf2JR7VQIY8vZG0 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-svg-EKf2JR7VQIY8vZG0 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-svg-EKf2JR7VQIY8vZG0 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-svg-EKf2JR7VQIY8vZG0 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-svg-EKf2JR7VQIY8vZG0 .marker{fill:#333333;stroke:#333333;}#mermaid-svg-EKf2JR7VQIY8vZG0 .marker.cross{stroke:#333333;}#mermaid-svg-EKf2JR7VQIY8vZG0 svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-svg-EKf2JR7VQIY8vZG0 .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#333;}#mermaid-svg-EKf2JR7VQIY8vZG0 .cluster-label text{fill:#333;}#mermaid-svg-EKf2JR7VQIY8vZG0 .cluster-label span{color:#333;}#mermaid-svg-EKf2JR7VQIY8vZG0 .label text,#mermaid-svg-EKf2JR7VQIY8vZG0 span{fill:#333;color:#333;}#mermaid-svg-EKf2JR7VQIY8vZG0 .node rect,#mermaid-svg-EKf2JR7VQIY8vZG0 .node circle,#mermaid-svg-EKf2JR7VQIY8vZG0 .node ellipse,#mermaid-svg-EKf2JR7VQIY8vZG0 .node polygon,#mermaid-svg-EKf2JR7VQIY8vZG0 .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-svg-EKf2JR7VQIY8vZG0 .node .label{text-align:center;}#mermaid-svg-EKf2JR7VQIY8vZG0 .node.clickable{cursor:pointer;}#mermaid-svg-EKf2JR7VQIY8vZG0 .arrowheadPath{fill:#333333;}#mermaid-svg-EKf2JR7VQIY8vZG0 .edgePath .path{stroke:#333333;stroke-width:2.0px;}#mermaid-svg-EKf2JR7VQIY8vZG0 .flowchart-link{stroke:#333333;fill:none;}#mermaid-svg-EKf2JR7VQIY8vZG0 .edgeLabel{background-color:#e8e8e8;text-align:center;}#mermaid-svg-EKf2JR7VQIY8vZG0 .edgeLabel rect{opacity:0.5;background-color:#e8e8e8;fill:#e8e8e8;}#mermaid-svg-EKf2JR7VQIY8vZG0 .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-svg-EKf2JR7VQIY8vZG0 .cluster text{fill:#333;}#mermaid-svg-EKf2JR7VQIY8vZG0 .cluster span{color:#333;}#mermaid-svg-EKf2JR7VQIY8vZG0 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-svg-EKf2JR7VQIY8vZG0 :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}



















































































































































































































公有









私有









私有








































































































































































































































 SCEUA类 











 Optimize 











 成员函数 











 成员变量 











 小工具 











 scemain 











 目标函数 











 scein 











 sceua 











 sceout 











 GenerateSample 











 RankPoints 











 Partition2Complexes 











 cce 











 ShuffleComplexes 











 CheckConvergence 











 待优化参数信息 











 SCE-UA算法控制参数 











 收敛检查参数 











 标志变量 











 优化结果 











 收敛判据 











 functn 











 getpnt 











 sort 











 chkcst 










## 参考文献


[【算法】02 SCE-UA简介及源代码](https://blog.csdn.net/weixin_43012724/article/details/121401083)


需要指出的是，SCE-UA算法最权威的阐述是段老师在1993年发表的文章[《Shuffled complex evolution approach for effective and efficient global minimization》](https://doi.org/10.1007/BF00939380)，但我认为SCE计算流程图有部分不足，应当改为下图更为合理。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/494fc49e30644bef9f7c9c4ffd1ecc12.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


王书功博士在[《水文模型参数估计方法及参数估计不确定性研究》](https://book.douban.com/subject/5377630/)（P73-79）中对SCE-UA的介绍虽然比较清楚，但没有把CCE翻译完整；而宋晓猛博士在[《流域水文模型参数不确定性量化理论方法与应用》](https://baike.baidu.com/item/%E6%B5%81%E5%9F%9F%E6%B0%B4%E6%96%87%E6%A8%A1%E5%9E%8B%E5%8F%82%E6%95%B0%E4%B8%8D%E7%A1%AE%E5%AE%9A%E6%80%A7%E9%87%8F%E5%8C%96%E7%90%86%E8%AE%BA%E6%96%B9%E6%B3%95%E4%B8%8E%E5%BA%94%E7%94%A8/22358719)（P128-131）中虽然详细翻译了段老师的文章，但翻译得较为生硬，不易于理解，而且在CCE部分存在3个明显的错误，分别是


* 第130页，第（4）步，第3小步，“计算期函数值 

 

 

 f 


 r 

 


 f\_r 


 fr​” 应当改为 “计算**其**函数值 

 

 

 f 


 r 

 


 f\_r 


 fr​”
* 第131页，第（4）步，第5小步，“以 

 


 z 

 

 z 


 z代替 

 


 r 

 

 r 


 r，以 

 

 

 f 


 z 

 


 f\_z 


 fz​代替 

 

 

 f 


 r 

 


 f\_r 


 fr​” 应当改为 “以 

 


 z 

 

 z 


 z代替 

 

 

 u 


 q 

 


 u\_q 


 uq​，以 

 

 

 f 


 z 

 


 f\_z 


 fz​代替 

 

 

 f 


 q 

 


 f\_q 


 fq​”
* 第131页，第（6）步，“重复步骤（1）~（5） 

 


 β 

 

 \beta 


 β次” 应当改为 “重复步骤（2）~（5） 

 


 β 

 

 \beta 


 β次”


SCE-UA算法**入门**建议直接看段老师[原文](https://doi.org/10.1007/BF00939380)，SCE部分可以参考王书功的介绍，CCE部分可以参考宋晓猛的介绍，但要注意他的三个错误之处。下面给出我自己的总结，供参考。


## SCE


![在这里插入图片描述](https://img-blog.csdnimg.cn/312621db4ad3468eab495a40d94986f4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_18,color_FFFFFF,t_70,g_se,x_16#pic_center)


1. **初始化**。问题维数（目标函数有n个变量）：n，复形个数  

 


 p 


 ≥ 


 1 

 

 p\ge1 


 p≥1，复形内点个数  

 


 m 


 ≥ 


 n 


 + 


 1 

 

 m \ge n+1 


 m≥n+1，样本规模（缓冲区所有点个数） 

 


 s 


 = 


 p 


 × 


 m 

 

 s = p \times m 


 s=p×m
2. **生成样本**。在参数空间均匀分布采样，采集 

 


 s 

 

 s 


 s个点，并计算每个点 

 

 

 x 


 i 

 


 x\_i 


 xi​函数值 

 

 

 f 


 i 

 


 f\_i 


 fi​
3. **样本点排序**。在缓冲区 

 


 D 

 

 D 


 D中升序排列 

 


 s 

 

 s 


 s个点， 

 


 D 


 = 


 { 


 ( 

 

 x 


 1 

 

 , 

 

 f 


 1 

 

 ) 


 , 


 ( 

 

 x 


 2 

 

 , 

 

 f 


 2 

 

 ) 


 ⋯ 


 ( 

 

 x 


 s 

 

 , 

 

 f 


 s 

 

 ) 


 } 

 

 D=\{(x\_1,f\_1), (x\_2,f\_2) \cdots (x\_s,f\_s)\} 


 D={(x1​,f1​),(x2​,f2​)⋯(xs​,fs​)}
4. **划分复形**。 

 


 D 

 

 D 


 D中点划分到 

 


 p 

 

 p 


 p个复形中，每个复形含有 

 


 m 

 

 m 


 m个点，在复形 

 

 

 C 


 k 

 


 C^k 


 Ck中， 

 

 

 C 


 k 

 

 = 


 { 


 ( 

 

 x 


 i 

 

 , 

 

 f 


 i 

 

 ) 


 ∣ 


 j 


 = 


 p 


 ( 


 i 


 − 


 1 


 ) 


 + 


 k 


 , 


 i 


 = 


 1 


 , 


 2 


 , 


 ⋯ 
   

 , 


 m 


 } 

 

 C^k=\{(x\_i,f\_i)|j=p(i-1)+k,i=1,2,\cdots,m\} 


 Ck={(xi​,fi​)∣j=p(i−1)+k,i=1,2,⋯,m}
5. **复形演化**。对每个复形进行CCE演化
6. **重洗复形**。所有复形中的点放入缓冲区 

 


 D 

 

 D 


 D中，按函数值升序排列
7. **收敛判断**。若收敛，则终止。否则，转到步骤 4


## CCE


![在这里插入图片描述](https://img-blog.csdnimg.cn/a8a618c10ed042a680c4ba87cb62b22a.png#pic_center)


1. **初始化权重**。确定 

 


 2 


 ≤ 


 q 


 ≤ 


 m 

 

 2\le q \le m 


 2≤q≤m， 

 


 α 


 ≥ 


 1 

 

 \alpha \ge 1 


 α≥1， 

 


 β 


 ≥ 


 1 

 

 \beta \ge 1 


 β≥1
2. **分配权重**。计算复形内每个点的选择概率
3. **选择父辈群体**。按选择概率选取 

 


 q 

 

 q 


 q个点到 

 


 B 

 

 B 


 B中， 

 

 

 u 


 1 

 

 , 

 

 u 


 2 

 

 , 


 ⋯ 
   

 , 

 

 u 


 q 

 


 u\_1,u\_2,\cdots ,u\_q 


 u1​,u2​,⋯,uq​，记录位置 

 


 L 

 

 L 


 L
4. **产生下一代群体**。  
 a. 对 

 


 B 

 

 B 


 B和 

 


 L 

 

 L 


 L按函数值升序排列，找出 

 


 B 

 

 B 


 B中最差点，记为 

 

 

 u 


 q 

 


 u\_q 


 uq​，计算其余 

 


 q 


 − 


 1 

 

 q-1 


 q−1个点**形心**  

 


 g 

 

 g 


 g  
 b. **反射**。计算最差点的反射点  

 


 r 


 = 


 2 


 g 


 − 

 

 u 


 q 

 


 r=2g-u\_q 


 r=2g−uq​  
 c. **突变**。若 

 


 r 

 

 r 


 r在参数可行空间内，计算 

 

 

 f 


 r 

 


 f\_r 


 fr​。否则随机生成点 

 


 z 

 

 z 


 z，计算 

 

 

 f 


 z 

 


 f\_z 


 fz​，令 

 


 r 


 = 


 z 

 

 r=z 


 r=z， 

 

 

 f 


 r 

 

 = 

 

 f 


 z 

 


 f\_r=f\_z 


 fr​=fz​  
 d. **收缩**。若 

 

 

 f 


 r 

 

 < 

 

 f 


 q 

 


 f\_r<f\_q 


 fr​<fq​，以 

 


 r 

 

 r 


 r代替最差点 

 

 

 u 


 q 

 


 u\_q 


 uq​，转到 f；否则，计算 

 


 c 


 = 


 ( 


 g 


 + 

 

 u 


 q 

 

 ) 


 / 


 2 

 

 c=(g+u\_q)/2 


 c=(g+uq​)/2和 

 

 

 f 


 c 

 


 f\_c 


 fc​  
 e. **突变**。若 

 

 

 f 


 c 

 

 < 

 

 f 


 q 

 


 f\_c<f\_q 


 fc​<fq​，以 

 


 c 

 

 c 


 c代替最差点 

 

 

 u 


 q 

 


 u\_q 


 uq​，转到 f；否则，随机生成点 

 


 z 

 

 z 


 z，计算 

 

 

 f 


 z 

 


 f\_z 


 fz​，以 

 


 z 

 

 z 


 z代替最差点 

 

 

 u 


 q 

 


 u\_q 


 uq​，令 

 

 

 u 


 q 

 

 = 


 z 

 

 u\_q=z 


 uq​=z， 

 

 

 f 


 q 

 

 = 

 

 f 


 z 

 


 f\_q=f\_z 


 fq​=fz​  
 f. **重复** a-e  

 


 α 

 

 \alpha 


 α次
5. **以子代取代父代**。用 

 


 B 

 

 B 


 B代替复形 

 

 

 C 


 k 

 


 C^k 


 Ck储存在 

 


 L 

 

 L 


 L中的原位置，将复形按函数值升序排列
6. **迭代**。重复步骤 2-5  

 


 β 

 

 \beta 


 β次， 

 


 β 

 

 \beta 


 β决定每个复形进化代数。


## 代码用法


### 文件结构


程序代码共有三个文件，分别为主函数`optimize.cpp`、头文件`SCEUA.h`、源文件`SCEUA.cpp`。参数输入文件名为`scein.txt`，优化结果输出文件名为`sceout.txt`。在Visual Studio 2019下新建项目，添加以上三个代码文件，在当前路径下创建文本文件`scein.txt`，即可点击本地Windows调试器运行。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/c69abe863b4a4380aa3b43b1b8a01fa6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/62e990ee4b9b4403a01381e8b8caebea.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/600414ac858947f3af84da4e052a118d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


### 开发环境


程序采用Win 11系统下Visual Studio 2019开发环境，默认C++14标准，编译通过。程序全部采用STL标准模板库函数，不需要额外安装库。Visual Studio 2019默认使用C++14标准，如果要使用测试函数1Rastrigin 函数和3 Ackley函数，由于其用到圆周率常数，请包含头文件`<numbers>`，并修改Visual Studio 2019的C++标准为C++20，如何修改标准请参阅[VS2019修改C++标准](https://blog.csdn.net/qq_40946921/article/details/90645890)、[Visual Studio 2019(2017以后)设定语言标准后(C++)仍然为C++98问题解决](https://blog.csdn.net/weixin_45485719/article/details/105171534)，并注意配置为所有配置，平台为所有平台。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/eddba7c3790c4d0481c2363fc0b0ddf1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/119d2c3c25794ddf8f92b1d7f6bfeb60.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


## 测试


利用常见的的优化算法测试函数来测试SCE-UA算法的性能。测试过程中需要修改的地方主要有两处：


* 目标函数定义`functn()`，即输入参数，返回目标函数值。 输入待优化的参数数量和各参数构成的数组，输出目标函数值；
* 参数范围设置文件`scein.txt`：用于设置参数名、初始值、下限、上限和SCE-UA控制参数。


###  

 


 R 


 a 


 s 


 t 


 r 


 i 


 g 


 i 


 n 


 函数 

 

 Rastrigin函数 


 Rastrigin函数


#### 问题描述



 

 

 m 


 i 


 n 


 f 


 ( 


 x 


 ) 


 = 


 10 


 n 


 + 

 

 ∑ 

 

 i 


 = 


 1 

 

 n 

 

 [ 

 

 x 


 i 


 2 

 

 − 


 10 


 c 


 o 


 s 


 ( 


 2 


 π 

 

 x 


 i 

 

 ) 


 ] 

 

 minf(x)=10n+\sum\_{i=1}^n[x\_i^2-10cos(2\pi x\_{i})] 


 minf(x)=10n+i=1∑n​[xi2​−10cos(2πxi​)]  

 

 


 x 


 i 

 

 ∈ 


 [ 


 − 


 5.12 


 , 


 5.12 


 ] 


 , 


 i 


 = 


 1 


 , 


 . 


 . 


 . 


 , 


 n 

 

 x\_i\in[-5.12,5.12],i=1,...,n 


 xi​∈[−5.12,5.12],i=1,...,n  
 ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE1LmNuYmxvZ3MuY29tL2Jsb2cvMTA4ODAzNy8yMDE3MDIvMTA4ODAzNy0yMDE3MDIwNjIwMDEyMDcxMy05MzQ1OTExMzUucG5n?x-oss-process=image/format,png#pic_center)  
 在(0,0,…,0)处取到最小值0


#### 代码修改


在`SCEUA.cpp`文件底部的`functn()`函数中，按如下书写：



```
double SCEUA::functn(const int& nopt, const std::vector<double>& x)
{
    //================测试函数================
    //1. Rastrigin 函数
    //x in [-5.12, 5.12]
    double sum = 0.0;

    for (int i = 0; i < nopt; i++)
    {
        sum += pow(x.at(i), 2) - 10 \* cos(2 \* std::numbers::pi \* x.at(i));
    }

    sum += 10.0 \* nopt;

    return sum;
}

```

#### 输入文件修改


在程序目录下，创建`scein.txt`文件，按如下书写：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/45e87eeb4d5c4a2ea8f04231545be2e0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16)



```
6
x1  1.9 -5.12 5.12
x2  2.7 -5.12 5.12
x3  5.5 -5.12 5.12
x4  -3.2 -5.12 5.12
x5  -3.6 -5.12 5.12
x6  -2.4 -5.12 5.12
0

```

#### 运行结果


点击本地Windows调试器，在CMD窗口查看优化结果，优化结果也会自动保存到同目录下的`sceout.txt`文件中。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/b75d1553973f499ea008ec21173a67c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16)  
 CMD窗口：



```
==================================================
                  进入SCE-UA全局搜索
==================================================
目标函数调用次数  函数值变化率  参数变化范围
141  10000  0.806087
277  10000  0.657656
417  10000  0.402064
554  10000  0.181341
700  10000  0.178682
813  10000  0.0359462
915  10000  0.0140927
1018  10000  0.0121415
1129  10000  0.00891704
1234  6.07113  0.00459872
1341  4.38542  0.00319089
1452  4.25121  0.00141247
经过13次洗牌演化，种群已经收敛到一个预先指定的小参数空间
1565  1.63487  0.000996996
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6
参数初始值：1.9  2.7  5.5  -3.2  -3.6  -2.4
参数下限：-5.12  -5.12  -5.12  -5.12  -5.12  -5.12
参数上限：5.12  5.12  5.12  5.12  5.12  5.12
初始参数函数值：148.2
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6
0.994054  -0.000107047  -0.000346417  0.000287252  0.993954  0.994001
最优函数值:2.98546
14次洗牌演化的最优点函数值：
53.1644  28.175  20.1182  8.64779  5.03524  4.82513  3.44209  3.29447  3.06247  3.01294  2.99318  2.99009  2.98662  2.98546
E:\Research\OptA\SCEUA\SCEUA\Debug\SCEUA.exe (进程 66060)已退出，代码为 0。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .

```

`sceout.txt`文件:



```
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6  
参数初始值：1.9  2.7  5.5  -3.2  -3.6  -2.4  
参数下限：-5.12  -5.12  -5.12  -5.12  -5.12  -5.12  
参数上限：5.12  5.12  5.12  5.12  5.12  5.12  
初始参数函数值：148.2
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6  
0.994054  -0.000107047  -0.000346417  0.000287252  0.993954  0.994001  
最优函数值:2.98546
14次洗牌演化的最优点函数值：
53.1644  28.175  20.1182  8.64779  5.03524  4.82513  3.44209  3.29447  3.06247  3.01294  2.99318  2.99009  2.98662  2.98546  

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c04a1677c40e4f30b4b89275c09f22a9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


###  

 


 S 


 c 


 h 


 a 


 f 


 f 


 e 


 r 


 函数 

 

 Schaffer函数 


 Schaffer函数


#### 问题描述



 

 

 f 


 ( 


 x 


 ) 


 = 


 0.5 


 + 

 


 s 


 i 

 

 n 


 2 

 

 ( 

 

 x 


 1 


 2 

 

 + 

 

 x 


 2 


 2 

 

 ) 


 − 


 0.5 

 


 [ 


 1 


 + 


 0.001 


 ( 

 

 x 


 1 


 2 

 

 + 

 

 x 


 2 


 2 

 

 ) 

 

 ] 


 2 

 

 


 f(x)=0.5+\frac{sin^2(x\_1^2+x\_2^2)-0.5}{[1+0.001(x\_1^2+x\_2^2)]^2} 


 f(x)=0.5+[1+0.001(x12​+x22​)]2sin2(x12​+x22​)−0.5​  

 

 


 x 


 i 

 

 ∈ 


 [ 


 − 


 100 


 , 


 100 


 ] 


 , 


 i 


 = 


 1 


 , 


 2 

 

 x\_i\in[-100,100],i=1,2 


 xi​∈[−100,100],i=1,2  
 ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE1LmNuYmxvZ3MuY29tL2Jsb2cvMTA4ODAzNy8yMDE3MDIvMTA4ODAzNy0yMDE3MDIwNjIwMDY0Mzc5MS0yNjEyMDM4ODEucG5n?x-oss-process=image/format,png#pic_center)  
 在很多位置可以取到最小值0


#### 代码修改


在`SCEUA.cpp`文件底部的`functn()`函数中，按如下书写：



```
double SCEUA::functn(const int& nopt, const std::vector<double>& x)
{
    //================测试函数================
    //2. Schaffer 函数
    // x in [-100, 100]
	double sum = 0.0;

	double temp = 0.0;

	for (int i = 0; i < nopt; i++)
	{
		temp += pow(x.at(i), 2);
	}

	sum = 0.5 + (pow(sin(temp), 2) - 0.5) / pow(1.0 + 0.001 \* temp, 2);

	return sum;
}

```

#### 输入文件修改


在程序目录下，创建`scein.txt`文件，按如下书写：



```
6
x1 52 -100 100
x2 -3 -100 100
x3 -25 -100 100
x4 14 -100 100
x5 -90 -100 100
x6 38 -100 100
0

```

#### 运行结果


点击本地Windows调试器，在CMD窗口查看优化结果，优化结果也会自动保存到同目录下的`sceout.txt`文件中。  
 CMD窗口：



```
==================================================
                  进入SCE-UA全局搜索
==================================================
目标函数调用次数  函数值变化率  参数变化范围
174  10000  0.756129
294  10000  0.585613
423  10000  0.453379
551  10000  0.317855
676  10000  0.237178
803  10000  0.178389
936  10000  0.129672
1068  10000  0.128942
1212  10000  0.0896534
1362  1.33518  0.0804222
1522  1.52314  0.0704985
1691  1.78068  0.0627689
1855  2.06417  0.0462108
2024  2.09962  0.0391693
2173  1.52905  0.0277407
2289  1.70291  0.0044745
经过17次洗牌演化，种群已经收敛到一个预先指定的小参数空间
2416  1.24436  0.000638887
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6
参数初始值：52  -3  -25  14  -90  38
参数下限：-100  -100  -100  -100  -100  -100
参数上限：100  100  100  100  100  100
初始参数函数值：0.498433
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6
-2.92341  -1.31889  -1.55793  -2.1283  5.61835  6.74085
最优函数值:0.0824208
18次洗牌演化的最优点函数值：
0.494488  0.486214  0.465307  0.440219  0.382816  0.272162  0.262982  0.19976  0.132647  0.112091  0.109819  0.0933646  0.0829123  0.0824432  0.0824271  0.0824208  0.0824208  0.0824208
E:\Research\OptA\SCEUA\SCEUA\Debug\SCEUA.exe (进程 64712)已退出，代码为 0。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .

```

`sceout.txt`文件:



```
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6  
参数初始值：52  -3  -25  14  -90  38  
参数下限：-100  -100  -100  -100  -100  -100  
参数上限：100  100  100  100  100  100  
初始参数函数值：0.498433
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6  
-2.92341  -1.31889  -1.55793  -2.1283  5.61835  6.74085  
最优函数值:0.0824208
18次洗牌演化的最优点函数值：
0.494488  0.486214  0.465307  0.440219  0.382816  0.272162  0.262982  0.19976  0.132647  0.112091  0.109819  0.0933646  0.0829123  0.0824432  0.0824271  0.0824208  0.0824208  0.0824208  

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e0b8370044cf4bb295db43244f1b72ba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


###  

 


 A 


 c 


 k 


 l 


 e 


 y 


 函数 

 

 Ackley函数 


 Ackley函数


#### 问题描述



 

 

 f 


 ( 


 x 


 ) 


 = 


 − 


 a 


 e 


 x 


 p 


 ( 


 − 


 b 

 

 

 1 


 d 

 


 ∑ 

 

 i 


 = 


 1 

 

 d 

 


 x 


 i 


 2 

 

 

 ) 


 − 


 e 


 x 


 p 


 ( 

 

 1 


 d 

 


 ∑ 

 

 i 


 = 


 1 

 

 d 

 

 c 


 o 


 s 


 ( 


 c 

 

 x 


 i 

 

 ) 


 ) 


 + 


 a 


 + 


 e 

 

 f(x)=-aexp(-b\sqrt{\frac{1}{d}\sum\_{i=1}^dx\_i^2})-exp(\frac{1}{d}\sum\_{i=1}^dcos(cx\_i))+a+e 


 f(x)=−aexp(−bd1​i=1∑d​xi2​


)−exp(d1​i=1∑d​cos(cxi​))+a+e  

 

 

 a 


 = 


 20 


 , 


 b 


 = 


 0.2 


 , 


 c 


 = 


 2 


 π 


 , 

 

 x 


 i 

 

 ∈ 


 [ 


 − 


 32.768 


 , 


 32.768 


 ] 


 , 


 i 


 = 


 1 


 , 


 . 


 . 


 . 


 , 


 d 

 

 a=20,b=0.2,c=2\pi,x\_i\in[-32.768,32.768],i=1,...,d 


 a=20,b=0.2,c=2π,xi​∈[−32.768,32.768],i=1,...,d  
 ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE1LmNuYmxvZ3MuY29tL2Jsb2cvMTA4ODAzNy8yMDE3MDIvMTA4ODAzNy0yMDE3MDIwNjIwMjExNTY2Ni00MjYyNzQxMDAucG5n?x-oss-process=image/format,png#pic_center)  
 在(0,0,…,0)处取到最小值0


#### 代码修改


在`SCEUA.cpp`文件底部的`functn()`函数中，按如下书写：



```
double SCEUA::functn(const int& nopt, const std::vector<double>& x)
{
    //================测试函数================
    //3. Ackley函数
    // x in [-32.768, 32.768]
    double sum = 0.0;

    int a = 20;

    double b = 0.2;

    double c = 2 \* std::numbers::pi;

    double temp1 = 0.0;

    double temp2 = 0.0;

    for (int i = 0; i < nopt; i++)
    {
        temp1 += pow(x.at(i), 2);

        temp2 += pow(cos(c \* x.at(i)), 2);
    }

    sum = -a \* exp(-b \* sqrt(temp1 / double(nopt))) - exp(temp2 / double(nopt)) + a + std::numbers::e;

    return sum;
}

```

#### 输入文件修改


在程序目录下，创建`scein.txt`文件，按如下书写：



```
6
x1 5 -32.768 32.768
x2 -5 -32.768 32.768
x3 20 -32.768 32.768
x4 -12 -32.768 32.768
x5 30 -32.768 32.768
x6 -6 -32.768 32.768
0

```

#### 运行结果


点击本地Windows调试器，在CMD窗口查看优化结果，优化结果也会自动保存到同目录下的`sceout.txt`文件中。  
 CMD窗口：



```
==================================================
                  进入SCE-UA全局搜索
==================================================
目标函数调用次数  函数值变化率  参数变化范围
146  10000  0.813193
253  10000  0.5853
360  10000  0.389368
463  10000  0.3824
564  10000  0.195066
672  10000  0.134101
779  10000  0.116711
886  10000  0.0838089
993  10000  0.0617902
1105  3.33261  0.0441422
1219  3.97831  0.0281231
1327  4.44315  0.0220852
1439  4.4147  0.00641458
1553  4.31094  0.00406771
1666  4.87592  0.00242985
1781  5.41024  0.0026466
1890  6.26544  0.00233324
1996  6.97229  0.00138674
2110  9.4186  0.00115304
2229  7.58798  0.00112468
经过21次洗牌演化，种群已经收敛到一个预先指定的小参数空间
2341  9.71868  0.000595636
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6
参数初始值：5  -5  20  -12  30  -6
参数下限：-32.768  -32.768  -32.768  -32.768  -32.768  -32.768
参数上限：32.768  32.768  32.768  32.768  32.768  32.768
初始参数函数值：19.1796
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6
-7.89202e-05  -0.000248076  -4.58397e-05  8.3223e-05  0.000140595  0.000286515
最优函数值:0.000693411
22次洗牌演化的最优点函数值：
17.3081  14.3859  11.0278  7.52119  5.11036  3.86798  2.7742  1.96933  1.28868  0.894885  0.409979  0.265917  0.118727  0.0676969  0.0399813  0.0214244  0.0114567  0.00612104  0.00332559  0.0019657  0.00109502  0.000693411
E:\Research\OptA\SCEUA\SCEUA\Debug\SCEUA.exe (进程 76764)已退出，代码为 0。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .

```

`sceout.txt`文件:



```
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6  
参数初始值：5  -5  20  -12  30  -6  
参数下限：-32.768  -32.768  -32.768  -32.768  -32.768  -32.768  
参数上限：32.768  32.768  32.768  32.768  32.768  32.768  
初始参数函数值：19.1796
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6  
-7.89202e-05  -0.000248076  -4.58397e-05  8.3223e-05  0.000140595  0.000286515  
最优函数值:0.000693411
22次洗牌演化的最优点函数值：
17.3081  14.3859  11.0278  7.52119  5.11036  3.86798  2.7742  1.96933  1.28868  0.894885  0.409979  0.265917  0.118727  0.0676969  0.0399813  0.0214244  0.0114567  0.00612104  0.00332559  0.0019657  0.00109502  0.000693411  

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d7cf6e52abdf4fed9146634a5ddc3712.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


###  

 


 G 


 r 


 i 


 e 


 w 


 a 


 n 


 k 


 函数 

 

 Griewank函数 


 Griewank函数


#### 问题描述



 

 

 f 


 ( 


 x 


 ) 


 = 

 

 ∑ 

 

 i 


 = 


 1 

 

 d 

 

 

 x 


 i 


 2 

 

 4000 

 

 − 

 

 ∏ 

 

 i 


 = 


 1 

 

 d 

 

 c 


 o 


 s 


 ( 

 


 x 


 i 

 


 i 

 


 ) 


 + 


 1 

 

 f(x)=\sum\_{i=1}^d\frac{x\_i^2}{4000}-\prod\_{i=1}^{d}cos(\frac{x\_i}{\sqrt{i}})+1 


 f(x)=i=1∑d​4000xi2​​−i=1∏d​cos(i


xi​​)+1  

 

 


 x 


 i 

 

 ∈ 


 [ 


 − 


 600 


 , 


 600 


 ] 


 , 


 i 


 = 


 1 


 , 


 … 


 , 


 d 

 

 x\_i \in [-600, 600], i = 1, …, d 


 xi​∈[−600,600],i=1,…,d  
 ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE1LmNuYmxvZ3MuY29tL2Jsb2cvMTA4ODAzNy8yMDE3MDIvMTA4ODAzNy0yMDE3MDIwNjIwMjUzNzM1NC0xMjgwNzE2ODU4LmpwZw?x-oss-process=image/format,png#pic_center)  
 在很多点处取到最小值0


#### 代码修改


在`SCEUA.cpp`文件底部的`functn()`函数中，按如下书写：



```
double SCEUA::functn(const int& nopt, const std::vector<double>& x)
{
    //================测试函数================
    //4. Griewank 函数
    // x in [-600, 600]
    double sum = 0.0;

    double temp1 = 0.0;

    double temp2 = 1.0;

    for (size_t i = 0; i < nopt; i++)
    {
        temp1 += pow(x.at(i), 2) / 4000.0;

        temp2 \*= cos(x.at(i) / double(sqrt(i + 1)));
    }

    sum = temp1 - temp2 + 1;

    return sum;
}

```

#### 输入文件修改


在程序目录下，创建`scein.txt`文件，按如下书写：



```
6
x1 5 -600 600
x2 50 -600 600
x3 400 -600 600
x4 -24.5 -600 600
x5 -600.0 -600 600
x6 -475.3 -600 600
0

```

#### 运行结果


点击本地Windows调试器，在CMD窗口查看优化结果，优化结果也会自动保存到同目录下的`sceout.txt`文件中。  
 CMD窗口：



```
==================================================
                  进入SCE-UA全局搜索
==================================================
目标函数调用次数  函数值变化率  参数变化范围
143  10000  0.789804
244  10000  0.61858
349  10000  0.534725
446  10000  0.306614
547  10000  0.22838
647  10000  0.137765
747  10000  0.112164
850  10000  0.0816326
955  10000  0.079917
1092  12.6861  0.0548753
1254  9.08118  0.0413758
1375  8.48923  0.0190456
1446  4.74838  0.00693574
1529  3.90409  0.00528711
经过15次洗牌演化，种群已经收敛到一个预先指定的小参数空间
1616  3.25838  0.000356751
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6
参数初始值：5  50  400  -24.5  -600  -475.3
参数下限：-600  -600  -600  -600  -600  -600
参数上限：600  600  600  600  600  600
初始参数函数值：188.258
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6
6.34053  -4.5545  -5.72791  -0.87887  7.40813  -7.94033
最优函数值:0.182114
16次洗牌演化的最优点函数值：
63.401  24.0059  12.3005  4.81506  2.86486  1.86036  1.20093  0.944046  0.55871  0.552614  0.438076  0.417222  0.346194  0.228343  0.206737  0.182114
E:\Research\OptA\SCEUA\SCEUA\Debug\SCEUA.exe (进程 62592)已退出，代码为 0。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .


```

`sceout.txt`文件:



```
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6  
参数初始值：5  50  400  -24.5  -600  -475.3  
参数下限：-600  -600  -600  -600  -600  -600  
参数上限：600  600  600  600  600  600  
初始参数函数值：188.258
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6  
6.34053  -4.5545  -5.72791  -0.87887  7.40813  -7.94033  
最优函数值:0.182114
16次洗牌演化的最优点函数值：
63.401  24.0059  12.3005  4.81506  2.86486  1.86036  1.20093  0.944046  0.55871  0.552614  0.438076  0.417222  0.346194  0.228343  0.206737  0.182114  

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/daf23aa911df4a37a85bd91c7c1b2feb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


###  

 


 R 


 o 


 s 


 e 


 n 


 b 


 r 


 o 


 c 


 k 


 函数 

 

 Rosenbrock函数 


 Rosenbrock函数


#### 问题描述


[Rosenbrock 函数](https://www.docin.com/p-1039869305.html)是无约束最优化理论与方法中一个非常经典的函数测试问题，它是衡量无约束算法优劣的一个重要工具。在数值优化领域中，该函数是由Howard H.Rosenbrock于 1960年提出来的，是一个典型的非凸函数，主要用于优化算法的性能测试，并以 Rosenbrock 山谷函数或Rosenbrock香蕉函数的美名而闻名。现在许多优化算法对大部分测试函数能达到很好的优化性能，但大量文献表明许多高效的算法甚至许多智能搜索算法对Rosenbrock函数的优化都难以找到全局极小值，如梯度法、遗传算法等。  

 

 

 m 


 i 


 n 


 f 


 ( 


 x 


 ) 


 = 

 

 ∑ 

 

 i 


 = 


 1 

 


 n 


 − 


 1 

 

 

 ( 


 100 


 ( 

 

 x 

 

 i 


 + 


 1 

 


 − 

 


 x 


 i 

 

 2 

 


 ) 


 2 

 

 + 


 ( 

 

 x 


 i 

 

 − 


 1 

 

 ) 


 2 

 

 ) 

 


 min f(x)=\sum\_{i=1}^{n-1}{(100(x\_{i+1}-{x\_i}^2)^2+(x\_i - 1)^2)} 


 minf(x)=i=1∑n−1​(100(xi+1​−xi​2)2+(xi​−1)2)  

 

 


 x 


 i 

 

 ∈ 


 [ 


 − 


 30 


 , 


 30 


 ] 


 , 


 i 


 = 


 1 


 , 


 … 


 , 


 n 

 

 x\_i \in [-30, 30], i = 1, …, n 


 xi​∈[−30,30],i=1,…,n  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/a2e5a063b75b47ab86f3dff2159fab73.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


在(1,1,…,1)处取到全局最小值0


#### 代码修改


在`SCEUA.cpp`文件底部的`functn()`函数中，按如下书写：



```
double SCEUA::functn(const int& nopt, const std::vector<double>& x)
{
    //================测试函数================
    //5. Rosenbrock 函数
    // x in [-30, 30]
	double sum = 0.0;

	for (int i = 0; i < nopt - 1; i++)
	{
		sum += 100.0 \* pow(x.at(i + 1) - pow(x.at(i), 2), 2) + pow(x.at(i) - 1.0, 2);
	}

	return sum;
}

```

#### 输入文件修改


在程序目录下，创建`scein.txt`文件，按如下书写：



```
6
x1 5.3 -30 30
x2 8.9 -30 30
x3 -9.8 -30 30
x4 3 -30 30
x5 -28.4 -30 30
x6 18.9 -30 30
0

```

#### 运行结果


点击本地Windows调试器，在CMD窗口查看优化结果，优化结果也会自动保存到同目录下的`sceout.txt`文件中。  
 CMD窗口：



```
==================================================
                  进入SCE-UA全局搜索
==================================================
目标函数调用次数  函数值变化率  参数变化范围
133  10000  0.871923
230  10000  0.608642
331  10000  0.375339
433  10000  0.297105
531  10000  0.223799
641  10000  0.188927
742  10000  0.187849
848  10000  0.120739
962  10000  0.0418225
1064  29.436  0.0249663
1169  132.126  0.0173409
1270  47.1479  0.0121491
1349  22.4509  0.0150714
1427  3.12154  0.0190393
1522  4.42121  0.0214466
1615  1.3002  0.0231896
1698  0.942168  0.0209919
1784  1.1971  0.0189214
1875  1.49871  0.0228183
1965  1.87271  0.021105
2061  2.34019  0.0167605
2156  2.94132  0.012494
2254  3.23524  0.00999358
2353  3.84114  0.00852257
2454  4.52342  0.00742001
2559  5.67369  0.00426485
2658  8.42041  0.0028277
2748  7.44417  0.00228662
2837  4.18147  0.00223993
2917  3.53551  0.00374342
3011  1.78374  0.00364193
3109  1.41553  0.00305223
3209  1.13291  0.00230688
3294  1.11536  0.00210329
3376  1.13935  0.002853
3464  1.23012  0.00298238
3557  1.54183  0.00293937
3651  1.72958  0.002604
3746  2.16031  0.00269265
3842  2.10738  0.00284265
3926  1.61799  0.00197136
4017  2.09637  0.00201972
4104  2.53011  0.00167506
4197  3.12058  0.00133909
4285  3.52603  0.00162244
4379  3.77201  0.00147092
4473  2.79433  0.00123216
4566  2.56601  0.00133923
4668  3.51567  0.00107873
4771  4.56132  0.00130118
经过51次洗牌演化，种群已经收敛到一个预先指定的小参数空间
4870  4.96879  0.00063381
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6
参数初始值：5.3  8.9  -9.8  3  -28.4  18.9
参数下限：-30  -30  -30  -30  -30  -30
参数上限：30  30  30  30  30  30
初始参数函数值：6.38765e+07
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6
0.999812  0.998869  0.99768  0.996354  0.992386  0.984878
最优函数值:0.000244629
52次洗牌演化的最优点函数值：
4.92447e+06  1.55545e+06  97415  14517.8  1751.61  1675.6  552.29  388.118  388.118  352.653  332.229  325.96  308.055  254.639  217.94  176.974  141.917  114.649  59.3991  24.9424  16.3874  7.94885  5.51331  4.36767  3.91434  3.54638  3.21177  3.11098  2.67046  2.59218  2.14319  1.4774  1.4774  1.36785  1.23349  1.01523  0.768097  0.450236  0.311698  0.311698  0.277714  0.20127  0.150037  0.0937996  0.0723941  0.0369871  0.0324179  0.00721972  0.0030461  0.0030461  0.00195031  0.000244629
E:\Research\OptA\SCEUA\SCEUA\Debug\SCEUA.exe (进程 60092)已退出，代码为 0。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .


```

`sceout.txt`文件:



```
===================参数初始值及上下限===================
参数名：x1  x2  x3  x4  x5  x6  
参数初始值：5.3  8.9  -9.8  3  -28.4  18.9  
参数下限：-30  -30  -30  -30  -30  -30  
参数上限：30  30  30  30  30  30  
初始参数函数值：6.38765e+07
===================SCE-UA搜索的结果===================
最优点:
x1  x2  x3  x4  x5  x6  
0.999812  0.998869  0.99768  0.996354  0.992386  0.984878  
最优函数值:0.000244629
52次洗牌演化的最优点函数值：
4.92447e+06  1.55545e+06  97415  14517.8  1751.61  1675.6  552.29  388.118  388.118  352.653  332.229  325.96  308.055  254.639  217.94  176.974  141.917  114.649  59.3991  24.9424  16.3874  7.94885  5.51331  4.36767  3.91434  3.54638  3.21177  3.11098  2.67046  2.59218  2.14319  1.4774  1.4774  1.36785  1.23349  1.01523  0.768097  0.450236  0.311698  0.311698  0.277714  0.20127  0.150037  0.0937996  0.0723941  0.0369871  0.0324179  0.00721972  0.0030461  0.0030461  0.00195031  0.000244629  

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/53234a3f6aab4bad905ebd84db6ad6a1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


## 输入文件格式说明


### `scein.txt`


示例：



```
6
x1 5.3 -30 30
x2 8.9 -30 30
x3 -9.8 -30 30
x4 3 -30 30
x5 -28.4 -30 30
x6 18.9 -30 30
0

```

格式说明：


* 第一行数字`6`表示待优化参数个数；
* 以下六行为参数信息，每一行从左到右分别为参数名、参数初始值、参数下限、参数上限；
* 最后一行数字0表示SCE-UA算法控制参数采用默认值；
* 如果将最后一行数字设置为1，则可以继续在下面设置复形数量、子复形内点数等参数，具体参见代码`SCEUA.cpp`的`scein()`函数的`if (m_ideflt == true)`部分，按照顺序设置参数即可按照自己设置的参数优化目标函数。


## 源代码


### `optimize.cpp`



```

#include <iostream>

#include "SCEUA.h"

//测试SCEUA算法
int main()
{
	SCEUA optimization;

	optimization.Optimize();

	return 0;
}

```

### `SCEUA.h`



```
#pragma once

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
文件名: SCEUA.h
作者: 卢家波
单位：河海大学水文水资源学院
邮箱：lujiabo@hhu.edu.cn
版本：2021.11 创建 V1.0
版权: MIT
引用格式：卢家波，SCEUA算法C++实现. 南京：河海大学，2021.
 LU Jiabo, Shuffled Complex Evolution in C++. Nanjing:Hohai University, 2021.
参考文献：[1]段青云,SCEUA的原始Fortran代码,1992, https://shxy.hhu.edu.cn/2019/0904/c12296a195177/page.htm
 [2]L. Shawn Matott改编的C++代码,2009, https://github.com/MESH-Model/MESH\_Project\_Baker\_Creek/blob/7e0a7e588213916deb2b6c11589df0d132d9b310/Model/Ostrich/SCEUA.h
 [3]Van Hoey S改编的Python代码,2011
 [4]Mostapha Kalami Heris, Shuffled Complex Evolution in MATLAB (URL: https://yarpiz.com/80/ypea110-shuffled-complex-evolution), Yarpiz, 2015.
 [5]Duan, Q.Y., Gupta, V.K. & Sorooshian, S. Shuffled complex evolution approach for effective and efficient global minimization. J Optim Theory Appl 76, 501–521 (1993). https://doi.org/10.1007/BF00939380.
 [6]Duan, Q., Sorooshian, S., and Gupta, V. (1992), Effective and efficient global optimization for conceptual rainfall-runoff models, Water Resour. Res., 28( 4), 1015– 1031, https://doi.org/10.1029/91WR02985.
 [7]Duan, Q., Sorooshian, S., & Gupta, V. K. (1994). Optimal use of the SCE-UA global optimization method for calibrating watershed models. Journal of Hydrology, 158(3-4), 265-284. https://doi.org/10.1016/0022-1694(94)90057-4.
 [8]王书功. 水文模型参数估计方法及参数估计不确定性研究[M]. 河南:黄河水利出版社,2010.(https://book.douban.com/subject/5377630/)
 [9]王书功. 水文模型参数估计方法及参数估计不确定性研究[D]. 北京:中国科学院研究生院,2006.(https://jz.docin.com/p-87849994.html)

==========================================================================
考虑最小化问题

待优化参数信息:
 NOPT = 待优化的参数数量，问题维数，目标函数含有的变量数，简记为n
 XNAME(.) = 变量名
 A(.) = 初始参数集
 BL(.) = 参数下限
 BU(.) = 参数上限

SCE-UA算法控制参数：
 NGS = 初始种群中的复形数量，简记为p
 NPG = 每个复形中的点的个数，简记为m
 NPS = 子复形中的点数，简记为q
 ALPHA = CCE步骤4中 a-e 重复的次数，即复形洗牌前每个复形允许的进化步骤数
 BETA = CCE步骤3-5重复的次数
 NPT = 初始种群中的总点数(NPT=NGS\*NPG)，简记为s

收敛检查参数：
 MAXN = 优化终止前允许的最大试验次数
 KSTOP = 在终止优化之前，标准值必须改变给定百分比的洗牌循环数
 PCENTO = 给定数量的随机循环中，标准值必须改变的百分比
 PEPS = 给定数量的随机循环中，函数值最低变化率

标志变量:
 INIFLG = 标记是否将初始点包括在种群中
 = false, 不包括
 = true, 包括
 IPRINT = 用于控制每个洗牌循环后的打印输出的标志
 = false, 打印关于种群最佳点的信息
 = true, 打印关于种群每个点的信息
 IDEFLT = 是否采用默认参数值标志
 = false, 采用
 = true, 不采用

优化结果：
 BESTX(.,.) = 每次洗牌循环的最佳点
 BESTF(.) = 每次循环的最佳点函数值

收敛判据：
 ICALL = 模型调用次数
 TIMEOU = 最近KSTOP次数函数值变化率
 GNRNG = 参数范围的归一化几何平均值
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/

#include <vector>
#include <string>


class SCEUA
{
public:
	SCEUA();
	~SCEUA();

	//优化函数
	void Optimize();  


private:
	//===================封装SCE-UA算法===================
	void scemain(); 

	//===================输入优化参数===================
	void scein(std::string StrPath = "");  

	//===================输出优化结果===================
	void sceout(std::string StrPath = "");  
	
	//===================SCE-UA算法===================
	void sceua();  

	//2.[生成样本]在参数空间内生成一个含有NPT个点的初始集合 
	void GenerateSample(std::vector<std::vector<double>>& x, std::vector<double>& xf, int& icall);

	//3.[样本点排序]样本点按函数值升序排列
	void RankPoints(std::vector<std::vector<double>>& x, std::vector<double>& xf);

	//4.[划分复形群体]将样本点划分到ngs个复形中，每个复形含npg个点
	void Partition2Complexes(const int& k, const std::vector<std::vector<double>>& x, const std::vector<double>& xf, std::vector<std::vector<double>>& cx, std::vector<double>& cf);

	//5.[复形演化]根据竞争复形演化算法(CCE)，对每个复形进行独立地演化计算
	void cce(std::vector<std::vector<double>>& cx, std::vector<double>& cf, const std::vector<double>& xnstd, int& icall);

	//6.[复形洗牌]将所有复形内的点放回缓冲区D，按函数值升序排列
	void ShuffleComplexes(const int& k, std::vector<std::vector<double>>& x, std::vector<double>& xf, const std::vector<std::vector<double>>& cx, const std::vector<double>& cf);
		
	//7.[收敛判断]如果满足收敛判据，则算法终止，否则回到步骤4
	void CheckConvergence(const std::vector<std::vector<double>>& x, std::vector<double>& xnstd, const std::vector<double>& bound, const int& nloop, int& icall, double& timeou, double& gnrng);

	//===================小工具===================
	//在可行区域内生成一个新点，重载getpnt函数，分别实现均匀分布和正态分布
	void getpnt(std::vector<double>& snew);

	void getpnt(const std::vector<double>& xi, const std::vector<double>& std, std::vector<double>& snew);

	//按函数值的升序对一组点数组和对应的函数值数组进行排序
	void sort(std::vector< std::vector<double> >& x, std::vector<double>& xf);
		
	//检查试验点是否满足所有约束
	void chkcst(const std::vector<double>& xx, bool& ibound);
	
	//===================目标函数===================
	double functn(const int& nopt, const std::vector<double>& x);

	//===================成员变量===================
	
	//每个变量的参数名、初始值、下限、上限
	int m_nopt;  //待优化的参数数量，问题维数，目标函数含有的变量数，简记为n
	std::vector<std::string> m_xname; //变量名
	std::vector<double> m_a;   //参数初始值
	std::vector<double> m_bl;  //参数下限
	std::vector<double> m_bu;  //参数上限
	
	//SCE控制参数 
	int m_ngs;   //种群中的复形数量，简记为p
	int m_npg;   //每个复形中的点的个数，简记为m
	int m_nps;   //子复形即单纯形中的点数，简记为q
	int m_alpha;   //CCE步骤4中 a-e 重复的次数，即复形洗牌前每个复形允许的进化步骤数
	int m_beta;    //CCE步骤3-5重复的次数 
	int m_npt;   //种群中的总点数(NPT=NGS\*NPG)，简记为s

	//收敛判据参数
	int m_maxn;    //优化终止前允许的最大试验次数
	int m_kstop;   //在终止优化之前，计算函数值变化率的洗牌循环数
	double m_pcento;  //给定数量的随机循环中，函数值最低变化率
	double m_peps;    //判断所有参数收敛的标准

	//标志变量
	bool m_iniflg;  //标记是否将初始点包括在种群中
	bool m_iprint;  //用于控制每个洗牌循环后的打印输出的标志
	bool m_ideflt;  //采用默认值标志

	//存储结果
	std::vector< std::vector<double> > m_bestx; //每次洗牌循环的最佳点
	std::vector<double> m_bestf; //每次循环的最佳点函数值

	//存储收敛判据
	std::vector<int> m_icall;  //优化目标试验次数
	std::vector<double> m_timeou;  //最近kstop次洗牌循环中，函数值变化率
	std::vector<double> m_gnrng;  //参数范围的归一化几何平均值
	
};

```

### `SCEUA.cpp`



```
/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
文件名: SCEUA.cpp
作者: 卢家波
单位：河海大学水文水资源学院
邮箱：lujiabo@hhu.edu.cn
版本：2021.11 创建 V1.0
版权: MIT
引用格式：卢家波，SCEUA算法C++实现. 南京：河海大学，2021.
 LU Jiabo, Shuffled Complex Evolution in C++. Nanjing:Hohai University, 2021.
参考文献：[1]段青云,SCEUA的原始Fortran代码,1992, https://shxy.hhu.edu.cn/2019/0904/c12296a195177/page.htm
 [2]L. Shawn Matott改编的C++代码,2009, https://github.com/MESH-Model/MESH\_Project\_Baker\_Creek/blob/7e0a7e588213916deb2b6c11589df0d132d9b310/Model/Ostrich/SCEUA.h
 [3]Van Hoey S改编的Python代码,2011
 [4]Mostapha Kalami Heris, Shuffled Complex Evolution in MATLAB (URL: https://yarpiz.com/80/ypea110-shuffled-complex-evolution), Yarpiz, 2015.
 [5]Duan, Q.Y., Gupta, V.K. & Sorooshian, S. Shuffled complex evolution approach for effective and efficient global minimization. J Optim Theory Appl 76, 501–521 (1993). https://doi.org/10.1007/BF00939380.
 [6]Duan, Q., Sorooshian, S., and Gupta, V. (1992), Effective and efficient global optimization for conceptual rainfall-runoff models, Water Resour. Res., 28( 4), 1015– 1031, https://doi.org/10.1029/91WR02985.
 [7]Duan, Q., Sorooshian, S., & Gupta, V. K. (1994). Optimal use of the SCE-UA global optimization method for calibrating watershed models. Journal of Hydrology, 158(3-4), 265-284. https://doi.org/10.1016/0022-1694(94)90057-4.
 [8]王书功. 水文模型参数估计方法及参数估计不确定性研究[M]. 河南:黄河水利出版社,2010.(https://book.douban.com/subject/5377630/)
 [9]王书功. 水文模型参数估计方法及参数估计不确定性研究[D]. 北京:中国科学院研究生院,2006.(https://jz.docin.com/p-87849994.html)
 
==========================================================================
考虑最小化问题

待优化参数信息:
 NOPT = 待优化的参数数量，问题维数，目标函数含有的变量数，简记为n
 XNAME(.) = 变量名
 A(.) = 初始参数集
 BL(.) = 参数下限
 BU(.) = 参数上限
 
SCE-UA算法控制参数：
 NGS = 初始种群中的复形数量，简记为p
 NPG = 每个复形中的点的个数，简记为m 
 NPS = 子复形中的点数，简记为q
 ALPHA = CCE步骤4中 a-e 重复的次数，即复形洗牌前每个复形允许的进化步骤数
 BETA = CCE步骤3-5重复的次数 
 NPT = 初始种群中的总点数(NPT=NGS\*NPG)，简记为s

收敛检查参数：
 MAXN = 优化终止前允许的最大试验次数
 KSTOP = 在终止优化之前，标准值必须改变给定百分比的洗牌循环数
 PCENTO = 给定数量的随机循环中，标准值必须改变的百分比
 PEPS = 给定数量的随机循环中，函数值最低变化率

标志变量:
 INIFLG = 标记是否将初始点包括在种群中
 = false, 不包括
 = true, 包括
 IPRINT = 用于控制每个洗牌循环后的打印输出的标志
 = false, 打印关于种群最佳点的信息
 = true, 打印关于种群每个点的信息
 IDEFLT = 是否采用默认参数值标志
 = false, 采用
 = true, 不采用

优化结果：
 BESTX(.,.) = 每次洗牌循环的最佳点
 BESTF(.) = 每次循环的最佳点函数值

收敛判据：
 ICALL = 模型调用次数
 TIMEOU = 最近KSTOP次数函数值变化率
 GNRNG = 参数范围的归一化几何平均值
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/

#include <iostream>
#include <fstream>
#include <random>
#include <vector>
#include <algorithm>
#include <numeric>
#include <string>
#include <numbers> //使用圆周率常数

#include "SCEUA.h"

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Optimize()
使用 SCEUA 最小化目标函数
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::Optimize()
{
    scemain(); //执行SCE程序
}

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 scemain()
 执行SCE程序，调用scein()、sceua()、sceout()函数
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::scemain()
{
	scein();   //设置优化参数

	sceua();   //SCE-UA算法

	sceout();  //输出优化结果
}

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 scein()
 该函数读取用于全局优化的SCE方法的输入变量

 输入参数列表：
 StrPath = 文件路径，默认为空，即可执行程序所在路径
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::scein(std::string StrPath)
{
    //初始化 I/O 变量
    std::ifstream fin(StrPath + "scein.txt");
    
    //读取参数个数
    fin >> m_nopt;

    //创建参数名、初始值、上下限数组
    m_xname = std::vector<std::string>(m_nopt);
    m_a = std::vector<double>(m_nopt);
    m_bl = std::vector<double>(m_nopt);
    m_bu = std::vector<double>(m_nopt);

    //依次读取每个变量的参数名、初始值、下限、上限
    for (int i = 0; i < m_nopt; i++)
    {
        fin >> m_xname.at(i)  //读取变量名
            >> m_a.at(i)  //读取参数初始值
            >> m_bl.at(i)  //读取参数下限
            >> m_bu.at(i); //读取参数上限
    }

    //读取默认值标识
    fin >> m_ideflt;

    //如果IDEFLT等于1，接着读入SCE控制参数、收敛判据参数、标志变量
    if (m_ideflt == true)
    {
		fin >> m_ngs
			>> m_npg
			>> m_nps
            >> m_alpha
            >> m_beta

            >> m_maxn
			>> m_kstop
			>> m_pcento
            >> m_peps
 
            >> m_iniflg 
            >> m_iprint;
    }
    //否则将SCE控制参数设置为推荐值即默认值
    else
    {
        m_ngs = 5;

        m_npg = 2 \* m_nopt + 1;

        m_nps = m_nopt + 1;

        m_alpha = 1;

        m_beta = 2 \* m_nopt + 1;

        m_maxn = 5000;

        m_kstop = 10;

        m_pcento = 0.1;
        
        m_peps = 0.001;


        m_iniflg = true;

        m_iprint = false;
    }

    //计算初始种群中的总点数
    m_npt = m_ngs \* m_npg;

	//检查SCE控制参数是否有效


    fin.close();
    
}

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 sceout()
 输出SCE-UA算法运行结果
 
 输入参数列表：
 StrPath = 文件路径，默认为空，即可执行程序所在路径
 
 局部变量列表：
 fout = 文件输出句柄
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::sceout(std::string StrPath)
{   
    //=================1.控制台输出=================
	//打印初始点及其函数值
	std::cout << "===================参数初始值及上下限===================" << std::endl;

    std::cout << "参数名：";

	for (int i = 0; i < m_nopt; i++)
	{
		std::cout << m_xname[i] << " ";
	}

	std::cout << std::endl;

    std::cout << "参数初始值：";

	for (int i = 0; i < m_nopt; i++)
	{
		std::cout << m_a[i] << " ";
	}

	std::cout << std::endl;

    std::cout << "参数下限：";

	for (int i = 0; i < m_nopt; i++)
	{
		std::cout << m_bl[i] << " ";
	}

	std::cout << std::endl;

    std::cout << "参数上限：";

	for (int i = 0; i < m_nopt; i++)
	{
		std::cout << m_bu[i] << " ";
	}

	std::cout << std::endl;

	std::cout << "初始参数函数值：" << functn(m_nopt, m_a) << std::endl;

	//打印优化结果
    std::cout << "===================SCE-UA搜索的结果===================" << std::endl;

    std::cout << "最优点:"  << std::endl;

	for (int i = 0; i < m_nopt; i++)
	{
		std::cout << m_xname[i] << " ";
	}

    std::cout << std::endl;

    for (auto i : m_bestx.back())
    {
        std::cout << i << " ";
    }

	std::cout << std::endl;

    std::cout << "最优函数值:" << m_bestf.back() << std::endl;

    std::cout << m_bestf.size() << "次洗牌演化的最优点函数值：" << std::endl;
	
    for (auto i : m_bestf)
	{
		std::cout << i << " ";
	}

    //=================2.文件输出=================
    std::ofstream fout(StrPath + "sceout.txt");  //创建输出文件
    
	//打印初始点及其函数值
    fout << "===================参数初始值及上下限===================" << std::endl;

    fout << "参数名：";

	for (int i = 0; i < m_nopt; i++)
	{
        fout << m_xname[i] << " ";
	}

    fout << std::endl;

    fout << "参数初始值：";

	for (int i = 0; i < m_nopt; i++)
	{
        fout << m_a[i] << " ";
	}

    fout << std::endl;

    fout << "参数下限：";

	for (int i = 0; i < m_nopt; i++)
	{
        fout << m_bl[i] << " ";
	}

    fout << std::endl;

    fout << "参数上限：";

	for (int i = 0; i < m_nopt; i++)
	{
        fout << m_bu[i] << " ";
	}

    fout << std::endl;

    fout << "初始参数函数值：" << functn(m_nopt, m_a) << std::endl;

	//打印优化结果
    fout << "===================SCE-UA搜索的结果===================" << std::endl;

    fout << "最优点:" << std::endl;

	for (int i = 0; i < m_nopt; i++)
	{
        fout << m_xname[i] << " ";
	}

    fout << std::endl;

	for (auto i : m_bestx.back())
	{
        fout << i << " ";
	}

    fout << std::endl;

    fout << "最优函数值:" << m_bestf.back() << std::endl;

    fout << m_bestf.size() << "次洗牌演化的最优点函数值：" << std::endl;

	for (auto i : m_bestf)
	{
        fout << i << " ";
	}

    fout.close();  //关闭输出文件
}


/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 sceua()
 该算法对目标函数最小化问题进行全局优化搜索

 局部变量列表：
 X(.,.) = 种群中点的坐标
 XF(.) = X(.,.)的函数值
 CX(.,.) = 复形中点的坐标
 CF(.) = CX(.,.)的函数值
 XNSTD(.) = 种群中参数的标准差
 BOUND(.) = 优化变量的约束
 ICALL = 模型调用次数
 TIMEOU = 最近kstop次数函数值变化率
 GNRNG = 参数范围的归一化几何平均值
 NLOOP = 主循环次数 
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::sceua()
{   
	std::cout << "==================================================" << std::endl
		<< " 进入SCE-UA全局搜索 " << std::endl
		<< "==================================================" << std::endl;

    //===========1.[初始化]定义局部变量及数组，并分配内存=========== 
       
    std::vector< std::vector<double> > x(m_npt, std::vector<double>(m_nopt));  //种群中点的坐标
    
    std::vector<double> xf(m_npt);  //种群中点的坐标X(.,.)的函数值
	    
	std::vector< std::vector<double> > cx(m_npg, std::vector<double>(m_nopt)); //复形中点的坐标
    
    std::vector<double> cf(m_npg);  //复形中点的坐标CX(.,.)的函数值
       
    std::vector<double> xnstd(m_nopt, 0.1);   //种群中参数的标准差，初始化为0.1 
    
    std::vector<double> bound(m_nopt);   //优化变量的约束范围 
	
    int icall = 0;  //模型调用次数

    double timeou = 10000.0;  //最近kstop次洗牌循环中函数值变化率，默认为一较大数值

    double gnrng = 10000.0; //参数范围的归一化几何平均值，默认为一较大数值

	int nloop = 0;  //主循环次数 
   
	for (int j = 0; j < m_nopt; j++)
	{
		bound[j] = m_bu[j] - m_bl[j];  //计算待优化参数约束范围
	}
	
    //===========2.[生成样本]在参数空间内生成一个含有NPT个点的初始集合===========
    GenerateSample(x, xf, icall);

    //===========3.[样本点排序]样本点按函数值升序排列===========
    RankPoints(x, xf);

    std::cout << "目标函数调用次数" << " " <<  "函数值变化率" << " " << "参数变化范围" << std::endl;

    //===========7.[收敛判断]不满足收敛判据，返回步骤4===========
    while ((icall < m_maxn) && (timeou > m_pcento) && (gnrng > m_peps))
    {
        nloop += 1;  //主循环次数计数

		//对每个复形进行独立演化计算，并放回到缓冲区D中
		for (int k = 0; k < m_ngs; k++)
		{
			//===========4.[划分复形群体]将样本点划分到p个复形中，每个复形含m个点===========
			Partition2Complexes(k, x, xf, cx, cf);

			//===========5.[复形演化]根据CCE对每个复形独立演化计算===========
			cce(cx, cf, xnstd, icall);

			//===========6.[复形洗牌]将所有复形内的点放回缓冲区D===========
			ShuffleComplexes(k, x, xf, cx, cf);
		}

		//===========3.[样本点排序]将所有复形内的点按函数值升序排列===========
		RankPoints(x, xf);

		//===========7.[收敛判断]如果满足收敛判据，则算法终止，否则回到步骤4===========
        CheckConvergence(x, xnstd, bound, nloop, icall, timeou, gnrng);

        std::cout << icall << " " << timeou << " " << gnrng << std::endl;
    }
       
}


/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 GenerateSample()
 2.[生成样本]在参数空间内生成一个含有NPT个点的初始集合

 输入参数列表： 
 X(.,.) = 种群中点的坐标
 XF(.) = X(.,.)的函数值
 ICALL = 模型调用次数

 局部变量列表：
 XX(.) = 种群中点X(.,.)中单点的坐标
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::GenerateSample(        
    std::vector< std::vector<double> >& x,
    std::vector<double>& xf,
    int& icall
)
{
	std::vector<double> xx(m_nopt);   //种群中点X(.,.)中单点的坐标

	//生成在参数空间均匀分布的NPT个随机点
	for (int i = 0; i < m_npt; i++)
	{
		//在参数空间内按照均匀分布随机生成点
		getpnt(xx);

		//将生成的点赋值给种群
		x.at(i) = xx;
	}

	//如果INIFLG等于true，设置X(0,.)为初始点A(.)，将初始点包含在初始集合中
	if (m_iniflg == true)
	{
		x.front() = m_a;
	}

    //计算NPT个随机点对应的函数值
    for (int i = 0; i < m_npt; i++)
    {
        xf.at(i) = functn(m_nopt, x.at(i));  //计算生成点的函数值

        icall += 1; //模型调用次数增加1
    }
}

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 RankPoints()
 3.[样本点排序]样本点按函数值升序排列

 输入参数列表：
 X(.,.) = 种群中点的坐标
 XF(.) = X(.,.)的函数值
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::RankPoints(	
	std::vector< std::vector<double> >& x,
    std::vector<double>& xf
)
{
	sort(x, xf);  //根据目标函数值将样本点按从小到大排序

	//记录最好的点

	m_bestx.push\_back(x.front());  //最好点的坐标，即第一个点

	m_bestf.push\_back(xf.front());  //最好点的函数值，即第一个点的函数值
}


/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 Partition2Complexes()
 4.[划分复形群体]将样本点划分到ngs个复形中，每个复形含npg个点

 输入参数列表：
 K = 复形的序号，K = 1,2,···,ngs
 X(.,.) = 种群中点的坐标
 XF(.) = X(.,.)的函数值
 CX(.,.) = 复形中点的坐标
 CF(.) = 复形中点的坐标CX(.,.)的函数值

 局部变量列表：
 INDEX = 按照规则（k + ngs \* j）从种群中提取点到复形中的下标
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::Partition2Complexes
(
    const int& k,  
    const std::vector< std::vector<double> >& x,
    const std::vector<double>& xf,
    std::vector< std::vector<double> >& cx,
    std::vector<double>& cf
)
{
    int index = 0;   //种群的下标

    //遍历复形中的每个点，为其从种群中赋值
    for (int j = 0; j < m_npg; j++)
    {
        index = k + m_ngs \* j;   //计算下标

        cx.at(j) = x.at(index);   //划分样本点到复形

        cf.at(j) = xf.at(index);  //划分对应的函数值
    }
}

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 cce()
 5.[复形演化]根据CCE对每个复形独立演化计算

 输入参数列表：
 CX(.,.) = 复形中点的坐标
 CF(.) = 复形中点的坐标CX(.,.)的函数值
 XNSTD(.) = 种群中参数的归一化标准差
 ICALL = 模型调用次数

 局部变量列表：
 s(.,.) = 当前单纯形中点的坐标
 sf(.) = 当前单纯形中点的坐标S(.,.)的函数值
 sb(.) = 单纯形的最佳点
 sw(.) = 单纯形的最坏点WO（WORST POINT，简称WO）
 ce(.) = 单纯形排除最坏点（WO）的形心
 snew(.) = 从单纯形生成的新点
 par(.) = 除最差点以外的参数取值集合
 lcs(.) = 索引定位S(.,.)在 CX(.,.)中的位置
 wts = 复形中每个点的权重,weights
 vals = 经过均匀随机扰动的复形中每个点的权重
 valsWithIndices = 权重及下标索引
 gen = 随机数引擎
 u = 产生[0,1)均匀分布随机数
 fw = 最差点的函数值
 fnew = 新点SNEW的函数值 
 ibound = 违反约束指标
 = false, 没有违反
 = true, 违反
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::cce
(
	std::vector< std::vector<double> >& cx,
	std::vector<double>& cf,
    const std::vector<double>& xnstd,  
    int& icall
)
{
    //===========1.[初始化]定义局部变量=========== 
    
    //定义数组并分配内存 
	std::vector< std::vector<double> > s(m_nps, std::vector<double>(m_nopt)); //当前单纯形中点的坐标
    
    std::vector<double> sf(m_nps);  //当前单纯形中点的坐标S(.,.)的函数值
    
    std::vector<double> sb(m_nopt); //子复形（即单纯形）的最佳点，其函数值最小
    
    std::vector<double> sw(m_nopt); //子复形（即单纯形）的最差点，其函数值最大
    
    std::vector<double> ce(m_nopt); //子复形（即单纯形）排除最坏点（WO）的形心
    
    std::vector<double> snew(m_nopt); //从子复形（即单纯形）生成的新点
    
    std::vector<double> par(m_nps - 1); //除最差点以外的参数取值集合
    
    std::vector<int> lcs(m_nps);   //索引定位S(.,.)在 CX(.,.)中的位置
    
    std::vector<int> wts(m_npg); //用于存储复形中每个点的权重
    
    std::vector<double> vals;  //用于存储经过随机扰动的复形中每个点的权重
    
    std::vector<std::pair<int, double>> valsWithIndices; //存储下标索引及其权重
	
	auto gen = std::mt19937{ std::random_device{}() }; //随机数引擎采用设备熵值保证随机性
	
	std::uniform_real_distribution<double> u(0.0, 1.0); //产生[0,1)均匀分布随机数

    double fw = 0.0;   //最差点的函数值
    
    double fnew = 0.0;   //新点SNEW的函数值
    
    bool ibound = true;  //违反约束指标，默认违反

    //===========2.[分配权重]根据三角概率分布，为复形内每个点分配概率===========
    //pi=2(m+1-i)/m(m+1),i=1,2,...,m 由于2/m(m+1)相同，故可以省略
    //各下标索引的权重为整数 m+1-i
    //权重赋值，这里的 i=0,1,...,m-1
    //例：npg=5即m=5时，权重依次为5,4,3,2,1
    for (int i = 0; i < m_npg; i++)
    {
        wts.at(i) = m_npg - i;
    }    
        
    //===========6.[迭代]重复步骤 3-5 beta次===========
    for (int ibeta = 0; ibeta < m_beta; ibeta++)
    {
        //===========3.[选择父辈群体]从复形中加权不放回抽取q个点===========
        //并将q个点及其函数值存储于缓冲区B中

        //清空权重数组为排序做准备
        vals.clear();
        valsWithIndices.clear();

        //把权重值作为指数，均匀分布随机数作为底
        //相当于给权重增加了均匀随机扰动
        for (auto iter : wts)
        {
            vals.push\_back(std::pow(u(gen), 1.0 / iter));
        }

        //构成<下标索引,权重>对的数组
        for (int i = 0; i < m_npg; i++)
        {
            valsWithIndices.emplace\_back(i, vals[i]);
        }

        //按照扰动后的权重值从大到小排序，并扩展到下标索引
        std::sort(valsWithIndices.begin(), valsWithIndices.end(), [](auto x, auto y) {return x.second > y.second; });

        //按照扰动后的权重值从大到小抽样q个点，记录下标索引到lcs数组中
        for (int i = 0; i < m_nps; i++)
        {
            lcs.at(i) = valsWithIndices[i].first;
        }

        //对抽取到的q个点的下标按照从小到大升序排列
        std::sort(lcs.begin(), lcs.end());

        //由抽取到的q个点构成单纯形，记录单纯形中点的坐标及函数值
        for (int i = 0; i < m_nps; i++)
        {
            s.at(i) = cx.at(lcs.at(i));  //从复形中取单纯形中点的坐标

            sf.at(i) = cf.at(lcs.at(i));  //从复形函数值数组中取单纯形中点的函数值
        }


        //===========4.[产生下一代群体]按照下山单纯形法的6步对q个点演化计算,重复alpha次===========
        /\* --------------------------------------------------
 确定子复形S的最坏点WO
 计算剩余点的形心CE（CENTROID，简称CE）
 计算步长，最坏点WO和质心CE之间的向量
 确定最坏的函数值FW（The worst function value，简称FW）
 --------------------------------------------------- \*/

        for (int ialpha = 0; ialpha < m_alpha; ialpha++)
        {

            sb = s.front(); //复制最佳点坐标

            sw = s.back();  //复制最差点坐标

            fw = sf.back();  //最坏的函数值是单纯形S的最后一个点，函数值最大

            //遍历当前单纯形中点的坐标
            for (int j = 0; j < m_nopt; j++)
            {
                for (int i = 0; i < m_nps - 1; i++)
                {
                    par[i] = s[i][j];   //将除最差点以外的参数取值赋值给par数组
                }

                //a.[形心]计算除最差点以外nps-1个点的平均值，即为形心ce
                ce[j] = std::accumulate(par.begin(), par.end(), 0.0) / double(m_nps - 1);
            }

            //计算新点SNEW
            //b.[反射]首先尝试反射步骤，计算最差点的反射点r=2g-uq,
            //这里符号记为snew=2ce-sw
            for (int j = 0; j < m_nopt; j++)
            {
                snew[j] = 2 \* ce[j] - sw[j];
            }

            //检查SNEW是否违反约束,即是否在可行域内
            chkcst(snew, ibound);

            /\* ---------------------------------------------------
 c.[突变]如果SNEW在界外，
 按正态分布在可行区域内随机选择一个点，
 子复形的最佳点作为种群的均值
 标准差作为STD
 ----------------------------------------------------- \*/
            if (ibound == true)
            {
                //产生一个正态分布的满足约束的新点
                getpnt(sb, xnstd, snew);
            }

            //计算SNEW点的函数值FNEW
            fnew = functn(m_nopt, snew);
            icall += 1; //模型调用次数增加1

            //将FNEW与最差函数值FW进行比较
            //如果FNEW小于FW，接受新点SNEW并返回
            if (fnew < fw)
            {
                //用新点替换最坏点
                s.back() = snew;

                sf.back() = fnew;
            }
            //d.[收缩]否则计算新点snew=(g+sw)/2和fnew,若fnew<fw,以snew代替最差点sw
            else
            {
                for (int j = 0; j < m_nopt; j++)
                {
                    snew[j] = (ce[j] + sw[j]) / 2.0;  //即为snew = (ce+sw)/2
                }
                //计算新点SNEW的函数值FNEW
                fnew = functn(m_nopt, snew);
                icall += 1; //模型调用次数增加1

                //如果收缩成功，fnew小于fw，接受新点snew，并替换sw
                if (fnew < fw)
                {
                    //用新点替换最坏点
                    s.back() = snew;

                    sf.back() = fnew;
                }
                //e.[突变]此时反射和收缩都失败，
                //则根据正态分布随机生成新点snew,以snew代替sw
                //子复形的最佳点作为种群的平均值并且标准偏差作为STD
                else
                {
                    //产生一个正态分布的新点
                    getpnt(sb, xnstd, snew);

                    //计算新点SNEW的函数值FNEW
                    fnew = functn(m_nopt, snew);
                    icall += 1; //模型调用次数增加1

                    //用新点替换最坏点
                    s.back() = snew;

                    sf.back() = fnew;
                }
            }

            //按照函数值对单纯形中的点从小到大排序
            sort(s, sf);
        }

        //===========5.[以子代取代父代群体]将q个点按照位置放回复形，升序排列===========
        for (int i = 0; i < m_nps; i++)
        {
            cx.at(lcs.at(i)) = s.at(i);

            cf.at(lcs.at(i)) = sf.at(i);
        }

        sort(cx, cf);  //按照函数值升序排列
    }    
}  //cce 结束

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 ShuffleComplexes()
 6.[复形洗牌]将所有复形内的点放回缓冲区D

 输入参数列表：
 K = 复形的序号，K = 1,2,...,ngs
 X(.,.) = 种群中点的坐标
 XF(.) = X(.,.)的函数值
 CX(.,.) = 复形中点的坐标
 CF(.) = 复形中点的坐标CX(.,.)的函数值

 局部变量列表：
 INDEX = 按照规则（k + ngs \* j）从复形中提取点到种群中的下标
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::ShuffleComplexes
(
	const int& k,
	std::vector< std::vector<double> >& x,
	std::vector<double>& xf,
	const std::vector< std::vector<double> >& cx,
	const std::vector<double>& cf
)
{
	int index = 0;   //种群的下标

    //遍历复形中的每个点，将其值赋给种群
	for (int j = 0; j < m_npg; j++)
	{
		index = k + m_ngs \* j;   //计算下标

		x.at(index) = cx.at(j);   //样本点从复形赋值到种群

		xf.at(index) = cf.at(j);  //函数值从复形赋值到种群
	}
}

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 CheckConvergence()
 7.[收敛判断]如果满足收敛判据，则算法终止，否则回到步骤4

 输入参数列表：
 X(.,.) = 种群中所有点的坐标
 XNSTD(.) = 种群中参数的归一化标准差
 BOUND(.) = 优化变量的约束
 NLOOP = 主循环次数
 ICALL = 模型调用次数 
 TIMEOU = 最近kstop次数函数值变化率
 GNRNG = 参数范围的归一化几何平均值

 局部变量列表：
 denomi = 分母denominator
 xmax(.) = 各个维度参数值的最大值
 xmin(.) = 各个维度参数值的最小值
 xmean(.) = 各个维度参数值的平均值
 par(.) = 某个参数的所有取值集合
 delta = 极小值，防止log真值为0
 parsum = 某个参数值总和
 parsum2 = 某个参数的平方和
 gsum = 所有参数的几何平均值
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::CheckConvergence
(
	const std::vector< std::vector<double> >& x, //种群中所有点的坐标
	std::vector<double>& xnstd,  //种群中参数的归一化标准差
	const std::vector<double>& bound,  //优化变量的约束 
    const int& nloop,
    int& icall,
    double& timeou,
    double& gnrng  
)
{
    //==========1.检查是否超过了函数评估次数的最大值==========
	if (icall > m_maxn)
	{
		//搜索终止
		std::cout << "经过" << nloop << "次洗牌演化，"
            << "优化搜索已经终止，因为超过了最大试验次数" << m_maxn << "次的限制" << std::endl;
	}

    //==========2.检查无函数值改进的连续循环次数==========

    double denomi = 0.0;    //分母denominator

    if (nloop >= m_kstop)
    {
        //计算分母denominator，最近kstop次数的函数值绝对值的平均值
		denomi = std::fabs(std::accumulate(m_bestf.end() - m_kstop, m_bestf.end(), 0.0)) / double(m_kstop);
		
        //最近kstop次数函数值变化率
        timeou = fabs(m_bestf.at(nloop - 1) - m_bestf.at(nloop - m_kstop)) / denomi;
        
        //如果变化率小于pcento，则已经收敛
		if (timeou < m_pcento)
		{
			std::cout << "最佳点在最近" << m_kstop << 
                "次循环中函数值变化率小于阈值" << m_pcento << std::endl;

            std::cout << "经过" << nloop << "次洗牌演化，"
                << "基于目标函数标准已经实现了收敛！" << std::endl;
		}
    }
    
    //==========3.检查种群是否聚集到一个足够小的空间==========

	std::vector<double> xmax(m_nopt);  //各个维度参数值的最大值
	std::vector<double> xmin(m_nopt);  //各个维度参数值的最小值
	std::vector<double> xmean(m_nopt); //各个维度参数值的平均值
	std::vector<double> par(m_npt); //某个参数的所有取值集合

	const double delta = 1.0e-20;   //极小值，防止log真值为0

	//计算参数值的最大值、最小值和标准差
	double parsum = 0.0; //某个参数值总和
	double parsum2 = 0.0; //某个参数的平方和
	double gsum = 0.0;  //所有参数的几何平均值

	for (int k = 0; k < m_nopt; k++)
	{

		for (int i = 0; i < m_npt; i++)
		{
			par[i] = x[i][k];  //存储某个参数的所有取值
		}

		//计算参数值的最大值、最小值、总和、平均值、标准差、归一化标准差
		xmax[k] = \*std::max\_element(par.begin(), par.end()); //最大值

		xmin[k] = \*std::min\_element(par.begin(), par.end()); //最小值

		parsum = std::accumulate(par.begin(), par.end(), 0.0); //总和

		xmean[k] = parsum / double(m_npt);  //平均值

        parsum2 = std::inner\_product(par.begin(), par.end(), par.begin(), 0.0);  //计算平方和sum(xi^2)

		//根据方差的性质3：Var(x) = E(x^2) - E(x)^2，期望平方内减外
		xnstd[k] = sqrt(parsum2 / double(m_npt) - pow(xmean[k], 2)); //标准差
		xnstd[k] = xnstd[k] / bound[k];  //归一化标准差

		//对每个参数的几何平均值求和
		gsum += log(delta + (xmax[k] - xmin[k]) / bound[k]);
	}

	gnrng = exp(gsum / double(m_nopt));   //种群参数变化范围的相对值

	if (gnrng < m_peps)
	{
        std::cout << "经过" << nloop << "次洗牌演化，"
            << "种群已经收敛到一个预先指定的小参数空间" << std::endl;
	}
  
    //==========4.存储每次洗牌循环的收敛判据==========

	m_icall.push\_back(icall);  //记录优化目标试验次数

	m_timeou.push\_back(timeou);  //记录最近kstop次洗牌循环中，函数值变化率

	m_gnrng.push\_back(gnrng);  //记录参数范围的归一化几何平均值
}


/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 getpnt()
 在可行区域内根据均匀分布生成一个新点

 输入参数列表：
 SNEW(.) = 产生的新点

 局部变量列表：
 gen = 随机引擎
 ibound = 违反约束指标
 = false, 没有违反
 = true, 违反
 u = 均匀分布
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::getpnt(std::vector<double>& snew) 
{
	
    //随机数引擎采用设备熵值保证随机性
	auto gen = std::mt19937{ std::random_device{}() };

    //违反约束指标，默认违反
    bool ibound = true; 

    while (ibound == true)
    {
        //根据要求产生新点
		for (int j = 0; j < m_nopt; j++)
		{
			//产生均匀分布随机数
			std::uniform_real_distribution<double> u(m_bl[j], m_bu[j]);

			snew[j] = u(gen);
		}

        //显性和隐性检查
        chkcst(snew, ibound);
    }
}

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 getpnt()
 在可行区域内根据正态分布生成一个新点

 输入参数列表：
 SNEW(.) = 产生的新点
 std(.) = 概率分布的标准差
 xi(.) = 焦点

 局部变量列表：
 gen = 随机引擎
 ibound = 违反约束指标
 = false, 没有违反
 = true, 违反
 n = 正态分布
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::getpnt
(
    const std::vector<double>& xi,   //焦点
	const std::vector<double>& std,  //概率分布的标准差
	std::vector<double>& snew  //产生的新点
)
{	
    //随机数引擎采用设备熵值保证随机性
	auto gen = std::mt19937{ std::random_device{}() };

	//显性和隐性检查
	bool ibound = true; //违反约束指标，默认违反

	while (ibound == true)
	{
		//根据要求产生新点
		for (int j = 0; j < m_nopt; j++)
		{

			//产生正态分布随机数
            std::normal_distribution<double> n{ xi[j], std[j] };

			snew[j] = n(gen);

		}

		//显性和隐性检查
		chkcst(snew, ibound);
	}
}



/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 sort()
 按函数值的升序对一组点数组和对应的函数值数组进行排序
 
 输入参数列表：
 X(.,.) = 种群中所有点的坐标
 XF(.) = X(.,.)的函数值

 局部变量列表：
 XXF(.) = 存储坐标和函数值对的数组
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::sort
(
    std::vector< std::vector<double> >& x, 
    std::vector<double>& xf  
)
{
    //将点的坐标与函数值对应上，以便于扩展排序
    std::vector< std::pair<std::vector<double>, double> > xxf;
	
    //构造向量xxf，存储坐标和函数值对
    for (size_t i = 0; i < xf.size(); i++)
    {
        xxf.emplace\_back(x[i], xf[i]);
    }

    //匿名函数定义比较规则，用于pair数据结构按照函数值升序排列
    std::sort(xxf.begin(), xxf.end(), [](auto a, auto b) { return a.second < b.second; });
    
    //将排序后的xxf赋值给点数组x和函数值数组xf
    for (size_t i = 0; i < xxf.size(); i++)
    {
        x[i] = xxf[i].first;  //取出点坐标

        xf[i] = xxf[i].second;   //取出函数值
    }
}

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 chkcst()
 检查试验点是否满足所有约束

 输入参数列表：
 XX(.) = X 中单点的坐标
 IBOUND = 违反约束指标
 = false, 没有违反
 = true, 违反
 
 局部变量列表：
 i = 数组 x, bl 和 bu 的第i个变量
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void SCEUA::chkcst
(
    const std::vector<double>& xx, //X 中单点的坐标
    bool& ibound  //违反约束指标
)
{
	ibound = true;   //默认违反约束

	//检查是否违反了显式边界约束
	for (int i = 0; i < m_nopt; i++)
	{
        if ((xx[i] < m_bl[i]) || (xx[i] > m_bu[i]))
		{
            // 至少违反了一个约束
            return;  //终止函数运行
		}
	}
   
	// 检查是否违反了隐式约束
    // 此处用于填写隐式约束
	// 此函数没有隐式约束
	


	// 通过上述检查，表明没有违反约束
	ibound = false;   
}

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 functn()
 计算目标函数值

 输入参数列表：
 NOPT = 待优化的参数数量，问题维数，目标函数含有的变量数，简记为n
 X(.) = 单点的坐标，即各参数构成的数组
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
double SCEUA::functn(const int& nopt, const std::vector<double>& x)
{
    //================测试函数================
    //1. Rastrigin 函数
    //x in [-5.12, 5.12]
    //double sum = 0.0;

    //for (int i = 0; i < nopt; i++)
    //{
    // sum += pow(x.at(i), 2) - 10 \* cos(2 \* std::numbers::pi \* x.at(i));
    //}

    //sum += 10.0 \* nopt;

    //return sum;

    //2. Schaffer 函数
    // x in [-100, 100]
	//double sum = 0.0;

	//double temp = 0.0;

	//for (int i = 0; i < nopt; i++)
	//{
	// temp += pow(x.at(i), 2);
	//}

	//sum = 0.5 + (pow(sin(temp), 2) - 0.5) / pow(1.0 + 0.001 \* temp, 2);

	//return sum;

    //3. Ackley函数
    // x in [-32.768, 32.768]
    //double sum = 0.0;

    //int a = 20;

    //double b = 0.2;

    //double c = 2 \* std::numbers::pi;

    //double temp1 = 0.0;

    //double temp2 = 0.0;

    //for (int i = 0; i < nopt; i++)
    //{
    // temp1 += pow(x.at(i), 2);

    // temp2 += pow(cos(c \* x.at(i)), 2);
    //}

    //sum = -a \* exp(-b \* sqrt(temp1 / double(nopt))) - exp(temp2 / double(nopt)) + a + std::numbers::e;

    //return sum;

    //4. Griewank 函数
    // x in [-600, 600]
    //double sum = 0.0;

    //double temp1 = 0.0;

    //double temp2 = 1.0;

    //for (size\_t i = 0; i < nopt; i++)
    //{
    // temp1 += pow(x.at(i), 2) / 4000.0;

    // temp2 \*= cos(x.at(i) / double(sqrt(i + 1)));
    //}

    //sum = temp1 - temp2 + 1;

    //return sum;

    //5. Rosenbrock 函数
    // x in [-30, 30]
	double sum = 0.0;

	for (int i = 0; i < nopt - 1; i++)
	{
		sum += 100.0 \* pow(x.at(i + 1) - pow(x.at(i), 2), 2) + pow(x.at(i) - 1.0, 2);
	}

	return sum;

}

//构造函数
SCEUA::SCEUA()
{
    m_nopt = 5;
    m_xname = std::vector<std::string>{ "x1", "x2", "x3", "x4", "x5" };
    m_a = std::vector<double>{ 0, 0, 0, 0, 0 };
    m_bl = std::vector<double>{ -2, -2, -2, -2, -2 };
    m_bu = std::vector<double>{ 2, 2, 2, 2, 2 };

	m_ngs = 5;
	m_npg = 2 \* m_nopt + 1;
	m_nps = m_nopt + 1;
	m_alpha = 1;
	m_beta = 2 \* m_nopt + 1;
    m_npt = m_ngs \* m_npg;

	m_maxn = 3000;
	m_kstop = 10;
	m_pcento = 0.1;
	m_peps = 0.001;

	m_iniflg = true;
	m_iprint = false;
    m_ideflt = false;
}

//析构函数
SCEUA::~SCEUA()
{
}

```

## 下载地址


您可以直接复制本文源代码按照说明构建项目运行程序。


如果您愿意支持本人的研究，也可以通过点击此链接[优化算法 SCEUA算法C++实现 附Python、MATLAB、Fortran代码](https://download.csdn.net/download/weixin_43012724/72372688)下载已经构建完成的项目文件和参考代码等，并附有使用说明，感谢支持与信任！


**感谢您看到这里，如有问题，欢迎交流探讨！** 邮箱：lujiabo@hhu.edu.cn 卢家波  
 来信请说明博客标题及链接，谢谢。





