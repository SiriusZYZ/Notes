# 句式

## 概要

1. 并列的细节：
   - One / Two / ... major XXX, the first one is,... the second one is, ....

2. 解释一件事
   - 这是说明
   


## 举例

- for example,...
- such as, …


## 观点
- to my perspective
- in my opinion 
- 

## 备注说明
- ... which is/are/v. ...

# 期望
- 找工作的期望是什么
- 你会看什么行业：
  - 决定了公司能不能走下去，或者它最多能走到什么程度，以及自己能走到什么程度
- 什么公司：
  - 公司排名，大小
  - 员工关怀
  - 公司氛围
- 对公司规模大小的期待
- 薪酬、福利、团队、wlb期待
- 有做过什么关键决策，是怎么做的？
  - 主要要体现你有什么选择、你是怎么了解不同选择优劣的
- 如何化解冲突：
  - 
- 看
 
- 30-40min 面试一般能过
- 面试官聊一些深挖的问题，比如行业看法、入职后的准备啥的
- 点头、小动作之类的
- 你还拿了哪些offer、薪资水平之类的、入职时间


# 面试准备
了解该岗位要一个什么样的人，
- 软硬技能
- 技术语言、分析工具

了解所在行业，查看行业报告
- 不同行业对该职位的需求？
- 其他公司做得如何

1. 简历准备好，哪些可能会被问到
2. 自身优势劣势
3. 保洁八大问
4. 面试 **着装正式**， 环境和背景、设备要好，摄像头要高清

## 无领导小组面试
- 30min 4个校招同学：对于成功，是个人的努力更重要还是社会环境更重要
- 选哪个无所谓，只要自圆其说就可以
- 重点是如何和其他人配合的
  角色都可以：有没有把对应的技能发挥出来
  1. leader
  2. 记笔记的
  3. 积极发言的

## 禁忌

1. 带亲友一起面试
2. 迟到不打招呼
3. 太多抱怨和吐槽
4. 正式穿着
5. 说话没有逻辑，不要惜字如金
6. 网络畅通

## 看简历

1. 教育背景：会不会保研？考研？
2. 项目经历/面试经历：不同工作之间有没有间隔、跳槽次数、在职时间长度、薪酬晋升之类的
3. 岗位职责：你的产出是怎么样的
4. 兴趣爱好：聊一聊
5. 自我评价：核心优势etc,

## 英文自我介绍

校招：
- 简单讲讲你的履历(教育背景或实习经历)
- 展现潜力：过去项目的收获和产出
- 为什么来面试，为什么想来本公司的职位

- brief intro. : 
  1. name
  2. from where
  3. university and major
  4. how many years () on this position
- top 2-3 achievements related to the job you are applying for




# self-introduction

Hi, interviewer! It's my pleasure to take part in today's interview.
My name is Zhou Yinze. I am currently studying as a postgraduate student in the University of Electronic Science and Technology of China , and I am about to graduate in twenty-twenty-four. 
My major is electronic information, which is a pretty much generalized. What I'm actually studying is the application of distributed fiber optic sensing in oilfield monitoring and production optimization.   
During these year of study, I have many opportunities to combine petroleum science and information technology in my research, including parameters adjusting, modeling, visualization, inversion, and so on.
I knew slb when I was the T.A. of my supervisor lecture, and joined the intern day event two weeks ago. And found myself very interested in the position of software engineering. 
That's all of my introduction.
# SLB background

## What do you know about SLB?
- Background:
  1. founded by Schlumberger Brothers, in 20
- Business scope
  1. **Conventional** Oilfield service:  from: 
     - **突出一个全周期**
     - exploration
     - drill
     - completion
     - field test
     - optimization
  2. 
- Why so success?
  1. the cutting edge technology
  2. 
## how do you know SLB?


## Why SLB?



# Profile

## biggest achievement?

## your weakness? and how to compensate?

## what do you see yourself in 3-5 years?


## A word to describe yourself?



# Education

## curriculum details?
1. Advanced math
   - Mathematics and physical equation: Partial Differential Equation, including wave equation, heat conduct equation etc., 
   - Optimization : linear & nonlinear optimization,  newton, gradient descending 
   - Random Process: 
2. Signal processing
   - discrete signal processing :
   - digital image processing :

## graduation project?

## the most important thing learned?
- self learning 
- why:
  1. class time

- how do i learn thing myself?
  1. the terms 
  2. whats the problem
  3. related to a problem / any terms i've already known?
  4. 
# Project Experience

## Borehole acoustic evaluation project.

 - a consignment from an state-owned oil filed company. 
 - determine the data quality of borehole acoustic instrument
 - a team of 8 ppl
   1. Matlab 2, which is the original implementation of the algorithms 
   2. rest of us : Cpp
 - Why CPP: these state-owned company wants to guarantee their software products would not contain any incontrollable foreign tech, matlab for example.
 - We need to port every functions the Matlab crew wrote, along with the some of the Matlab built-in functions.
 - among them, there is a function called vmd, which is the variational mode decomposition, and it has been called by more than 20 other function, which makes it vital for the whole project.
 - Process:
   1. At first I want to get the Matlab source code of it, but failed. 
   2. Then i turn to the academic society, and found out that vmd was introduced in 2014 in an IEEE paper. In this paper, the authors attached the original source code of vmd in Matlab. And the script is open-source with an MIT license.  
   3. The first thought come to my head was translating them as usual, but then i think why dont i look it up on github, see if anyone has implement it using cpp. And I did find one, the code was published by a guy called Hugo, he mentioned his implementation was somehow, slow and laggy.
   4. So I tested that code, and found out that it took up too much memory, we were seeing an consumption of up to 4 giga bytes. which is totally unacceptable. But, still the answer is correct.
   5. So there must be some kind of problem with the code. Maybe it doesn't free a pointer, or duplicate something repeatedly. To address the problem, I read the code very carefully (Both the original Matlab code and the Hugo's implementation) and found the problem. 
   6. Vmd is done by iteration to minimize a certain loss function. And each iteration would takes the result of last iteration as part of the input. The result we are talking about is an array, which has the same size as the signal to be decomposed.
   7. So if you want to save results of all the iteration for debugging, and your input is so long. That would be a huge problem. And this is the feature of the original Matlab code, and so does Hugo's code. Maybe he didn't notice that when he was porting from Matlab.
   8. So the solution is clear and simple. As the iteration only use the result of last iteration, we only keep the last one. When the new result comes out, we override the last result. By doing this, we can save a huge space, the times we save is depend on how many iteration are done in the process. And in some cases, we are seeing a reduction of memory usage by more than 100 times.
   9. After I refactored the code, the problem was solved. And I also publish it on github if someone need it.

## Seismic Data
### P波S波


|        | P波                          | S波                          |
| ------ | ---------------------------- | ---------------------------- |
| 中文名 | 纵波                         | 横波                         |
| 英文名 | Pressure Wave                | Sheer Wave                   |
| 方向   | 粒子震动方向与波传播方向一致 | 粒子震动方向与波传播方向垂直 |
| 速度   | 更快                         | 更慢                         |
| 介质   | 固液气                       | 固体                             |
- P波
![](assets/164b4fc94546ffc46ba1fb3b42808000.gif)
- S波:
![](assets/a96a9389ef5d54b94dd963612aa8784d.gif)

### 面波


- vawt(): variable area wiggle trace 变面积摆动轨迹图

### MASW多道面波分析
- multi-channel analysis surface wave
假设面波信号为
$$\begin{align} f(x, t) & = cos(\omega_0 t -k_0x) \\& = cos\left[\omega_0 (t-k_0/\omega_0 x)\right]\\ &=cos\left[\omega_0(t-1/c_0 x)\right] \end{align}$$
对面波信号做傅里叶变化：
$$F(x, \omega)= \pi[\delta(\omega-\omega_0) + \delta(\omega+\omega_0)]e^{-j\frac{1}{c_0}x}$$
求相位信息:
$$\begin{align}P(x, \omega) &= \frac{F(x, \omega)}{\left|F(x, \omega) \right|} \\
&= e^{-j\frac{1}{c_0}x} \end{align}$$
我们进行给定一个面波速度$c$ 进行扫描，将其造成的时延放到$P(x, \omega)$ 相位信息中
$$\begin{align}P_x(c, \omega) 
&= P(x, \omega) \times e^{j\frac{1}{c}x} \\
&= e^{-j \left(\frac{1}{c_0} - \frac{1}{c}\right)x}
\end{align}$$
在多道信号中，假设$x$ 序列为$\{x_1, ... x_n\}$， 如果我们对所有的序列进行扫描，可以得到，求和后取其幅值:
$$\begin{align} MASW(c) &= \left|\sum^n_{i = 0}P_x(c, \omega)\right| \\
&= \left|\sum^n_{i=0} e^{-j \left(\frac{1}{c_0} - \frac{1}{c}\right)x_i}\right| \end{align}$$
易知$\left(\frac{1}{c_0} - \frac{1}{c}\right) \to 0$ 时，$MASW(c)$ 取得最大值。通过选取不同的c来计算MASW可，求得一极值可以得到某一面波速度。

## 频散曲线正演

- disba : 第三方库，用于输入速度模型计算频散曲线，输入包括p波速度和s波速度，层厚和密度

### GA遗传算法

- why GA： 从论文上看的，就是说频散曲线反演是个非线性问题，解的域连续非离散，而GA适合解决这个问题
- 整个流程就是：评分->交叉->小概率变异->评分->... 

- 求解
- 群体：可行解的集合()
- 适应度函数：相当于损失函数的反义词
- 交叉：选取适应度高的解，让这些解互相交换一部分参数
- 变异：对交换以后的解引入一定的随机，变异可能引入优势也可能引入劣势，因此**只能小概率**进行
- 无改进空间：指连续多次的适应度函数的区间不再显著变化

| 词           | 类比   | 定义                             |
| ------------ | ------ | -------------------------------- |
| 可行解集合   | 种群   | 一系列可行的解的集合             |
| 可行解       | 个体   | 单个可行解                       |
| 可行解的分量 | 基因   | 可行解中的某个字段的值           |
| 适应度       | 适应度 | 可行解对问题的解决优良程度       |
| 交叉         | 杂交   | 两个可行解交换其部分分量的值     |
| 变异         | 变异   | 交叉后的可行解某些分量增加随机值 |
| 近似最优解   | 进化   | --                                 |



# Highlights
## Python and C++

> What are the differences between cpp and Python?
> 1. **Syntax**: **Static Type vs Dynamic Type**. I would say Python is more flexible, and could do more in generic programming.
> 2. **Performance**: **Python is based on C**. The scripts need to be translated to C and then compile. cpp can just compile and run. So cpp and C is relatively faster than Python.
> 3. **Memory Management:** **manual or automatic** 
>    - cpp manual management: manually allocate and release 
>    - Python automatic management
> 4. **Community:** Both have active community.
> personally, I think Python is more suitable for validate your idea. We have a vast variety of libraries to use, and you don't need to build an existing wheel. But when speaking of implementation, Cpp should be a better option. We want the code running in relatively high speed.

## details

## MySQL

> Which engine is used in mysql?
> 1. InnoDB
> 2. MyISAM
> 3. Memory
> difference: 
> 1. InnoDB supprot transaction, foreign keys

# Honors & Rewards

# Other Activities

## Teaching Assistant

# Interests


# Questions for slb

- In the intern day, I learned that each freshman should attend an Association Program, and then would be assigned to different positions. Can you tell me more about this process? Especially on how does slb decide which person would go to which department.
- will there be a mentor to guide you through the program and the following job?
- what is the daily routine of different kind of job, backend for example?
- how will ones' performance be evaluated?
- How does slb promote its employees? 
- how long is the probation?
