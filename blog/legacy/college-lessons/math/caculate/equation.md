---
title: 常微分方程
description: 在这里记录了一下高数常微分方程的重点和方法
tags: 
  - 高等数学
---

# 一些方程相关的概念

向量空间是线性的空间，里面的向量和映射都是线性的(可加，数乘)
$$
\begin{align*}
L(X_{1}+X_{2})=L(X_{1})+L(X_{2})\\
L(\lambda X)=\lambda L(X)
\end{align*}
$$

齐次方程就是左边是自变量，右边是零的方程。它的解叫做**通解**，是一种**向量空间**哦
$$
\begin{align*}
F(x_{1},x_{2},...,x_{n})=0
\end{align*}
$$

非齐次方程就是左边是自变量，右边还有一个常数。它的解叫做**特解**
$$
\begin{align*}
F(x_{1},x_{2},...,x_{n})=b
\end{align*}
$$

如果右边那个常数变成零了之后，非齐次方程就成为齐次方程。
实际上，非齐次方程加上特解，就等效为齐次方程。
$$
\begin{align*}
&Ax=b \quad 非齐次方程\\
&Ax^{*}=b \quad x^{*}是非齐次方程的特解\\
\\
&所以A(x-x^{*}) =0\\
&令t=x-x^{*}，则At=0 \quad 转变为齐次方程\\
\end{align*}
$$



而常微分方程的一般形式就是
$$
\begin{align*}
L(y)=f(x)
\end{align*}
$$
对于这个方程来说，$L(y)$ 就是齐次部分，$f(x)$ 就是非齐次部分
如果 $L(y)$ 是线性的，那么这个方程就是线性常微分方程。否则就是非线性常微分方程


对于偏微分方程来说，下面关于线性方程的分类很重要，但是对于常微分来说一般般
线性方程可以分成常系数的，就像 $\sum\limits_{i=0} ^{n} a_{i}y^{(i)}=f(x)$ 其中 $a_{i}$ 是常数
也可以分成变系数的，就像 $\sum\limits_{i=0}^{n}a_{i}(x)y^{(n)}=f(x)$

微分方程的概念就是自变量，未知函数，未知函数的导数

积分方程的初值条件隐藏在积分上下限里，比如 $\displaystyle \begin{align}\int_{x_{0}}^{x}\phi (x)=... \\隐含\phi (x_{0})=...\end{align}$ 代入 $\displaystyle x_{0}$ 到积分方程
多阶积分方程会隐含多个初值条件，积分方程求导的公式
对这个函数  $\displaystyle F(x)=\int ^{\phi_{1}(x)}_{\phi_{2}(x)} f(x,t)\,dt$ 求导得到 $\displaystyle F'(x)=f(x,\phi_{1}(x))\phi_{1}(x)'-f(x,\phi_{2}(x))\phi_{2}'(x)+\int ^{\phi_{1}(x)}_{\phi_{2}(x)}\frac{\partial f(x,t)}{\partial x}\, dt$

在处理多阶积分方程一定要注意初值条件

# 变量分离法(一阶线性齐次、非齐次通吃)

基本的原则就是，分离变量，然后积分。
或者遇到整体的换一个元，遇到齐次的除下去换元

第一种最简单的是遇到 $x,y$ 变量可以直接分离，不耦合的情况。

$$
\begin{align*}
\frac{dy}{dx}=f(x)g(y)\\
\frac{dy}{g(y)}=f(x)dx\\
两边同时积分
\end{align*}
$$

像上面的那个要除过去(严谨的说法是做变量替换)，把变量都分离干净了再积分

稍微难一点就是要**分解因式**之后，才能看出来变量分离(遇到含有非线性的高次多项式项要想到)


第二种是遇到整体耦合在一起
$$
\begin{align*}
\frac{dy}{dx}=f(ax+by+c)
\end{align*}
$$
令 $\displaystyle\boxed{t=ax+by+c}$，再计算 $\displaystyle\frac{dt}{dx}$，然后**优先考虑分离变量**
$$
\begin{align*}
\frac{dt}{dx}=\frac{d(ax+by+c)}{dx} =a+b· \frac{dy}{dx}=a+b·f(t)\\
这个式子就可以分离变量了:\frac{dt}{dx}=a+b·f(t)
\end{align*}
$$


第三种是遇到齐次式
$$
\begin{align*}
f(x,y)=\frac{dy}{dx}
\end{align*}
$$
如果 $f(x,y)=f(tx,ty)$ 那么就说 $f(x,y)$ 是齐次的，$f(x,y)$ 可以被写成 $\displaystyle h(\frac{y}{x})$
例子就是

$$
\begin{align*}
\frac{A_{1}x^{2}+B_{1}xy+C_{1}y^{2}}{A_{2}x^{2}+B_{2}xy+C_{2}y^{2}} \to \frac{A_{1}+B_{1} \frac{y}{x}+C_{1}{\frac{y}{x}}^{2}}{A_{2}+B_{2}{\frac{y}{x}}+C_{2}{\frac{y}{x}}^{2}}
\end{align*}
$$
这时候要令 $\boxed{\frac{y}{x}=u}$，再计算 $\displaystyle \frac{dy}{dx}$ 
$$
\begin{align*}
f(x,y)=\frac{dy}{dx}=\frac{d(ux)}{dx}=u+\frac{du}{dx}=h(u)\\
这个式子就可以分离变量了:h(u)=u+ \frac{du}{dx}
\end{align*}
$$

第四种最难的就是遇到
$$
\begin{align*}
\frac{dy}{dx}=f(\frac{A_{1}x+B_{1}y+C_{1}}{A_{2}x+B_{2}y+C_{2}})
\end{align*}
$$
下面分类讨论
如果 $C_{1}=C_{2}=0$ 那么就变成齐次式，和第三种情况处理一样
如果分子和分母对应的各个项的系数成比例，那么就设分子是 $t$，分母是 $\lambda t$，之后就和第二种情况的处理一样
如果没有上面的两种特殊情况，就要先解这个方程 $\begin{align}\begin{cases}A_{1}x+B_{1}y+C_{1}=0\\A_{2}x+B_{2}y+C_{2}=0\end{cases}\end{align}$ 得到 $\begin{cases}x_{0} \\ y_{0}\end{cases}$
然后再令 $\begin{cases}u=x-x_{0} \\ v=y-y_{0}\end{cases}$ 把原来 $\displaystyle f(\frac{A_{1}x+B_{1}y+C_{1}}{A_{2}x+B_{2}y+C_{2}})$ 里面的 $x,y$ 替换成 $u,v$ 这样就可以得到

$$
\begin{align*}
\frac{dy}{dx}=f(\frac{A_{1}x+B_{1}y+C_{1}}{A_{2}x+B_{2}y+C_{2}})=f(\frac{A_{1}u+B_{1}v}{A_{2}u+B_{2}v})=\frac{du}{dv}\\
这个式子就可以用处理齐次式的方法了:\boxed{f(\frac{A_{1}u+B_{1}v}{A_{2}u+B_{2}v})=\frac{du}{dv}}
\end{align*}
$$

# 常数变易法(非齐次)

常数变易法是处理一阶非齐次线性常微分方程的方法，一阶线性常微分方程的标准形式长成下面这样
$$
\begin{align*}
标准形式:\boxed{\frac{dy}{dx}+P(x)y=Q(x)}
\end{align*}
$$
注意拿到一个一阶线性常微分方程，要转化成**标准形式**，这个可要写对奥

然后先把方程看成下面这个齐次方程，用**分离变量法**求方程的**通解 $y_{0}$**
$$
\begin{align*}
\frac{dy}{dx}+P(x)y=0
\end{align*}
$$
得到下面的通解 $y_{0}$
$$
\begin{align*}
y_{0}=C·(与x有关的单变量函数)
\end{align*}
$$

这时候把常数 $C$ 看成关于 $x$ 的函数 $C(x)$，用下面的式子计算方程的**特解 $y_{x}=C(x)(与x有关的单变量函数)$**
$$
\begin{align*} 
C'(x)·(与x有关的单变量函数)=Q(x) \quad \quad  \quad  \quad  \quad  
\\ C(x)=\int C'(x)dx \quad(因为这是特解，所以积分后不用加常数)\\
y_{x}=C(x)(与x有关的单变量函数)
\end{align*}
$$

所以这个非齐次方程的解等于特解加通解
$$
\begin{align*}
y=y_{0}+y_{x}=C(与x有关的单变量函数)+C(x)(与x有关的单变量函数)
\end{align*}
$$


如果遇到类似下面这种可以凑微分的形式，也可以不用常数变易法直接凑微分
$$
\begin{align*}
&\frac{dp}{dx}=P(x)(p-1) \\ \\
&dp  \quad 可以凑成  \quad d(p-1)\\
\\
&\frac{dp}{dx}=\frac{d(p-1)}{dx}=P(x)(p-1)
\end{align*}
$$

# 伯努利方程(一阶线性非齐次+ $y^{\alpha}$)

所谓的伯努利方程，就是一个非齐次方程的右边还有 $y^{\alpha}$ 次方部分的方程
$$
\begin{align*}
\frac{dy}{dx}+P(x)y=Q(x)y^{\alpha}
\end{align*}
$$
遇到这样的方程，就是要令 $\boxed{p=y^{1-\alpha}}$ 换元
$$
\begin{align*}
\\
&除一个y^{\alpha} \quad \quad\\
&\frac{1}{1-\alpha}·d\frac{(y^{1-\alpha})}{dx}+P(x)y^{1-\alpha}=Q(x)\\
&这样就令 \quad p=y^{1-\alpha}  \quad换元\\
&直接可以得到:\boxed{\frac{1}{1-\alpha} \frac{dp}{dx}+P(x)p=Q(x)} 
\end{align*}
$$


# 可降阶的多阶微分方程

## $F(x,y',y'')$ 类型

把 $\begin{cases}y'=p \\ y''=p'=\frac{dp}{dx}\end{cases}$ 变成 $F(x,p,\frac{dp}{dx})$ 

## $F(y,y',y'')$ 类型

把 $\displaystyle \begin{cases}y'=p \\ y''=p \frac{dp}{dy}\end{cases}$

这里原本是 $y''=\frac{dp}{dx}$，但是这个微分方程现在的自变量是 $y$，未知函数是 $p$
如果未知函数的导数是 $\frac{dp}{dx}$ 的话要引入 $x$ 自变量，这样不行

于是未知函数的导数 $y''=\frac{dp}{dy} ·\frac{dy}{dx}=p \frac{dp}{dy}$

## $y^{(n)}=f(x)$ 类型

连续积分 $n$ 次就行啦

## $F(x,y',y'')$ 派生 $F(y',y'')$ 

这样就直接令 $\begin{cases}y'=p \\ y''=\frac{dp}{dx}\end{cases}$

## $y^{(n)}=f(x)$ 和 $F(x,y',y'')$ 派生 $F(y^{(n)},y^{(n+1)})$

## $y^{(n)}=f(x)$ 和 $F(x,y',y'')$ 派生 $F(x,y^{(n)},y^{(n+1)})$

## $y^{(n)}=f(x)$ 和 $F(y,y',y'')$ 派生 $F(x,y^{(n)},y^{(n+1)})$



# 只能用全微分/积分因子的一阶微分方程

如果这么巧遇到是全微分的方程，就用曲线积分的知识找出原函数就行啦
$$
\begin{align*}
P(x,y)dx+Q(x,y)dy=0是某个函数U(x,y)的全微分的条件是\\
\frac{\partial P}{\partial y}-\frac{\partial Q}{\partial x}=0  \quad  \quad  \quad  \quad  \quad  \quad  \quad  \quad  \quad  \quad   
\end{align*}
$$

如果不是全微分的话，要找到一个积分因子乘以原来的方程，来让它变成全微分方程
首先遇到一个微分方程
$$
\begin{align*}
Mdx+Ndy=0
\end{align*}
$$

先计算 $\begin{cases}\frac{\partial M}{\partial y} \\ \frac{\partial N}{\partial x}\end{cases}$ ，再计算下面这个式子
$$
\begin{align*}
\frac{\partial M}{\partial y}-\frac{\partial N}{\partial x} 
\end{align*}
$$
看这个式子是只含 $x$ 还是只含 $y$ 
如果只含 $x$，这个式子就除 $N$，意味着待会计算出来的积分因子是 $\mu(x)$ 
$$
\begin{align*}
记F(x)=\frac{\frac{\partial M}{\partial y}-\frac{\partial N}{\partial x}}{N} 
\end{align*}
$$
则
$$
\begin{align*}
\mu(x)=e^{\int F(x)dx}
\end{align*}
$$

之后再把积分因子 $\mu$ 乘到原来的方程，再用处理全微分方程的办法做


如果只含 $y$，这个式子就除 $M$，意味着待会计算出来的积分因子是 $\mu(y)$
并且 $F(y)$ 上面相减的部分会有一个负号的区别



整个过程的原理就是把下面的式子展开
$$
\begin{align*}
\frac{\partial  (\mu M)}{\partial y}=\frac{\partial (\mu N)}{\partial x}
\end{align*}
$$






# 凑微分法(通吃各种情况)

一般是在处理某些特殊情况下可以使用，核心思想就是把一切东西都拿到 $d()$ 后面
如果方程左右两边都能拿进 $d()$ 里面，或者可以有一部分拿进 $d()$ 里面就可以考虑

简单的三角函数，多项式函数，多项式派生的根号，都可以考虑凑微分
一般要观察一些暗示，比如 $3y^{2}dy$ 就可以干干净净地拿进变成 $d(y^{3})$ 就是这种暗示


| 注意的点                                             |                                                                  |
| ------------------------------------------------ | ---------------------------------------------------------------- |
| 纯项可以干干净净地拿进去                                     | 比如 $3y^{2}dy$ 就可以干干净净地拿进变成 $d(y^{3})$ 就是这种暗示                     |
| 交叉的项，可以凑成相乘后取微分拆开的形式                             | 比如 $x^{2}dy^{3}+y^{3}dx^{2}$ 就可以合并成 $d(x^{2}y^{3})$，如果可以这么做估计就稳了 |
| 可以利用 $dx=d(x+C)$ 凑成统一的式子，或者利用 $xdx$ 提进去可以凑成统一的次数 | 比如$\boxed{\frac{xdx}{\sqrt{1+x^{2}}}}$是可以凑的                      |


不用常数变易法解非齐次方程
如果遇到类似下面这种，**可以把一部分拿进 $d()$ 来把非齐次变成齐次方程的形式**，也可以不用常数变易法直接凑微分
$$
\begin{align*}
\frac{dp}{dx}=P(x)(p-1) \quad  \quad \\
dp  \quad 可以凑成  \quad d(p-1)\\
\\
\frac{dp}{dx}=\frac{d(p-1)}{dx}=P(x)(p-1)
\end{align*}
$$

不用积分因子解全微分方程
如果遇到类似下面这种**左右两边全都能拿进 $d()$ 里的形式**，也可不用找积分因子直接凑微分
$$
\begin{align*}
(x+y)^{2}(dx-dy)=dx+dy\\
把 \quad dx+dy  \quad 凑成 \quad d(x+y)\\
d(x-y)=\frac{d(x+y)}{(x+y)^{2}}\\
\boxed{关键是这方程右边还可以拿进d()里面}\\
d(x-y)=-d(\frac{1}{x+y})\\
d(x-y+ \frac{1}{x+y})=0\\
x-y + \frac{1}{x+y}=C
\end{align*}
$$
还有
$$
\begin{align*}
\sin y·dx+\cos ·dy=0\\
dx=-  \frac{\cos y·dy}{\sin y}\\
dx=- \frac{d(\sin y)}{\sin y}\\\boxed{关键是这方程右边还可以拿进d()里面}\\
dx=-d(\ln|\sin y|)\\
d(x+\ln|\sin y|)=0\\
x+\ln|\sin y|=C
\end{align*}
$$

还有
$$
\begin{align*}
xdx=(2xy^{3}dx+3x^{2}y^{2}dy)\sqrt{1+x^{2}}\\
\\
\frac{xdx}{\sqrt{1+x^{2}}}=2xy^{3}dx+3x^{2}y^{2}dy
\\\\

\boxed{\frac{xdx}{\sqrt{1+x^{2}}}=\frac{d(x^{2}+1)}{\sqrt{1+x^{2}} }=d(\sqrt{1+x^{2}})}\\\\

\boxed{2xy^{3}dx+3x^{2}y^{2}dy=y^{3}d(x^{2})+x^{2}d(y^{3})=d(x^{2}y^{3})}\\
\\
于是 \quad d(x^{2}y^{3}-\sqrt{1+x^{2}})=0
\end{align*}
$$

还有
$$
\begin{align*}
y(x+1)dx+x(y+1)dy=0  \quad  \quad  \quad \\
一看都是多项式，就可以凑微分，全部拆开\\
\\
xydx+ydx+xydy+xdy=0\\
xy(dx+dy)+d(xy)=0\\
xy·d(x+y)=-d(xy)\\
d(x+y)=\frac{-d(xy)}{xy}\\ \boxed{关键是这方程右边还可以拿进d()里面}\\

d(x+y)=-d\ln|xy|\\
\\ d(x+y+\ln|xy|)=0\\

x+y+\ln(xy)=C
\end{align*}
$$
# 二阶非齐次线性方程

对于 $\displaystyle 2$ 阶线性齐次方程，就要找 $\displaystyle 2$ 个线性无关的解 $\displaystyle y_{1}$ 和 $\displaystyle y_{2}$，这些解的线性组合 $\displaystyle C_{1}y_{1}+C_{2}y_{2}$ 就是线性方程的通解 $\displaystyle y_{0}$

对于 $\displaystyle 2$ 阶线性非齐次方程，就要先把它的非齐次部分忽略掉，然后找出那齐次情况下的通解 $\displaystyle y_{0}$，再回到原方程解个特解 $\displaystyle y^*$ 出来，那么这个方程的解就是 $\displaystyle y^*+y_{0}$

对于 $\displaystyle 2$ 阶线性非齐次方程 $\displaystyle L(y_{1}+y_{2})=f_{1}(x)+f_{2}(x)$ 可以把它复杂的非齐次部分拆解开，化成多个简单的非齐次部分的非齐次方程 $\displaystyle \begin{cases}L(y_{1})=f_{1}(x)\\ L(y_{2})=f_{2}(x)\end{cases}$

## 降阶法法求线性齐次方程的另一个线性无关的解 (不考)

二阶线性齐次方程 $\displaystyle y''+P(x)y'+Q(x)y=0$，如果 $\displaystyle y_{1}$ 为一个非零解
则令 $\displaystyle y_{2}=u(x)y_{1}$ 且 $\displaystyle y_{1}$ 和 $\displaystyle y_{2}$ 线性无关

那么代入 $\displaystyle y_{2}$ 到方程中会得到 $\displaystyle y_{1}u''+(2y_{1}'+P(x)y_{1})u'=0$ 这个方程。再令 $\displaystyle v=u'$ 就可以降阶

最后解得 $\displaystyle y_{2}=y_{1}\int \frac{1}{y_{1}^{2}}e^{ -\int P(x) \, dx } \, dx$

## 常数变易法 (不考)

面对二阶线性非齐次方程 $\displaystyle y''+P(x)y'+Q(x)y=f(x)$ 已经知道了它在齐次状态下的通解 $\displaystyle y_{0}=C_{1}y_{1}+C_{2}y_{2}$ 的话，就把常数都看成函数 ($\displaystyle y=C_{1}(x)y_{1}+C_{2}(x)y_{2}$)

然后解方程 $\displaystyle \begin{cases}y_{1}C_{1}'+y_{2}C_{2}'=0\\ y_{1}'C_{1}'+y_{2}'C_{2}'=f(x)\end{cases}$ 得到 $\displaystyle C_{1}(x)$ 和 $\displaystyle C_{2}(x)$ 代回到 $\displaystyle y=C_{1}(x)y_{1}+C_{2}(x)y_{2}$ 就是非齐次方程的解了


# 二阶线性常系数方程 (考)

## 齐次方程的通解

对于 $\displaystyle y''+Py'+Q=0$ 设 $\displaystyle y=e^{ \lambda x }$ 得到特征方程 $\displaystyle \lambda^{2}+P\lambda+Q=0$

| 特征方程解的情况                                                                                                         | 基底                                                                                                                                                                                                                        | 原函数就是基底张成的线性空间                                                                                          |
| ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| 如果 $\displaystyle \lambda_{1}\ne \lambda_{2}$                                                                    | $\displaystyle \begin{cases}e^{ \lambda_{1}x }\\ e^{ \lambda_{2}x }\end{cases}$                                                                                                                                           | $\displaystyle y(x)=C_{1}e^{ \lambda_{1}x }+C_{2}e^{ \lambda_{2}x }$                                    |
| 如果 $\displaystyle \lambda_{1}=\lambda_{2}$ (二重实根)                                                                | $\displaystyle \begin{cases}e^{ \lambda_{1}x }\\ xe^{ \lambda_{1}x }\end{cases}$                                                                                                                                          | $\displaystyle y(x)=C_{1}e^{ \lambda_{1}x }+C_{2}xe^{ \lambda_{1}x }$                                   |
| 如果 $\displaystyle \lambda_{1}$ 和 $\displaystyle \lambda_{2}$ 都是共轭复根，那么设 $\displaystyle \lambda=\alpha\pm i\beta$ | $\displaystyle \begin{cases}e^{ \alpha x }\cos \beta x \\ e^{ \alpha x }i\sin \beta x\end{cases}$                                                                                                                         | $\displaystyle y(x)=C_{1}\cos \beta xe^{ \alpha x }+C_{2}i\sin \beta xe^{ \alpha x }$                   |
| 如果有 $\displaystyle k$ 重实根                                                                                        | $\displaystyle e^{ \lambda x }$ 前依次乘 $\displaystyle x,x^{2},\dots,x^{k-1}$ 作为基底 $\displaystyle \begin{cases}(1):e^{ \lambda_{1}x }\\ (2):xe^{ \lambda_{1}x }\\  \quad \vdots\\ (k): x^{k-1}e^{ \lambda_{1}x }\end{cases}$ | $\displaystyle y(x)=C_{1}e^{ \lambda_{1}x }+C_{2}xe^{ \lambda_{1}x }+\dots+C_{k}x^{k-1}e^{ \lambda x }$ |
| 如果有 $\displaystyle k$ 重共轭复根                                                                                      | 也是在这两个 $\displaystyle \begin{cases}e^{ \alpha x }\cos \beta x \\ e^{ \alpha x }i\sin \beta x\end{cases}$ 前依次乘 $\displaystyle x,x^{2},\dots,x^{k-1}$ 作为基底                                                                  |                                                                                                         |

这是由 $\displaystyle e^{ (\alpha\pm i\beta)x }=e^{ \alpha x }·e^{ i\beta x }=e^{ \alpha x }(\cos \beta x+i\sin \beta x)$ 得到的两个基底为 $\displaystyle \begin{cases}e^{ \alpha x }\cos \beta x \\ e^{ \alpha x }i\sin \beta x\end{cases}$

如果遇到其中一个项($\displaystyle y$、$\displaystyle y'$) 没有时，就把它们对应的系数写成 0 继续套特征方程

## 非齐次方程的特解

对于  $\displaystyle y''+Py'+Q=P_{n}(x)e^{ tx }$ 其中如果右边不是 $\displaystyle P_{n}(x)e^{ tx }$ 这种形式，那就只能用常数变易法

| 讨论 $\displaystyle t$ 和特征方程的关系   | 特解，其中 $\displaystyle Q_{n}(x)=b_{0}+b_{1}x+\dots+b_{n}x^{n}$ |
| ------------------------------- | ------------------------------------------------------------ |
| 如果 $\displaystyle t$ 不是是特征方程的根  | $\displaystyle y^*=Q_{n}(x)e^{ tx }$                         |
| 如果 $\displaystyle t$ 是特征方程的单实根  | $\displaystyle y^{*}=xQ_{n}(x)e^{ tx }$                      |
| 如果 $\displaystyle t$ 是特征方程的二重实根 | $\displaystyle y^*=x^{2}Q_{n}(x)e^{ tx }$                    |

注意遇到右边是纯多项式的时候，意味着 $\displaystyle t=0$，此时要看看 $\displaystyle t=0$ 是不是特征方程的根

遇到右边是只有一种三角函数的，先算右边是指数函数的样子，求出解之后再根据欧拉公式 $\displaystyle e^{ ix }=\cos x+i\sin x$ 来取实部(原来是 $\displaystyle \cos$)或者虚部(原来是 $\displaystyle \sin$)

遇到右边是有两种三角函数的，可以使用线性方程的叠加原理，把右边拆成两个单独的，也可以用下面的公式

| $\displaystyle y''+Py+Q=P_{n}(x)e^{ \alpha x }(a\cos \beta x+b\sin \beta x)$ | $\displaystyle A_{m}$ 和 $\displaystyle B_{m}$ 是 $\displaystyle m$ 次多项式 ($\displaystyle m$ 是 $\displaystyle a,b$ 中次数最高的次数) |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| 如果 $\displaystyle \alpha\pm i \beta$ 不是特征根                                   | $y^*=\displaystyle e^{ \alpha x }(A_{m}\cos \beta x+B_{m}\sin \beta x)$                                                   |
| 如果 $\displaystyle \alpha\pm i\beta$ 是一对特征根                                   | $y^*=\displaystyle e^{ \alpha x }x(A_{m}\cos \beta x+B_{m}\sin \beta x)$                                                  |
| 如果 $\displaystyle \alpha\pm i \beta$ 是 $\displaystyle k$ 对特征根                | $y^*=\displaystyle e^{ \alpha x }(x^k)(A_{m}\cos \beta x+B_{m}\sin \beta x)$                                              |

在遇到右边是两个类型的函数，就分成两个方程来做，利用叠加原理

# 欧拉方程

对于 $\displaystyle a_{0}x^{n}y^{(n)}+a_{1}x^{n-1}y^{(n-1)}+\dots+a_{n}x^{0}y=0$

令 $\displaystyle x=e^{t}$，则上面方程的每一项可以简化成下面这样 $\displaystyle x^{k}y^{(k)}=D(D-1)\dots(D-k+1)y$，其中 $\displaystyle D=\frac{d}{dt},D^{n}=\frac{d^n}{dt^n}$

再带回去原来的方程，得到 $\displaystyle(d_{0}+d_{1}D+d_{2}D^{2}+\dots+d_{n}D^{n})y=0$

那么 $\displaystyle \frac{dy}{dt}$ 的特征方程就是 $\displaystyle d_{n}\lambda^n+\dots+d_{1}\lambda_{1}+d_{0}=0$
这样就能求出 $\displaystyle y(t)$ 的通解，然后再代入 $\displaystyle x=e^{t}$ 得到 $\displaystyle y(x)$ 的通解

# 一些不太重要的

如果知道这样一个初值问题 $\begin{cases}\displaystyle \frac{dy}{dx}=f(x,y)\\y_{0}(x)=y_{0} \end{cases}$，那么就可以用下面的递推式依次递推去近似解出原函数 $\displaystyle y_{n}(x)=y_{0}+\int_{x_{0}}^{x}f(x,y_{n-1}(x))dx$ 先代入 $\displaystyle y_{0}(x)$ 求 $\displaystyle y_{1}(x)$ 开始，依次递推求解

如果想要知道解的区间，就还得知道这个初值点所在的区间 $\displaystyle R:\begin{cases}\mid x-x_{0}\mid \leq a\\ \mid y-y_{0} \mid \leq b \end{cases}$
然后这个解就落在 $\displaystyle \mid x-x_{0}\mid \le h$ 其中 $\displaystyle h=\min\left( a,\frac{b}{f_{MAX inR}(x,y)} \right)$ 


对于二阶线性齐次方程 $\displaystyle y''+P(x)y'+Q(x)y=0$，$\displaystyle W(x)=W(x_{0})e^{ -\int_{x_{0}}^{x}P(t)dt }$
其中 $\displaystyle \phi_{1}(x)$ 和 $\displaystyle \phi_{2}(x)$ 是方程的两个解 $\displaystyle W(x)=\left |\begin{matrix} \phi_{1}(x)& \phi_{2}(x)\\ \phi'_{1}(x)&\phi_{2}'(x)\end{matrix}\right|$
如果 $\displaystyle \phi_{1}(x)$ 和 $\displaystyle \phi_{2}(x)$ 是线性相关的，那么 $\displaystyle W(x)=0$
如果 $\displaystyle \phi_{1}(x)$ 和 $\displaystyle \phi_{2}(x)$ 是线性无关的，那么 $\displaystyle W(x)\ne0$，而且 $\displaystyle \phi_{1}$ 和 $\displaystyle \phi_{2}$ 也不会存在公共零点


$\displaystyle \frac{dy}{dx}=f(x,y)$ 存在唯一解 $\displaystyle y=\phi(x)$ 则当 $\displaystyle 0=\phi(x_{0})$ 时，$\displaystyle \phi'(x_{0})\ne 0$；
是这样吗，问问