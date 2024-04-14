---
layout: page
permalink: /blogs/mcmc/index.html
title: mcmc
---






## 目标


2022/4/17-2022/5/10


实现自适应的MCMC方法（Adaptive Metropolis Algorithm）


本地目录：`E:\Research\OptA\MCMC`


**如有问题，欢迎交流探讨！** 邮箱：lujiabo@hhu.edu.cn 卢家波  
 来信请说明博客标题及链接，谢谢。


## MCMC简介


MCMC方法是基于贝叶斯理论框架，通过建立平衡分布为 

 


 π 


 ( 


 x 


 ) 

 

 \pi(x) 


 π(x)的马尔可夫链，并对其平衡分布进行采样，通过不断更新样本信息而使马尔可夫链能充分搜索模型参数空间，最终收敛于高概率密度区，因此，MCMC方法是对理想的贝叶斯推断过程的一种近似。MCMC方法的关键是如何构造有效的推荐分布，确保按照推荐分布抽取的样本收敛于高概率密度区。


MCMC方法论为建立实际的统计模型提供了一种有效工具，能将一些复杂的高维问题转化为一系列简单的低维问题，因此适用于复杂统计模型的贝叶斯计算。目前，在贝叶斯分析中应用最为广泛的MCMC方法主要基于Gibbs采样方法和Metropolis-Hastings 方法[2,8-9]。在此基础上，后人对这些方法开展了进一步的改进和推广研究，Haario等人(2001)[1]提出了自适应的MCMC方法（Adaptive Metropolis Algorithm），其主要特点是将推荐分布定义为参数空间多维正态分布，在进化过程中自适应地调整协方差矩阵，从而大大提高算法的收敛速度。


![在这里插入图片描述](https://img-blog.csdnimg.cn/f644b16a86eb4ecfa11a5134f3310410.jpeg#pic_center)


## 模型描述


### 线性模型


利用AM-MCMC估计线性回归参数，模型如下：



 

 

 y 


 = 


 k 


 x 


 + 


 b 

 

 y=kx+b 


 y=kx+b


![在这里插入图片描述](https://img-blog.csdnimg.cn/529f25f5f1ba432d9533cbc291559840.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


### 参数


进行AM-MCMC不确定性估计的2个参数为  

 


 k 

 

 k 


 k 和  

 


 b 

 

 b 


 b




| 参数名 | 含义 | 真值 | 下限 | 上限 |
| --- | --- | --- | --- | --- |
|  

 


 k 

 

 k 


 k | 斜率 | 2 | -3 | 3 |
|  

 


 b 

 

 b 


 b | 截距 | -1 | -3 | 3 |


### 实测值


实测值取-5到5之间的整数对应的函数值，每个点增加正态随机数N(0, 0.16)扰动，即



 

 

 y 


 = 


 − 


 11 


 , 


 − 


 9 


 , 


 − 


 7 


 , 


 − 


 5 


 , 


 − 


 3 


 , 


 − 


 1 


 , 


 1 


 , 


 3 


 , 


 5 


 , 


 7 


 , 


 9 


 , 


 x 


 ∈ 


 [ 


 − 


 5 


 , 


 5 


 ] 


 → 

 

 y=-11,-9,-7,-5, -3, -1, 1, 3, 5, 7, 9, x\in[-5,5]\rightarrow 


 y=−11,−9,−7,−5,−3,−1,1,3,5,7,9,x∈[−5,5]→



 

 

 y 


 = 


 − 


 10.79 


 , 


 − 


 9.4 


 , 


 − 


 6.77 


 , 


 − 


 5.3 


 , 


 − 


 3.52 


 , 


 − 


 0.97 


 , 


 1.3 


 , 


 3.13 


 , 


 5.16 


 , 


 7.56 


 , 


 8.8 


 , 


 x 


 ∈ 


 [ 


 − 


 5 


 , 


 5 


 ] 

 

 y=-10.79, -9.4, -6.77, -5.3, -3.52, -0.97, 1.3, 3.13, 5.16, 7.56, 8.8,x\in[-5,5] 


 y=−10.79,−9.4,−6.77,−5.3,−3.52,−0.97,1.3,3.13,5.16,7.56,8.8,x∈[−5,5]


### AM-MCMC参数设置


参数初始协方差矩阵 

 

 

 C 


 0 

 


 C\_0 


 C0​为对角矩阵，方差取参数取值范围的 

 


 1 


 / 


 20 

 

 1/20 


 1/20；初始迭代次数 

 

 

 t 


 0 

 

 = 


 100 

 

 t\_0=100 


 t0​=100。算法平行运行5次，每次采样5000个样本。




| 模拟次数 | 参数个数 | 并行抽样次数 | 置信水平% | 热身期 |
| --- | --- | --- | --- | --- |
| 5000 | 2 | 5 | 90 | 500 |


### 似然函数


采用BOX&Tiao给出的似然函数，参考[5]p.64(4-14)



 

 

 p 


 ( 

 

 θ 


 t 

 

 ∣ 


 y 


 ) 


 ∝ 


 [ 

 

 ∑ 

 

 i 


 = 


 1 

 

 N 

 

 e 


 ( 

 

 θ 


 t 

 


 ) 


 i 


 2 

 


 ] 

 

 − 

 

 1 


 2 

 

 N 

 

 

 p(\theta^{t}|y) \propto [\sum\_{i=1}^{N}e(\theta^{t})\_i^2]^{-\frac{1}{2}N} 


 p(θt∣y)∝[i=1∑N​e(θt)i2​]−21​N


## 分析结果


### 收敛判断


针对单序列是否稳定，可采用平均值法和方差法， 考察迭代过程中的参数平均值和方差是否稳定[3]。针对多序列是否收敛，采用Gelman[7]在1992年提出的收敛诊断指标 

 

 

 R 

 


 \sqrt{R} 


 R


——比例缩小得分，计算每一参数的比例缩小得分 

 

 

 R 

 


 \sqrt{R} 


 R


， 若接近于1表示参数收敛到了后验分布上。Gelman和Rubin建议将 

 

 

 R 

 

 < 


 1.2 

 

 \sqrt{R}<1.2 


 R


<1.2作为多序列抽样算法收敛判断条件。


#### 单序列


单一采样序列的平均值和方差的变化过程图。从图可看出，当 

 


 t 


 > 


 500 

 

 t>500 


 t>500后，参数 

 


 k 

 

 k 


 k和 

 


 b 

 

 b 


 b的平均值和方差基本达到稳定， 因此单一序列是收敛的。


##### 斜率k


![在这里插入图片描述](https://img-blog.csdnimg.cn/9311030ff51842938056f808163d7f5d.png#pic_center)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/3c9b95dc855a4beaba8e145c2f18f635.png#pic_center)


##### 截距b


![在这里插入图片描述](https://img-blog.csdnimg.cn/fdf95fd7f89c45709b77361553fe1a9f.png#pic_center)


![在这里插入图片描述](https://img-blog.csdnimg.cn/ea7f6480838a41e4a5c789a4ddf307b6.png#pic_center)


#### 多序列



 

 


 R 

 


 \sqrt{R} 


 R


变化过程，在迭代初期， 

 

 

 R 

 


 \sqrt{R} 


 R


变化剧烈；当 

 


 t 


 > 


 200 

 

 t>200 


 t>200后， 

 

 

 R 

 


 \sqrt{R} 


 R


趋于稳定，并接近于1.0，说明参数 

 


 k 

 

 k 


 k和 

 


 b 

 

 b 


 b的MCMC采样序列均能稳定收敛到其参数的后验分布，且算法全局收敛。综合考虑上述单序列和多序列参数的收敛情况， 将每一序列的前500次舍去， 这样５次平行试验共采集了22500个样本用于参数后验分布的统计分析。


##### 斜率k


![在这里插入图片描述](https://img-blog.csdnimg.cn/34c4332327c54572a441d3b205564ffd.png#pic_center)


##### 截距b


![在这里插入图片描述](https://img-blog.csdnimg.cn/ea9bae344f1e409285e7271f46a72c89.png#pic_center)


### 参数后验分布


根据上述MCMC抽样得到参数 

 


 k 

 

 k 


 k和 

 


 b 

 

 b 


 b的后验分布。由图可知， 

 


 b 

 

 b 


 b比 

 


 k 

 

 k 


 k具有更大的不确定性。


#### 斜率k


![在这里插入图片描述](https://img-blog.csdnimg.cn/b33c61db0d604ca08f10da3302d41bd9.png#pic_center)


#### 截距b


![在这里插入图片描述](https://img-blog.csdnimg.cn/fa277719766a486eb5024811274e517e.png#pic_center)


### 置信区间


将似然函数归一化权重根据抽样结果的升序排列，计算5%、50%、95%置信度所在模拟结果，绘制如下图。大多数实测数据都包含于90%的置信区间内。


![在这里插入图片描述](https://img-blog.csdnimg.cn/e3310193f81c439aa67e68308841450d.png#pic_center)


## 结论


与Metropolis-Hastings算法相比，Adaptive Metropolis(AM)算法不需要事先确定推荐分布；与Gibbs采样相比，AM不需要导出条件分布，对于复杂的水文模型，通常很难导出单个参数的条件分布；与GLUE方法相比，AM基于贝叶斯理论框架，是对理想贝叶斯推断过程的一种近似，而GLUE主要通过随机抽样估计参数不确定性。


## 参考文献


[1]Haario, H., Saksman, E., & Tamminen, J. (2001). An Adaptive Metropolis Algorithm. Bernoulli, 7(2), 223–242. https://doi.org/10.2307/3318737  
 [2]Kuczera G, Parent E. (1998). Monte Carlo assessment of parameter uncertainty in conceptual catchment models: the Metropolis algorithm. Journal of Hydrology, 211(1), 69-85. https://doi.org/10.1016/S0022-1694(98)00198-X  
 [3]梁忠民,戴荣,綦晶. 基于MCMC的水文模型参数不确定性及其对预报的影响分析[C]. 环境变化与水安全——第五届中国水论坛论文集.,2007:126-130.  
 [4]李向阳. 水文模型参数优选及不确定性分析方法研究[D].大连理工大学,2006.  
 [5]曹飞凤. 基于MCMC方法的概念性流域水文模型参数优选及不确定性研究[D].浙江大学,2010.  
 [6]Thiemann M, Trosset M, Gupta H, Sorooshian S. (2001). Bayesian recursive parameter estimation for hydrologic models. Water Resources Research, 37(10), 2521-2535. https://doi.org/10.1029/2000WR900405  
 [7]Gelman, A., & Rubin, D. B. (1992). Inference from iterative simulation using multiple sequences. Statistical science, 7(4), 457-472. https://doi.org/10.1214/ss/1177011136  
 [8]W. K. Hastings, Monte Carlo sampling methods using Markov chains and their applications, Biometrika, Volume 57, Issue 1, April 1970, Pages 97–109, https://doi.org/10.1093/biomet/57.1.97  
 [9]MetropoUs, N., Rosenbluth, A., Rosenbluth, M., Teller, A., & Teller, E. (1953). Equation of state calculations by fast computing machines. Journal of Chem. Physics, 21, 1087-1092.https://doi.org/10.1063/1.1699114  
 [10]Armadillo. C++ library for linear algebra & scientific computing. http://arma.sourceforge.net/  
 [11]Conrad Sanderson and Ryan Curtin. Armadillo: a template-based C++ library for linear algebra. Journal of Open Source Software, Vol. 1, pp. 26, 2016. http://dx.doi.org/10.21105/joss.00026  
 [12]Conrad Sanderson and Ryan Curtin. An Adaptive Solver for Systems of Linear Equations. International Conference on Signal Processing and Communication Systems, pp. 1-6, 2020.  
 [13]Matplotlib for C++. https://github.com/lava/matplotlib-cpp


## 源码


AM为单次抽样程序，PAM为平行抽样程序，继承于AM类。由于高度耦合，因此AM类成员都设置为公开`public`。`main.cpp`为主函数，负责创建PAM类，调用参数估计`Estimate()`函数。参数的上下限、初始值、实测值、算法参数在AM类的`Initialize()`函数中设置。平行抽样、置信度在PAM类的构造函数中设置。


程序中用到了Matplotlib-cpp[13]，用于数据可视化，使用方法参见[【C++】11 Visual Studio 2019 C++安装matplotlib-cpp绘图](https://blog.csdn.net/weixin_43012724/article/details/124051588)。


还用到了线性代数库Armadillo[10-12]，用于多元正态分布抽样，使用方法参见[【C++】13 多元正态分布抽样](https://blog.csdn.net/weixin_43012724/article/details/124555304)。


### AM.h



```
#pragma once

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
文件名: AM.h
作者: 卢家波
单位：河海大学水文水资源学院
邮箱：lujiabo@hhu.edu.cn
QQ：1847096852
版本：2022.5 创建 V1.0
版权: MIT
引用格式：卢家波，AM-MCMC算法C++实现. 南京：河海大学，2022.
 LU Jiabo, Adaptive Metropolis algorithm of Markov Chain Monte Carlo in C++. Nanjing:Hohai University, 2022.
参考文献：[1]Haario, H., Saksman, E., & Tamminen, J. (2001). An Adaptive Metropolis Algorithm. Bernoulli, 7(2), 223–242. https://doi.org/10.2307/3318737
 [2]Kuczera G, Parent E. (1998). Monte Carlo assessment of parameter uncertainty in conceptual catchment models: the Metropolis algorithm. Journal of Hydrology, 211(1), 69-85. https://doi.org/10.1016/S0022-1694(98)00198-X
 [3]梁忠民,戴荣,綦晶. 基于MCMC的水文模型参数不确定性及其对预报的影响分析[C]. 环境变化与水安全——第五届中国水论坛论文集.,2007:126-130.
 [4]李向阳. 水文模型参数优选及不确定性分析方法研究[D].大连理工大学,2006.
 [5]曹飞凤. 基于MCMC方法的概念性流域水文模型参数优选及不确定性研究[D].浙江大学,2010.
 [6]Thiemann M, Trosset M, Gupta H, Sorooshian S. (2001). Bayesian recursive parameter estimation for hydrologic models. Water Resources Research, 37(10), 2521-2535. https://doi.org/10.1029/2000WR900405
 [7]Gelman, A., & Rubin, D. B. (1992). Inference from iterative simulation using multiple sequences. Statistical science, 7(4), 457-472. https://doi.org/10.1214/ss/1177011136
 [8]W. K. Hastings, Monte Carlo sampling methods using Markov chains and their applications, Biometrika, Volume 57, Issue 1, April 1970, Pages 97–109, https://doi.org/10.1093/biomet/57.1.97
 [9]MetropoUs, N., Rosenbluth, A., Rosenbluth, M., Teller, A., & Teller, E. (1953). Equation of state calculations by fast computing machines. Journal of Chem. Physics, 21, 1087-1092.https://doi.org/10.1063/1.1699114
 [10]Armadillo. C++ library for linear algebra & scientific computing. http://arma.sourceforge.net/
 [11]Conrad Sanderson and Ryan Curtin. Armadillo: a template-based C++ library for linear algebra. Journal of Open Source Software, Vol. 1, pp. 26, 2016. http://dx.doi.org/10.21105/joss.00026
 [12]Conrad Sanderson and Ryan Curtin. An Adaptive Solver for Systems of Linear Equations. International Conference on Signal Processing and Communication Systems, pp. 1-6, 2020.
 [13]Matplotlib for C++. https://github.com/lava/matplotlib-cpp

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/

#include <vector>
#include <armadillo> //线性代数运算库

class AM
{
public:
	//AM-MCMC不确定性估计函数
	virtual void Estimate();

	//===============1.输入估计参数及实测值===============
	void Setup();

	//====================2.运行模型====================
	void Run();

//private:
	//===================3.不确定性估计===================
	void Analysis();

	//===================4.输出估计结果===================
	void Save();

	//1.初始化，t=0
	void Initialize();

	//2.计算协方差矩阵Ct
	void CalculateCovariance();

	//3.从推荐分布抽样获得候选点
	void Sample();

	//4.计算接受概率alpha
	void CalAcceptanceProbability();

	//5.产生[0, 1]随机数u
	void GenerateRandomNumber();

	//6.判断是否接受候选点
	void IfAcceptCandidatePoint();

	//7.重复2-6直到产生足够的样本为止
	void Repeat();

	//计算经验协方差矩阵
	void CalEmpiricalCovariance();

	//协方差递推公式
	void RecurseCovariance();

	//候选点参数检查
	bool CheckBound();

	//目标概率密度函数,取对数
	double logPDF();

	//调用待分析模型，得到模拟值
	void CallModel();

	//计算似然函数值
	void CalculateLikeliFun();

	//计算纳什系数
	void CalculateNSE();

	//计算均方误差
	void CalculateMSE();

	//计算BOX&Tiao给出的似然函数，参考[5]p.64(4-14)
	void CalculateBOX();

	//收敛判断指标
	void CheckConvergence();

	//参数后验分布频率直方图
	virtual void PlotPostDistribution();

	//绘制置信区间
	virtual void PlotConfidenceInterval();

	int dimension;   //参数的空间维度
	int t0;          //初始迭代次数
	int iterationNumber;  //迭代总次数
	int currentIterNum;   //当前迭代次数
	int burnInNumber;     //热身次数

	double sd;       //比例因子，仅取决于参数的空间维度
	double epsilon;  //为一个较小的数，以确保Ci不为奇异矩阵
	double alpha;    //候选点接受概率
	double logDensity;   //目标分布的对数密度，$\pi(x)$
	double candidatePointLogDensity;  //候选点的对数密度
	double randomNumber;  //随机数

	//实测值
	std::vector<double> observedData;

	//每次待分析模型运行得到的模拟值、模拟值数组
	std::vector<double> modelledData;
	std::vector< std::vector<double> > modelledDataArray;
	
	//似然函数值
	std::vector<double> NSE;   //纳什效率系数
	std::vector<double> MSE;   //均方误差
	std::vector<double> BOX;   //BOX似然函数

	std::vector<std::string> paramName;  //参数名
	arma::vec lowerBound;   //参数下限
	arma::vec upperBound;   //参数上限
	arma::vec candidatePoint;  //候选点
	arma::vec currentMean;   //前t次迭代样本点的均值
	arma::vec previousMean;  //前t-1次迭代样本点的均值
	arma::vec sample;     //当前样本点

	arma::mat sampleGroup;  //样本点集合
	arma::mat sampleMean;   //样本点各次迭代均值
	arma::mat sampleVariance;  //样本点各次迭代方差
	arma::mat sampleGroupBurnIn;  //除去热身期的样本
	arma::mat C0;    //初始协方差矩阵
	arma::mat Ct;    //第t次迭代协方差矩阵
	arma::mat Id;    //单位矩阵
	arma::mat empiricalCovariance;  //cov(X0, ..., Xt0)，经验协方差矩阵

};


```

### PAM.h



```
#pragma once

//Parallel Adaptive Metropolis algorithm

#include "AM.h"
#include <armadillo> //线性代数运算库

class PAM : public AM
{
public:
	void Estimate() override;

	PAM();

private:
	void ParallelRun();

	void CalculateSRF();

	void PlotSRF();

	void PlotPostDistribution() override;

	void PlotConfidenceInterval() override;

	void Plot();

	//计算似然函数值对应的归一化权重
	void CalculateWeight();

	//经验累积分布函数（CDF）的预测
	void ecdfPred();

	//按模拟值的升序对一组权重数组和对应的模拟值数组进行排序
	void sort(std::vector<double>& w, std::vector<double>& x);

	//计算权重累加之和
	std::vector<double> cumsum(const std::vector<double>& w);

	//计算各百分位数下的样本索引值
	std::vector<double> CalPercentiles(const std::vector<double>& modValue);

	//通过置信水平计算百分位数
	void SetPercentile();

	int parallelSampleNumber;  //并行抽样次数
	int modelledDataSize;  //模拟值数据大小

	double confidenceLevel; //置信水平

	std::vector<double> parallelBOXBurnIn;  //并行抽样除去热身期的BOX似然函数
	std::vector<double> weight;
	std::vector< std::vector<double> > modelledDataArrayBurnIn;  //去除热身期模拟值数组

	//不确定性边界预测
	std::vector<double> percentile;  //置信区间的百分位数
	std::vector<double> ecdf;        //经验累积分布函数
	std::vector<double> sampLowerPtile;  //低分位数
	std::vector<double> sampUpperPtile;  //高分位数
	std::vector<double> sampMedian;      //50%分位数

	arma::mat scaleReductionFactor;  //比例缩小得分SRF$\sqrt{R}$
	arma::cube parallelSampleMean;   //并行抽样样本点各次迭代均值
	arma::cube parallelSampleVariance;  //并行抽样各次迭代方差
	arma::cube parallelSampleGroupBurnIn;  //并行抽样除去热身期的样本

};



```

### AM.cpp



```
/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
文件名: AM.cpp
作者: 卢家波
单位：河海大学水文水资源学院
邮箱：lujiabo@hhu.edu.cn
QQ：1847096852
版本：2022.5 创建 V1.0
版权: MIT
引用格式：卢家波，AM-MCMC算法C++实现. 南京：河海大学，2022.
 LU Jiabo, Adaptive Metropolis algorithm of Markov Chain Monte Carlo in C++. Nanjing:Hohai University, 2022.
参考文献：[1]Haario, H., Saksman, E., & Tamminen, J. (2001). An Adaptive Metropolis Algorithm. Bernoulli, 7(2), 223–242. https://doi.org/10.2307/3318737
 [2]Kuczera G, Parent E. (1998). Monte Carlo assessment of parameter uncertainty in conceptual catchment models: the Metropolis algorithm. Journal of Hydrology, 211(1), 69-85. https://doi.org/10.1016/S0022-1694(98)00198-X
 [3]梁忠民,戴荣,綦晶. 基于MCMC的水文模型参数不确定性及其对预报的影响分析[C]. 环境变化与水安全——第五届中国水论坛论文集.,2007:126-130.
 [4]李向阳. 水文模型参数优选及不确定性分析方法研究[D].大连理工大学,2006.
 [5]曹飞凤. 基于MCMC方法的概念性流域水文模型参数优选及不确定性研究[D].浙江大学,2010.
 [6]Thiemann M, Trosset M, Gupta H, Sorooshian S. (2001). Bayesian recursive parameter estimation for hydrologic models. Water Resources Research, 37(10), 2521-2535. https://doi.org/10.1029/2000WR900405
 [7]Gelman, A., & Rubin, D. B. (1992). Inference from iterative simulation using multiple sequences. Statistical science, 7(4), 457-472. https://doi.org/10.1214/ss/1177011136
 [8]W. K. Hastings, Monte Carlo sampling methods using Markov chains and their applications, Biometrika, Volume 57, Issue 1, April 1970, Pages 97–109, https://doi.org/10.1093/biomet/57.1.97
 [9]MetropoUs, N., Rosenbluth, A., Rosenbluth, M., Teller, A., & Teller, E. (1953). Equation of state calculations by fast computing machines. Journal of Chem. Physics, 21, 1087-1092.https://doi.org/10.1063/1.1699114
 [10]Armadillo. C++ library for linear algebra & scientific computing. http://arma.sourceforge.net/
 [11]Conrad Sanderson and Ryan Curtin. Armadillo: a template-based C++ library for linear algebra. Journal of Open Source Software, Vol. 1, pp. 26, 2016. http://dx.doi.org/10.21105/joss.00026
 [12]Conrad Sanderson and Ryan Curtin. An Adaptive Solver for Systems of Linear Equations. International Conference on Signal Processing and Communication Systems, pp. 1-6, 2020.
 [13]Matplotlib for C++. https://github.com/lava/matplotlib-cpp

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/

#include "AM.h"

#include <random>
#include <numeric>
#include <armadillo> //线性代数运算库
#include "matplotlibcpp.h" //matplotlib的C++接口，用法见【C++】11 Visual Studio 2019 C++安装matplotlib-cpp绘图（https://blog.csdn.net/weixin\_43012724/article/details/124051588）
namespace plt = matplotlibcpp;

double AM::logPDF()
{
	CallModel();

	CalculateLikeliFun();

	//BOX效果最好，MSE次之，NSE最差
	//if (NSE.back() <= 0)
	//{
	// return log(1.0e-6);
	//}
	//else
	//{
	// return log(NSE.back());
	//}

	//return log(1.0 / MSE.back());

	return BOX.back();
	
}

void AM::CallModel()
{
	//测试模型为线性模型 y = k \* x + b, x in [-5, 5]
	//k 真值为 2，b 真值为 -1
	for (int x = -5; x <= 5; ++x)
	{
		double temp = candidatePoint(0) \* x + candidatePoint(1);

		modelledData.emplace\_back(temp);
	}

	modelledDataArray.emplace\_back(modelledData);
}

void AM::CalculateLikeliFun()
{
	CalculateNSE();

	CalculateMSE();

	CalculateBOX();

	modelledData.clear();
}

void AM::CalculateNSE()
{
	double measuredValuesSum = std::accumulate(observedData.begin(), observedData.end(), 0.0);

	double measuredValuesAvg = measuredValuesSum / observedData.size();

	double numerator = 0.0;

	double denominator = 0.0;

	for (double val : observedData)
	{
		denominator += pow(val - measuredValuesAvg, 2);
	}

	for (int index = 0; index < observedData.size(); ++index)
	{
		numerator += pow(modelledData.at(index) - observedData.at(index), 2);
	}

	double nse = 1 - numerator / denominator;

	NSE.emplace\_back(nse);
}

void AM::CalculateMSE()
{
	double squareErrorSum = 0;

	int dataSize = observedData.size();

	for (int i = 0; i < dataSize; ++i)
	{
		squareErrorSum += pow(modelledData.at(i) - observedData.at(i), 2);
	}

	double mse = squareErrorSum / static\_cast<double>(dataSize);

	MSE.emplace\_back(mse);
}

void AM::CalculateBOX()
{
	double squareErrorSum = 0;

	int dataSize = observedData.size();

	for (int i = 0; i < dataSize; ++i)
	{
		squareErrorSum += pow(modelledData.at(i) - observedData.at(i), 2);
	}

	double box = log(pow(squareErrorSum, -static\_cast<double>(dataSize) / 2.0));

	BOX.emplace\_back(box);

}

void AM::CheckConvergence()
{
	for (int i = 0; i < dimension; ++i)
	{
		arma::rowvec paramMeanVec = sampleMean.row(i);  //参数均值
		
		std::vector<double> paramMean //将arma::vec转化为std::vector<double>类型
			= arma::conv\_to< std::vector<double> >::from(paramMeanVec);  
		
		plt::plot(paramMean);
		plt::title("Change process diagram of the mean value of parameter " + paramName.at(i));
		plt::xlabel("Number of simulations");
		plt::ylabel("Mean of " + paramName.at(i));
		plt::save(paramName.at(i) + "\_Mean" + ".png");
		plt::show();
		
		arma::rowvec paramVarianceVec = sampleVariance.row(i);   //参数方差
		
		std::vector<double> paramVariance 
			= arma::conv\_to< std::vector<double> >::from(paramVarianceVec);
		
		plt::plot(paramVariance);
		plt::title("Change process diagram of the variance value of parameter " + paramName.at(i));
		plt::xlabel("Number of simulations");
		plt::ylabel("Variance of " + paramName.at(i));
		plt::save(paramName.at(i) + "\_Variance" + ".png");
		plt::show();
	}

	
}

void AM::PlotPostDistribution()
{
	sampleGroupBurnIn = sampleGroup.cols(burnInNumber, iterationNumber);
	
	for (int i = 0; i < dimension; ++i)
	{
		arma::rowvec paramChainVec = sampleGroupBurnIn.row(i);
		
		std::vector<double> paramChain 
			= arma::conv\_to< std::vector<double> >::from(paramChainVec);
		
		plt::hist(paramChain);

		plt::show();
	}
	
}

void AM::PlotConfidenceInterval()
{
}


void AM::Estimate()
{
	Setup();

	Run();

	Analysis();

	Save();
}

void AM::Setup()
{
	Initialize();
}

void AM::Run()
{
	Repeat();
}

void AM::Analysis()
{
	CheckConvergence();

	PlotPostDistribution();

	PlotConfidenceInterval();
}


void AM::Save()
{
	sampleGroup.save("sampleGroup.csv", arma::csv_ascii);
	sampleGroupBurnIn.save("sampleGroupBurbIn.csv", arma::csv_ascii);
	sampleMean.save("sampleMean.csv", arma::csv_ascii); 
	sampleVariance.save("sampleVariance.csv", arma::csv_ascii);

	std::ofstream fout("likelihood.csv");
	fout << "NSE" << "," << "MSE" << "," << "logBOX" << ",\n";

	int likelihoodSize = BOX.size();

	for (int i = 0; i < likelihoodSize; ++i)
	{
		fout << NSE.at(i) << "," << MSE.at(i) << "," << BOX.at(i) << ",\n";
	}
	fout.close();
}

void AM::Initialize()
{
	dimension = 2;
	t0 = 100;
	iterationNumber = 5000;
	currentIterNum = 0;
	burnInNumber = 500;

	sd = 2.4 \* 2.4 / dimension;
	epsilon = 1.0e-5;
	alpha = 0;
	candidatePointLogDensity = 0;
	randomNumber = 0;

	observedData = { -10.79, -9.4, -6.77, -5.3, -3.52, -0.97, 1.3, 3.13, 5.16, 7.56, 8.8 };  //每个点增加正态随机数N(0, 0.16)扰动

	paramName = { "k", "b" };
	lowerBound = {-3, -3};
	upperBound = {3, 3 };
	candidatePoint.zeros(dimension);
	currentMean = { 1, 1 };
	previousMean.zeros(dimension);
	sample = {1, 1};
	logDensity = logPDF();

	sampleGroup.zeros(dimension, iterationNumber + 1);
	sampleGroup.col(0) = sample;
	sampleMean.zeros(dimension, iterationNumber + 1);
	sampleMean.col(0) = currentMean;
	sampleVariance.zeros(dimension, iterationNumber + 1);
	C0.zeros(dimension, dimension);
	C0.diag() = (upperBound - lowerBound) / 20.0;
	Ct = C0;
	Id.eye(dimension, dimension);
	empiricalCovariance.zeros(dimension, dimension);
}

void AM::CalculateCovariance()
{
	if (currentIterNum == t0 + 1)
	{
		CalEmpiricalCovariance();

		Ct = sd \* empiricalCovariance + sd \* epsilon \* Id;
	}

	if (currentIterNum > t0 + 1)
	{
		RecurseCovariance();
	}
}

void AM::Sample()
{
	candidatePoint = arma::mvnrnd(sample, Ct);
}

void AM::CalAcceptanceProbability()
{
	if (CheckBound())
	{
		candidatePointLogDensity = logPDF();

		double temp = candidatePointLogDensity - logDensity;

		alpha = std::min(1.0, exp(temp));
	}
	else
	{
		alpha = 0.0;

		modelledDataArray.emplace\_back(modelledDataArray.back());

		NSE.emplace\_back(NSE.back());

		MSE.emplace\_back(MSE.back());

		BOX.emplace\_back(BOX.back());

	}
	
}

void AM::GenerateRandomNumber()
{
	std::random_device rd;
	std::mt19937 gen(rd());
	std::uniform_real_distribution<double> u(0.0, 1.0);

	randomNumber = u(gen);
}

void AM::IfAcceptCandidatePoint()
{
	if (alpha >= randomNumber)
	{
		sample = candidatePoint;

		logDensity = candidatePointLogDensity;
	}

	sampleGroup.col(currentIterNum) = sample;

	//更新方差，注意不要对整个sampleGroup求方差，因为包含初始化的0
	sampleVariance.col(currentIterNum) = arma::var(sampleGroup.cols(0, currentIterNum), 0, 1);  //对矩阵每一行求方差, N-1

	//更新均值
	previousMean = currentMean;

	currentMean = (currentIterNum \* previousMean + sample) 
		/ static\_cast<double>(currentIterNum + 1);

	sampleMean.col(currentIterNum) = currentMean;
}

void AM::Repeat()
{
	while (currentIterNum < iterationNumber)
	{
		currentIterNum++;

		CalculateCovariance();

		Sample();

		CalAcceptanceProbability();

		GenerateRandomNumber();

		IfAcceptCandidatePoint();
	}

	sampleGroupBurnIn = sampleGroup.cols(burnInNumber, iterationNumber);
}

void AM::CalEmpiricalCovariance()
{
	arma::mat tempSum(dimension, dimension, arma::fill::zeros);

	for (int col = 0; col < sampleGroup.n_cols; col ++)
	{
		arma::vec x = sampleGroup.col(col);

		tempSum += x \* x.t();
	}

	empiricalCovariance = 
		(tempSum - (t0 + 1) \* previousMean \* previousMean.t())
		/ static\_cast<double>(t0);
}

void AM::RecurseCovariance()
{
	int t = currentIterNum;

	Ct = (static\_cast<double>(t - 1) / static\_cast<double>(t)) \* Ct
		+ sd / static\_cast<double>(t)
		\* (t \* previousMean \* previousMean.t()
			- (t + 1) \* currentMean \* currentMean.t()
			+ sample \* sample.t()
			+ epsilon \* Id);
}

bool AM::CheckBound()
{
	for (size_t i = 0; i < dimension; i++)
	{
		if ((candidatePoint(i) < lowerBound(i)) || (candidatePoint(i) > upperBound(i)))
		{
			return false;
		}
	}

	return true;
}


```

### PAM.cpp



```
#include "PAM.h"

#include <algorithm>
#include "matplotlibcpp.h" //matplotlib的C++接口，用法见【C++】11 Visual Studio 2019 C++安装matplotlib-cpp绘图（https://blog.csdn.net/weixin\_43012724/article/details/124051588）
namespace plt = matplotlibcpp;

void PAM::Estimate()
{
	ParallelRun();

	CheckConvergence();

	CalculateSRF();

	PlotSRF();

	PlotPostDistribution();

	PlotConfidenceInterval();

	Save();
}

PAM::PAM()
{
	Setup();

	confidenceLevel = 0.9;
	parallelSampleNumber = 5;

	scaleReductionFactor = arma::mat(dimension, iterationNumber, arma::fill::zeros);  //比例缩小得分SRF$\sqrt{R}$
	parallelSampleMean = arma::cube(dimension, iterationNumber + 1, parallelSampleNumber);   //并行抽样样本点各次迭代均值
	parallelSampleVariance = arma::cube(dimension, iterationNumber + 1, parallelSampleNumber);  //并行抽样各次迭代方差
	parallelSampleGroupBurnIn = arma::cube(dimension, iterationNumber - burnInNumber + 1, parallelSampleNumber);  //并行抽样除去热身期的样本
}

void PAM::ParallelRun()
{
	for (int i = 0; i < parallelSampleNumber; ++i)
	{
		Setup();

		Run();

		parallelSampleMean.slice(i) = sampleMean;

		parallelSampleVariance.slice(i) = sampleVariance;

		parallelSampleGroupBurnIn.slice(i) = sampleGroupBurnIn;

	}
}

void PAM::CalculateSRF()
{
	arma::mat tempMean(dimension, parallelSampleNumber, arma::fill::zeros);
	arma::mat tempVariance(dimension, parallelSampleNumber, arma::fill::zeros);

	for (int i = 1; i < iterationNumber + 1; ++i)
	{
		for (int j = 0; j < parallelSampleNumber; ++j)
		{
			tempMean.col(j) = parallelSampleMean.slice(j).col(i);
			tempVariance.col(j) = parallelSampleVariance.slice(j).col(i);
		}

		arma::vec B = arma::var(tempMean, 0, 1);  //对矩阵每一行求方差, N-1
		arma::vec W = arma::mean(tempVariance, 1); //对矩阵每一行求均值

		scaleReductionFactor.col(i - 1) 
			= arma::sqrt(
				static\_cast<double>((i - 1)) / static\_cast<double>(i)
				+ ((parallelSampleNumber + 1) / static\_cast<double>(parallelSampleNumber))
				\* B / W);
	}
	
	
}

void PAM::PlotSRF()
{
	for (int i = 0; i < dimension; ++i)
	{
		arma::rowvec paramSRFVec = scaleReductionFactor.row(i);  //参数均值

		std::vector<double> SRFVec //将arma::vec转化为std::vector<double>类型
			= arma::conv\_to< std::vector<double> >::from(paramSRFVec);

		plt::plot(SRFVec);
		plt::title("Change process diagram of Scale Reduction Factor of parameter " + paramName.at(i));
		plt::xlabel("Number of simulations");
		plt::ylabel("Scale Reduction Factor $\\sqrt{R}$");
		plt::save(paramName.at(i) + "\_Scale Reduction Factor" + ".png");
		plt::show();

	}
}

void PAM::PlotPostDistribution()
{
	int size = iterationNumber - burnInNumber + 1;

	arma::mat tempSampleBurnIn = arma::mat(dimension, size \* parallelSampleNumber, arma::fill::zeros);
	
	//去除热身期的样本
	for (int i = 0; i < parallelSampleNumber; ++i)
	{
		tempSampleBurnIn.cols(i \* size, (i + 1) \* size - 1) = parallelSampleGroupBurnIn.slice(i);
	}

	for (int i = 0; i < dimension; ++i)
	{
		arma::rowvec paramChainVec = tempSampleBurnIn.row(i);

		std::vector<double> paramChain
			= arma::conv\_to< std::vector<double> >::from(paramChainVec);

		plt::hist(paramChain);
		plt::title("Posterior distribution histogram for parameter " + paramName.at(i));
		plt::xlabel(paramName.at(i));
		plt::ylabel("Frequency");
		plt::save(paramName.at(i) + "\_Posterior distribution histogram" + ".png");
		plt::show();
	}

}

void PAM::PlotConfidenceInterval()
{
	CalculateWeight();

	SetPercentile();

	ecdfPred();

	Plot();
}

void PAM::Plot()
{
	std::vector<double> xAxis = { -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5 };
	plt::plot(xAxis, sampLowerPtile);
	plt::plot(xAxis, sampUpperPtile);
	plt::plot(xAxis, sampMedian);
	plt::scatter(xAxis, observedData, 30, { {"color", "red"}, {"marker", "o"} });
	plt::title("Median and " + std::to\_string(static\_cast<int>(confidenceLevel \* 100)) + "% AM-MCMC prediction limits with BOX");
	plt::xlabel("x");
	plt::ylabel("y");
	plt::save("ConfidenceInterval.png");
	plt::show();
}

void PAM::CalculateWeight()
{
	int size = iterationNumber - burnInNumber + 1;
	int sizeTotal = iterationNumber + 1;

	modelledDataArray.erase(modelledDataArray.begin());
	NSE.erase(NSE.begin());
	MSE.erase(MSE.begin());
	BOX.erase(BOX.begin()); //删除因为PAM构造函数生成的第一行

	arma::vec BOXvec(BOX);  //转成arma的矢量
	arma::vec BOXBurnIn = arma::vec(size \* parallelSampleNumber);

	for (int i = 0; i < parallelSampleNumber; ++i)
	{
		BOXBurnIn.rows(i \* size, (i + 1) \* size - 1) =
			BOXvec.rows(burnInNumber + i \* sizeTotal, iterationNumber + i \* sizeTotal);
	}

	parallelBOXBurnIn
		= arma::conv\_to< std::vector<double> >::from(BOXBurnIn);

	double parallelBOXBurnInSum = std::accumulate(parallelBOXBurnIn.begin(), parallelBOXBurnIn.end(), 0.0);

	for (auto box : parallelBOXBurnIn)
	{
		double wgt = box / parallelBOXBurnInSum;

		weight.emplace\_back(wgt);
	}
}

void PAM::ecdfPred()
{
	int size = iterationNumber - burnInNumber + 1;
	int sizeTotal = iterationNumber + 1;

	modelledDataArrayBurnIn.assign(size \* parallelSampleNumber, { 0 });

	for (int i = 0; i < parallelSampleNumber; ++i)
	{
		std::copy(modelledDataArray.begin() + burnInNumber + i \* sizeTotal, 
			modelledDataArray.begin() + (i + 1) \* sizeTotal, 
			modelledDataArrayBurnIn.begin() + i \* size);
	}

	modelledDataSize = modelledDataArray[0].size();

	for (size_t i = 0; i < modelledDataSize; ++i)
	{
		std::vector<double> modValue; //某时刻所有的模拟值，不包含热身期
		for (auto modData : modelledDataArrayBurnIn)
		{
			modValue.emplace\_back(modData.at(i));
		}
		
		//按模拟值的升序对权重和对应的模拟值数组进行排序
		sort(weight, modValue);

		ecdf = cumsum(weight);

		std::vector<double> sampPtile = CalPercentiles(modValue);

		sampLowerPtile.emplace\_back(sampPtile.at(0));
		sampUpperPtile.emplace\_back(sampPtile.at(1));
		sampMedian.emplace\_back(sampPtile.at(2));
	}
}

void PAM::sort(std::vector<double>& w, std::vector<double>& x)
{
	//将权重与模拟值对应上，以便于扩展排序
	std::vector< std::pair<double, double> > wx;

	//构造向量wx，存储权重和模拟值对
	for (size_t i = 0; i < w.size(); ++i)
	{
		wx.emplace\_back(w[i], x[i]);
	}

	//匿名函数定义比较规则，用于pair数据结构按照权重升序排列
	std::sort(wx.begin(), wx.end(), [](auto a, auto b) { return a.second < b.second; });

	//将排序后的wx赋值给权重数组w和函模拟值数组x
	for (size_t i = 0; i < wx.size(); i++)
	{
		w[i] = wx[i].first;  //取出权重

		x[i] = wx[i].second;   //取出模拟值
	}
}

std::vector<double> PAM::cumsum(const std::vector<double>& w)
{
	std::vector<double> ecdf;

	for (size_t i = 0; i < w.size(); i++)
	{
		double wgt = std::accumulate(w.begin(), w.begin() + i, 0.0);

		ecdf.emplace\_back(wgt);
	}

	return ecdf;
}

std::vector<double> PAM::CalPercentiles(const std::vector<double>& modValue)
{
	std::vector<double> sampPtile;

	for (size_t i = 0; i < percentile.size(); i++)
	{
		double percent = percentile.at(i);

		//构造临时向量以寻找各分位数的位置
		std::vector<double> tempVector;
		for (double val : ecdf)
		{
			double temp = fabs(val - percent);

			tempVector.emplace\_back(temp);
		}

		auto iter = std::min\_element(tempVector.begin(), tempVector.end());

		//确保所求范围大于等于给定百分位数范围
		if (percent <= 0.5)
		{
			if (\*iter > percent)
			{
				iter -= 1;
			}
		}
		else
		{
			if (\*iter < percent)
			{
				iter += 1;
			}
		}

		sampPtile.emplace\_back(modValue.at(iter - tempVector.begin()));
	}

	return sampPtile;
}

void PAM::SetPercentile()
{
	double lowerBound, upperBound;

	lowerBound = (1 - confidenceLevel) / 2.0;

	upperBound = 1 - lowerBound;

	percentile = { lowerBound, upperBound, 0.5 };
}


```

### main.cpp



```
#include <iostream>

#include "PAM.h"

int main()
{
	PAM pamAlgorithm;

	pamAlgorithm.Estimate();

}

```

## 文档说明


本地目录：`E:\Research\OptA\MCMC`


`Gibbs`：吉布斯采样估计线性回归模型参数


`AM`：自适应metropolis算法C++实现


`RefCode`:参考代码


`RefPaper`：参考文献


## 进度


2022/3/18，复现博客 [Gibbs sampling for Bayesian linear regression in Python](https://kieranrcampbell.github.io/blog/2016/05/15/gibbs-sampling-bayesian-linear-regression.html)，实现吉布斯采样估计线性回归参数


2022/4/17 看MCMC首次引入水文模型参数不确定性估计的论文 Monte Carlo assessment of parameter uncertainty in conceptual catchment models Metropolis algorithm，文中所用的采样算法为Metropolis算法


2022/4/18 从Github下载AM相关代码，从知网下载AM相关论文。


论文中关于AM描述位置：


* 基于AM-MCMC的地震反演方法研究\_李远 P27
* 水文模型参数优选及不确定性分析方法研究 P39
* 基于MCMC方法的概念性流域水文模型参数优选及不确定性研究 P63
* 基于AM-MCMC算法的贝叶斯概率洪水预报模型\_邢贞相 P2
* Haario-2001-An-adaptive-metropolis-algorithm AM算法出处


2022/4/26 阅读了上述文献中的方法介绍，准备参考`E:\Research\OptA\MCMC\RefCode\adaptive_metropolis-master\adaptive_metropolis-master`代码实现AM算法


2022/4/27 创建AM文件夹，用于C++实现AM算法


2022/4/28 尝试用C++实现[多元正态分布抽样](https://blog.csdn.net/weixin_43012724/article/details/124555304)


2022/4/29 编写AM程序


2022/5/4 初步完成程序编写，可以运行出结果


2022/5/5 上午，完成二元一次方程测试；下午，将似然函数从纳什效率底数NSE改为均方误差MSE和BOX，效果显著，参数分布明显收敛；或许可以研究不同似然函数的影响。


2022/5/6 补充绘图、收敛判断、参考文献


2022/5/9 并行抽样


2022/5/10 完成AM-MCMC程序编写，测试线性模型参数估计通过，撰写博客





