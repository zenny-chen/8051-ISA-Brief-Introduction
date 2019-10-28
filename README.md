# 8051 ISA Brief Introduction
8051 ISA简介

<br />

## 概述

**Intel MCS-51**（俗称**8051**）是一个单片微控制器（**MCU**）系列，由Intel在1980年代研发而成，用于嵌入式系统。Intel MCS-51的指令集架构师为John H. Wharton。此芯片的初始版本在1980年代到1990年代非常受欢迎，并且其增强的二进制兼容衍生品在当今仍大受欢迎。它属于一种复杂指令集的计算机（**CISC**），并且对程序指令与数据具有独立的存储空间。

Intel初始的MCS-51家族使用N类型的金属氧化物半导体（**NMOS**）技术，跟其之前的Intel MCS-48类似。但对于后续版本，在其名字中使用了字母 **C** 进行标识（比如，80C51），它使用了互补的金属氧化物半导体（**CMOS**）技术，并且比之前的NMOS技术消耗更少的电量。这使得该技术非常适用于以电池来驱动的设备。

该家族持续到1996年一直使用增强的8位MCS-151以及二进制兼容的8/16/32位MCS-251家族微控制器。而随后，Intel不再制造MCS-51、MCS-151以及MCS-251家族，增强的二进制兼容衍生品由不少生产商制作，至今仍受欢迎。某些衍生品还集成了一块数字信号处理器（**DSP**）。除了这些物理设备，有些公司也提供了MCS-51衍生品作为IP核心用于现场可编程门阵列（**FPGA**），或应用特定的集成电路（**ASIC**）设计。

<br />

## 重要特性与应用

8051架构在一个包内提供了许多功能（中央处理单元（CPU），随机访问存储器（RAM），只读存储器（ROM），输入/输出（I/O），中断逻辑，定时器等等），详细结构如下：

・8位算术逻辑单元（ALU）与累加器，8位寄存器（一个16位寄存器，用于特殊搬运指令），8位数据总线和两个16位地址总线，程序计数器（PC），数据指针寄存器（DPTR），还有8/11/16位相关操作；因而，8051架构主要作为一款8位的微控制器。

・具有17条指令的布尔处理器，单比特累加器，32个寄存器（4比特可寻址的8位寄存器），以及多达144个特殊单比特可寻址的RAM变量（累加器A与累加器B各8位，一共16位，然后加上RAM中有128位可单比特寻址的，一共144位比特）。

・乘法、除法与比较指令

・4个快速可切换寄存器段，每个段含有8个寄存器（基于存储器映射）

・带有可选寄存器段切换的快速中断

・带有可选择优先级的中断与线程

・128或256个字节的片上RAM（IRAM）

・双16位地址总线；它可以访问2×2<sup>16</sup>个存储位置：64KB的ROM（PMEM）以及64KB的外部RAM（XRAM）

・片上ROM（不包括803x系列）

・四个8位双向输入/输出端口，可单比特寻址

・UART（串口）

・两个16位计数器/定时器

・省电模式（在某些衍生品上）

![i8051微架构](https://github.com/zenny-chen/8051-ISA-Brief-Introduction/blob/master/Intel_8051_arch.svg)

8051核心的一个特性是包含了布尔处理引擎，它允许比特级的布尔逻辑操作，对选择内部寄存器、端口以及选择RAM位置进行直接而有效地执行。此特性的加持巩固了8051在工业控制应用领域的欢迎度，因为它缩减了30%之多的代码尺寸。另一个特性是包含了四个段可选择的工作寄存器组，这极大地减少了执行上下文切换以进入或离开中断服务例程所需的时间。只需一条指令，8051就能切换寄存器段，避免了用于将临界寄存器传输到RAM这一任务的时间消耗。

如果有需要，一旦配置了一个UART和一个定时器，程序员只需要写一个简单的中断例程来填充*发送*移位寄存器，每当最后一个比特被UART移出时；以及/或清空已满的*接收*移位寄存器（将数据拷贝到其他地方）。主程序然后只需要对8位数据执行串行的从栈上读或写到栈上。


如果有需要，一旦配置了一个UART和一个定时器，程序员只需要写一个简单的中断例程来填充*发送*移位寄存器，每当最后一个比特被UART移出时；以及/或清空已满的*接收*移位寄存器（将数据拷贝到其他地方）。主程序然后只需要对8位数据执行串行的从栈上读或写到栈上。

![i8051引脚](https://github.com/zenny-chen/8051-ISA-Brief-Introduction/blob/master/Pinagem8031.jpg)

<br />

#### 衍生特性

截至2013年，许多主要芯片制作商仍然在开发新的衍生品，而且主要编译器提供商，诸如[IAR Systems](https://www.iar.com)、[Keil](http://www.keil.com)以及[Altium Tasking](https://www.tasking.com)不断地发布更新。

基于MCS-51的微控制器一般包含了一般包含了一个或两个UART，两个或三个定时器，128或256个字节的内部数据RAM（其中有16字节是可按位寻址的），高达128字节的I/O，512字节到64KB的内部程序存储器，并且有些衍生品还有扩展数据RAM（ERAM）位于外部数据空间。外部RAN和ROM共享数据与地址总线。原始的8051核心可以以每个机器周期12个时钟周期运行，大部分指令以一或两个机器周期进行执行。假设一块8051具有12MHz时钟频率，那么该8051可以每秒执行一百万条单周期指令，或是每秒五十万条两个周期的指令。增强的8051核心现在一般每个机器周期运行在六个、四个、两个，或甚至一个时钟周期，并且还拥有高达100MHz的时钟频率，并从而甚至能达到每秒更多的指令数。所有Sillicon Labs，还有某些Dallas和一些Atmel的设备具有单周期的核心。

8051的变种可以包含带有掉电检测的重置定时器、片上振荡器、自可编程的flash ROM程序存储器、内建的外部RAM、额外的内部程序贮存器，在ROM、EEPROM非挥发数据贮存器中的引导程序代码，I<sup>2</sup>C、SPI以及USB主机接口，CAN或LIN总线，ZigBee或蓝牙无线电模块，PWM生成器、模拟比较器，A/D与D/A转换器、RTC（实时时钟），额外的计数器和定时器，电路内的调试工具，更多的中断源，额外的省电模式，更多/更少的并行端口等等。Intel制造了一个可屏蔽编程版本————8052AH-BASIC，它在ROM中带有一个BASIC解释器，能够运行加载到RAM中的用户程序。

基于MCS-51的微控制器已经在极端环境下所采用了。高温度变体的样例有Tekmos TK8H51家族，用于-40°C到+250°C，或Honeywell HT83C51，用于-55°C到+225°C（在+300°C环境下可运行多达1年）。Radiation-hardenend MCS-51微控制器用于宇宙航天。

在某些工程学校，8051微控制器用于工业微控制器课程。

<br />

## 家族命名协定

8051是由Intel设计出的原始名称，带有4KB的ROM与128字节的RAM。从87开始的变体具有一个用户可编程的EPROM存储器，有时UV可擦写。带有一个**C**的变体作为第三代则是某种CMOS。8031与8032是无ROM版本，带有128和256字节的RAM。最后一个数字指示存储器大小，比如8052带有8KB的ROM，87C54带有16KB的EPROM，还有87C58带有32KB的EPROM，所有这些都具有256字节的RAM。

<br />

## 存储器架构

MCS-51具有四种不同类型的存储器————内部RAM（**IRAM**），特殊功能寄存器（**SFR**），程序存储器（**PMEM**），以及外部数据存储器（**XRAM**）。

8051被设计为一个经修改的Von-Neumann架构，它具有分开的存储器（数据与指令）；它只能从程序存储器来获取代码进行执行，并且无法将指令写入到程序存储器。而这与Harvard架构类似。

大部分8051系统会遵从这个特征，并因而无法下载并直接执行新的程序。尽管8051的架构是唯一的，不过访问这两种类型的存储器的总线是相同的，只有数据总线、地址总线和控制总线。

<br />

#### 内部RAM

内部RAM（IRAM）具有一个8位地址空间，使用地址0到0xFF。IRAM从0x00到0x7F可以直接寻址访问，使用一个8位绝对地址作为指令的一部分。IRAM也可以进行寄存器间接访问：地址值可以加载到R0或R1中，而存储器则使用 **`@R0`** 或 **`@R1`** 语法进行访问。

原始的8051只有128字节的IRAM。8052添加了从0x80到0xFF的IRAM，这个区域的IRAM*只能*通过间接访问；而如果对此地址区间做直接访问，那么访问到的是特殊功能寄存器。大多数8051衍生品也都具有一个完整的256字节的IRAM。

从0x00到0x1F的32字节的存储器被映射到8个寄存器R0到R7。一次可用8个单字节的寄存器；程序状态字（**PSW**）中有2个比特对四个可能的段进行选择。

在IRAM中有16个字节（128位）的存储位置0x20到0x2F，是可按位寻址的。

<br />

#### 程序存储器

程序存储器（**PMEM**，比起IRAM与XRAM用得少一些）是一块多达64KB的只读存储器，在一个独立的地址空间中起始地址为0。它可以在片上也可以在片外，依赖于所使用芯片的特定模型。程序存储器是只读的，即便某些8051的变种使用片上flash存储器并提供一种在系统内或应用内对该存储器重新编程的方法。

除了代码之外，也可以将只读数据存放到程序存储器中，比如查找表，然后通过`MOVC A, @A + DPTR`或`MOVC A, @A + PC`指令进行获取。该地址被计算为8位累加器与一个16位寄存器（PC或DPTR）的和。

特殊的跳转和调用指令（`AJMP`和`ACALL`）可以在同一2KB的程序存储器内进行访问，所需的开销会稍微小点。

<br />

#### 外部数据存储器

