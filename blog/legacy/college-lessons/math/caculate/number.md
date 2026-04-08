---
title: 级数
description: 在这里记录了一下高数级数的重点和方法
tags: 
  - 高等数学
---

| 笔记来源       | 讲课老师                                         |
| ---------- | -------------------------------------------- |
| 中山大学工科高数课堂 | @[杨奇林](https://math.sysu.edu.cn/teacher/569) |

收敛的级数具有线性性，两个收敛的级数的线性组合、数乘还是收敛的

对一个收敛的级数在前面若干项做变动是不会影响级数的收敛性的

# 等比级数

如果比例为 $\displaystyle r$，那么等比级数的部分和为 $\displaystyle S_{n}=\frac{a_{1}-a_{n}r}{1-r}$ ($\displaystyle \frac{首项}{}$)
如果 $\displaystyle \mid r\mid< 1$ 那么等比级数的和为 $\displaystyle \frac{a_{1}}{1-r}$ ($\displaystyle \frac{首项}{1-公比}$)
如果 $\displaystyle r=1$，则级数和为 $\displaystyle \infty$，不收敛
如果 $\displaystyle r=-1$，级数和也不收敛

# 正项级数收敛

正项级数收敛的充要条件是它的部分和 $\displaystyle S_{n}$ 有上界

正项级数收敛，它的子列也收敛
正项级数的子列收敛，它本身也收敛
# $\displaystyle \frac{1}{n^{p}}$ 级数

这个 $\displaystyle \sum \limits_{n=1}^{\infty}\frac{1}{n^{p}}$ 级数，当 $\displaystyle p>1$ 时收敛，当 $\displaystyle p\leq 1$ 时发散
经常利用这个级数去拿你要证明的级数进行比较判别法辅助判定

# 比较判别法

核心就是如果要证明一个复杂级数的收敛性，就要找到另外一个好算的级数来辅助
判别的方法就是两个级数，**大的如果收敛，小的也一定收敛；小的如果发散，大的也一定发散**

除了直接凑比大小，还可以**使用泰勒展开来得到等价的级数**。因为这个级数的收敛其实就是无穷小的运算，分析通项是不是趋近于 0，如果不趋近于 0 那根本不可能收敛，如果趋于 0 就是无穷小量，可以使用泰勒展开。要注意可以通过**泰勒展开**那个原来级数的式子，找到和原来级数等价的多项式展开式。这样子就可以用简单的等价多项式辅助证明原来复杂的级数了

具体的验证方法就是，已知一个复杂的级数 $\displaystyle u_{n}$ 我们用泰勒展开构造了一个好算的级数 $\displaystyle v_{n}$ 接下来要去验证这二者是否可以等价，就是看 $\displaystyle \lim_{ n \to \infty } \frac{u_{n}}{v_{n}}$ 是否等于常数，如果等于常数，就找到了等价的好算级数 $\displaystyle v_{n}$ 这时候 $\displaystyle u_{n}$ 和 $\displaystyle v_{n}$ 的收敛性是一样的

在这里经常会遇见 $\displaystyle \frac{1}{n^{p}}$ 还有一些类似的变形，比如 $\displaystyle nu_{n} = \frac{u_{n}}{\frac{1}{n}}$ 以及 $\displaystyle n^{p}u_{n}= \frac{u_{n}}{\frac{1}{n^p}}$
或者知道 $\displaystyle n^{p}a_{n}^{p}$ 收敛时，可以作为条件用极限的拆分运算推出 $\displaystyle a_{n}$ 收敛



# 比值判别法

$\displaystyle \lim_{ n \to \infty }  \frac{u_{n+1}}{u_{n}} = \frac{后一项}{前一项}=l$ 本质上是看后一项比前一项的比值是大于 1 还是小于 1
其实就是拿这个级数等效成等比级数，如果等效出来的比例小于 1，那等比级数就是收敛的嘛

$\displaystyle \lim_{ n \to \infty }  \frac{u_{n+1}}{u_{n}} = \frac{后一项}{前一项}=l \begin{cases}l<1  \quad 是衰减的，收敛\\ l>1  \quad 不是衰减的，发散\\ l=1  \quad 换方法吧\end{cases}$

这个比值判别法适合阶乘、多项式和常数的 $\displaystyle n$ 次方连乘的情况

在做比值的时候，要考虑两个重要的极限 $\displaystyle \sin x=x$ 和 $\displaystyle \left( 1+\frac{1}{n} \right)^n【1的无穷大】=e$

遇到比值为 1，就换 $\displaystyle \lim_{ n \to \infty } \frac{u_{n+1}}{u_{n}}=1+\frac{p}{n}$ 泰勒展开

# 柯西判别法

$\displaystyle \lim_{ n \to \infty } (u_{n})^{\frac{1}{n}}=l$ 这个极限如果存在，就是和比值判别法的极限相同的，所以结论也相同
$\displaystyle \lim_{ n \to \infty }(u_{n})^{\frac{1}{n}} =l \begin{cases}l<1  \quad 是衰减的，收敛\\ l>1  \quad 不是衰减的，发散\\ l=1  \quad 换方法吧\end{cases}$

柯西判别法适合处理 $\displaystyle n$ 的 $\displaystyle n$ 次方的

# 级数和对应的函数具有相同收敛性

一个 $\displaystyle x$ 连续变化的函数 $\displaystyle f(x)$ 和一个 $\displaystyle n$ 离散变化的级数 $\displaystyle a_{n}=f(n)$ 是具有相同收敛性的

写成公式就是 $\displaystyle \sum \limits_{n=1}^{\infty}a_{n}=\int_{1}^{A} f(x) \, dx$ 一旦级数 $\displaystyle a_{n}$ 收敛，那么积分结果 $\displaystyle \int_{1}^{A} f(x) \, dx  =P(A)$ 在 $\displaystyle A\to\infty$ 的时候极限存在

所以遇到难算的级数可以把 $\displaystyle n$ 改成 $\displaystyle x$ 用函数积分计算
具体来说就是计算这个函数积分得到 $\displaystyle \int_{1}^{A} f(x) \, dx  =P(A)$ ，再看 $\displaystyle A\to \infty$ 之后，$\displaystyle P(A)$ 存不存在，如果存在就是收敛，不存在就发散

经常用看成函数的方法处理下面的级数和结论

| 级数                                                                                                                         | 结论                                                                     |
| -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
|  $\displaystyle \sum \limits_{n=2}^{\infty} \frac{1}{n(\ln n)^{p}}\begin{cases}p<1  \quad 发散 \\ p>1  \quad 收敛\end{cases}$  | 当 $\displaystyle n\to \infty$ 有 $\displaystyle (\ln n)^{\ln n}>n^{常数}$ |

# 利用极限运算的技巧

在计算级数极限是否收敛时，还要回顾一下极限的运算技巧


如果遇到式子比较复杂，看看出现高次多项式 $\displaystyle \frac{n}{An^{5}+Bn^{4}+Cn^{3}+Dn^{2}+En +F}$ 或者出现多层对数或者指数 $\displaystyle \ln \ln n、\ln n、e^{n}$ 时要注意抓大头，抓住占主导的部分，忽略次要的部分

| 高次多项式    | $\displaystyle \frac{n}{An^{5}+Bn^{4}+Cn^{3}+Dn^{2}+En +F}$ | 分母主导是 $\displaystyle An^{5}$                          |
| -------- | ----------------------------------------------------------- | ----------------------------------------------------- |
| 有指数或多层对数 | $\displaystyle \frac{1}{n(\ln n)^{p}(\ln \ln n)^{q}}$       | 一般情况下有 $\displaystyle \ln$ 忽略 $\displaystyle \ln \ln$ |
