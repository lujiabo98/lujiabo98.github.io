---
layout: page
permalink: /blogs/river1d/index.html
title: river1d
---








#### 文章目录


* [代码及资料](#_6)
* [建模背景](#_10)
* + [记录原因](#_11)
	+ [模拟工况](#_13)
	+ [边界条件](#_15)
	+ [所需资料](#_19)
* [模型结构](#_26)
* + [圣维南方程组](#_27)
	+ [Preissmann隐式差分格式](#Preissmann_42)
	+ [离散形式](#_51)
	+ [方程组离散](#_58)
	+ [边界条件离散](#_72)
* [模型求解](#_94)
* [参考资料](#_97)
* [Matlab程序](#Matlab_101)



**如有问题，欢迎交流探讨！** 邮箱：lujiabo2017@gmail.com 卢家波  
 来信请说明博客标题及链接，谢谢。


## 代码及资料

[**点我下载**](https://download.csdn.net/download/weixin_43012724/11974357)  
 [**点我下载**](https://download.csdn.net/download/weixin_43012724/11974357)  
 [**点我下载**](https://download.csdn.net/download/weixin_43012724/11974357)


## 建模背景


### 记录原因


本文是前文[矩形河道水动力学建模](https://blog.csdn.net/weixin_43012724/article/details/101069780)的后续，介绍了一般河道如何建立一维水动力学数值模拟模型。


### 模拟工况


单一河道，江阴以下长江干流，断面为不规则形状，糙率都取n=0.025，初始水深为-10m，初始流量为0，时间步长300s，模拟总时间24h，重力加速度g取9.81m/s2。**资料见[附件](https://download.csdn.net/download/weixin_43012724/11974357)**。


### 边界条件


上边界条件：水位恒定为-5m；  
 下边界条件：水位恒定为-15m。  
 无旁侧如流。


### 所需资料


请从[**附件**](https://download.csdn.net/download/weixin_43012724/11974357)下载。  
 请从[**附件**](https://download.csdn.net/download/weixin_43012724/11974357)下载。  
 请从[**附件**](https://download.csdn.net/download/weixin_43012724/11974357)下载。


重要的事情说三遍！


## 模型结构


### 圣维南方程组


连续方程： 

 

 


 ∂ 


 A 

 


 ∂ 


 t 

 


 + 

 


 ∂ 


 Q 

 


 ∂ 


 x 

 


 = 


 0 

 

 \frac{\partial{A}}{\partial{t}}+\frac{\partial{Q}}{\partial{x}}=0 


 ∂t∂A​+∂x∂Q​=0


动量方程： 

 

 


 ∂ 


 Q 

 


 ∂ 


 t 

 


 + 

 


 ∂ 

 

 Q 


 u 

 

 

 ∂ 


 x 

 


 + 


 g 


 A 

 


 ∂ 


 Z 

 


 ∂ 


 x 

 


 + 


 g 


 A 

 

 S 


 f 

 

 = 


 0 

 

 \frac{\partial{Q}}{\partial{t}}+\frac{\partial{Qu}}{\partial{x}}+gA\frac{\partial{Z}}{\partial{x}}+gAS\_f=0 


 ∂t∂Q​+∂x∂Qu​+gA∂x∂Z​+gASf​=0


式中： 

 


 A 

 

 A 


 A为断面面积； 

 


 Q 

 

 Q 


 Q为通过断面的流量； 

 


 u 

 

 u 


 u为断面流速； 

 


 g 

 

 g 


 g为重力加速度；



 

 

 Z 

 

 Z 


 Z为水位； 

 

 

 S 


 f 

 


 S\_f 


 Sf​为摩阻比降， 

 

 

 S 


 f 

 

 = 

 

 

 n 


 2 

 

 Q 


 ∣ 


 Q 


 ∣ 

 

 

 A 


 2 

 


 R 

 

 4 


 3 

 

 

 

 S\_f=\frac{n^2Q|Q|}{A^2R^{\frac{4}{3}}} 


 Sf​=A2R34​n2Q∣Q∣​。


一般河道简化为  
 连续方程： 

 


 B 

 


 ∂ 


 Z 

 


 ∂ 


 t 

 


 + 

 


 ∂ 


 Q 

 


 ∂ 


 x 

 


 = 


 0 

 

 B\frac{\partial{Z}}{\partial{t}}+\frac{\partial{Q}}{\partial{x}}=0 


 B∂t∂Z​+∂x∂Q​=0


动量方程： 

 

 


 ∂ 


 Q 

 


 ∂ 


 t 

 


 + 


 u 

 


 ∂ 


 Q 

 


 ∂ 


 x 

 


 + 


 g 


 A 

 


 ∂ 


 Z 

 


 ∂ 


 x 

 


 + 


 g 

 

 

 n 


 2 

 

 Q 


 ∣ 


 u 


 ∣ 

 


 R 

 

 4 


 3 

 

 

 = 


 0 

 

 \frac{\partial{Q}}{\partial{t}}+u\frac{\partial{Q}}{\partial{x}}+gA\frac{\partial{Z}}{\partial{x}}+g\frac{n^2Q|u|}{R^{\frac{4}{3}}}=0 


 ∂t∂Q​+u∂x∂Q​+gA∂x∂Z​+gR34​n2Q∣u∣​=0


式中： 

 


 Z 

 

 Z 


 Z为水位； 

 


 Q 

 

 Q 


 Q为流量； 

 


 B 

 

 B 


 B为过水断面宽度； 

 


 u 

 

 u 


 u为过水断面流速； 

 


 A 

 

 A 


 A为过水断面面积； 

 


 n 

 

 n 


 n为河道糙率； 

 


 g 

 

 g 


 g为重力加速度； 

 


 R 

 

 R 


 R为水力半径。


### Preissmann隐式差分格式


简化四点线性隐格式  

 

 

 f 

 

 ∣ 


 M 

 

 = 

 

 

 f 

 

 j 


 + 


 1 

 

 n 

 

 + 

 

 f 


 j 


 n 

 


 2 

 


 f\vert\_M=\frac{f^n\_{j+1}+f^n\_j}{2} 


 f∣M​=2fj+1n​+fjn​​



 

 

 

 ∂ 


 f 

 


 ∂ 


 x 

 

 

 ∣ 


 M 

 

 = 


 θ 


 ( 

 

 

 f 

 

 j 


 + 


 1 

 


 n 


 + 


 1 

 


 − 

 

 f 


 j 

 

 n 


 + 


 1 

 

 


 Δ 


 x 

 


 ) 


 + 


 ( 


 1 


 − 


 θ 


 ) 


 ( 

 

 

 f 

 

 j 


 + 


 1 

 

 n 

 

 − 

 

 f 


 j 


 n 

 

 

 Δ 


 x 

 


 ) 

 

 \frac{\partial{f}}{\partial{x}}\vert\_M=\theta(\frac{f^{n+1}\_{j+1}-f^{n+1}\_j}{\Delta x}) + (1-\theta)(\frac{f^{n}\_{j+1}-f^{n}\_j}{\Delta x}) 


 ∂x∂f​∣M​=θ(Δxfj+1n+1​−fjn+1​​)+(1−θ)(Δxfj+1n​−fjn​​)



 

 

 

 ∂ 


 f 

 


 ∂ 


 t 

 

 

 ∣ 


 M 

 

 = 

 

 

 f 

 

 j 


 + 


 1 

 


 n 


 + 


 1 

 


 + 

 

 f 


 j 

 

 n 


 + 


 1 

 


 − 

 

 f 

 

 j 


 + 


 1 

 

 n 

 

 − 

 

 f 


 j 


 n 

 

 

 2 


 Δ 


 t 

 

 

 \frac{\partial{f}}{\partial{t}}\vert\_M=\frac{f^{n+1}\_{j+1} +f^{n+1}\_{j} - f^{n}\_{j+1} -f^{n}\_j}{2\Delta t} 


 ∂t∂f​∣M​=2Δtfj+1n+1​+fjn+1​−fj+1n​−fjn​​


式中： 

 


 θ 

 

 \theta 


 θ为加权系数， 

 


 0 


 ≤ 


 θ 


 ≤ 


 1 

 

 0\leq\theta\leq1 


 0≤θ≤1。 

 


 θ 


 = 


 0 

 

 \theta=0 


 θ=0为显式； 

 


 0 


 < 


 θ 


 < 


 1 

 

 0<\theta<1 


 0<θ<1为半隐式； 

 


 θ 


 = 


 1 

 

 \theta=1 


 θ=1为全隐式。 

 


 0.5 


 ≤ 


 θ 


 ≤ 


 1 

 

 0.5\leq\theta\leq1 


 0.5≤θ≤1时离散稳定。


### 离散形式


本文采用全隐式格式，即取 

 


 θ 


 = 


 1 

 

 \theta=1 


 θ=1。  

 

 

 

 ∂ 


 Z 

 


 ∂ 


 t 

 


 ≈ 

 


 Δ 


 Z 

 


 Δ 


 t 

 


 = 

 

 

 Z 

 

 i 


 + 

 

 1 


 2 

 

 

 t 


 + 


 1 

 


 − 

 

 Z 

 

 i 


 + 

 

 1 


 2 

 


 t 

 

 

 Δ 


 t 

 


 = 

 

 

 

 Z 


 i 

 

 t 


 + 


 1 

 


 + 

 

 Z 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 

 

 2 

 

 − 

 

 

 Z 


 i 


 t 

 

 + 

 

 Z 

 

 i 


 + 


 1 

 

 t 

 


 2 

 

 

 Δ 


 t 

 

 

 \frac{\partial{Z}}{\partial{t}} \approx\frac{\Delta Z}{\Delta t}=\frac{Z\_{i+\frac{1}{2}} ^{t+1}-Z\_{i+\frac{1}{2}} ^{t}}{\Delta t}=\frac{\frac{Z\_i^{t+1}+Z\_{i+1}^{t+1}}{2}-\frac{Z\_i^{t}+Z\_{i+1}^{t}}{2}}{\Delta t} 


 ∂t∂Z​≈ΔtΔZ​=ΔtZi+21​t+1​−Zi+21​t​​=Δt2Zit+1​+Zi+1t+1​​−2Zit​+Zi+1t​​​  

 

 

 

 ∂ 


 Q 

 


 ∂ 


 x 

 


 ≈ 

 


 Δ 


 Q 

 


 Δ 


 x 

 


 = 

 

 

 [ 


 θ 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 + 

 

 ( 


 1 


 − 


 θ 


 ) 

 


 Q 

 

 i 


 + 


 1 

 

 t 

 

 ] 

 

 − 

 

 [ 


 θ 

 

 Q 


 i 

 

 t 


 + 


 1 

 


 + 

 

 ( 


 1 


 − 


 θ 


 ) 

 


 Q 


 i 


 t 

 

 ] 

 

 

 Δ 


 x 

 


 = 

 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 − 

 

 Q 


 i 

 

 t 


 + 


 1 

 

 


 Δ 


 x 

 

 

 \frac{\partial{Q}}{\partial{x}} \approx\frac{\Delta Q}{\Delta x}=\frac{\left[\theta Q\_{i+1}^{t+1}+\left(1-\theta \right)Q\_{i+1}^t\right] - \left[\theta Q\_{i}^{t+1}+\left(1-\theta \right)Q\_{i}^t\right]}{\Delta x}=\frac{Q\_{i+1}^{t+1}-Q\_{i}^{t+1}}{\Delta x} 


 ∂x∂Q​≈ΔxΔQ​=Δx[θQi+1t+1​+(1−θ)Qi+1t​]−[θQit+1​+(1−θ)Qit​]​=ΔxQi+1t+1​−Qit+1​​



 

 

 u 


 = 

 

 

 u 


 i 


 t 

 

 + 

 

 u 

 

 i 


 + 


 1 

 

 t 

 


 2 

 


 u=\frac{u\_i^t+u\_{i+1}^t}{2} 


 u=2uit​+ui+1t​​  

 

 

 Q 


 = 

 

 

 Q 


 i 


 t 

 

 + 

 

 Q 

 

 i 


 + 


 1 

 

 t 

 


 2 

 


 Q=\frac{Q\_i^t+Q\_{i+1}^t}{2} 


 Q=2Qit​+Qi+1t​​


### 方程组离散


将离散形式代入简化的圣维南方程组中，可以得到圣维南方程组的Preissmann离散形式  
 连续方程：  

 

 

 B 

 

 

 Z 


 i 

 

 t 


 + 


 1 

 


 + 

 

 Z 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 − 

 

 Z 


 i 


 t 

 

 − 

 

 Z 

 

 i 


 + 


 1 

 

 t 

 

 

 2 


 Δ 


 t 

 


 + 

 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 − 

 

 Q 


 i 

 

 t 


 + 


 1 

 

 


 Δ 


 x 

 


 = 


 0 

 

 B\frac{Z\_i^{t+1}+Z\_{i+1}^{t+1}-Z\_i^t-Z\_{i+1}^t}{2\Delta t }+\frac{Q\_{i+1}^{t+1}-Q\_i^{t+1}}{\Delta x}=0 


 B2ΔtZit+1​+Zi+1t+1​−Zit​−Zi+1t​​+ΔxQi+1t+1​−Qit+1​​=0  

 

 


 Z 


 i 

 

 t 


 + 


 1 

 


 − 

 

 1 


 B 

 

 

 2 


 Δ 


 t 

 


 Δ 


 x 

 

 

 Q 


 i 

 

 t 


 + 


 1 

 


 + 

 

 Z 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 + 

 

 1 


 B 

 

 

 2 


 Δ 


 t 

 


 Δ 


 x 

 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 = 

 

 Z 


 i 


 t 

 

 + 

 

 Z 

 

 i 


 + 


 1 

 

 t 

 


 Z\_i^{t+1}-\frac{1}{B}\frac{2\Delta t}{\Delta x}Q\_i^{t+1}+Z\_{i+1}^{t+1}+\frac{1}{B}\frac{2\Delta t}{\Delta x}Q\_{i+1}^{t+1}=Z\_i^t+Z\_{i+1}^t 


 Zit+1​−B1​Δx2Δt​Qit+1​+Zi+1t+1​+B1​Δx2Δt​Qi+1t+1​=Zit​+Zi+1t​  
 动量方程：  

 

 

 


 Q 


 i 

 

 t 


 + 


 1 

 


 + 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 − 

 

 Q 


 i 


 t 

 

 − 

 

 Q 

 

 i 


 + 


 1 

 

 t 

 

 

 2 


 Δ 


 t 

 


 + 


 u 

 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 − 

 

 Q 


 i 

 

 t 


 + 


 1 

 

 


 Δ 


 x 

 


 + 


 g 


 A 

 

 

 Z 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 − 

 

 Z 


 i 

 

 t 


 + 


 1 

 

 


 Δ 


 x 

 


 + 

 


 g 

 

 n 


 2 

 

 ∣ 


 u 


 ∣ 

 


 R 

 

 4 


 3 

 

 

 


 Q 


 i 

 

 t 


 + 


 1 

 


 + 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 

 

 2 

 

 = 


 0 

 

 \frac{Q\_i^{t+1}+Q\_{i+1}^{t+1}-Q\_i^t-Q\_{i+1}^t}{2\Delta t }+u\frac{Q\_{i+1}^{t+1}-Q\_i^{t+1}}{\Delta x}+gA\frac{Z\_{i+1}^{t+1}-Z\_i^{t+1}}{\Delta x}+\frac{gn^2|u|}{R^{\frac{4}{3}}}\frac{Q\_i^{t+1}+Q\_{i+1}^{t+1}}{2}=0 


 2ΔtQit+1​+Qi+1t+1​−Qit​−Qi+1t​​+uΔxQi+1t+1​−Qit+1​​+gAΔxZi+1t+1​−Zit+1​​+R34​gn2∣u∣​2Qit+1​+Qi+1t+1​​=0  

 

 

 − 


 g 


 A 

 


 2 


 Δ 


 t 

 


 Δ 


 x 

 

 

 Z 


 i 

 

 t 


 + 


 1 

 


 + 


 ( 


 1 


 − 


 u 

 


 2 


 Δ 


 t 

 


 Δ 


 x 

 


 + 

 


 g 

 

 n 


 2 

 

 ∣ 


 u 


 ∣ 


 Δ 


 t 

 


 R 

 

 4 


 3 

 

 

 ) 

 

 Q 


 i 

 

 t 


 + 


 1 

 


 + 


 g 


 A 

 


 2 


 Δ 


 t 

 


 Δ 


 x 

 

 

 Z 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 + 


 ( 


 1 


 + 


 u 

 


 2 


 Δ 


 t 

 


 Δ 


 x 

 


 + 

 


 g 

 

 n 


 2 

 

 ∣ 


 u 


 ∣ 


 Δ 


 t 

 


 R 

 

 4 


 3 

 

 

 ) 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 = 

 

 Q 


 i 


 t 

 

 + 

 

 Q 

 

 i 


 + 


 1 

 

 t 

 


 -gA\frac{2 \Delta t}{\Delta x}Z\_i^{t+1}+(1-u\frac{2 \Delta t}{\Delta x}+\frac{gn^2|u|\Delta t}{R^{\frac{4}{3}}})Q\_i^{t+1}+gA\frac{2\Delta t}{\Delta x}Z\_{i+1}^{t+1}+(1+u\frac{2 \Delta t}{\Delta x}+\frac{gn^2|u|\Delta t}{R^{\frac{4}{3}}})Q\_{i+1}^{t+1}=Q\_i^t+Q\_{i+1}^t 


 −gAΔx2Δt​Zit+1​+(1−uΔx2Δt​+R34​gn2∣u∣Δt​)Qit+1​+gAΔx2Δt​Zi+1t+1​+(1+uΔx2Δt​+R34​gn2∣u∣Δt​)Qi+1t+1​=Qit​+Qi+1t​  
 设方程组形式如下：  

 

 

 A 

 

 Z 


 i 

 

 t 


 + 


 1 

 


 + 


 B 

 

 Q 


 i 

 

 t 


 + 


 1 

 


 + 


 C 

 

 Z 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 + 


 D 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 = 

 

 C 


 1 

 


 AZ\_i^{t+1}+BQ\_i^{t+1}+CZ\_{i+1}^{t+1}+DQ\_{i+1}^{t+1}=C\_1 


 AZit+1​+BQit+1​+CZi+1t+1​+DQi+1t+1​=C1​  

 

 

 E 

 

 Z 


 i 

 

 t 


 + 


 1 

 


 + 


 F 

 

 Q 


 i 

 

 t 


 + 


 1 

 


 + 


 G 

 

 Z 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 + 


 H 

 

 Q 

 

 i 


 + 


 1 

 


 t 


 + 


 1 

 


 = 

 

 C 


 2 

 


 EZ\_i^{t+1}+FQ\_i^{t+1}+GZ\_{i+1}^{t+1}+HQ\_{i+1}^{t+1}=C\_2 


 EZit+1​+FQit+1​+GZi+1t+1​+HQi+1t+1​=C2​  

 

 

 λ 


 = 

 


 2 


 Δ 


 t 

 


 Δ 


 x 

 

 

 \lambda=\frac{2\Delta t}{\Delta x} 


 λ=Δx2Δt​，则各系数为：  

 

 

 A 


 = 


 1 


 , 


 B 


 = 


 − 

 

 1 


 B 

 

 λ 


 , 


 C 


 = 


 1 


 , 


 D 


 = 

 

 1 


 B 

 

 λ 


 , 

 

 C 


 1 

 

 = 

 

 Z 


 i 


 t 

 

 + 

 

 Z 

 

 i 


 + 


 1 

 

 t 

 


 A=1,B=-\frac{1}{B}\lambda,C=1 ,D=\frac{1}{B}\lambda,C\_1=Z\_i^t+Z\_{i+1}^t 


 A=1,B=−B1​λ,C=1,D=B1​λ,C1​=Zit​+Zi+1t​  

 

 

 E 


 = 


 − 


 g 


 A 


 λ 


 , 


 F 


 = 


 1 


 − 


 u 


 λ 


 + 

 


 g 

 

 n 


 2 

 

 ∣ 


 u 


 ∣ 


 Δ 


 t 

 


 R 

 

 4 


 3 

 

 

 , 


 G 


 = 


 g 


 A 


 λ 


 , 


 H 


 = 


 1 


 + 


 u 


 λ 


 + 

 


 g 

 

 n 


 2 

 

 ∣ 


 u 


 ∣ 


 Δ 


 t 

 


 R 

 

 4 


 3 

 

 

 , 

 

 C 


 2 

 

 = 

 

 Q 


 i 


 t 

 

 + 

 

 Q 

 

 i 


 + 


 1 

 

 t 

 


 E=-gA\lambda,F=1-u\lambda+\frac{gn^2|u|\Delta t}{R^{\frac{4}{3}}},G=gA\lambda,H=1+u\lambda+\frac{gn^2|u|\Delta t}{R^{\frac{4}{3}}},C\_2=Q\_i^t+Q\_{i+1}^t 


 E=−gAλ,F=1−uλ+R34​gn2∣u∣Δt​,G=gAλ,H=1+uλ+R34​gn2∣u∣Δt​,C2​=Qit​+Qi+1t​


### 边界条件离散


上下边界可以是水深或单宽流量，在五对角矩阵的第一行和最后一行添加即可。  
 五对角矩阵 

 


 A 


 x 


 = 


 B 

 

 Ax=B 


 Ax=B形式如下：  

 

 

 


 ( 

 

 


 1 

 

 


 0 

 

 


 0 

 

 


 0 

 

 


 … 

 

 


 … 

 

 


 … 

 

 


 … 

 

 


 … 

 

 


 … 

 

 


 0 

 

 

 

 

 A 

 

 1 


 − 


 2 

 

 

 

 

 B 

 

 1 


 − 


 2 

 

 

 

 

 C 

 

 1 


 − 


 2 

 

 

 

 

 D 

 

 1 


 − 


 2 

 

 

 

 

 

 E 

 

 1 


 − 


 2 

 

 

 

 

 F 

 

 1 


 − 


 2 

 

 

 

 

 G 

 

 1 


 − 


 2 

 

 

 

 

 H 

 

 1 


 − 


 2 

 

 

 

 

 

 

 

 

 

 

 A 

 

 2 


 − 


 3 

 

 

 

 

 B 

 

 2 


 − 


 3 

 

 

 

 

 C 

 

 2 


 − 


 3 

 

 

 

 

 D 

 

 2 


 − 


 3 

 

 

 

 

 

 

 

 

 

 

 E 

 

 2 


 − 


 3 

 

 

 

 

 F 

 

 2 


 − 


 3 

 

 

 

 

 G 

 

 2 


 − 


 3 

 

 

 

 

 H 

 

 2 


 − 


 3 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 


 A 

 

 ( 


 n 


 − 


 1 


 ) 


 − 


 n 

 

 

 

 

 B 

 

 ( 


 n 


 − 


 1 


 ) 


 − 


 n 

 

 

 

 

 C 

 

 ( 


 n 


 − 


 1 


 ) 


 − 


 n 

 

 

 

 

 D 

 

 ( 


 n 


 − 


 1 


 ) 


 − 


 n 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 


 E 

 

 ( 


 n 


 − 


 1 


 ) 


 − 


 n 

 

 

 

 

 F 

 

 ( 


 n 


 − 


 1 


 ) 


 − 


 n 

 

 

 

 

 G 

 

 ( 


 n 


 − 


 1 


 ) 


 − 


 n 

 

 

 

 

 H 

 

 ( 


 n 


 − 


 1 


 ) 


 − 


 n 

 

 

 

 


 0 

 

 


 … 

 

 


 … 

 

 


 … 

 

 


 … 

 

 


 … 

 

 


 … 

 

 


 0 

 

 


 0 

 

 


 1 

 

 


 0 

 

 


 ) 

 

 ⏟ 

 

 A 

 

 


 ( 

 

 

 

 Z 


 1 

 

 

 

 


 Q 


 1 

 

 

 

 


 Z 


 2 

 

 

 

 


 Q 


 2 

 

 

 

 


 Z 


 3 

 

 

 

 


 Q 


 3 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 


 Z 


 n 

 

 

 

 


 Q 


 n 

 

 

 

 ) 

 

 ⏟ 

 

 x 

 

 = 

 

 

 ( 

 

 

 


 Z 


 1 

 

 ( 


 t 


 ) 

 

 

 

 


 C 

 

 1 


 ( 


 1 


 − 


 2 


 ) 

 

 

 

 

 

 C 

 

 2 


 ( 


 1 


 − 


 2 


 ) 

 

 

 

 

 

 C 

 

 1 


 ( 


 2 


 − 


 3 


 ) 

 

 

 

 

 

 C 

 

 2 


 ( 


 2 


 − 


 3 


 ) 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 C 

 

 1 


 [ 


 ( 


 n 


 − 


 1 


 ) 


 − 


 n 


 ] 

 

 

 

 

 

 C 

 

 2 


 [ 


 ( 


 n 


 − 


 1 


 ) 


 − 


 n 


 ] 

 

 

 

 

 


 Z 


 n 

 

 ( 


 t 


 ) 

 

 

 

 ) 

 

 ⏟ 

 

 B 

 


 \underbrace{\begin{pmatrix} 1&0&0&0&\dots&\dots&\dots&\dots&\dots&\dots&0\\[4pt] A\_{1-2}&B\_{1-2}&C\_{1-2}&D\_{1-2}\\[4pt] E\_{1-2} & F\_{1-2} & G\_{1-2}&H\_{1-2} \\[4pt] & & A\_{2-3} & B\_{2-3} &C\_{2-3} &D\_{2-3} \\[4pt] & & E\_{2-3} & F\_{2-3} &G\_{2-3} &H\_{2-3} \\[4pt] & & &&&&&&\\[4pt] & & &&&&&&\\[4pt] & & &&&&&&\\[4pt] & & &&&&&&\\[4pt] & &&&&&& A\_{(n-1)-n} & B\_{(n-1)-n} &C\_{(n-1)-n} &D\_{(n-1)-n} \\[4pt] & & &&&&&E\_{(n-1)-n} & F\_{(n-1)-n} &G\_{(n-1)-n} &H\_{(n-1)-n} \\[4pt] 0&\dots &\dots &\dots&\dots&\dots&\dots&0 & 0 &1 &0 \\[4pt] \end{pmatrix}}\_{A} \underbrace{\begin{pmatrix} Z\_{1} \\[4pt] Q\_{1} \\[4pt] Z\_{2} \\[4pt] Q\_{2} \\[4pt] Z\_{3} \\[4pt] Q\_{3} \\[4pt] \\[4pt] \\[4pt] \\[4pt] \\[4pt] Z\_{n} \\[4pt] Q\_{n} \\[4pt] \end{pmatrix}}\_{x}=\underbrace{\begin{pmatrix} Z\_{1}(t) \\[4pt] C\_{1(1-2)} \\[4pt] C\_{2(1-2)} \\[4pt] C\_{1(2-3)} \\[4pt] C\_{2(2-3)} \\[4pt] \\[4pt] \\[4pt] \\[4pt] \\[4pt] C\_{1[(n-1)-n]} \\[4pt] C\_{2[(n-1)-n]} \\[4pt] Z\_{n}(t) \\[4pt] \end{pmatrix}}\_{B} 


 A











1A1−2​E1−2​0​0B1−2​F1−2​…​0C1−2​G1−2​A2−3​E2−3​…​0D1−2​H1−2​B2−3​F2−3​…​…C2−3​G2−3​…​…D2−3​H2−3​…​……​…A(n−1)−n​E(n−1)−n​0​…B(n−1)−n​F(n−1)−n​0​…C(n−1)−n​G(n−1)−n​1​0D(n−1)−n​H(n−1)−n​0​


​x











Z1​Q1​Z2​Q2​Z3​Q3​Zn​Qn​​


​=B











Z1​(t)C1(1−2)​C2(1−2)​C1(2−3)​C2(2−3)​C1[(n−1)−n]​C2[(n−1)−n]​Zn​(t)​


​


## 模型求解


利用Matlab矩阵求解器可以得到五对角矩阵的解。


## 参考资料


[1]王船海，李光炽，向小华，等.实用河网水流计算[M].河海大学出版社,2015.(**方法来源**)  
 [2]吴晓玲,王船海,向小华. 实时校正中的旁侧入流反演方法[J]. 水科学进展,2009,20(1):52-57. DOI:10.3321/j.issn:1001-6791.2009.01.008.（**数据来源**）


## Matlab程序


**下载[附件](https://download.csdn.net/download/weixin_43012724/11974357)**，运行General\_river.m。 版本：MATLAB R2016a



```
%--------------------------------------------------
%2019.11.12最后修改
%模拟工况：一维不规则河道水动力学建模
%边界条件：上游水位为-5m，下游水位为-15m，初始水位-10m，初始流量为0。
%程序思路：先将断面数据处理成数据表，每隔0.5m确定大断面的河宽、过水面积、湿周和水力半径
%         然后对圣维南方程组进行Preissmann离散，最后求解五对角矩阵即得各断面水位和流量。
%--------------------------------------------------

%--------------------------------------------------
%定义变量及初始化

clc,clear;

%导入断面原始测量数据，共115个断面，每个断面给出了若干个起点距和高程对
load('data.mat'); 

%设置大断面水位间距dh,T为计算时间段(s)，dt为时间步长（s）,a,c:连续方程离散后的系数，
%Zu、Zd分别为上下游水位，q0为河道初始流量，n0为糙率
dh=0.5;T=24\*3600;dt=300;a=1;c=1;Zu=-5;Z0=-10;Zd=-15;q0=0;g=9.81;n0=0.025;

%以下为矩阵分配存储空间
%data(1,1)为断面总数，n为河段总数，N为断面数\*2即Zi,Qi变量总数，t为时间分段数，x为Zi,Qi解向量
n=data(1,1)-1;N=2\*data(1,1);t=ceil(T/dt);x=zeros(1,N);

%B：五对角矩阵方程的常数项，I,J,K,M,O:五对角矩阵A的五列
B=zeros(1,N);I=zeros(1,N);J=zeros(1,N-1);K=zeros(1,N-2);M=zeros(1,N-1);O=zeros(1,N-2);

%Z，Q分别为模拟时段内的水位和流量过程，X：水位、流量组合矩阵
Z=zeros(t+1,n+1);Q=zeros(t+1,n+1);X=zeros(N,t+1);

%x解向量初始化
for i=1:2:N
    x(i)=Z0;x(i+1)=q0;
end
%上下边界水位初始化
x(1)=Zu;x(N-1)=Zd;
%将初始时刻的解向量放入组合矩阵中存储
X(:,1)=x';

%初始化B,I,J,M向量
B(1)=Zu;B(N)=Zd;
I(1)=1;I(N)=0;
J(1)=0;M(N-1)=1;
%--------------------------------------------------

%--------------------------------------------------
%处理大断面实测数据data，处理结果为各个断面的二维表，含有每增加0.5m水深，
%对应的水面宽度、水面面积、湿周、水力半径

%先转置原矩阵，再将矩阵各列合并成一个长的列向量
AA=data';BB=AA(:);

%将列向量BB分成两列的矩阵Combinedata
%使若干个起点距和高程对能够对齐
for i=2:2:size(BB)
    Combinedata(i/2,1)=BB(i-1,1);
    Combinedata(i/2,2)=BB(i,1);
end 

%处理断面间距，Combinedata(17,1)数据表示第一个断面距离河道起点的距离，以此类推
%nsection表示断面数量
nsection=Combinedata(1,1);
for i=1:1:nsection
    dx(i)=Combinedata(17+230\*(i-1),1); 
end 
%差分求相邻断面间距,存储在dx向量中
dx=diff(dx);

%生成每个断面的数据表
for i=1:1:nsection 
    
    %把一个断面的起点距和高程数据赋值给临时矩阵
    temp=Combinedata(31+230\*(i-1):233+230\*(i-1),:);
    
    %第二列的最小值及行号，即为大断面最低点
    [min_value,min_row]=min(temp(:,2));
    
    %由于起点处有堤坝，而河道对岸没有堤坝，所以大断面最后一个测量点的高程总是小于第一个
    %因此需要找到次最大值，即大断面最后一个测量点的高程，以便确定断面数据表的行数
    max_value=max(temp(min_row:end,2));
    
    %nrow表示断面数据表格的行数
    %Sct为断面数据总表
    %前五列分别为水位、水面宽、过水断面面积、湿周和水力半径，
    %往后就是第二个断面的水位、水面宽、过水断面面积、湿周和水力半径，以此类推
    nrow=ceil((max_value-min_value)/dh)+1;
    Sct(1:nrow,1+5\*(i-1):5+5\*(i-1))=zeros(nrow,5);
    
    for j=1:1:nrow 
        
        %水位
        z=min_value+dh\*(j-1);
        
        %Plus_minus记录断面各点高程与所给水位的大小关系，用-1和0表示
        Plus_minus=diff(temp(:,2)>z);
        
        %记录下由正转负的零点位置
        bigtosmall=find(Plus_minus==-1);
        
        %记录下由负转正的零点位置
        smalltobig=find(Plus_minus==1);
        
        %判断正零点和负零点的个数是否相同
        %是为了防止次最大值并不是对岸最后一点，而是最大值右方某个凸起
        %这是应该在最右岸添加一个提防
        if numel(bigtosmall)>numel(smalltobig) 
            smalltobig(numel(bigtosmall),1)=size(temp,1);
            temp(size(temp,1)+1,1)=temp(end,1);temp(end,2)=z;
        end
        
        %生成断面数据表
        for k=1:1:size(bigtosmall)
            
            %求左边交点起点距
            dl=temp(bigtosmall(k)+1,1)-(z-temp(bigtosmall(k)+1,2))\*(temp(bigtosmall(k)+1,1) ...
                -temp(bigtosmall(k),1))/(temp(bigtosmall(k),2)-temp(bigtosmall(k)+1,2));
            
            %求右边交点起点距
            dr=temp(smalltobig(k),1)+(z-temp(smalltobig(k),2))\*(temp(smalltobig(k)+1,1) ...
                -temp(smalltobig(k),1))/(temp(smalltobig(k)+1,2)-temp(smalltobig(k),2));
            
            %Width表示相应水深的水面宽
            Width=dr-dl;
            
            %计算过水断面面积
            darea=(z-temp(bigtosmall(k)+1,2))\*(temp(bigtosmall(k)+2,1)-dl)/2 ...
                +(z-temp(smalltobig(k),2))\*(dr-temp(smalltobig(k)-1,1))/2;
            %累加梯形面积
            for m=bigtosmall(k)+2:1:smalltobig(k)-1 
                darea=darea+(z-temp(m,2))\*(temp(m+1,1)-temp(m-1,1))/2;
            end
            
            %计算湿周
            dwetcycle=sqrt((temp(bigtosmall(k)+1,1)-dl)^2+(temp(bigtosmall(k)+1,2)-z)^2) ...
                +sqrt((temp(smalltobig(k),1)-dr)^2+(temp(smalltobig(k),2)-z)^2);
            %累加湿周
            for n=bigtosmall(k)+2:1:smalltobig(k) 
                dwetcycle=dwetcycle+sqrt((temp(n,1)-temp(n-1,1))^2+(temp(n,2)-temp(n-1,2))^2);
            end
            
            %分别为水位、水面宽、过水断面面积、湿周和水力半径
            Sct(j,1+5\*(i-1))=z;
            Sct(j,2+5\*(i-1))=Sct(j,2+5\*(i-1))+Width;
            Sct(j,3+5\*(i-1))=Sct(j,3+5\*(i-1))+darea;
            Sct(j,4+5\*(i-1))=Sct(j,4+5\*(i-1))+dwetcycle;
            Sct(j,5+5\*(i-1))=Sct(j,3+5\*(i-1))/Sct(j,4+5\*(i-1));
        end 
    end
end

%--------------------------------------------------
%分为t个时间层循环，求解五对角矩阵
for k=1:1:t 
    for i=1:2:N-2
        
        %利用差分确定前后断面与相应水位的大小关系
        p_m1=diff(Sct(:,1+5\*(i-1)/2)>x(i));
        p_m2=diff(Sct(:,6+5\*(i-1)/2)>x(i+2));
        %记录下前一个断面由负转正的零点位置
        stob1=find(p_m1==1);
        %记录下后一个断面由负转正的零点位置
        stob2=find(p_m2==1);
        
        %Zu1表示内插后上断面水位较小值，Zu2表示上断面水位较大值
        Zu1=Sct(stob1,1+5\*(i-1)/2);
        Zu2=Sct(stob1+1,1+5\*(i-1)/2);
        %x(i)表示上断面实际水位，alpha表示上断面的(Z-Z1)/(Z2-Z1)
        alpha=(x(i)-Zu1)/(Zu2-Zu1);
        
        %Zd1表示内插后下断面水位较小值，Zd2表示下断面水位较大值
        Zd1=Sct(stob2,6+5\*(i-1)/2);
        Zd2=Sct(stob2+1,6+5\*(i-1)/2);
        %x(i+2)表示下断面实际水位，beta表示下断面的(Z-Z1)/(Z2-Z1)
        beta=(x(i+2)-Zd1)/(Zd2-Zd1);
        
        %Bu1表示内插后上断面水面宽较小值，Bu2表示上断面水面宽较大值，Bu表示上断面实际水面宽
        Bu1=Sct(stob1,2+5\*(i-1)/2);
        Bu2=Sct(stob1+1,2+5\*(i-1)/2);
        Bu=Bu1+alpha\*(Bu2-Bu1);
        %Bd1表示内插后下断面水面宽较小值，Bd2表示下断面水面宽较大值，Bd表示下断面实际水面宽
        Bd1=Sct(stob2,7+5\*(i-1)/2);
        Bd2=Sct(stob2+1,7+5\*(i-1)/2);
        Bd=Bd1+beta\*(Bd2-Bd1);
        %Baverage表示上下断面水面宽平均值
        Baverage=(Bu+Bd)/2;
        
        %Au1表示内插后上断面过水面积较小值，Au2表示上断面过水面积较大值，Au表示上断面实际过水面积
        Au1=Sct(stob1,3+5\*(i-1)/2);
        Au2=Sct(stob1+1,3+5\*(i-1)/2);
        Au=Au1+alpha\*(Au2-Au1);
        %Ad1表示内插后下断面过水面积较小值，Ad2表示下断面过水面积较大值，Ad表示下断面实际过水面积
        Ad1=Sct(stob2,8+5\*(i-1)/2);
        Ad2=Sct(stob2+1,8+5\*(i-1)/2);
        Ad=Ad1+beta\*(Ad2-Ad1);
        %Aaverage表示上下断面过水面积平均值
        Aaverage=(Au+Ad)/2;
        
        %uu表示上断面流速，ud表示下断面流速，x(i+1)、x(i+3)分别表示Qi、Q_i+1
        uu=x(i+1)/Au;ud=x(i+3)/Ad;
        %uaverage表示上下断面平均流速
        uaverage=(uu+ud)/2;
        
        %Ru1表示内插后上断面水力半径较小值，Ru2表示上断面水力半径较大值，Ru表示上断面实际水力半径
        Ru1=Sct(stob1,5+5\*(i-1)/2);
        Ru2=Sct(stob1+1,5+5\*(i-1)/2);
        Ru=Ru1+alpha\*(Ru2-Ru1);
        %Rd1表示内插后下断面水力半径较小值，Rd2表示下断面水力半径较大值，Rd表示下断面实际水力半径
        Rd1=Sct(stob2,10+5\*(i-1)/2);
        Rd2=Sct(stob2+1,10+5\*(i-1)/2);
        Rd=Rd1+beta\*(Rd2-Rd1);
        %Raverage表示上下断面水力半径平均值
        Raverage=(Ru+Rd)/2;
        
        %计算B向量各元素，B(i+1),B(i+2)分别为河段上下断面水位和流量之和即C1，C2
        B(i+1)=x(i)+x(i+2);
        B(i+2)=x(i+1)+x(i+3);
        
        %lambda表示网格比
        lambda=2\*dt/dx((i+1)/2);
        %计算五对角阵对角线向量I,分别为B,G
        I(i+1)=-lambda/Baverage;I(i+2)=g\*Aaverage\*lambda;
        
        %计算对角线上一行向量J,分别为C,H
        J(i+1)=c;
        J(i+2)=1+uaverage\*lambda+g\*n0\*n0\*abs(uaverage)\*dt/Raverage^(4/3);
        
        %计算对角线上两行向量K
        K(i)=0;K(i+1)=-I(i+1);
        
        %计算对角线下一行向量
        M(i)=a;M(i+1)=J(i+2)-2\*uaverage\*lambda;
        
        %O为对角线下两行向量.O(i)为方程组系数E,E=-G，O(i+1)为0已经初始化
        O(i)=-I(i+2);
    end
    
    %构造五对角矩阵A
    A=diag(I)+diag(J,1)+diag(K,2)+diag(M,-1)+diag(O,-2);
    %解五对角矩阵差分方程组并赋给X向量存储
    X(:,k+1)=A\B';
    %更新解向量
    x=X(:,k+1)';
end

%--------------------------------------------------
%从组合矩阵X中提取出水位Z和流量Q
%X的每一列表示一个时段的计算结果
%Z,Q的每一行表示一个时段的计算结果
for i=1:2:N
j=(i+1)/2;
Z(:,j)=X(i,:)';Q(:,j)=X(i+1,:)';
end  

%绘制出水位和流量三维图
figure(1);
mesh(Z);
xlabel('河段n');
ylabel('时间k/300s');
zlabel('水位/m');

figure(2);
mesh(Q);
xlabel('河段n');
ylabel('时间k/300s');
zlabel('流量(m^2/s)');

%-------------------------------------------------- 

```




