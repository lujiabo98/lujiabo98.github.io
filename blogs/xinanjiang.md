---
layout: page
permalink: /blogs/xinanjiang/index.html
title: xinanjiang
---

## 模型简介

> 新安江模型是河海大学水文院赵人俊教授于上世纪80年代提出的一个具有世界影响力的水文模型[1](#fn1)[2](#fn2)[3](#fn3)[4](#fn4)。新安江模型是分布式模型，可用于湿润地区与半湿润地区的湿润季节。当流域面积较小时，新安江模型采用集总模型，当面积较大时，采用分块模型。它把全流域分为许多块单元流域，对每个单元流域作产汇流计算，得出单元流域的出口流量过程。再进行出口以下的河道洪水演算，求得流域出口的流量过程。把每个单元流域的出流过程相加，就求得了流域的总出流过程。  
> 该模型按照**三层蒸散发模式**计算流域蒸散发，按**蓄满产流概念**计算降雨产生的总径流量，采用流域蓄水曲线考虑下垫面不均匀对产流面积变化的影响。在径流成分划分方面，对三水源情况，按“山坡水文学”产流理论用一个具有有限容积和测孔、底孔的自由水蓄水库把总径流划分成**饱和地面径流**、**壤中水径流**和**地下水径流**。在汇流计算方面，单元面积的地面径流汇流一般采用**单位线法**，壤中水径流和地下水径流的汇流则采用**线性水库法**。河网汇流一般采用分段连续演算的**Muskingum法**或滞时演算法[5](#fn5)。


## 源码下载


该项目已在Github开源，您可以点击[此处](https://github.com/lujiabo98/XinanjiangModel)下载。


个人编写，难免有理解不到位和疏漏的地方，敬请谅解，仅供参考！  
 感谢导师、大李老师、小李老师、姚老师、小蕾在模型原理、程序设计、率定验证等方面的指导！


如有问题，欢迎交流探讨！ 邮箱：lujiabo2017@gmail.com 卢家波  
 来信请说明博客标题及链接，谢谢。


## 设计思路


严格按照“流域分单元，蒸散发分层次，产流分水源，汇流分阶段”的建模流程，以面向对象的方式编写三水源新安江模型C++程序。


将流域分块、蒸散发、产流、汇流等模块封装成单独的C++类，便于移植和复用。


## 程序验证


![在这里插入图片描述](https://img-blog.csdnimg.cn/ac26cb44fdc94ee6911ba60f50495217.png#pic_center)


## 程序模块


以下仅为主要模块代码，完整程序请至[Github](https://github.com/lujiabo98/XinanjiangModel)下载。


### 流域分块


#### Watershed.h

```
#pragma once
#include <string>
#include <vector>

class IO    //从文本中导入降雨和蒸发数据，输出流量过程到文本中
{
public:

	void ReadFromFile(std::string StrPath = "");   //从文本中导入流域降雨或蒸发数据

	void WriteToFile(std::string StrPath = "");    //输出流域出口断面流量过程到文本中

	IO();

	~IO();

public:

	std::vector< std::vector<double> > m_P;  //各雨量站逐时段降雨量，mm

	std::vector< std::vector<double> > m_EM;  //各蒸发站逐时段水面蒸发量，mm

	double\* m_Q;    //流域出口断面流量过程，m3/s

	int nrows;     //数据行数

	int ncols;     //数据列数

};

//流域分块
class Watershed
{
public:
	void ReadFromFile(std::string StrPath = "");
	
	void SetValues(std::string name, double area, 
		int numRainfallStation, int numEvaporationStation, int numSubWatershed,
		double\* areaSubWatershed, std::vector< std::vector<double> > rateRainfallStation,
		std::vector< std::vector<double> > rateEvaporationStation,
		std::string\* nameRainfallStation, std::string\* nameEvaporationStation,
		std::vector< std::vector<double> > P, std::vector< std::vector<double> > EM);

	void calculate(const IO \* io);   //计算各单元流域降雨量和水面蒸发量

	double GetP(int nt, int nw);   //得到nt时刻，第nw单元流域的降雨量，mm

	double GetEM(int nt, int nw);   //得到nt时刻，第nw单元流域的水面蒸发量，mm

	double GetF(int nw);    //得到第nw单元流域的面积，km2

	int GetnW();   //得到单元流域个数

	Watershed();

	~Watershed();

protected:
private:
	std::string m_name;	//流域名称

	double m_area;	//流域控制面积，km2

	int m_numRainfallStation;		//雨量站个数

	int m_numEvaporationStation;    //蒸发站个数

	int m_numSubWatershed;	//单元流域个数

	double\* m_areaSubWatershed;	//单元流域面积，km2

	std::vector< std::vector<double> > m_rateRainfallStation;  //各单元流域对应各雨量站比例，按泰森多边形分配

	std::vector< std::vector<double> > m_rateEvaporationStation;  //各单元流域对应各蒸发站比例，按泰森多边形分配

	std::string\* m_nameRainfallStation;	//雨量站点名称

	std::string\* m_nameEvaporationStation;  //蒸发站点名称

	std::vector< std::vector<double> > m_P;  //各单元流域逐时段降雨量，mm

	std::vector< std::vector<double> > m_EM;  //各单元流域逐时段水面蒸发量，mm

};



```

#### Watershed.cpp



```
#include "Watershed.h"
#include <fstream>

void Watershed::ReadFromFile(std::string StrPath)
{
	std::ifstream fin(StrPath + "watershed.txt");

	fin >> m_name
		>> m_area
		>> m_numRainfallStation
		>> m_numEvaporationStation
		>> m_numSubWatershed;

	//读取单元流域面积
	m_areaSubWatershed = new double[m_numSubWatershed];

	for (int i = 0; i < m_numSubWatershed; i++)
	{
		fin >> m_areaSubWatershed[i];
	}

	//读取雨量站分配比例
	//创建子流域行\*雨量站列的二维动态向量，并初始化为0
	m_rateRainfallStation = 
		std::vector< std::vector<double> >(m_numSubWatershed, std::vector<double>(m_numRainfallStation, 0.0));

	for (int i = 0; i < m_numSubWatershed; i++)
	{
		for (int j = 0; j < m_numRainfallStation; j++)
		{
			fin >> m_rateRainfallStation[i][j];
		}
	}

	//读取蒸发站分配比例
    //创建子流域行\*蒸发站列的二维动态向量，并初始化为0
	m_rateEvaporationStation = 
		std::vector< std::vector<double> >(m_numSubWatershed, std::vector<double>(m_numEvaporationStation, 0.0));

	for (int i = 0; i < m_numSubWatershed; i++)
	{
		for (int j = 0; j < m_numEvaporationStation; j++)
		{
			fin >> m_rateEvaporationStation[i][j];
		}
	}

	//读取雨量站点名称
	m_nameRainfallStation = new std::string[m_numRainfallStation];

	for (int j = 0; j < m_numRainfallStation; j++)
	{
		fin >> m_nameRainfallStation[j];
	}

	//读取蒸发站点名称
	m_nameEvaporationStation = new std::string[m_numEvaporationStation];

	for (int j = 0; j < m_numEvaporationStation; j++)
	{
		fin >> m_nameEvaporationStation[j];
	}

	fin.close();
}

void Watershed::SetValues(std::string name, double area,
	int numRainfallStation, int numEvaporationStation, int numSubWatershed,
	double\* areaSubWatershed, std::vector< std::vector<double> > rateRainfallStation,
	std::vector< std::vector<double> > rateEvaporationStation,
	std::string\* nameRainfallStation, std::string\* nameEvaporationStation,
	std::vector< std::vector<double> > P, std::vector< std::vector<double> > EM)
{
	m_name = name;

	m_area = area;

	m_numRainfallStation = numRainfallStation;

	m_numEvaporationStation = numEvaporationStation;

	m_numSubWatershed = numSubWatershed;

	m_areaSubWatershed = areaSubWatershed;

	m_rateRainfallStation = rateRainfallStation;

	m_rateEvaporationStation = rateEvaporationStation;

	m_nameRainfallStation = nameRainfallStation;

	m_nameEvaporationStation = nameEvaporationStation;

	m_P = P;

	m_EM = EM;
}

void Watershed::calculate(const IO \* io)
{
	int nrows = io->nrows;   //记录条数，即时段数

	int ncols = m_numSubWatershed;    //单元流域个数

	//计算各单元流域逐时段降雨量，mm
	m_P = std::vector< std::vector<double> >(nrows, std::vector<double>(ncols, 0.0)); 

	for (int r = 0; r < nrows; r++)
	{
		for (int c = 0; c < ncols; c++)
		{
			for (int i = 0; i < m_numRainfallStation; i++)
			{
				m_P[r][c] += io->m_P[r][i] \* m_rateRainfallStation[c][i];   //按比例计算单元流域降雨量
			}
		}
	}

	//计算各单元流域逐时段水面蒸发量，mm
	m_EM = std::vector< std::vector<double> >(nrows, std::vector<double>(ncols, 0.0));

	for (int r = 0; r < nrows; r++)
	{
		for (int c = 0; c < ncols; c++)
		{
			for (int i = 0; i < m_numEvaporationStation; i++)
			{
				m_EM[r][c] += io->m_EM[r][i] \* m_rateEvaporationStation[c][i];   //按比例计算单元流域水面蒸发量
			}
		}
	}
}

double Watershed::GetP(int nt, int nw)
{
	return m_P[nt][nw];
}

double Watershed::GetEM(int nt, int nw)
{
	return m_EM[nt][nw];
}

double Watershed::GetF(int nw)
{
	return m_areaSubWatershed[nw];
}

int Watershed::GetnW()
{
	return m_numSubWatershed;
}

Watershed::Watershed()
{
	m_name = "默认流域";

	m_area = 0.0;

	m_numRainfallStation = 0;

	m_numEvaporationStation = 0;

	m_numSubWatershed = 0;

	m_areaSubWatershed = nullptr;

	m_rateRainfallStation = 
		std::vector< std::vector<double> >(m_numSubWatershed, std::vector<double>(m_numRainfallStation, 0.0));

	m_rateEvaporationStation = 
		std::vector< std::vector<double> >(m_numSubWatershed, std::vector<double>(m_numEvaporationStation, 0.0));

	m_nameRainfallStation = nullptr;

	m_nameEvaporationStation = nullptr;

	m_P = std::vector< std::vector<double> >(0, std::vector<double>(m_numSubWatershed, 0.0));

	m_EM = std::vector< std::vector<double> >(0, std::vector<double>(m_numSubWatershed, 0.0));
}

Watershed::~Watershed()
{
	delete[] m_areaSubWatershed;

}


void IO::ReadFromFile(std::string StrPath)
{
	std::ifstream fin(StrPath + "P.txt");

	//读入降雨数据的行数和列数，行数表示时段数，列数表示雨量站数
	fin >> nrows >> ncols;

	//创建动态二维数据，用于存储各雨量站降雨量，mm
	m_P = std::vector< std::vector<double> >(nrows, std::vector<double>(ncols, 0.0));

	//读取各雨量站降雨量数据，mm
	for (int r = 0; r < nrows; r++)
	{
		for (int c = 0; c < ncols; c++)
		{
			fin >> m_P[r][c];
		}
	}

	fin.close();

	fin.open(StrPath + "EM.txt");

	//读入蒸发数据的行数和列数，行数表示时段数，列数表示蒸发站数
	fin >> nrows >> ncols;

	//创建动态二维数据，用于存储各蒸发站蒸发量，mm
	m_EM = std::vector< std::vector<double> >(nrows, std::vector<double>(ncols, 0.0));

	//读取各蒸发站蒸发量数据，mm
	for (int r = 0; r < nrows; r++)
	{
		for (int c = 0; c < ncols; c++)
		{
			fin >> m_EM[r][c];
		}
	}

	fin.close();
}

void IO::WriteToFile(std::string StrPath)
{
	//打开Q.txt输出流域出口断面流量过程，没有该文件则新建
	std::ofstream fout(StrPath + "Q.txt");

	//输出流量过程
	for (int i = 0; i < nrows; i++)
	{
		fout << m_Q[i] << std::endl;
	}

	fout.close();
}

IO::IO()
{
	m_Q = nullptr;

	nrows = 0;

	ncols = 0;

	m_P = std::vector< std::vector<double> >(nrows, std::vector<double>(ncols, 0.0));

	m_EM = std::vector< std::vector<double> >(nrows, std::vector<double>(ncols, 0.0));
}

IO::~IO()
{
	delete[] m_Q;
}

```

### 蒸散发


#### Evapotranspiration.h



```
#pragma once
#include "Data.h"

//三层蒸散发模型
class Evapotranspiration
{
public:
	void SetParmameter(const Parameter \* parameter);   //设置参数

	void SetState(const State\* state);   //设置状态

	void UpdateState(State\* state);    //更新状态

	void calculate();    //计算三层蒸散发量

	Evapotranspiration(double kc = 0.8, double lm = 80.0, double c = 0.15,
		double wu = 10, double wl = 30, double ep = 0.0,
		double e = 0.0, double eu = 0.0, double el = 0.0, double ed = 0.0, 
		double p = 0.0, double em = 0.0);	//构造函数

	~Evapotranspiration();	 //析构函数

protected:
private:
	//========模型参数========//

	double KC;   //流域蒸散发折算系数，敏感

	double LM;   //下层张力水容量/mm，敏感，60~90

	double C;    //深层蒸散发折算系数，不敏感，0.10~0.20

	//========模型状态========//

	double WU;   //上层张力水蓄量，mm

	double WL;   //下层张力水蓄量，mm

	double EP;   //单元流域蒸发能力，mm

	double E;    //总的蒸散发量，mm

	double EU;   //上层蒸散发量，mm

	double EL;   //下层蒸散发量，mm

	double ED;   //深层蒸散发量，mm


	//========外部输入========//

	double P;    //降雨量，mm

	double EM;   //水面蒸发量，mm

};

```

#### Evapotranspiration.cpp



```
#include "Evapotranspiration.h"

void Evapotranspiration::SetParmameter(const Parameter\* parameter)
{
	KC = parameter->m_KC;

	LM = parameter->m_LM;

	C = parameter->m_C;
}

void Evapotranspiration::SetState(const State\* state)
{
	WU = state->m_WU;

	WL = state->m_WL;


	P = state->m_P;

	EM = state->m_EM;

}

void Evapotranspiration::UpdateState(State\* state)
{
	state->m_EP = EP;

	state->m_E = E;

	state->m_EU = EU;

	state->m_EL = EL;

	state->m_ED = ED;
}

void Evapotranspiration::calculate()
{
	//三层蒸散发计算
	EP = KC \* EM;    //计算流域蒸发能力

	if (P + WU >= EP)
	{
		EU = EP;
		EL = 0;
		ED = 0;
	}
	else
	{
		EU = P + WU;

		if (WL >= C \* LM)
		{
			EL = (EP - EU) \* WL / LM;
			ED = 0;
		}
		else
		{
			if (WL >= C \* (EP - EU))
			{
				EL = C \* (EP - EU);
				ED = 0;
			}
			else
			{
				EL = WL;
				ED = C \* (EP - EU) - EL;
			}
		}
	}

	//计算总的蒸散发量
	E = EU + EL + ED;
}


Evapotranspiration::Evapotranspiration(double kc, double lm, double c, 
	double wu, double wl, double ep,
	double e, double eu, double el, double ed, 
	double p, double em)
{
	KC = kc;

	LM = lm;

	C = c;


	WU = wu;

	WL = wl;

	EP = ep;

	E = e;

	EU = eu;

	EL = el;

	ED = ed;


	P = p;

	EM = em;
}

Evapotranspiration::~Evapotranspiration()
{

}

```

### 产流


#### Source.h



```
#pragma once
#include "Data.h"

//水源划分
class Source
{
public:

	void SetParmameter(const Parameter\* parameter);   //设置参数

	void SetState(const State\* state);   //设置状态

	void UpdateState(State\* state);    //更新状态

	void calculate();   //划分三水源

	Source(double sm = 20.0, double ex = 1.5, double kg = 0.35, double ki = 0.35,
		double r = 0.0, double rs = 0.0, double ri = 0.0, double rg = 0.0,
		double pe = 0.0, double fr = 0.0, double s0 = 0.0, double s = 0.0,
		double m = 0.0, double kid = 0.0, double kgd = 0.0,
		double smm = 0.0, double smmf = 0.0, double smf = 0.0, double au = 0.0,
		double rsd = 0.0, double rid = 0.0, double rgd = 0.0, double fr0 = 0.0,
		int n = 1, double q = 0.0, double kidd = 0.0, double kgdd = 0.0, double dt_ = 0.0);

	~Source();

protected:
private:
	//========模型参数========//

	double SM;   //表层自由水蓄水容量/mm ，敏感

	double EX;   //表层自由水蓄水容量方次，不敏感，1.0~1.5

	double KG;   //表层自由水蓄水库对地下水的日出流系数，敏感

	double KI;   //表层自由水蓄水库对壤中流的日出流系数，敏感

	//========模型状态========//

	double R;	 //总径流量，mm

	double RS;   //地面径流，mm

	double RI;   //壤中流，mm

	double RG;   //地下径流，mm

	double PE;	 //净雨量，mm

	double FR;	 //本时段产流面积比例

	double S0;   //本时段初的自由水蓄量，mm

	double S;    //本时段的自由水蓄量，mm


	double M;    //一天划分的计算时段数

	double KID;   //表层自由水蓄水库对壤中流的计算时段出流系数，敏感

	double KGD;    //表层自由水蓄水库对地下水的计算时段出流系数，敏感

	double SMM;    //全流域单点最大的自由水蓄水容量，mm

	double SMMF;  //产流面积上最大一点的自由水蓄水容量，mm

	double SMF;   //产流面积上的平均自由水蓄水容量深，mm

	double AU;   //相应平均蓄水深的最大蓄水深，S0值对应的纵坐标，mm

	double RSD;  //计算步长地面径流，mm

	double RID;  //计算步长壤中流，mm

	double RGD;  //计算步长地下径流，mm

	double FR0;   //上一时段产流面积比例

	int N;       //N 为计算时段分段数，每一段为计算步长

	double Q;     //Q 是每个计算步长内的净雨量，mm

	double KIDD;   //表层自由水蓄水库对壤中流的计算步长出流系数，敏感

	double KGDD;   //表层自由水蓄水库对地下水的计算步长出流系数，敏感

	double dt;   //模型计算时段长,h
};

```

#### Source.cpp



```
#include "Source.h"

void Source::SetParmameter(const Parameter\* parameter)
{
	SM = parameter->m_SM;

	EX = parameter->m_EX;

	KG = parameter->m_KG;

	KI = parameter->m_KI;
}

void Source::SetState(const State\* state)
{
	R = state->m_R;

	PE = state->m_PE;

	FR = state->m_FR;

	S0 = state->m_S0;

	dt = state->m_dt;
}

void Source::UpdateState(State\* state)
{
	state->m_RS = RS;

	state->m_RI = RI;

	state->m_RG = RG;

	state->m_S0 = S;

	state->m_FR = FR;

}

void Source::calculate()
{
	//出流系数换算
	M = 24.0 / dt;   //一天划分的计算时段数

	KID = (1 - pow(1 - (KI + KG), 1.0 / M)) / (1 + KG / KI);   //表层自由水蓄水库对壤中流的计算时段出流系数，敏感

	KGD = KID \* KG / KI;   //表层自由水蓄水库对地下水的计算时段出流系数，敏感

	//三分水源[4]
	if (PE <= 1e-5)
	{
		//净雨量小于等于0时,这里认为净雨量小于1e-5时即为小于等于0
		RS = 0;

		RI = KID \* S0 \* FR;   /\*当净雨量小于等于0时，消耗自由水蓄水库中的水
 产流面积比例仍为上一时段的比例不变\*/
		RG = KGD \* S0 \* FR;

		S = S0 \* (1 - KID - KGD);   //更新下一时段初的自由水蓄量

	}
	else
	{
		//净雨量大于0时
		SMM = SM \* (1 + EX);   //全流域单点最大的自由水蓄水容量，mm

		FR0 = FR;   //上一时段产流面积比例

		FR = R / PE;   //计算本时段产流面积比例

		if (FR > 1)    //如果FR由于小数误差而计算出大于1的情况，则强制置为1
		{
			FR = 1;
		}

		S = S0 \* FR0 / FR;

		N = int(PE / 5.0) + 1;   //N 为计算时段分段数，每一段为计算步长

		Q = PE / N;    //Q 是每个计算步长内的净雨量，mm
		                      //R 是总径流量，PE是单元流域降雨深度。
				              //把R按5mm分，实际操作就是把PE按5mm分，是为了减小产流面积变化的差分误差。
				              //自由水蓄水库只发生在产流面积上，其底宽为产流面积FR，显然FR是随时间变化的。
				              //产流量R进入水库即在产流面积上产生PE的径流深。
				              //FR = f / F = R / PE 

		KIDD = (1 - pow(1 - (KID + KGD), 1.0 / N)) / (1 + KGD / KID);  //表层自由水蓄水库对壤中流的计算步长出流系数，敏感

		KGDD = KIDD \* KGD / KID;  //表层自由水蓄水库对地下水的计算步长出流系数，敏感

		//把该时段的RS、RI、RG置0，用于后续累加计算步长内的RSD、RID、RGD
		RS = 0.0;

		RI = 0.0;

		RG = 0.0;

		//计算产流面积上最大一点的自由水蓄水容量 SMMF
		if (EX == 0.0)
		{
			SMMF = SMM;   //EX等于0时，流域自由水蓄水容量分布均匀
		}
		else
		{
			//假定SMMF与产流面积FR及全流域上最大点的自由水蓄水容量SMM仍为抛物线分布
			//则SMMF应该用下式计算
			SMMF = (1 - pow(1 - FR, 1.0 / EX)) \* SMM;
		}

		SMF = SMMF / (1.0 + EX);

		//将每个计算时段的入流量R分成N段，计算各个计算步长内的RSD、RID、RGD，再累加得到RS、RI、RG
		for (int i = 1; i <= N; i++)
		{
			if (S > SMF)
			{
				S = SMF;
			}

			AU = SMMF \* (1 - pow(1 - S / SMF, 1.0 / (1 + EX)));

			if (Q + AU <= 0)
			{
				RSD = 0;

				RID = 0;

				RGD = 0;

				S = 0;
			}
			else
			{
				if (Q + AU >= SMMF)
				{
					//计算步长内的净雨量进入自由水蓄水库后，使得自由水蓄水深超过产流面积上最大单点自由水蓄水深
					RSD = (Q + S - SMF) \* FR;

					RID = SMF \* KIDD \* FR;

					RGD = SMF \* KGDD \* FR;

					S = SMF \* (1 - KIDD - KGDD);
				}
				else
				{
					//自由水蓄水深未超过产流面积上最大单点自由水蓄水深
					RSD = (S + Q - SMF + SMF \* pow(1 - (Q + AU) / SMMF, 1 + EX)) \* FR;

					RID = (S \* FR + Q \* FR - RSD) \* KIDD;

					RGD = (S \* FR + Q \* FR - RSD) \* KGDD;

					S = S + Q - (RSD + RID + RGD) / FR;
				}
			}

			//累加计算时段内的地面径流、壤中流和地下径流
			RS = RS + RSD;

			RI = RI + RID;

			RG = RG + RGD;
		}
	}
}


Source::Source(double sm, double ex, double kg, double ki,
	double r, double rs, double ri, double rg,
	double pe, double fr, double s0, double s,
	double m, double kid, double kgd,
	double smm, double smmf, double smf, double au,
	double rsd, double rid, double rgd, double fr0,
	int n, double q, double kidd, double kgdd, double dt_)
{
	SM = sm;

	EX = ex;

	KG = kg;

	KI = ki;

	R = r;

	RS = rs;

	RI = ri;

	RG = rg;

	PE = pe;

	FR = fr;

	S0 = s0;

	S = s;

	M = m;

	KID = kid;

	KGD = kgd;

	SMM = smm;

	SMMF = smmf;

	SMF = smf;

	AU = au;

	RSD = rsd;

	RID = rid;

	RGD = rgd;

	FR0 = fr0;

	N = n;

	Q = q;

	KIDD = kidd;

	KGDD = kgdd;

	dt = dt_;
}

Source::~Source()
{

}

```

### 单元流域汇流


#### Confluence.h



```
#pragma once
#include "Data.h"

//单元流域汇流，包括坡地汇流和河网汇流，都采用线性水库法
class Confluence
{
public:
	void SetParmameter(const Parameter\* parameter);   //设置参数

	void SetState(const State\* state);   //设置状态

	void UpdateState(State\* state);    //更新状态

	void calculate();   //坡地汇流和河网汇流

	Confluence(double cs = 0.1, double ci = 0.6, double cg = 0.95, double cr = 0.2, double im = 0.01,
		double qs = 0.0, double qi = 0.0, double qg = 0.0, double qt = 0.0, double qu = 0.0,
		double rs = 0.0, double ri = 0.0, double rg = 0.0, double rim = 0.0,
		double qi0 = 0.0, double qg0 = 0.0, double qu0 = 0.0, double f = 0.0,
		double u = 0.0, double m = 0.0, double csd = 0.0, 
		double cid = 0.0, double cgd = 0.0, double crd = 0.0, double dt_ = 24);

	~Confluence();
protected:
private:
	//========模型参数========//

	double CS;   //地面径流消退系数，敏感

	double CI;   //壤中流消退系数，敏感

	double CG;   //地下水消退系数，敏感

	double CR;   //河网蓄水消退系数，敏感

	double IM;   //不透水面积占全流域面积的比例，不敏感

	//========模型状态========//

	double QS;   //单元流域地面径流，m3/s

	double QI;   //单元流域壤中流，m3/s

	double QG;   //单元流域地下径流，m3/s

	double QT;   //单元流域河网总入流
				 //（进入单元面积的地面径流、壤中流和地下径流之和），m3/s

	double QU;   //单元流域出口流量，m3/s

	double RS;   //地面径流量，mm

	double RI;   //壤中流径流量，mm

	double RG;   //地下径流量，mm

	double RIM;   //不透水面积上的产流量，mm

	double QI0;  //QI(t-1)，前一时刻壤中流，m3/s

	double QG0;  //QG(t-1)，前一时刻地下径流，m3/s

	double QU0;  //QU(t-1)，前一时刻单元流域出口流量，m3/s

	double F;   //单元流域面积，km2

	double U;   //单位转换系数

	double M;   //一天划分的计算时段数

	double CSD;   //计算时段内地面径流蓄水库的消退系数

	double CID;   //计算时段内壤中流蓄水库的消退系数

	double CGD;   //计算时段内地下水蓄水库的消退系数

	double CRD;   //计算时段内河网蓄水消退系数

	double dt;   //模型计算时段长,h
};

```

#### Confluence.cpp



```
#include "Confluence.h"

void Confluence::SetParmameter(const Parameter\* parameter)
{
	CS = parameter->m_CS;

	CI = parameter->m_CI;

	CG = parameter->m_CG;

	CR = parameter->m_CR;

	IM = parameter->m_IM;
}

void Confluence::SetState(const State\* state)
{
	RIM = state->m_RIM;

	RS = state->m_RS;

	RI = state->m_RI;

	RG = state->m_RG;

	QS = state->m_QS;

	QI = state->m_QI;

	QG = state->m_QG;

	QU0 = state->m_QU;

	F = state->m_F;

	dt = state->m_dt;
}

void Confluence::UpdateState(State\* state)
{
	state->m_QS = QS;

	state->m_QI = QI;

	state->m_QG = QG;

	state->m_QU = QU;

	state->m_QU0 = QU0;
}

void Confluence::calculate()
{
	//把透水面积上的产流量均摊到单元流域上

	RS = RS \* (1 - IM);

	RI = RI \* (1 - IM);

	RG = RG \* (1 - IM);

	//出流系数换算

	M = 24.0 / dt;   //一天划分的计算时段数

	CSD = pow(CS, 1.0 / M);   //计算时段内地面径流蓄水库的消退系数

	CID = pow(CI, 1.0 / M);   //计算时段内壤中流蓄水库的消退系数

	CGD = pow(CG, 1.0 / M);   //计算时段内地下水蓄水库的消退系数

	CRD = pow(CR, 1.0 / M);   //计算时段内河网蓄水消退系数

	//坡地汇流
	U = F / 3.6 / 24;   //单位转换系数

	QS = CSD \* QS + (1 - CSD) \* (RS + RIM) \* U;   //地面径流流入地面径流水库，
	                                              //经过消退(CSD)，成为地面径流对河网的总入流QS

	QI = CID \* QI + (1 - CID) \* RI \* U;    //壤中流流入壤中流水库，
										   //经过消退(CID)，成为壤中流对河网的总入流QI

	QG = CGD \* QG + (1 - CGD) \* RG \* U;    //地下径流进入地下水蓄水库，经过地下水
										   //蓄水库的消退(CGD)，成为地下水对河网的总入流QG

	QT = QS + QI + QG;

	//河网汇流，采用线性水库法，且仅当单元流域面积大于200km2时才计算河网汇流
	if (F < 200)
	{
		QU = QT;   //单元流域面积不大且河道较短，对水流运动的调蓄作用通常较小
		           //将这种调蓄作用合并在地面径流和地下径流中一起考虑所带来的误差通常可以忽略
	}
	else
	{
		QU = CRD \* QU + (1 - CRD) \* QT;   //线性水库法
		                                  //只有在单元流域面积较大或流域坡面汇流极其复杂时
		                                  //才考虑单元面积内的河网汇流
	}
}

Confluence::Confluence(double cs, double ci, double cg, double cr, double im,
	double qs, double qi, double qg, double qt, double qu,
	double rs, double ri, double rg, double rim,
	double qi0, double qg0, double qu0, double f,
	double u, double m, double csd, double cid, double cgd, double crd, double dt_)
{
	CS = cs;

	CI = ci;

	CG = cg;
	
	CR = cr;

	IM = im;

	QS = qs;

	QI = qi;

	QG = qg;

	QT = qt;

	QU = qu;

	RS = rs;

	RI = ri;

	RG = rg;

	RIM = rim;

	QI0 = qi0;

	QG0 = qg0;

	QU0 = qu0;

	F = f;

	U = u;

	M = m;

	CSD = csd;

	CID = cid;

	CGD = cgd;

	CRD = crd;

	dt = dt_;
}

Confluence::~Confluence()
{

}

```

### 河道汇流


#### Muskingum.h



```
#pragma once
#include "Data.h"

//河道汇流：马斯京根分段连续演算法（分段马法）
class Muskingum
{
public:
	void SetParmameter(const Parameter\* parameter);   //设置参数

	void SetState(const State\* state);   //设置状态

	void UpdateState(State\* state);    //更新状态

	void calculate();

	Muskingum(double ke = 0.0, double xe = 0.0, 
		double kl = 0.0, double xl = 0.0, 
		int n = 1, double c0 = 0.0, 
		double c1 = 0.0, double c2 = 0.0,
		double i1 = 0.0, double i2 = 0.0, 
		double o1 = 0.0, double o2 = 0.0, double\* o = nullptr, double dt_ = 24);

	~Muskingum();

protected:
private:
	//========模型参数========//

	double KE;   //马斯京根法演算参数，h，敏感，KE = N \* ∆t

	double XE;   //马斯京根法演算参数，敏感，0.0~0.5

	//========模型状态========//

	double KL;   //子河段的马斯京根法演算参数/h

	double XL;   //子河段的马斯京根法演算参数

	int N;    //单元河段数，即分段数

	double C0;    //马斯京根流量演算公式I2系数

	double C1;    //马斯京根流量演算公式I1系数

	double C2;    //马斯京根流量演算公式O1系数

	double I1;    //时段初的河段入流量

	double I2;    //时段末的河段入流量

	double O1;    //时段初的河段出流量

	double O2;    //时段末的河段出流量

	double \* O;   //各子河段出流量

	double dt;   //模型计算时段长,h

};

```

#### Muskingum.cpp



```
#include "Muskingum.h"

void Muskingum::SetParmameter(const Parameter\* parameter)
{
	KE = parameter->m_KE;

	XE = parameter->m_XE;
}

void Muskingum::SetState(const State\* state)
{
	I1 = state->m_QU0;

	I2 = state->m_QU;

	O = state->m_O;

	dt = state->m_dt;
}

void Muskingum::UpdateState(State\* state)
{
	state->m_O = O;

	state->m_O2 = O2;
}

void Muskingum::calculate()
{
	KL = dt;   //为了保证马斯京根法的两个线性条件，每个单元河取 KL = ∆t

	N = int(KE / KL);    //单元河段数

	XL = 0.5 - N \* (1 - 2 \* XE) / 2;    //计算单元河段XL

	double denominator = 0.5 \* dt + KL - KL \* XL;   //计算分母

	C0 = (0.5 \* dt - KL \* XL) / denominator;

	C1 = (0.5 \* dt + KL \* XL) / denominator;

	C2 = (-0.5 \* dt + KL - KL \* XL) / denominator;

	if (!O)
	{
		O = new double[N];  //创建存储单元流域在子河段出口断面的出流量的动态数组
		
		for (int n = 0; n < N; n++)
		{
			O[n] = 0.0;    //单元流域在子河段出口断面的出流量为0
		}
	}
	
	for (int n = 0; n < N; n++)
	{
		O1 = O[n];   //子河段时段初出流量

		O2 = C0 \* I2 + C1 \* I1 + C2 \* O1;   //计算时段末单元流域在子河段出口断面的出流量，m3/s

		O[n] = O2;   //更新子河段时段初出流量

		I1 = O1;    //上一河段时段初出流为下一河段时段初入流

		I2 = O2;    //上一河段时段末出流为下一河段时段末入流
	}
}

Muskingum::Muskingum(double ke, double xe, double kl, double xl, 
	int n, double c0, double c1, double c2,
	double i1, double i2, double o1, 
	double o2, double\* o, double dt_)
{
	KE = ke;

	XE = xe;

	KL = kl;

	XL = xl;

	N = n;

	C0 = c0;

	C1 = c1;

	C2 = c2;

	I1 = i1;

	I2 = i2;

	O1 = o1;

	O2 = o2;

	O = o;

	dt = dt_;
}

Muskingum::~Muskingum()
{

}

```

## 参考文献

1. 赵人俊.流域水文模型——新安江模型与陕北模型[M]. 北京：水利电力出版社，1983. [↩︎](#fnref1)
2. 包为民.水文预报，第5版[M]，北京：中国水利水电出版社，2017 [↩︎](#fnref2)
3. 李致家.现代水文模拟与预报技术[M]，南京：河海大学出版社，2010 [↩︎](#fnref3)
4. 翟家瑞.常用水文预报算法和计算程序[M]，郑州：黄河水利出版社，1995 [↩︎](#fnref4)
5. [新安江模型](https://baike.baidu.com/item/%E6%96%B0%E5%AE%89%E6%B1%9F%E6%A8%A1%E5%9E%8B/8901233)，百度百科 [↩︎](#fnref5)






