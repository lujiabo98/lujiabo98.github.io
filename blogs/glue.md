---
layout: page
permalink: /blogs/glue/index.html
title: glue
---




## 目标


2022/4/2 - 2022/4/12  
 用C++编写GLUE程序，并以线性回归模型参数估计为例测试GLUE程序


**如有问题，欢迎交流探讨！** 邮箱：lujiabo@hhu.edu.cn 卢家波  
 来信请说明博客标题及链接，谢谢。


## GLUE简介


针对模型参数的等效性，Beven和 Binley (1992）提出了普适似然不确定性估计方法，[Generalized Likelihood Uncertainty Estimation(GLUE)]用于分析水文数学模型预报的不确定性。GLUE是基于Hornberger和 Spear (1981)的 RSA(Regionalized Sensitivity Analysis)方法发展起来的。


GLUE方法中一个很重要的观点就是：导致模型模拟结果的好与差不是模型的单个参数，而是模型参数的组合。


1. 在预先设定的参数分布取值空间内，利用Monte-Carlo随机采样方法获取模型的参数值组合，运行模型。
2. 选定似然目标函数，计算模型预报结果与观测值之间的似然函数值，再计算这些函数值的权重，得到各参数组合的似然值。
3. 在所有的似然值中，设定一个临界值。当然这个临界值的选取带有一定的主观性。低于该临界值的参数组似然值被赋为零，表示这些参数组不能表征模型的功能特征；高于该临界值则表示这些参数组能够表征模型的功能特征。
4. 对高于临界值的所有参数组似然值重新归一化，按照似然值的大小，求出在某置信度下模型预报的不确定性范围(Freer 和 Beven，1996)。


Monte-Carlo参数值采样方法可以克服自动搜索、随机搜索、试错率定等寻优方法在水文模型高维参数空间中应用的一些缺陷，比如，临界参数、互相关、自相关、残差的异方差性和不敏感参数等所导致的参数响应曲面奇异现象，诸如局地最小值、凹陷和凸起等。然而对于具有多个参数的模型结构，参数值的组合方式非常多，常常需要几万或几十万，甚至上百万次的参数采样，因此 Monte-Carlo方法需要消耗大量的计算资源。


## 模型描述


### 线性模型


利用GLUE估计线性回归参数，模型如下：



 

 

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


进行普适似然不确定性估计的2个参数为  

 


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


 k | 斜率 | 2 | -4 | 4 |
|  

 


 b 

 

 b 


 b | 截距 | -1 | -4 | 4 |


### 实测值


实测值取-5到5之间的整数对应的函数值，即



 

 

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

 

 y=-11,-9,-7,-5, -3, -1, 1, 3, 5, 7, 9, x\in[-5,5] 


 y=−11,−9,−7,−5,−3,−1,1,3,5,7,9,x∈[−5,5]


### GLUE参数设置




| 模拟次数 | 参数个数 | 精度指标有效阈值 | 置信水平% |
| --- | --- | --- | --- |
| 10000 | 2 | 0.5 | 90 |


### 输入文件


`setup.txt`：



```
模拟次数 10000
参数个数 2
精度指标有效阈值 0.5
置信水平% 90
参数名 下限 上限
k  -6  6
b  -6  6
实测值
-11
-9
-7
-5
-3
-1
1
3
5
7
9

```

### 输出文件


`output.txt`：



```
k	      b	     NSE	    模拟值
-2.38	0.16	-3.82	12.04	9.67	7.29	4.91	2.53	0.16	-2.22	-4.60	-6.97	-9.35	-11.73	
-3.29	-5.21	-6.45	11.25	7.96	4.67	1.37	-1.92	-5.21	-8.50	-11.80	-15.09	-18.38	-21.68	
-2.97	1.80	-5.37	16.66	13.69	10.71	7.74	4.77	1.80	-1.17	-4.14	-7.12	-10.09	-13.06	
3.14	4.65	-0.12	-11.04	-7.90	-4.76	-1.63	1.51	4.65	7.79	10.92	14.06	17.20	20.34	
4.69	-1.00	-0.81	-24.47	-19.77	-15.08	-10.39	-5.69	-1.00	3.70	8.39	13.08	17.78	22.47	
-3.02	-0.55	-5.31	14.56	11.54	8.52	5.49	2.47	-0.55	-3.57	-6.59	-9.62	-12.64	-15.66	
-5.46	1.86	-13.13	29.19	23.72	18.26	12.79	7.33	1.86	-3.60	-9.07	-14.53	-19.99	-25.46	
-2.29	-0.97	-3.59	10.45	8.17	5.88	3.60	1.31	-0.97	-3.26	-5.54	-7.83	-10.11	-12.40	
-3.97	5.24	-8.89	25.11	21.13	17.16	13.19	9.22	5.24	1.27	-2.70	-6.67	-10.64	-14.62	
-4.40	-1.11	-9.25	20.91	16.51	12.10	7.70	3.30	-1.11	-5.51	-9.91	-14.32	-18.72	-23.12	
-0.08	-2.68	-0.16	-2.26	-2.35	-2.43	-2.51	-2.60	-2.68	-2.76	-2.84	-2.93	-3.01	-3.09	
0.50	5.24	-0.54	2.76	3.26	3.76	4.25	4.75	5.24	5.74	6.23	6.73	7.22	7.72	
-4.92	-2.23	-11.02	22.38	17.46	12.54	7.61	2.69	-2.23	-7.15	-12.08	-17.00	-21.92	-26.85	
3.36	-5.20	0.09	-22.02	-18.66	-15.29	-11.93	-8.57	-5.20	-1.84	1.52	4.89	8.25	11.61	
-0.67	0.02	-0.81	3.38	2.71	2.03	1.36	0.69	0.02	-0.66	-1.33	-2.00	-2.68	-3.35	
-0.61	-3.41	-0.85	-0.37	-0.97	-1.58	-2.19	-2.80	-3.41	-4.02	-4.63	-5.24	-5.85	-6.46	
-2.12	-0.90	-3.24	9.69	7.57	5.46	3.34	1.22	-0.90	-3.01	-5.13	-7.25	-9.37	-11.48	
0.04	-2.06	0.01	-2.28	-2.24	-2.19	-2.15	-2.11	-2.06	-2.02	-1.98	-1.93	-1.89	-1.85	
-0.68	-0.08	-0.82	3.33	2.65	1.96	1.28	0.60	-0.08	-0.77	-1.45	-2.13	-2.81	-3.50	
···共10000条记录

```

## 分析结果


### 参数-似然判据值散点图


![在这里插入图片描述](https://img-blog.csdnimg.cn/1ce34049c7494891ada23c9c355d5b06.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


![在这里插入图片描述](https://img-blog.csdnimg.cn/0d763b9c8d5546f095ea1eceb4866e9b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


### 90%置信范围


![在这里插入图片描述](https://img-blog.csdnimg.cn/f08842ccc67643e0a198239b23fdc6e0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


### 分析


从散点图上可以看出，两个参数  

 


 k 

 

 k 


 k 和  

 


 b 

 

 b 


 b 的估计值与真值几乎相等；从不确定性范围图上可以看出，所有观测值（红点）几乎都落在50%分位数的中线（绿线）上，而且观测值也都在90%置信区间下限（蓝线）和上限（橙色）范围内。这说明GLUE方法能够很好地估计线性回归模型参数，对参数的不确定性估计是充分的。下一步，考虑将GLUE应用到新安江模型的参数不确定性估计上。


## 结论


总的来说，GLUE方法是一种穷举法，只要抽样次数足够多，必然能得到参数的分布特点，是单峰还是多峰，或者是几乎平直，这种特点很好地反映了参数的敏感性，也能规避寻优算法造成的“异参同效”现象，并能准确地估计参数。但是，采样次数的增多，意味着需要运行相同次数的待分析模型，如果模型较为复杂，单次运行较为费事，那将是一笔巨大的时间开销。因此可以考虑CPU并行，利用CPU多核特点，同时运行多个待分析模型；如果单次模型运行可以采用GPU并行加速，那就更能提高运行的效率了。


## 参考文献


[1]Keith Beven. Generalised Likelihood Uncertainty Estimation (GLUE), http://www.uncertain-future.org.uk/?page\_id=131  
 [2]Beven, K. and Binley, A. (1992), The future of distributed models: Model calibration and uncertainty prediction. Hydrol. Process., 6: 279-298. https://doi.org/10.1002/hyp.3360060305  
 [3]Freer, J., Beven, K., and Ambroise, B. (1996), Bayesian Estimation of Uncertainty in Runoff Prediction and the Value of Data: An Application of the GLUE Approach, Water Resour. Res., 32( 7), 2161– 2173, doi:10.1029/95WR03723.  
 [4]黄国如,解河海.基于GLUE方法的流域水文模型的不确定性分析[J].华南理工大学学报(自然科学版),2007(03):137-142+149.  
 [5]莫兴国,刘苏峡. GLUE方法及其在水文不确定性分析中的应用[C]//.水问题的复杂性与不确定性研究与进展——第二届全国水问题研究学术研讨会论文集.,2004:151-158.  
 [6]王书功. 水文模型参数估计方法及参数估计不确定性研究[M]. 河南:黄河水利出版社,2010.  
 [7]宋晓猛. 流域水文模型参数不确定性量化理论方法与应用[M]. 中国水利水电出版社, 2014.  
 [8]Keith Beven, 刘艳丽, 许钦, 王国庆. 环境模拟 : 一个不确定的未来? : Environmental modelling : an uncertain future?[M]. 中国水利水电出版社, 2015.


## 源码


### GLUE.h



```
#pragma once

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
文件名: GLUE.h
作者: 卢家波
单位：河海大学水文水资源学院
邮箱：lujiabo@hhu.edu.cn
QQ：1847096852
版本：2022.4 创建 V1.0
版权: MIT
引用格式：卢家波，GLUE算法C++实现. 南京：河海大学，2022.
 LU Jiabo, Generalized Likelihood Uncertainty Estimation in C++. Nanjing:Hohai University, 2022.
参考文献：[1]Keith Beven. Generalised Likelihood Uncertainty Estimation (GLUE), http://www.uncertain-future.org.uk/?page\_id=131
 [2]Beven, K. and Binley, A. (1992), The future of distributed models: Model calibration and uncertainty prediction. Hydrol. Process., 6: 279-298. https://doi.org/10.1002/hyp.3360060305
 [3]Freer, J., Beven, K., and Ambroise, B. (1996), Bayesian Estimation of Uncertainty in Runoff Prediction and the Value of Data: An Application of the GLUE Approach, Water Resour. Res., 32( 7), 2161– 2173, doi:10.1029/95WR03723.
 [4]黄国如,解河海.基于GLUE方法的流域水文模型的不确定性分析[J].华南理工大学学报(自然科学版),2007(03):137-142+149.
 [5]莫兴国,刘苏峡. GLUE方法及其在水文不确定性分析中的应用[C]//.水问题的复杂性与不确定性研究与进展——第二届全国水问题研究学术研讨会论文集.,2004:151-158.
 [6]王书功. 水文模型参数估计方法及参数估计不确定性研究[M]. 河南:黄河水利出版社,2010.
 [7]宋晓猛. 流域水文模型参数不确定性量化理论方法与应用[M]. 中国水利水电出版社, 2014.
 [8]Keith Beven, 刘艳丽, 许钦, 王国庆. 环境模拟 : 一个不确定的未来? : Environmental modelling : an uncertain future?[M]. 中国水利水电出版社, 2015.

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/

#include <vector>
#include <string>

class GLUE
{
public:
	//普适似然不确定性估计（GLUE）函数
	void estimate();

private:
	//===============1.输入估计参数及实测值===============
	void setup();

	//====================2.运行模型====================
	void run();

	//===================3.不确定性估计===================
	void analysis();

	//===================4.输出估计结果===================
	void save();

	//在参数空间内生成参数组
	void GenerateParamSet();
	
	//调用待分析模型，得到模拟值
	void CallModel();

	//计算似然函数值
	void CalculateObjFun();

	//计算精度评定指标纳什系数
	void CalculateNSE();

	//计算似然函数值对应的归一化权重
	void CalculateWeight();

	//绘制参数-似然函数值的散点图
	void PlotDottyPlot();

	//绘制预测分位数
	void PlotPredictionQuantiles();

	//经验累积分布函数（CDF）的预测
	void ecdfPred();

	//按模拟值的升序对一组权重数组和对应的模拟值数组进行排序
	void sort(std::vector<double>& w, std::vector<double>& x);
	
	//计算权重累加之和
	std::vector<double> cumsum(const std::vector<double>& w);
	
	//计算各百分位数下的模拟值
	std::vector<double> CalPercentiles(const std::vector<double>& modValue);

	//通过置信水平计算百分位数
	void SetPercentile();

	//读取参数名、下限、上限
	void ReadParamInfo(std::ifstream& fin);

	//读取实测值
	void ReadObsData(std::ifstream& fin);

	//===================成员变量===================

	//每个变量的参数名、下限、上限、参数组、阈值以上参数组数组
	std::vector<double> lowerParameter;
	std::vector<double> upperParameter;
	std::vector<double> parameterSet;
	std::vector<std::string> parameterName;
	std::vector< std::vector<double> > parameterSetArray;
	std::vector< std::vector<double> > parSetArrayAboveThreshold;

	//实测值
	std::vector<double> observedData;

	//每次待分析模型运行得到的模拟值、模拟值数组、似然函数值NSE在阈值以上对应的模拟值数组
	std::vector<double> modelledData;
	std::vector< std::vector<double> > modelledDataArray;
	std::vector< std::vector<double> > modDataArrayAboveThreshold;

	//似然函数值、在阈值以上的似然函数值、在阈值以上的似然函数值NSE对应的归一化似然函数值即权重
	std::vector<double> NSE;
	std::vector<double> nseAboveThreshold;
	std::vector<double> weight;

	//不确定性边界预测
	std::vector<double> percentile;  //置信区间的百分位数
	std::vector<double> ecdf;        //经验累积分布函数
	std::vector<double> sampLowerPtile;  //低分位数
	std::vector<double> sampUpperPtile;  //高分位数
	std::vector<double> sampMedian;      //50%分位数

	//不确定性估计参数
	int simulationNumber;   //模拟次数
	int parameterNumber;    //参数个数
	int modelledDataSize;   //单次模拟值个数
	double threshold;       //精度指标有效阈值
	double confidenceLevel; //置信水平
};




```

### GLUE.cpp



```
/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
文件名: GLUE.cpp
作者: 卢家波
单位：河海大学水文水资源学院
邮箱：lujiabo@hhu.edu.cn
QQ：1847096852
版本：2022.4 创建 V1.0
版权: MIT
引用格式：卢家波，GLUE算法C++实现. 南京：河海大学，2022.
 LU Jiabo, Generalized Likelihood Uncertainty Estimation in C++. Nanjing:Hohai University, 2022.
参考文献：[1]Keith Beven. Generalised Likelihood Uncertainty Estimation (GLUE), http://www.uncertain-future.org.uk/?page\_id=131
 [2]Beven, K. and Binley, A. (1992), The future of distributed models: Model calibration and uncertainty prediction. Hydrol. Process., 6: 279-298. https://doi.org/10.1002/hyp.3360060305
 [3]Freer, J., Beven, K., and Ambroise, B. (1996), Bayesian Estimation of Uncertainty in Runoff Prediction and the Value of Data: An Application of the GLUE Approach, Water Resour. Res., 32( 7), 2161– 2173, doi:10.1029/95WR03723.
 [4]黄国如,解河海.基于GLUE方法的流域水文模型的不确定性分析[J].华南理工大学学报(自然科学版),2007(03):137-142+149.
 [5]莫兴国,刘苏峡. GLUE方法及其在水文不确定性分析中的应用[C]//.水问题的复杂性与不确定性研究与进展——第二届全国水问题研究学术研讨会论文集.,2004:151-158.
 [6]王书功. 水文模型参数估计方法及参数估计不确定性研究[M]. 河南:黄河水利出版社,2010.
 [7]宋晓猛. 流域水文模型参数不确定性量化理论方法与应用[M]. 中国水利水电出版社, 2014.
 [8]Keith Beven, 刘艳丽, 许钦, 王国庆. 环境模拟 : 一个不确定的未来? : Environmental modelling : an uncertain future?[M]. 中国水利水电出版社, 2015.

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/

#include <iostream>
#include <fstream>
#include <random>
#include <numeric>
#include <string>
#include <algorithm>
#include <iomanip>

#include "GLUE.h"
#include "matplotlibcpp.h" //matplotlib的C++接口，用法见【C++】11 Visual Studio 2019 C++安装matplotlib-cpp绘图（https://blog.csdn.net/weixin\_43012724/article/details/124051588）


namespace plt = matplotlibcpp;


void GLUE::estimate()
{
	setup();

	run();

	analysis();

	save();
}

void GLUE::setup()
{
	std::ifstream fin("setup.txt");

	std::string temp = "";  //用于读取文字部分

	fin >> temp >> simulationNumber;

	fin >> temp >> parameterNumber;

	fin >> temp >> threshold;

	fin >> temp >> confidenceLevel;

	SetPercentile(); //初始化分位数

	fin >> temp >> temp >> temp;

	ReadParamInfo(fin);

	fin >> temp;

	ReadObsData(fin);

	fin.close();
}

void GLUE::run()
{
	for (size_t i = 0; i < simulationNumber; ++i)
	{
		//按照均匀分布在参数空间生成参数组
		GenerateParamSet();

	    //计算模型模拟结果
		CallModel();

		//计算目标函数
		CalculateObjFun();
	}
}

void GLUE::analysis()
{
	CalculateWeight();
	
	PlotDottyPlot();

	PlotPredictionQuantiles();

}

void GLUE::save()
{
	std::ofstream fout("output.txt");
	for (const std::string& name : parameterName)
	{
		fout << name << "\t";
	}

	fout << "NSE" << "\t" << "模拟值" << std::endl;

	fout.setf(std::ios::fixed); 
	fout.precision(2); //保留两位小数

	for (size_t i = 0; i < simulationNumber; i++)
	{
		for (double param : parameterSetArray.at(i))
		{
			fout << param << "\t";
		}

		fout << NSE.at(i) << "\t";

		for (double modData : modelledDataArray.at(i))
		{
			fout << modData << "\t";
		}

		fout << std::endl;
	}

	fout.close();
}

void GLUE::GenerateParamSet()
{
	std::random_device r;
	std::default_random_engine e(r());

	for (size_t i = 0; i < parameterNumber; ++i)
	{
		double lower = lowerParameter.at(i);
		double upper = upperParameter.at(i);
		std::uniform_real_distribution<double> uniformDist(lower, upper);
		double param = uniformDist(e);
		parameterSet.emplace\_back(param);
	}

	parameterSetArray.emplace\_back(parameterSet);
}

void GLUE::CallModel()
{
	//测试模型为 y = k \* x + b, x in [-5, 5]
	for (int x = -5; x <= 5; ++x)
	{
		double temp = parameterSet[0] \* x + parameterSet[1];

		modelledData.emplace\_back(temp);
	}

	modelledDataArray.emplace\_back(modelledData);
}

void GLUE::CalculateObjFun()
{
	CalculateNSE();

	parameterSet.clear();
	modelledData.clear();
}

void GLUE::CalculateNSE()
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

void GLUE::CalculateWeight()
{
	for (size_t i = 0; i < NSE.size(); ++i)
	{
		double nse = NSE.at(i);
		std::vector<double> parSet = parameterSetArray.at(i);
		std::vector<double> modData = modelledDataArray.at(i);

		if (nse > threshold)
		{
			nseAboveThreshold.emplace\_back(nse);

			parSetArrayAboveThreshold.emplace\_back(parSet);

			modDataArrayAboveThreshold.emplace\_back(modData);
		}
	}

	double nseAboveThresholdSum = std::accumulate(nseAboveThreshold.begin(), nseAboveThreshold.end(), 0.0);

	for (auto nse : nseAboveThreshold)
	{
		double wgt = nse / nseAboveThresholdSum;

		weight.emplace\_back(wgt);
	}
}

void GLUE::PlotDottyPlot()
{
	for (size_t i = 0; i < parameterNumber; ++i)
	{
		std::vector<double> parAboveThreshold;
		for (auto parSet : parSetArrayAboveThreshold)
		{
			parAboveThreshold.emplace\_back(parSet.at(i));
		}
		
		double minPar = \*std::min\_element(parAboveThreshold.begin(), parAboveThreshold.end());
		double maxPar = \*std::max\_element(parAboveThreshold.begin(), parAboveThreshold.end());
		
		auto iter = std::max\_element(nseAboveThreshold.begin(), nseAboveThreshold.end());
		int index = iter - nseAboveThreshold.begin();
		double minNSE = \*std::min\_element(nseAboveThreshold.begin(), nseAboveThreshold.end());
		double maxNSE = \*std::max\_element(nseAboveThreshold.begin(), nseAboveThreshold.end());
		
		std::string parName = parameterName.at(i);
		std::vector<double> xpoint = { parAboveThreshold.at(index)};
		std::vector<double> ymax = { maxNSE };
		std::string text = "(" + std::to\_string(xpoint.front()) + ", " + std::to\_string(ymax.front()) + ")";
		
		plt::scatter(parAboveThreshold, nseAboveThreshold);
		plt::scatter(xpoint, ymax, 50, { {"color", "red"}, {"marker", "\*"}});
		plt::text(xpoint.front(), ymax.front(), text);
		plt::xlabel(parameterName.at(i));
		plt::ylabel("NSE");
		plt::xlim(minPar, maxPar);
		plt::ylim(minNSE, maxNSE + 0.02);
		plt::save("ScatterPlot" + std::to\_string(i) + ".png"); 
		plt::show();
	}
}

void GLUE::PlotPredictionQuantiles()
{
	modelledDataSize = modelledDataArray[0].size();

	ecdfPred();

	std::vector<int> xAxis = { -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5 };
	plt::plot(xAxis, sampLowerPtile);
	plt::plot(xAxis, sampUpperPtile);
	plt::plot(xAxis, sampMedian);
	plt::scatter(xAxis, observedData, 50, { {"color", "red"}, {"marker", "o"} });
	plt::title("Median and " + std::to\_string(static\_cast<int>(confidenceLevel \* 100)) + "% GLUE prediction limits with NSE");
	plt::xlabel("x");
	plt::ylabel("y");
	plt::save("bound.png");
	plt::show();

}

//经验累积分布函数的预测
void GLUE::ecdfPred()
{
	for (size_t i = 0; i < modelledDataSize; ++i)
	{
		std::vector<double> modValue; //某时刻所有在阈值上的模拟值
		for (auto modData : modDataArrayAboveThreshold)
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

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 sort()
 按模拟值的升序对一组权重数组和对应的模拟值数组进行排序

 输入参数列表：
 W(.) = 权重数组
 X(.) = W(.)对应的模拟值数组

 局部变量列表：
 XXF(.) = 存储坐标和函数值对的数组
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
void GLUE::sort(std::vector<double>& w, std::vector<double>& x)
{
	//将权重与模拟值对应上，以便于扩展排序
	std::vector< std::pair<double, double> > wx;

	//构造向量wx，存储权重和模拟值对
	for (size_t i = 0; i < w.size(); ++i)
	{
		wx.emplace\_back(w[i], x[i]);
	}

	//匿名函数定义比较规则，用于pair数据结构按照模拟值升序排列
	std::sort(wx.begin(), wx.end(), [](auto a, auto b) { return a.second < b.second; });

	//将排序后的wx赋值给权重数组w和函模拟值数组x
	for (size_t i = 0; i < wx.size(); i++)
	{
		w[i] = wx[i].first;  //取出权重

		x[i] = wx[i].second;   //取出模拟值
	}
}

std::vector<double> GLUE::cumsum(const std::vector<double>& w)
{
	std::vector<double> ecdf;

	for (size_t i = 0; i < w.size(); i++)
	{
		double wgt = std::accumulate(w.begin(), w.begin() + i, 0.0);

		ecdf.emplace\_back(wgt);
	}

	return ecdf;
}

std::vector<double> GLUE::CalPercentiles(const std::vector<double>& modValue)
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

void GLUE::SetPercentile()
{

	double lowerBound, upperBound;

	confidenceLevel /= 100.0;

	lowerBound = (1 - confidenceLevel) / 2.0;

	upperBound = 1 - lowerBound;

	percentile = { lowerBound, upperBound, 0.5 };
}

void GLUE::ReadParamInfo(std::ifstream& fin)
{
	for (size_t i = 0; i < parameterNumber; ++i)
	{
		std::string name = "param";
		double lower = 0.0;
		double upper = 0.0;

		fin >> name >> lower >> upper;

		parameterName.emplace\_back(name);
		lowerParameter.emplace\_back(lower);
		upperParameter.emplace\_back(upper);
	}
}

void GLUE::ReadObsData(std::ifstream& fin)
{
	double obs = 0.0;

	while (fin >> obs)
	{
		observedData.emplace\_back(obs);
	}
}




```

### main.cpp



```
#include <iostream>

#include "GLUE.h"

int main()
{

	GLUE test;

	clock_t timeBegin = clock();

	test.estimate();

	clock_t timeEnd = clock();

	double duration = static\_cast<double>(timeEnd - timeBegin) / CLOCKS_PER_SEC;

	std::cout << std::endl << "优化用时/S: " << duration << std::endl;

	std::cout << "普适似然不确定性估计(GLUE)程序结束" << std::endl;
}

```

## 目录结构


本地目录：`E:\Research\OptA\GLUE`


`RefCode`：


* GLUE-with-swmm5-main、pyGLUE-master：从 Github 上搜集到的GLUE算法实现代码；
* R-GLUE、R-GLUE\_TS：从http://www.uncertain-future.org.uk/?page\_id=131下载得到的R语言代码
* CREDIBLE\_UE\_TOOLBOX\_R1.1：从https://www.lancaster.ac.uk/lec/sites/qnfm/credible/DownloadCure.htm 下载得到的MATLAB工具箱CURE


`RefPaper`：参考文献


`GLUE`：自己用C++编写的普适似然不确定性估计程序，主要参考R-GLUE\_TS


## 进度


2022/4/2 看完参考文献 Beven 经典文章，从Github下载源码，准备用于实现算法的参考


2022/4/4 给Beven写邮件索取GLUE源码，回复我网址 uncertain-future.org.uk 和 《环境模拟 一个不确定的未来？》这本书，从网站下载得到R语言和MATLAB的源码


2022/4/6 成功运行GLUE的R语言程序，下一步尝试编写C++版本的程序


2022/4/7 用C++编写GLUE程序，完成setup()、run()函数


2022/4/8 完成C++调用matplotlib库测试，可用于analysis()函数绘图


2022/4/11 完成GLUE的散点图部分C++程序编写，还剩余误差边界线的绘制部分


2022/4/12 完成GLUE的误差边界线的绘制与保存，完成了线性回归参数估计的GLUE实例。





