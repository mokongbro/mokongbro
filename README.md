# T801699 mh的魔法

## 题目背景

曾经的 [**紫杉大道东**](https://www.bilibili.com/video/BV1frNV6JEMp/) 流传着一则诡异的传说。

据说在 **27** 年以前，存在着一个神秘的**魔法序列**，一开始跟我们所拥有的东西一样——这个序列一无所有，以至于 [**紫杉大道东**](https://www.bilibili.com/video/BV1frNV6JEMp/) 熙熙攘攘的人群之中竟然没有一个人敢于肯定真的存在一个**魔法序列**。

> 大家都很害怕所谓一些不存在的东西，什么妖魔鬼怪啊，棍父棍母啊。大概是人们不知道自己传统经验认知以外的事物会对自己的生存造成什么很意想不到的影响，是好是坏可能也分不清楚。
>
> :::align{right}
> ——[mokongbro](https://www.luogu.com.cn/user/1310473)2026/8/9于广州

于是，我们的**小左**作为全紫东公认的精神病患者，打算采用神秘的咒语来召唤魔法序列之人。~因为紫东的神经病已经够多了，这里站不下这么多神人~

与小左希望柔和地请求魔法序列不同，**小右**希望尽可能地**折磨**可能是魔法序列的东西。比如说自己的眼睛耳朵鼻子嘴巴等。这通常被认为是一种功能认知障碍吧。~也就是神经病~

小左饲养了几个神秘函数——$\texttt{build}$ 可以将一个下标从 $0$ 开始的数组交给**魔法使**（可能吧），这样让魔法序列显现成那个数组的样子，**至少**从表面看过去一样，$\texttt{add}$ 可以把一个神秘区间中的全体数字提升一个层级（做加法），还有一个 $\texttt{mul}$ 更厉害，可以直接将区间中的数字翻倍（做乘法）！但是他们都不如 $\texttt{reset}$，这个函数可以在不论什么情况下，把这些数全部变成一样的（区间赋值）。

小右希望**监视**小左饲养的魔法序列，他可能会使用 $\texttt{r\\_sum}$ 来看看一个区间中所有数字的和，也可能使用 $\texttt{r\\_max}$ 来看看区间最大值，用来防止有天最大值威胁到他的生命安全。相应的，他也有 $\texttt{r\\_min}$ 来嘲笑区间最小值。

## 题目描述

**本题是一道交互题。**

本题同时作为**线段树**和**高精度**的模板练习。

你需要实现上面所述的这些函数：$\texttt{build},\texttt{add},\texttt{mul},\texttt{reset},\texttt{r\\_sum},\texttt{r\\_max},\texttt{r\\_min}$，并以此来向紫东的那些守旧派表明实际上魔法序列~看起来蛮傻~真的存在。他们的原型是：
```cpp
#include<vector>
class Bint;
extern "C"{
	extern void build(const std::vector<Bint>&a);
	extern void add(std::size_t l,std::size_t r,Bint d);
	extern void mul(std::size_t l,std::size_t r,Bint d);
	extern void reset(std::size_t l,std::size_t r,Bint d);
	extern Bint r_sum(std::size_t l,std::size_t r);
	extern Bint r_max(std::size_t l,std::size_t r);
	extern Bint r_min(std::size_t l,std::size_t r);
}
```
其中 $a$ 是那个数组，$[l,r]$ 是使用的区间，$d$ 是操作用到的数字。**交互库已提供高精度类 $\texttt{Bint}$ 的数据存储和基础运算符**（`*=int`, `+=int`, `-`, `<int`, `/int`, `%int`），可直接使用。

此外，因为 [**紫杉大道东**](https://www.bilibili.com/video/BV1frNV6JEMp/) 管控了小左小右使用电子设备的能力~他们在坐牢~，你还需要自行实现以下三个自由函数运算符，否则编译错误：

| 必须实现 | 说明 |
|---|---|
| `Bint& operator+=(Bint&, const Bint&)` | Bint 间加法（区间合并、懒标记累加） |
| `Bint& operator*=(Bint&, const Bint&)` | Bint 间乘法（懒标记累积） |
| `bool operator<(const Bint&, const Bint&)` | Bint 间比较（求区间最大/最小值） |

> $\texttt{Bint}$ 类的数据成员 `d`（`std::vector<int>`，低位在前，每 9 位一组，基数为 $10^9$）和 `neg`（`bool`，符号位）均为 `public`，辅助函数 `trim()`、`sub_abs()` 也可直接调用。

因为小左小右家里没那么大，所以你只有 $3.7$ 秒和 $512$ 兆字节的空间。

你不需要实现 $\operatorname{main}()$ 函数，交互库已提供。你只需实现上述 $7$ 个函数及 $3$ 个运算符即可。

## 输入格式

**注意**：你并不需要关心输入格式，因为你要实现的是**题目描述**中的那些函数。

对于交互库来说：

第一行是一个数 $n$ 表示数组 $a$ 的元素个数。

第二行是 $n$ 个数表示数组 $a$.

第三行是一个数 $q$ 表示有 $q$ 次调用这些函数。

接下来的 $q$ 行有 $3$ 或 $4$ 个数：

第一个数 $\operatorname{op}\in[1,6]\cap\mathbb{N}$ 表示操作的类型。

当 $\operatorname{op}\in[1,3]$ 时，接下来读入三个数 $l,r,d$ 表示传入 $\texttt{add},\texttt{mul},\texttt{reset}$ 的三个参数。且有如下的映射关系：

$$
1\to\texttt{add}
$$

$$
2\to\texttt{mul}
$$

$$
3\to\texttt{reset}
$$

当 $\operatorname{op}\in[4,6]$ 时，接下来读入两个数 $l,r$ 表示传入 $\texttt{r\\_sum},\texttt{r\\_max},\texttt{r\\_min}$ 的两个参数。且有如下的映射关系：

$$
4\to\texttt{r\\_sum}
$$

$$
5\to\texttt{r\\_max}
$$

$$
6\to\texttt{r\\_min}
$$

## 输出格式

**注意**：你并不需要关心输出格式，因为你要实现的是**题目描述**中的那些函数。

对于交互库来说：

一共有 $\sum_{i=1}^q[\operatorname{op}\in[4,6]]$ 行输出。

每一行一个数当 $\operatorname{op}\in[4,6]$ 时要求的结果。

## 输入输出样例 #1

### 输入 #1

```
6
0 1 2 3 4 5
1
5 1 5

```

### 输出 #1

```
5

```

## 输入输出样例 #2

### 输入 #2

```
11
0 1 2 3 4 5 6 7 8 9 10
2
1 2 7 27
6 1 10

```

### 输出 #2

```
1

```

## 说明/提示

线段树老师，我还记得你。\
一句一句把我拉进迷雾里。

### 数据范围

$0\le l\le r< n\le2\times10^{5};q\le2\times10^{5};|a_i|,|d|\le10^{10^{4}}$

另外：保证中间数据不超过 $10^{10^{4}}$.

#### 本题采用 Subtask 捆绑测试，每个子任务的得分为其所有测试点得分的最小值。

| 子任务 | 限制 | 分值 |
|:---:|:---:|:---:|
| subtask0 | 样例 | 不给分 |
| subtask1 | 仅有 $\operatorname{op}\in\{1,4\}$ | 满分 $100$ |
| subtask2 | 无特殊限制 | 满分 $1000$ |

**故本题满分1100**。

#### 本题采用 Special Judge，每个测试点按输出正确行数占总行数的比例线性给分。
