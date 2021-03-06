---
title: 1 计算机组成原理与体系结构🖥
categories:
- note
tags:
- 软考笔记
---

> 笔记书写于03-02

##  数据的表示🔢

###  不同进制的转换方式

* 按权展开法（R进制转为10进制）

![1551510160891](https://assest.iaoe.xyz/img/1551510160891.png)

* 短除法（十进制转R进制）

![1551510316553](https://assest.iaoe.xyz/img/1551510316553.png)

* 二进制转八进制和十六进制

![1551510380268](https://assest.iaoe.xyz/img/1551510380268.png)

###  原码反码补码

* 数值的表达

![1551510553505](https://assest.iaoe.xyz/img/1551510553505.png)

​	移码是在做浮点运算时的阶码时使用，如果将移码放在数轴上我们会发现，正数在后面，负数在前面，这样子就很漂亮

* 数值的取值范围

  ![1551510970387](https://assest.iaoe.xyz/img/1551510970387.png)

  因为补码的+0和-0都是0000 0000，所以负数多了一个数值

###  浮点数运算

* 浮点数运算

  ![1551511443280](https://assest.iaoe.xyz/img/1551511443280.png)

* 对阶

  比如9.99\*10^5 + 1\*10^3，对阶变成9.99\*10^5+0.01*10^5

* 尾数计算

  计算得到10*10^5

* 结果格式化

  需要格式化为1*10^6

 

##  计算机结构

![1551511731759](https://assest.iaoe.xyz/img/1551511731759.png)

* 运算器（做运算的职能）
  * 算术逻辑单元ALU——跟运算相关
  * 累加寄存器AC——通用运算寄存器，存运算相应的值
  * 数据缓冲寄存器DR——对内存储器进行读写操作时，用来暂存数据的
  * 状态条件寄存器PSW——用来存储运算过程中的相关标志位的，有些会溢出，进位，中断都会存在里面
* 控制器（控制部件的相关运作，控制CPU的运作）
  * 程序计数器PC——获取下一条指令的位置
  * 指令寄存器IR
  * 指令译码器
  * 时序部件

##  Flynn分类法

>  计算机体系结构的分类方法

![1551513258034](https://assest.iaoe.xyz/img/1551513258034.png)

##  CISC与RISC

![1551513564328](https://assest.iaoe.xyz/img/1551513564328.png)

##  流水线技术

###  概念

![1551513647204](https://assest.iaoe.xyz/img/1551513647204.png)

计算机在执行指令时候，分为三个步骤

![1551513754257](https://assest.iaoe.xyz/img/1551513754257.png)

那么有无使用流水线的时候，是这样的

![1551513807970](https://assest.iaoe.xyz/img/1551513807970.png)

###  流水线的计算

* 流水线 **周期** 指的是执行时间最长的一段（取指，分析，执行中选择）
* 流水线的 **执行时间**
  * 一条指令执行时间+（指令条数-1）*流水线周期
  * 一条指令的执行时间
    * 理论上是一条指令各操作加起来的时间和
    * 如果考试中找不到正确答案，可能用的是实践公式，就是把一条指令的各操作的时间看成周期时间，然后再加起来

* 流水线的 **吞吐率** 计算

  ![1551514891688](https://assest.iaoe.xyz/img/1551514891688.png)

* 流水线的 **最大吞吐量** ——1/周期

* 流水线的 **加速比** 计算

  ![1551515005528](https://assest.iaoe.xyz/img/1551515005528.png)

* 流水线的 **效率**

  ![1551515081645](https://assest.iaoe.xyz/img/1551515081645.png)

  也就是阴影面积/总面积



##  存储系统

###  层次化存储结构

![1551515298709](https://assest.iaoe.xyz/img/1551515298709.png)

从上到下，速度降低，存储空间变大，基于性价比考虑才做成这样

* **加入Cache的作用**
  * 存储的内容是内存(主存)的部分内容，内容很少，存储的主要是内存中经常使用的东西
  * Cache是按内容存取，即是`相联存储器`，可以快速查找内容

###  Cache

![1551515791540](https://assest.iaoe.xyz/img/1551515791540.png)

* Cache的**命中率**

  CPU找数据时，先向Cache找，再向内存找，直接从Cache找到的概率为命中率

* Cache+主存储的**系统平均周期**

  Cache的读取时间\*命中率+主存储器的读取时间\*(1-命中率)

###  局部性原理

![1551516215569](https://assest.iaoe.xyz/img/1551516215569.png)

###  主存

* 主存的分类

  ![1551516276736](https://assest.iaoe.xyz/img/1551516276736.png)

* 主存的编址

  ![1551517252539](https://assest.iaoe.xyz/img/1551517252539.png)

###  磁盘结构与参数

![1551518083498](https://assest.iaoe.xyz/img/1551518083498.png)

![1551518782638](https://assest.iaoe.xyz/img/1551518782638.png)

##  总线系统

![1551518817580](https://assest.iaoe.xyz/img/1551518817580.png)

* 内部总线——外围芯片和处理器之间的总线(芯片级)
* 系统总线——插线板和系统板之间的中线（插件级别)VGA接口PCI接口
  * 数据总线
    * 32位系统的一个字代表32的bit位，及总线宽度为32个bit位，一次传输的数据量
  * 地址总线
    * 32位系统，代表地址空间为2^32次方，4G，则可以管理的的空间为4G
  * 控制总线——发送控制信号的总线
* 外部总线——与外部设备的总线

##  可靠性

* 串联系统

  ![1551526159742](https://assest.iaoe.xyz/img/1551526159742.png)

  比如说一个系统的失效率为0.0003，则它的无障碍运行时间为3333小时，用1/0.0003求得

* 并联系统

  ![1551526205070](https://assest.iaoe.xyz/img/1551526205070.png)

  ![1551527214558](https://assest.iaoe.xyz/img/1551527214558.png)

* 模冗余系统

  ![1551526348604](https://assest.iaoe.xyz/img/1551526348604.png)

* 混合系统

  ![1551527318213](https://assest.iaoe.xyz/img/1551527318213.png)

  


##  校验码

###  码距和检错和纠错

![1551527593331](https://assest.iaoe.xyz/img/1551527593331.png)

![1551527778684](https://assest.iaoe.xyz/img/1551527778684.png)

###  循环检验码CRC

* 模2除法（使用异或）

  ![1551527880209](https://assest.iaoe.xyz/img/1551527880209.png)

* 实例——生成的编码

  ![1551528246643](https://assest.iaoe.xyz/img/1551528246643.png)

  之后把传输后的码与多项式再次模2，结果为0即无错误

###  海明检验码

* **校验位的位数**

  2^r>=x+r+1	x为信息位的个数，r为校验位的个数

* **放校验位的位置**

  分别放在第2^0，2^1，2^n次的位置

* **校验位的计算**

  比如说信息位在第7位，则7=2^2+2^1+2^0，则第一，二个检验位在计算时要和该信息位异或

* **纠错方法**

  收到的信息中提取信息位再次计算从而生成新的校验位，与收到信息中得信息位进行异或，如果异或得到001，则代表收到得信息的第一位是错的，需要更改

  ![1551529316060](https://assest.iaoe.xyz/img/1551529316060.png)

  

  

  

  