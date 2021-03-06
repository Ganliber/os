# 虚拟存储概念

[TOC]



## 覆盖和交换

| 对比 | Overlay                          | Swapping                   |
| ---- | -------------------------------- | -------------------------- |
| 单位 | 只能发生在`没有调用关系`的模块间 | 以`进程`为单位             |
| 条件 | 程序员须给出模块间的逻辑覆盖结构 | 不需要模块间的逻辑覆盖结构 |
| 时机 | 发生在运行程序的内部模块间       | 发生在内存进程间           |



## 局部性原理

> `principle of locality`

* 程序在执行过程中的一个较短时期，所执行的`指令`和指令的`操作数`地址，分别局限于一定区域

* 分类

  * 时间局限性

    > （1）一条指令的一次执行和下次执行，（2）一个数据的一次访问和下次访问都集中在一个较短时期内

  * 空间局限性

    > （1）当前指令和临近的几条指令，（2）当前访问的数据和临近的几个数据都集中在一个较小区域内

  * 分支局部性

    > 一条跳转指令的两次执行，很可能跳转到相同的内存位置

* 示例

  > 已知：page_size = 4 KB, 分配给每个 process 的物理页面数为 1, 有二维数组 `int a[1024][1024]`
  >
  > 分析：**则该数组按照行存放在内存中，每一行放在一个页面内**
  >
  > 1. ```C
  >    for(x=0; x<1024; x++)
  >      for(y=0; y<1024; y++)
  >        a[x][y] = 0;
  >    ```
  >
  >    发生了`1024`次缺页中断
  >
  > 2. ```C
  >    for(x=0; x<1024; x++)
  >      for(y=0; y<1024; y++)
  >        a[y][x] = 0;
  >    ```
  >
  >    发生了`1024*1024`次缺页中断
  >
  > 3. 如果这些数据都在内存中，时间接近一致；如果使用了虚拟存储，那么差别就会很大
  >
  > 4. 提高局部性有时候有助于提高程序的性能





## 虚拟存储的基本概念

* 思路：

  > 将不常用的部分**内存**块暂存到**外存**

* 原理：

  1. 装载程序时：

     > 只将当前**指令执行需要的部分页面**或段装入内存

  2. 指令执行中需要的**指令或数据**不在内存（缺页 or 缺段）时

     > 处理器通知操作系统将相应的页面或段调入内存

  3. 操作系统将**内存中暂时不用的页面或段保存到外存**

* 实现：

  1. 虚拟页式存储
  2. 虚拟段式存储

### 基本特征

> 改进了原来的`Overlap`和`Swapping`技术

```
			      P1	P2	P3	P4
			
	       --- <kernel>--(MMU) ---
	
   [Physical Memory]     [External Memory]
   
* Physical Memory  +  External Memory --> Vitual Storage
```

1. 不连续性
   1. 物理内存分配非连续
   2. 虚拟地址空间使用非连续
2. 大用户空间
   1. 提供给用户的虚拟内存可以大于时机的物理内存
3. 部分交换
   1. 虚拟存储支对部分虚拟地址空间进行调入和调出



### 支持技术

* 硬件
  * 页式或短时存储中的地址转换机制
* 操作系统
  * 管理内存和外存间页面或段的换入和换出





## 虚拟页式存储管理

> 在页式存储管理的基础上，增加请求调页和页面置换

* 思路

  * 当用户程序要装载到内存运行时，只装入部分页面，就启动程序运行
  * 进程在运行中发现有需要的代码或数据不再内存时，则向系统发出缺页异常请求

  

  

  













