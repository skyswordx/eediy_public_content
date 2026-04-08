---
title: 线性代数做题总结 v0.0
description: 在这里记录做线代作业遇到的知识点和方法，方便在期末周分清 ppt 里面哪些是考点
tags: 
  - 线性代数
---


| 主题  | 参考来源            |
| --- | ------- |
| 行列式 | @[一高数\|线性代数：覆盖本科到考研！1小时搞定行列式核心题型](https://www.bilibili.com/video/BV1nx4y1Q7Zs/?spm_id_from=333.337.search-card.all.click&vd_source=9c85d181a345808c304a6fa2780bb4da)<br> |


# 行列式

| 行列式的性质                                        |                                             |
| --------------------------------------------- | ------------------------------------------- |
| 行列式与它的转置行列式相等                                 | 行列式中行与列具有同等的地位，行列式的性质凡是对行成立的对列也同样成立         |
| 互换行列式的两行，行列式变号                                | 如果行列式有两行(列)完全相同，则此行列式为零                     |
| 行列式的某一行(列)中所有的元素都乘以同一个倍数 $k$，等于用 $k$ 乘以此行列式   |                                             |
| 如果行列式的某一列(行)的元素都是两数之和，那么可以拆开成两个对应列(行)的子行列式    |                                             |
| 把行列式的某一列(行)的各元素乘以同一个倍数然后加到另一列(行)对应的元素上去，行列式不变 | 以数 $k$ 乘第 $j$ 行加到第 $i$ 行上，记作 $r_{i}+kr_{j}$ |

| 场景                      | 适用的计算方法                                                                                      |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| 简单的二、三阶行列式              | 对角线法则                                                                                        |
| 高阶行列式                   | 行列式展开，注意了，行列式展开时，代数余值式是**有符号**的！！！                                                           |
| 行列式里面零元素多，非零元素较少，并且分布很散 | 利用行列式的定义，找准非零元素，充分利用不同行不同列的规则分类找全元素，结合**逆序数**判断符号                                            |
| 行列式非零元素较多               | 利用倍加行列式值不变的性质，创造更多的零元素，凑成按行展开(**在一行或者一列创造零元素**) 或者主对角(三角)行列式(**把每行第一个化成零元素，先上再下，从左到右创造零元素**) |
| 行相等型                    | 每一行的元素之和都相等，把每一行的元素都依次加到第一列，对第一列提取公因式，再倍加得到主对角线型                                             |
| 爪型                      | 把爪子中间斜着的部分倍加消去两边中的一行或者一列，得到主对角线型                                                             |
| 矩阵分块型                   | 需要用倍加的性质凑成主对角线区域是方阵的形式                                                                       |
| 加边法                     | 遇到每一行都有相似的构造                                                                                 |
| 么型                      | 按照开口方向的那一行或者那一列展开，会得到递推的么型和主对角线型                                                             |
| 川型                      | 先展一行，再展一列，把所有的子行列式变成川型再递推                                                                    |
## 一些经验

第 $n$ 行移动到第一行要经历 $n-1$ 次移动，而第n列移动到第一行也是要经历n-1次移动
如果为达成行的移动效果要付出x次步数，那么列也要得到同样的效果也要付出x次步数


## 行相等型/列相等型

就比如列相等型。遇到每一列元素之和都一样的行列式，把从第二行起的各行统统加到第一行，再提取公因式，在**利用元素都相等的第一行倍加消掉各行**，得到主对角行列式

$$
\begin{align*}
|1+a11122+a22333+a34444+a|
\end{align*}
$$
## 爪型

这种行列式只有爪子位置上的元素不是零，其余位置都是零，**注意爪子的朝向可以改变**
利用处于爪子中间的对角线元素把处于爪子两边的一行(或一列)元素倍加消掉，进而得到主对角行列式

$$
\begin{align*}
|1−1−1−1−11a12a23a34a4|
\end{align*}
$$
## 矩阵分块型



如果 $A$ 为 $m$阶矩阵，$B$ 为 $n$阶矩形，**只要 $A$ 和 $B$ 是方的即可！！**
$$
\begin{align*}
|AOOB|=|ACOB|=|AODB|=|A||\boldsymbol{B}| \
|OABO|=|OABC|=|DABO|=(−1)^{mn}|A||\boldsymbol{B}|
\end{align*}
$$

遇到 $0$ 或者元素摆的很整齐、很方正时(比如矩形或者叉型)，很多时候需要通过**交换行或者列**凑成上面的六种形式之一
$$
\begin{align*}
|0ab0a00b0cd0c00d|=
\end{align*}
$$
## 代数余值式型

计算同一套代数余值式 $A_{i1},A_{i2},...,A_{in}$ 的线性组合，把余值式之前的系数替换原来行列式对应的地方，反推出一个新的行列式，而那同一套代数余值式线性组合的值就是新行列式的值

如果遇到了无符号的余值式 $M_{i1},M_{i2},...,M_{in}$ 的线性组合，那么就用 $M_{ij}=(-1)^{i+j}A_{ij}$ 来转化成上面的情况

由于如果行列式任意一行或一列成比例，那么行列式值为零，所以有下面推论
$$
a_{i1}A_{j1}+a_{i2}A_{j2}+\cdots+a_{in}A_{jn}=\begin{cases}当i=j，a_{i1}A_{i1}+a_{i2}A_{i2}+\cdots+a_{in}A_{in}=D \\ 当i \ne j，a_{i1}A_{j1}+a_{i2}A_{j2}+\cdots+a_{in}A_{jn}=0\end{cases}
$$
因为当 $i$ 和 $j$ 不同时，替换之后的行列式就有两行相同(当然是成比例)，使行列式的值为零

$$
\begin{align*}
D=\begin{vmatrix}*&*&*&*\\a_{i1}&a_{i2}&a_{i3}&a_{i4}\\\vdots&\vdots&\vdots&\vdots\\a_{j1}&a_{j2}&a_{j3}&a_{j4}\end{vmatrix}\quad\longrightarrow\quad\begin{vmatrix}*&*&*&*\\a_{i1}&a_{i2}&a_{i3}&a_{i4}\\\vdots&\vdots&\vdots&\vdots\\a_{i1}&a_{i2}&a_{i3}&a_{i4}\end{vmatrix}_{第j行元素被第i行元素替换}\longrightarrow0
\end{align*}
$$


## 加边法

和行相等型场景相似，但使用范围广于它。观察原来行列式 $|A|$ 的**行或者列整体上有没有特点**，如果有的话，就是逆着使用行列式展开定理，并在 $*$ 那一行或者列填入所需的数字，然后利用 $*$ 的行或者列去化简原本的行列式即可
$$
|A|=\begin{vmatrix}1&0&\cdots&0\\ *&&&\\\vdots&&A&&\\ *&&&\end{vmatrix}=\begin{vmatrix}1&*&\cdots&*\\0&&&\\\vdots&&A&&\\0&&&\end{vmatrix}
$$

就比如下面这个行列式每一列都有相同的公共元素，那么要填 $*$ 的这一列就确定了，之后就使用加边法进行化简，会变成**爪型**行列式
$$
\begin{align*}
\begin{vmatrix}1+a_{1}&1&1&1\\2&2+a_{2}&2&2\\3&3&3+a_{3}&3\\4&4&4&4+a_{4}\end{vmatrix}
\end{align*}
$$

## 么型

这种行列式只有在类似 “么” 字的痕迹上的元素才不是零，其他位置的元素都是零。注意这个也有四个朝向的么型

按照么型开口的方向的那一行或者列展开就好，会得到一个小一点的么型构成递推式，和一个主对角行列式
$$
\begin{align*}
\begin{vmatrix}A&b&&\\&A&b&\\&&A&b\\A&A&A&A\end{vmatrix}_{n \times n}^{按照么型开口的方向展开}\longrightarrow \begin{vmatrix}A&b\\&A&b\\A&A&A\end{vmatrix}_{(n-1)\times(n-1)}^{小一点的么型构成递推式} +\begin{vmatrix}A&b&\\&A&b\\&&A\end{vmatrix}_{(n-1)\times(n-1)}^{主对角行列式}
\end{align*}
$$

例子
$$
\begin{align*}
D_n=\begin{vmatrix}2&0&\cdots&0&2\\-1&2&\cdots&0&2\\&-1&\ddots&\vdots&\vdots\\&&\ddots&2&2\\&&&-1&2\end{vmatrix}
\end{align*}
$$
## 川型

目的是把川型行列式的所有子行列式变成川型，以便按照递推式计算，先按行展开，再对非川型按列展开
$$
\begin{vmatrix}A&b&&\\c&A&b&\\&c&A&b\\&&c&A\end{vmatrix}_{n \times n}^{先展一行，再对非川型展一列}\longrightarrow \begin{vmatrix}A&b\\c&A&b\\&c&A\end{vmatrix}_{(n-1)\times(n-1)}^{展完一行后已得到川型} +\begin{vmatrix}c&b&\\&A&b\\&c&A\end{vmatrix}_{(n-1)\times(n-1)}^{非川型，再展一列}
$$

例子
$$
\begin{align*}
D_n=\begin{vmatrix}2a&1&&&\\a^2&2a&1&&\\&a^2&2a&\ddots&\\&&&\ddots&\ddots&1\\&&&&a^2&2a\end{vmatrix}
\end{align*}
$$
## 范德蒙行列式


关键是看到每一列都是逐次方递增的形式要想到这个，然后思路是把首行变成 1，这常常需要使用倍加的性质凑出来
$$
\begin{align*}
\left.V_n=\left|\begin{array}{cccc}1&1&\cdots&1\\x_1&x_2&\cdots&x_n\\x_1^2&x_2^2&\cdots&x_n^2\\\vdots&\vdots&\ddots&\vdots\\x_1^{n-1}&x_2^{n-1}&\cdots&x_n^{n-1}\end{array}\right.\right|=\prod_{n\geqslant i>j\geqslant1}(x_i-x_j)=\prod(\text{后面的项-前面的项})\\
固定一个后面的项，遍历减去前面的项
\end{align*}
$$
例子
$$
\begin{align*}
\begin{vmatrix}b+c&a+c&a+b\\a&b&c\\a^2&b^2&c^2\end{vmatrix}\\\\

\begin{vmatrix}x&k&x^2&x^3\\a&k&a^2&a^3\\b&k&b^2&b^3\\c&k&c^2&c^3\end{vmatrix}
\end{align*}
$$

# 矩阵

矩阵是存储信息的数表。可以用系数矩阵和增广矩阵存储方程组的信息
行列式的本质是数而已，和矩阵这个数表有本质的区别

矩阵同型是两个矩阵的行数和列数均相等

矩阵相等，就是说两个矩阵同型，并且元素均对应相等

| 矩阵的运算 | 要求                                                            | 例子                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 性质                                |                                                                                                             | 与行列式的区别              |
| ----- | ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- | ----------------------------------------------------------------------------------------------------------- | -------------------- |
| 加减法   | 同型                                                            | $A+B=\begin{pmatrix}a_{11}+b_{11}&a_{12}+b_{12}&\cdots&a_{1n}+b_{1n}\\a_{21}+b_{21}&a_{22}+b_{22}&\cdots&a_{2n}+b_{2n}\\\vdots&\vdots&\ddots&\vdots\\a_{m1}+b_{m1}&a_{n2}+b_{n2}&\cdots&a_{mn}+b_{mn}\end{pmatrix}_{m\times n}$                                                                                                                                                                                                                                                                                                                                                                                      | 交换律                               | 结合律                                                                                                         | 行列式的本质是数，所以可以化成数后随便搞 |
| 数乘    | 无                                                             | $\lambda A=\begin{pmatrix}\lambda a_{11}&\lambda a_{12}&\cdots&\lambda a_{1n}\\\lambda a_{21}&\lambda a_{22}&\cdots&\lambda a_{2n}\\\vdots&\vdots&\ddots&\vdots\\\lambda a_{m1}&\lambda a_{m2}&\cdots&\lambda a_{mn}\end{pmatrix}_{m\times n}$                                                                                                                                                                                                                                                                                                                                                                       | 结合律$(\lambda\mu)A=\lambda(\mu A)$ | 分配律$\begin{cases}(\lambda+\mu)A=\lambda A+\mu A\\\lambda(A+B)=\lambda A+\lambda B\end{cases}$               | 数乘行列式等于把数作用到某一行上     |
| 乘法    | 前提看内标，结果看外标<br>$A_{m\times s}B_{s\times n}=C_{m\times n}$<br> | 先判断是否可以相乘，再看看结果的矩阵是几行几列的。计算时左取第 $i$ 行，右取第 $j$ 列，对应相乘再相加，构成新矩阵处在 $(i,j)$位置的元素<br>$c_{ij}=(a_{i1},a_{i2},\cdots,a_{is})\text{与}\begin{pmatrix}b_{1j}\\b_{2j}\\\vdots\\b_{sj}\end{pmatrix}\text{对应相乘再相加}$ 就$\left.A_{m\times s} B_{s\times n}=\left(\begin{array}{ccc}{\cdots}&{\cdots}&{\cdots}\\{a_{i1}}&{\cdots}&{a_{is}}\\{\cdots}&{\cdots}&{\cdots}\end{array}\right.\right)\left(\begin{array}{ccc}{\vdots}&{b_{ij}}&{\vdots}\\{\vdots}&{\vdots}&{\vdots}\\{\vdots}&{b_{sj}}&{\vdots}\end{array}\right)=\left(\begin{array}{ccc}&{\vdots}\\{\cdots}&{c_{ij}}&{\cdots}\\&{\vdots}\end{array}\right)=C_{m\times n}$ | 一般没交换律                            | 一般没消去律$\begin{aligned}&AB=O\nRightarrow A=O\text{或}B=O\\&AB=AC\text{且}A\neq O\nRightarrow B=C\end{aligned}$ |                      |
|       |                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | 结合律$(AB)C=A(BC)$                  | 分配律$\begin{cases}\text{左分配律:}C(A+B)=CA+CB\\\text{右分配律:}(A+B)C=AC+BC\end{cases}$                             |                      |

## 矩阵乘法中的特殊矩阵

下面是矩阵乘法中的特殊矩阵，其中对角矩阵的幂很好计算
$$
\begin{align*}
矩阵:\begin{cases}零矩阵:O乘任何矩阵都得O \\
单位矩阵:E乘任何矩阵都得任何矩阵自身\\
数量矩阵:kE 乘任何矩阵都得k倍的任何矩阵\end{cases}  \quad 数:\begin{cases}0乘任何数都为0\\
1乘任何数都为1\\
k乘任何数都为k的倍数\end{cases}\\
\\
矩阵:\begin{cases}\text{对角矩阵：主对角线一侧的元素均为零}\\\text{三角矩阵：主对角线一侧的元素均为零}\\\text{对称矩阵：}A^{\mathrm{T}}=A,\text{元素关于主对角线对称的矩阵}\end{cases} \quad  \quad  \quad  \quad  \quad  \quad  \quad   \\
\begin{pmatrix}a&&\\&b&\\&&c\end{pmatrix}_{对角矩阵},\begin{pmatrix}a&d&e\\&b&f\\&&c\end{pmatrix}_{三角矩阵},\begin{pmatrix}a&d&e\\d&b&f\\e&f&c\end{pmatrix}_{对称矩阵} \quad  \quad  \quad  \quad  \quad 
\end{align*}
$$
这些特殊的矩阵，尤其是对角矩阵在分块矩阵中常常用到。倾向于把矩阵分块成对角矩阵。
单位阵在初等变换时也常常变换成初等矩阵，数量矩阵其实就是一种初等矩阵。

## 转置矩阵

转置在后面进行初等变换时，可以把需要进行初等列变换的变更成做初等行变换
$$
\begin{aligned}
转置的性质:\begin{cases}(A^{\mathrm{T}})^{\mathrm{T}}=A \\
(A+B)^\mathrm{~T}=A^\mathrm{T}+B^\mathrm{~T} \\
(\lambda A)^\mathrm{~T}=\lambda A^\mathrm{T} \\
(ABC)^{\mathrm{T}}=C^{\mathrm{T}}B^{\mathrm{T}}A^{\mathrm{T}} \\
\displaystyle AA^{T}=O \to A=O\end{cases}
\end{aligned}
$$

## 方阵的行列式

求方阵的行列式一定要注意，这个方阵是几阶的，方阵前的系数变成行列式前的系数时，就要乘几次方
$$
\begin{align*}
\begin{aligned}
方阵行列式的性质:\begin{cases}|A^{\mathrm{T}}|=|A| \\
\boxed{|\lambda A_{n \times n}|=\lambda^{n}|A_{n \times n}|} \\
|AB|=|A|\left|B\right|\Longrightarrow|ABC|=|A|\left|B\right|\left|C\right|\end{cases}
\end{aligned}
\end{align*}
$$

## 伴随矩阵

伴随矩阵，计算伴随矩阵的时候，一定要小心元素的位置，以及代数余子式的正负号问题
$$
\begin{align*}
\text{伴随矩阵}A^*=\begin{pmatrix}A_{11}&A_{12}&\cdots&A_{1n}\\A_{21}&A_{22}&\cdots&A_{2n}\\\vdots&\vdots&\ddots&\vdots\\A_{n1}&A_{n2}&\cdots&A_{nn}\end{pmatrix}^\mathrm{\boxed{T}}=\begin{pmatrix}A_{11}&A_{21}&\cdots&A_{n1}\\A_{12}&A_{22}&\cdots&A_{n2}\\\vdots&\vdots&\ddots&\vdots\\A_{1n}&A_{2n}&\cdots&A_{nn}\end{pmatrix},\text{其中}A_{ij}\text{表示矩阵}A\text{的代数余子式}
\end{align*}
$$
遇到 $n$ 阶伴随矩阵 $A^{*}$ 要想到两个式子，一个是 $A·A^{*}=|A|E_{n}$

对 $A·A^{*}=|A|E_{n}$ 这个式子再求一次行列式，这个和是几阶的矩阵有关系
如果 $A$ 是 $n$ 阶的矩阵，那么 $E$ 就是 $n$ 阶的矩阵，所以
$$
|A·A^{*}|=|A|·|A^{*}|=||A|E_{n}|=|A|^{n}
$$
事实上这个式子的证明就体现了为啥定义伴随矩阵要给对应元素的代数余子式加上转置。
因为要使得原矩阵左乘伴随矩阵一一对应。

## 逆矩阵

逆矩阵，若 $A,B$ 是同阶矩阵，则可逆的条件总结起来有下面三个
$$
\begin{align*}
\begin{aligned}&A\text{可逆的条件}\Longleftrightarrow\begin{cases}\text{具体矩阵:}|A|\neq0\\\text{抽象矩阵:存在}AB=E(\text{或}BA=E)\\
A 是满秩矩阵\end{cases}\end{aligned}
\end{align*}
$$
逆矩阵的性质总结如下
$$
\begin{aligned}
逆矩阵的性质: \begin{cases}若A可逆,则A^{-1}亦可逆,且(A^{-1})^{-1}=A \\
\text{若}A\text{ 可逆},\text{数}\lambda\neq0,\text{则}\lambda A\text{ 可逆},\text{且}(\lambda A)^{-1}=\frac1\lambda A^{-1} \\
\text{若}A,B\text{ 为同阶矩阵且均可逆},\text{则}AB\text{亦可逆},\text{且}(AB)^{-1}=B^{-1}A^{-1} \\
\text{若}A\text{ 可逆,则}A^\mathrm{T}\text{亦可逆,且}(A^\mathrm{T})^{-1}=(A^{-1})^\mathrm{T}\end{cases}
\end{aligned}
$$
由 $AA^{*}=|A|E$ 得 $A· \frac{A^{*}}{|A|}=E$，则 $A^{-1}=\boxed{\frac{A^{*}}{|A|}}$ 
这里其实体现了给定一个方程求特定抽象矩阵的逆矩阵，就是要凑出逆矩阵定义形式的思想。
$$
\begin{align*}
\boxed{某个矩阵} \times \boxed{它的逆矩阵}=E 
\end{align*}
$$
就比如随便已知一个暂时没有矩阵 $P$ 方程，要求 $P$ 的逆矩阵。就是把方程的一边变为 $E$ 单位阵，然后另一边也要凑出目标矩阵 $P$ 的形式，得到 $P(...)=E$ 的形式，那么除了 $P$ 以外的剩下的东东就是 $P$ 的逆矩阵啦

## 矩阵公式总结

| 矩阵公式总结        |                                                                                                                                                                                                                          |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 操作顺序可交换       | $(A^x)^y=(A^y)^x\Longrightarrow\begin{cases}(A^*)^{-1}=(A^{-1})^*\\(A^{\mathrm{T}})^*=(A^*)^\mathrm{T}\\(A^{-1})^\mathrm{T}=(A^\mathrm{T})^{-1}\end{cases}$                                                              |
| 对整体操作要对调      | $(AB)^x=B^xA^x\Longrightarrow\begin{cases}(AB)^{-1}=B^{-1}A^{-1}\\(AB)^{\mathrm{T}}=B^{\mathrm{T}}A^{\mathrm{T}}\\(AB)^{*}=B^{*}A^{*}\end{cases}$                                                                        |
| 重复操作会还原（伴随除外） | $(A^?)^?\Longrightarrow\begin{cases}(A^\mathrm{T})^\mathrm{T}=A\\(A^{-1})^{-1}=A\end{cases}$                                                                                                                             |
| 转置的优良线性       | $A^\mathrm{T}\Longrightarrow\begin{cases}\|A^\mathrm{T}\|=\|A\|\\(kA)^\mathrm{T}=kA^\mathrm{T}\\\boxed{(A+B)^\mathrm{T}=A^\mathrm{T}+B^\mathrm{T}}\end{cases}$                                                           |
| 逆矩阵           | $AA^{-1}=E\Longrightarrow\begin{cases}\left\|A^{-1}\right\|=\dfrac{1}{\left\|A\right\|}\\\left(kA\right)^{-1}=\dfrac{1}{k}A^{-1}\end{cases}$                                                                             |
| 伴随等于行列式乘逆     | $A^*=\|A\|A^{-1}\Longrightarrow\begin{cases}(kA)^*=k^{n-1}A^*\\\|A^*\|=\|A\|^{n-1}\\(A^*)^*=\|A\|^{n-2}A\end{cases}$                                                                                                     |
| 方阵的行列式        | $\begin{cases}\|A^{\mathrm{T}}\|=\|A\| \\<br>\boxed{\|\lambda A_{n \times n}\|=\lambda^{n}\|A_{n \times n}\|} \\<br>\|AB\|=\|A\|\left\|B\right\|\Longrightarrow\|ABC\|=\|A\|\left\|B\right\|\left\|C\right\|\end{cases}$ |

## 分块矩阵

分块矩阵想咋分咋分，但是要保证在运算时内部子块和外部结构都是合法的。

计算逆矩阵时，看看可不可以分块成下面两种对角型矩阵
$$
\begin{align*}
主对角型:\left (\begin{matrix} A& 0\\ 0&B\end{matrix}\right)^{-1}=\left (\begin{matrix} A^{-1}& 0\\ 0&B^{-1}\end{matrix}\right)\\
副对角型:\left (\begin{matrix} 0& A\\ B&0\end{matrix}\right)^{-1}=\left (\begin{matrix} 0& B^{-1}\\ A^{-1}&0\end{matrix}\right)
\end{align*}
$$

对角阵计算幂时十分方便。计算高次幂时，常常利用矩阵分块凑出对角阵
$$
\begin{align*}
\left (\begin{matrix} A& 0\\ 0&B\end{matrix}\right)^{n}=\left (\begin{matrix} A^{n}& 0\\ 0&B^{n}\end{matrix}\right)
\end{align*}
$$
并且如果 $A$ 为对角阵
$$
\begin{align*}
|A^{n}|=|A|^{n}
\end{align*}
$$

看到已知 $AP=P \Lambda$ 来求 $\phi(A)$，要看 $P$ 可不可逆
如果 $P$ 可逆，那么 $A=P\Lambda P^{-1}$，并且 $\phi(A)=P \phi(\Lambda) P^{-1}$ 
而如果 $\Lambda$ 是对角阵($\Lambda=diag(a_{1},a_{2},...,a_{n})$)，那么 $\phi(\Lambda)=diag(\phi(a_{1}),\phi(a_{2}),...,\phi(a_{n}))$

# 方程组/初等变换/秩

## 解方程组

解方程组，如果方程个数等于未知数个数，并且系数矩阵的行列式不等于0
那么就可以使用克拉默法则
$$
\begin{align*}
\begin{cases}a_{11}x_1+a_{12}x_2+\cdots+a_{1n}x_n=b_1\\a_{21}x_1+a_{22}x_2+\cdots+a_{2n}x_n=b_2\\......\\a_{n1}x_1+a_{n2}x_2+\cdots+a_{nn}x_n=b_n\end{cases}\Longrightarrow\begin{cases}|A|\neq0,\text{有唯一解} x_1=\frac{|A_1|}{|A|},x_2=\frac{|A_2|}{|A|},\cdots,x_n=\frac{|A_n|}{|A|}\\|A|=0,\text{无解或无穷多解}\end{cases}
\end{align*}
$$

条件就是
1. 系数矩阵是一个方阵，能写成行列式的形式，这需要方程组中未知数的个数 = 方程的个数
2. $\begin{cases}\text{若}|A|\neq0\Longrightarrow 能求出唯一解\\\text{若}|A|=0\Longrightarrow无解或无穷多解，并且无法确认到哪种情况\end{cases}$

方程组的解就只有：有唯一解、无解、无穷多解。可以用几何去几何表示

用高斯消元法解线性方程组，其实就是对增广矩阵进行初等行变换
初等行变换是同解变换，但是初等列变换就不是。

|          | 互换       | 倍加  | 倍乘                    |
| -------- | -------- | --- | --------------------- |
| 行列式的变换   | 要加负号     | 一样  | 某一行乘个常数k之后等价于行列式乘上一个k |
| 矩阵的初等行变换 | 可以直接换，同解 | 同解  | 可以直接乘，同解              |

类似于求解行列式要化成主对角线，解方程时要把增广矩阵换成行阶梯形矩阵
尽量把第一行的主元变成 1，然后往下打洞换成 0。再把第二行的主元变成 1，然后往下打洞换成0，以此类推，最终得到行阶梯形矩阵，再判断秩的大小

秩就是行阶梯型矩阵的非零行数，也是有效方程的个数
$$
\begin{align*}
线性方程组\begin{cases}非齐次线性方程组Ax=b\\
\begin{cases}a_{11}x_1+a_{12}x_2+\cdots+a_{1n}x_n=b_1\\a_{21}x_1+a_{22}x_2+\cdots+a_{2n}x_n=b_2\\\cdots\cdots\\a_{m1}x_1+a_{m2}x_2+\cdots+a_{mn}x_n=b_n\end{cases}\Longrightarrow \begin{cases}r(A)=r(A,b)=n&\text{仅有唯一解}\\r(A)=r(A,b)<n&\text{无穷多解}\\r(A)<r(A,b)&\text{无解}\end{cases}\\
\\ 齐次线性方程组Ax=0\\
\begin{cases}a_{11}x_1+a_{12}x_2+\cdots+a_{1n}x_n=0\\a_{21}x_1+a_{22}x_2+\cdots+a_{2n}x_n=0\\\cdots\cdots\\a_{m1}x_1+a_{m2}x_2+\cdots+a_{mn}x_n=0\end{cases}  \Longrightarrow \begin{cases}r(A)=r(A,0)=n&\text{仅有唯一解}(\text{零解})\\r(A)=r(A,0)<n&\text{无穷多解}\end{cases} \end{cases}
\end{align*}
$$
从增广矩阵写成方程组时，系数矩阵和增广矩阵的那一列之间要有等号 =

## 初等变换

初等行变换之后的矩阵行等价，是方程组同解的等价
$A\overset{r}{\operatorname*{\sim}}B:$ 矩阵$A$可以经过有限次初等行变换变成矩阵$B$
初等列变换之后的矩阵列等价，是向量组等价
$A\overset{c}{\operatorname*{\sim}}B:$ 矩阵$A$可以经过有限次初等列变换变成矩阵$B$

可以用矩阵乘法表示初等变换

由单位矩阵 $E$ 经过一次初等变换得到的矩阵称为初等矩阵
$$
\begin{align*}
\begin{pmatrix}1&0&0\\0&0&1\\0&1&0\end{pmatrix}_{\text{互换2,3行初/互换2，3列}}\quad\begin{pmatrix}1&0&0\\0&k&0\\0&0&1\end{pmatrix}_{\text{倍乘第2行/倍乘第2列}}\quad\begin{pmatrix}1&0&0\\0&1&k\\0&0&1\end{pmatrix}_{\text{第3行倍乘k加到第2行/第3列倍乘k加到第2列}}\\
\end{align*}
$$
对单位阵做什么样初等变换的操作得到这个初等矩阵，就表示对它作用的矩阵进行怎么用的初等变换的操作。

那到底是对对象做行变换还是列变换呢，就要看初等矩阵乘的位置，左行右列
左行：$A$ 左乘一个初等矩阵 $\Longleftrightarrow A$ 进行了一次相应的初等行变换
右列：$A$ 右乘一个初等矩阵 $\Longleftrightarrow A$ 进行了一次相应的初等列变换


首先如果 $A_{n\times n}$ 可逆的话，则$A_{n\times n}$ 矩阵必然满秩，$A_{n\times n}$ 矩阵必然与 $E_{n\times n}$等价
如果 $A_{n \times n}$ 不满秩，那 $A_{n \times n}$ 一定不可逆。

初等变换就相当于一个矩阵，用包含单位阵的分块矩阵进行初等变换来求矩阵

初等行变换要横着拼起来
$P(A,E)=(PA,P)$ 
$A^{-1}(A,E)=(AA^{-1},A^{-1}E)=(E,A^{-1})$

解 $AX=B \to X=A^{-1}B$ 可以使用初等行变换加分块矩阵
$A^{-1}(A,B) \to (E,A^{-1}B)$ 

在化简时也是先自上而下打洞，再自下而上打洞

解 $XA=B$ 可以先转置成 $(XA)^{T}=B^{T}\to A^{T}X^{T}=B^{T} \to X^{T}=(A^{T})^{-1}B^{T}$  
所以 $(A^{T})^{-1}(A^{T},B^{T}) \to (E,(A^{T})^{-1}B^{T})$ 最后得到 $X^{T}$ 之后再转置一下

## 秩

秩是行阶梯形矩阵的非零行行数，有效方程的个数，独立向量的个数，~~非零子式的最高阶数~~

求矩阵的秩，只用把矩阵变成行阶梯型矩阵，再数非零行数就行了

如果矩阵有参数，要分类讨论，先讨论最简单的情况

| 秩的性质             |                                                                                                                                           |                                  |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| 转置不改变秩           | $r(A)=r(A^{\mathrm{T}})=r(A^{\mathrm{T}}A)=r(AA^{\mathrm{T}})$                                                                            | $\displaystyle AA^{T}=O \to A=O$ |
| 不大于行数/列数         | $0\leqslant r\left(A_{m\times n}\right)\leqslant\min\{m,n\}$                                                                              | 有效方程的个数限制                        |
| 初等变换不改变秩         | $\begin{cases}\text{若}A\sim B,\text{则}r(A)=r(B)\\\text{若}P,Q\text{可逆},\text{则}r(PAQ)=r(A)\end{cases}$                                     | $\displaystyle R(A)=R(-A)$       |
| 秩越乘越小，越拼越大，分开加最大 | $r(AB)\leqslant\begin{cases}r(A)\\ or \\ r(B)\end{cases}\leqslant\begin{cases}r(A,B)\\ or \\ r\binom{A}{B}\end{cases}\leqslant r(A)+r(B)$ | $r(A+B)\le r(A)+r(B)$            |
| 矩阵相乘=0，则秩之和小于内标  | $A_{m\times n}B_{n\times l}=O,\text{则}r(A)+r(B)\leqslant n$                                                                               |                                  |

如果 $\displaystyle A_{m\times n}B_{n \times l}=C$ 并且 $\displaystyle R(A)=n$ 那么 $\displaystyle A$ 是列满秩矩阵
列数为 $\displaystyle n$ 的列满秩矩阵可以化简成 $\displaystyle \left (\begin{matrix} E_{n}\\ O\end{matrix}\right)$，此时 $\displaystyle R(B)=R(C)$
由此若 $\displaystyle AB=O$，且 $\displaystyle A$ 为列满秩矩阵，那么 $\displaystyle B=O$ 列满秩可以消去
# 向量组
一般都是用列向量来表示 $\displaystyle \boldsymbol{\alpha}=\begin{pmatrix}a_1\\a_2\\\vdots\\a_n\end{pmatrix}=(a_1, a_2,\cdots, a_n)^\text{T}$

向量组 $\displaystyle \begin{aligned}&\text{设}\boldsymbol{\alpha}_1,\boldsymbol{\alpha}_2,\cdots,\boldsymbol{\alpha}_m\text{均是}n\text{ 维列向量},\\&\text{则}\boldsymbol{\alpha}_1,\boldsymbol{\alpha}_2,\cdots,\boldsymbol{\alpha}_m\text{是一个向量组},\text{可以构成一个}\boldsymbol{A}_{n\times m}=(\boldsymbol{\alpha}_1,\boldsymbol{\alpha}_2,\cdots,\boldsymbol{\alpha}_m)\end{aligned}$
其实向量组是可以和矩阵对应起来的


线性方程组有方程的形式、增广矩阵的形式，以及向量组表示的形式

秩就是是增广矩阵化成行阶梯矩阵的非零行数，也是一般形式中有效方程的个数，也是向量组中独立向量的个数 

$\displaystyle \text{增广矩阵}\begin{pmatrix}1&0&0&1\\0&1&0&1\\0&1&0&1\end{pmatrix}\quad\Rightarrow \text{行向量组}\begin{pmatrix}\alpha_1^T\\\alpha_2^T\\\alpha_3^T\end{pmatrix}$ 其中第三行是无效方程，其实也就是不独立的向量

$\displaystyle \text{增广矩阵}\begin{pmatrix}1&0&0&1\\0&1&0&1\\0&0&1&1\end{pmatrix}\quad=\text{列向量组}(\alpha_1,\alpha_2,\alpha_3,\alpha_4)$ 其中第四列是非独立的向量，可以由独立向量线性表示

| 概念   | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 线性组合 | $\displaystyle \begin{align}\text{给定向量组:}\boldsymbol{\alpha}_1,\boldsymbol{\alpha}_2,\cdots,\boldsymbol{\alpha}_m,\text{对于任何一组常数}k_1, k_2,\cdots,k_m,\\\text{则}k_1\boldsymbol{\alpha}_1+k_2\boldsymbol{\alpha}_2+\cdots+k_m\boldsymbol{\alpha}_m\text{称为向量组的一个线性组合}\end{align}$                                                                                                                                                                                                                                                                                                                                                                                      |
| 线性表示 | $\displaystyle \begin{aligned}&\text{给定向量组:}\boldsymbol{\alpha}_1,\boldsymbol{\alpha}_2,\cdots,\boldsymbol{\alpha}_m\text{和向量}\boldsymbol{\beta},\text{若存在一组数}\lambda_1,\lambda_2,\cdots,\lambda_m,\\&\text{使}\boldsymbol{\beta}=\lambda_1\boldsymbol{\alpha}_1+\lambda_2\boldsymbol{\alpha}_2+\cdots+\lambda_m\boldsymbol{\alpha}_m,\\&\text{则向量}\boldsymbol{\beta}\text{是该向量组的一个线性组合},\text{向量}\boldsymbol{\beta}\text{能由该向量组线性表示}\\ &\Longleftrightarrow\text{方程组}x_1\boldsymbol{\alpha}_1+x_2\boldsymbol{\alpha}_2+\cdots+x_m\boldsymbol{\alpha}_m=\boldsymbol{\beta}\text{有解},\boldsymbol{x}=(\lambda_1,\lambda_2,\cdots,\lambda_m)^\mathrm{~T}\end{aligned}$<br> |

其实向量组的线性组合的那组系数是一组变量，而向量组通过线性表示达成的效果就是，那组系数已经固定下来了。
那组系数是方程 $\displaystyle x_1\begin{pmatrix}a_{11}\\a_{21}\\\vdots\\a_{n1}\end{pmatrix}+x_2\begin{pmatrix}a_{12}\\a_{22}\\\vdots\\a_{n2}\end{pmatrix}+\cdots+x_m\begin{pmatrix}a_{1m}\\a_{2m}\\\vdots\\a_{nm}\end{pmatrix}=\begin{pmatrix}b_1\\b_2\\\vdots\\b_n\end{pmatrix}$ 的一组解


再从向量组的角度来理解，就是 $r(\boldsymbol{\alpha}_1,\boldsymbol{\alpha}_2,\cdots,\boldsymbol{\alpha}_m)=r(\boldsymbol{\alpha}_1,\boldsymbol{\alpha}_2,\cdots,\boldsymbol{\alpha}_m,\boldsymbol{\beta})$
也就是说 $\displaystyle \beta$ 不是一个独立向量，是多余的，它确实可以被向量组 $\displaystyle (\boldsymbol{\alpha}_1,\boldsymbol{\alpha}_2,\cdots,\boldsymbol{\alpha}_m)$线性表示，那么**在秩的关系中，可以被表示的向量的就可以丢掉，不影响独立向量的个数**

| 描述                                                                                                                                  | 矩阵                                                                                          | 秩的关系                             |
| ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------- |
| 一个向量组 $\displaystyle B$ 和向量组 $\displaystyle A$ 等价 (可以互相线性表示)                                                                        |                                                                                             | $\displaystyle r(A)=r(B)=r(A,B)$ |
| 一个向量组 $\displaystyle B$ 可以被向量组 $\displaystyle A$ 线性表示                                                                               | 矩阵方程 $\displaystyle AX=B$ 有解，也就是增广矩阵 $\displaystyle (A,B)$ 表示的方程有解，它的解就是线性表示的系数             | $\displaystyle r(A)=r(A,B)$      |
| 一个向量 $\displaystyle \beta$ 可以被向量组 $\displaystyle A=(\boldsymbol{\alpha}_1,\boldsymbol{\alpha}_2,\cdots,\boldsymbol{\alpha}_m)$ 线性表示 | 矩阵方程 $\displaystyle Ax=\beta$ 有解，**也就是增广矩阵 $\displaystyle (A,\beta)$ 对应的方程有解**，它的解就是线性表示的系数 | $\displaystyle r(A)=r(A,\beta)$  |
| 向量组 $\displaystyle A$ 线性相关                                                                                                          | 矩阵方程 $\displaystyle Ax=0$ 一定有非零解(无穷多组解)                                                     | $\displaystyle r(A)<m$ 系数矩阵不满秩   |
| 向量组 $\displaystyle A$ 线性无关                                                                                                          | 矩阵方程 $\displaystyle Ax=0$ 只有零解(只有一组解)                                                       | $\displaystyle r(A)=m$ 系数矩阵是满秩的  |

向量组的线性表示的相关公式

| 线性表示                                                                                       | 秩(独立向量的关系)                                                                                            | 意思              |
| ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- | --------------- |
| $\displaystyle B$ 可由 $\displaystyle A$ 线性表示                                                | $\displaystyle r(B)\leq r(A)$ 和 $\displaystyle r(A,B)=r(A)$                                           | 被表示的秩不大，被表示的可丢掉 |
| $\displaystyle B$ 可由 $\displaystyle A$ 线性表示，但是 $\displaystyle A$ 不可由 $\displaystyle B$ 表示  | $\displaystyle r(B)<r(A)$                                                                             |                 |
| 向量组等价就是可以相互表示                                                                              | 三秩相等                                                                                                  |                 |
| $\displaystyle B$ 可由 $\displaystyle A$ 线性表示，但是 $\displaystyle B$ 中向量个数大于 $\displaystyle A$ | $\displaystyle B$ 的独立向量比 $\displaystyle A$ 少，但是总向量个数比 $\displaystyle A$ 多，那么 $\displaystyle B$ 一定线性相关 | 以少表多，多必相关       |

要证明线性表示的问题，就转换成对应的方程组的解或者矩阵的秩的问题。
如果要证向量组等价，就是要证明三秩相等，那么先算那个并起来矩阵的秩
如果要证明向量组 $\displaystyle B$ 可以被 $\displaystyle A$ 表示，但是 $\displaystyle A$ 不可被 $\displaystyle B$ 表示，直接算各个秩得到 $\displaystyle R(B)<R(A)$ 就行了

可以用一个矩阵乘向量组来表示另一个向量组
这时候如果这个矩阵可逆的话，那么那两个向量组的秩相等


| 概念   | 定义                                                                                                                                                                           | 线性表示的理解                                               |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| 线性相关 | 给定向量组：$\alpha_1,\alpha_2,\cdots,\alpha_m$，若存在不全为零的数 $k_1,k_2,\cdots,k_m$，使 $k_1\boldsymbol{\alpha}_1+k_2\boldsymbol{\alpha}_2+\cdots+k_m\boldsymbol{\alpha}_m=0$，则称向量组线性相关的  | 独立向量个数小于向量个数，**肯定存在多余向量 **$\displaystyle r(A)<m$      |
| 线性无关 | 给定向量组：$\alpha_1,\alpha_2,\cdots,\alpha_m$, 当且仅当 $k_1=k_2=\cdots=k_m=0$, 才有 $k_1\boldsymbol{\alpha}_1+k_2\boldsymbol{\alpha}_2+\cdots+k_m\boldsymbol{\alpha}_m=0$, 则称向量组线性无关的 | 独立向量个数等于向量个数，**没有多余向量，全是独立向量** $\displaystyle r(A)=m$ |
两个列向量如果线性相关，那么要讨论是不是都是零向量，如果不全是零向量，那么就一定成比例，如果不成比例，就一定线性无关
多个向量要判断线性无关还是线性相关，就转换成算那矩阵的秩

一个矩阵的行列式不等于 0，那个矩阵就是可逆的，一定是满秩的
而乘一个可逆矩阵，秩并不会变化，因为可逆矩阵可以从初等行变换得来

| 线性表示可以构造方程                                                                                                                                                                                                                                                                                                                                                                                        |                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 如果 $\displaystyle \beta=k_{1}\alpha_{1}+k_{2}\alpha_{2}+\dots+k_{m}\alpha_{m}$<br>那么 $\displaystyle (\alpha_{1},\alpha_{2},\dots \alpha_{m})\left (\begin{matrix} k_{1}\\k_{2}\\ \vdots\\k_{m} \end{matrix}\right)=\beta$                                                                                                                                                                         | 所以 $\displaystyle  \left (\begin{matrix} k_{1}\\k_{2}\\ \vdots\\k_{m} \end{matrix}\right)$ 是 $\displaystyle Ax=\beta$ 的一组解<br>因为 $\displaystyle Ax=\beta\to(\alpha_{1},\alpha_{2},\dots \alpha_{m})\left (\begin{matrix} x_{1}\\x_{2}\\ \vdots\\x_{m} \end{matrix}\right)=\beta$ |
| 如果 $\displaystyle \begin{cases}b_{1}=k_{1}\alpha_{1}+k_{2}\alpha_{2}+\dots+k_{m}\alpha _{m} \\  \quad  \quad  \quad  \quad  \quad \vdots\ \\ \quad  \quad  \quad  \quad  \quad \vdots\ \end{cases}$<br>那么 $\displaystyle (\alpha_{1},\alpha_{2},\dots \alpha_{m})\left (\begin{matrix} k_{1}\dots\\k_{2}\dots\\ \vdots  \quad  \quad \\k_{m}\dots \end{matrix}\right)=(b_{1},b_{2},\dots,b_{m} )$ | 遇到抽象的题目，常常利用定义处理，也可以构造一个系数矩阵得到目标向量组和已知向量组的关系 $\displaystyle Ax=B\to(a_{1},a_{2},a_{3})\left (\begin{matrix} & &\\ & &\end{matrix}\right)= (b_{1},b_{2},b_{3})$<br>一列一列地按照 $\displaystyle b_{n}=k_{1}a_{1}+k_{2}a_{2}+k_{3}a_{3}$ 填进去                                             |
已知一个向量组线性无关，证明另一个与已知向量组有关系的向量组也是无关的也可以构造系数矩阵


| 线性相关与无关的推论             | 独立向量个数的理解                                                                                                                                                                                                                                  | 理解                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------- |
| 本身相关，增加了还是相关           | 设 $\displaystyle \alpha_{1},\alpha_{2},\dots,\alpha_{m}$ 相关, 则再加入一个新的向量 $\displaystyle \alpha_{_{m+1}}$ 还是相关                                                                                                                               | 因为本身以及有多余向量，再加几个同维向量也还是有多余向量的           |
| 整体无关，部分无关              | 在无关向量组里面任取的子向量组，还是无关的                                                                                                                                                                                                                      | 因为本身向量组都全是独立向量，无内鬼                      |
| 常通过加加减减一个向量夹出秩的大小      | 向量组 $\displaystyle A(\alpha_{1},\alpha_{2},..,\alpha_m,\alpha_{m+1})$<br>$\alpha_1,\alpha_2,\cdots,\alpha_m$ 无关，$\displaystyle R(A)\geq m$<br>$\displaystyle \alpha_{1},\alpha_{2},\dots,\alpha_{m+1}$ 相关，$\displaystyle R(A)<m+1$<br><br> |                                         |
| 整体无关再加向量还是无关，说明加的是独立向量 | 设 $\alpha_1,\alpha_2,\cdots,\alpha_m$ 无关，再加一个新的向量 $\displaystyle \beta$ 如果 $\alpha_1,\alpha_2,\cdots,\alpha_m,\beta$ 仍无关，则 $\beta$ 不可由 $\alpha_1,\alpha_2,\cdots,\alpha_m$ 线性表示                                                            | 因为加完之后向量组还是无关的，说明加的是独立向量嘛               |
| 整体无关再加向量变成相关，说明加的是多余向量 | 设 $\alpha_1,\alpha_2,\cdots,\alpha_m$ 无关，再加一个新的向量 $\displaystyle \beta$ 如果 $\alpha_1,\alpha_2,\cdots,\alpha_m,\beta$ 相关，则 $\beta$ 可由 $\alpha_1,\alpha_2,\cdots,\alpha_m$ 线性表示，且表示方式唯一                                                      | 因为加完之后向量组是相关的，说明加的是多余的向量，那多余的部分当然可以被表示了 |
| 个数大于维数，必相关             | 设 $n$ 维向量组 $\alpha_1,\alpha_2,\cdots,\alpha_m$ 若 $m>n$ 时，则该向量组必定线性相关。因为 $\displaystyle r(A)\leq n<m$ 所以 $\displaystyle r(A)<m$                                                                                                             |                                         |

证明题经常用到，如果要证明 $\displaystyle \alpha_{4}$ 不能由 $\displaystyle \alpha_{1},\dots,\alpha_{3}$ 线性表示，可以用反证法，线假设可以表示进行推论。


极大线性无关组，就是一个向量组内所有的独立向量新构成了一个组

| 概念      | 定义                                                                                      | 理解                     |                         |
| ------- | --------------------------------------------------------------------------------------- | ---------------------- | ----------------------- |
| 极大线性无关组 | 若向量组 $\alpha_1,\alpha_2,\cdots,\alpha_m$ 内存在一个部分组，且满足该部分组线性无关，原向量组中的任一向量都能由该部分组线性表示<br> | 就是一个向量组内所有的独立向量新构成了一个组 | 极大线性无关组可能不唯一，但是向量个数是一样的 |

初等行变化不会改变列向量之间的线性关系，因为初等行变化是同解变化。所以求列向量组的一个最大无关组的步骤就是
1. 先用初等行变化把列向量组化成行阶梯形矩阵，这样主元所在的列就是独立向量。
2. 再用这些独立向量去表示其他的列，得到行阶梯型矩阵里面独立向量表示多余向量的线性关系，因为初等行变换是同解变换，所以这个线性关系就是原来列向量组对应列的线性关系

方程组解的结构

非齐次方程组的通解 = 齐次方程组的通解(由基础解系线性表示) + 非齐次方程组的一个特解

齐次线性方程组的通解，就是由多个**任意常数乘独立向量**相加得到的，这些彼此无关的向量就构成了基础解系，而实际上，齐次方程组的通解在于找到那些独立向量，**通解就是那些独立向量的线性组合**嘛

对于具体的方程组，基础解系可以用方程求通解的方法去求
对于抽象的方程组，就要明白基础解系有**三个构成条件**

| 基础解系的构成条件 | 描述                             |     |
| --------- | ------------------------------ | --- |
| 是解        | 首先是 $\displaystyle Ax=0$ 的解    |     |
| 无关        | 然后它们线性无关 (没有多余向量)              |     |
| 个数        | 个数一共有 $\displaystyle n-r(A)$ 个 |     |

事实上任意 $n-r(A)$ 个线性无关的解都可以组成 $Ax=0$ 的基础解系，基础解系并不唯一

在处理秩的证明题经常使用这个基础解系的方法

如果两个矩阵相乘等于零矩阵，$\displaystyle A_{m\times n}B_{n\times l}=O$ 那么 $\displaystyle r(A)+r(B)\leq n$
证明的思路就是把 $\displaystyle B$ 拆开成列向量的形式 $\displaystyle B=(b_{1},b_{2},\dots,b_{l})$ 
然后原来的矩阵相乘等于零矩阵的式子就变成了齐次方程的式子 $\displaystyle \begin{cases}Ab_{1}=0 \\Ab_{2}=0 \\  \quad \vdots \\Ab_{l}=0\end{cases}$
在遇到两矩阵相乘为零矩阵的证明题，可以把后面的矩阵拆成列向量来凑出齐次方程
再利用 $\displaystyle A$ 的基础解系向量的个数是 $\displaystyle n-r(A)$

如果两个同是 $\displaystyle n$ 阶的齐次方程 $\displaystyle Ax=0$ 和 $\displaystyle Bx=0$ 同解，那么实际上 $\displaystyle r(A)=r(B)$
证明的思路就是，同解基础解系相同，那么基础解系的个数 $\displaystyle n-r(A)$ 和 $\displaystyle n-r(B)$ 也相同

在遇到证明秩相等的问题，可以看看对应齐次方程是不是同解的，再利用 $\displaystyle A$ 的基础解系向量的个数是 $\displaystyle n-r(A)$ 进行操作

证明两个方程组同解的思路就是，先在 $\displaystyle A$ 方程组中任意取一个解 $\displaystyle \lambda_{A}$ 然后证明 $\displaystyle \lambda_{A}$ 也是方程组 $\displaystyle B$ 的解。之后再在 $\displaystyle B$ 方程组中任意取一个解 $\displaystyle \lambda_{B}$ 然后证明 $\displaystyle \lambda_{B}$ 也是方程组 $\displaystyle A$ 的解

如果面对具体的非齐次方程组，就直接按照上面面对具体齐次方程组先求一个齐次的通解，之后再加上一个非齐次方程组的特解就行。

如果遇到抽象的非齐次方程组

| 方程组解的结构                                                                               | 核心思想就是把这下面的一坨带进去方程里面                                                               |     |
| ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | --- |
| 设 $\displaystyle \lambda_{1}$ 和 $\displaystyle \lambda_{2}$ 是 $\displaystyle Ax=0$ 的解 | 第一坨 $\displaystyle \lambda_{1}+\lambda_{2}$ 也是 $\displaystyle Ax=0$ 的解             |     |
|                                                                                       | 第二坨 $\displaystyle k_{1}\lambda_{1}+k_{2}\lambda_{2}$ 也是 $\displaystyle Ax=0$ 的解   |     |
| 设 $\displaystyle \lambda_{3}$ 和 $\displaystyle \lambda_{4}$ 是 $\displaystyle Ax=b$ 的解 | 第三坨 $\displaystyle \lambda_{3}-\lambda_{4}$ 也是 $\displaystyle Ax=0$ 的解             |     |
|                                                                                       | 第四坨 $\displaystyle \lambda_{3}+\lambda_{1}+\lambda_{2}$ 也是 $\displaystyle Ax=b$ 的解 |     |
就是给出齐次方程 $\displaystyle Ax=0$ 的基础解系和非齐次方程 $\displaystyle Ax=b$ 的特解，然后让你判断非齐次方程 $\displaystyle Ax=b$ 的通解可能的形式。
第一种思路就是先判断非齐次特解部分满不满足，再判断是不是齐次方程的基础解系

求非齐次方程组的通解的另外一个方法
首先得到系数矩阵的秩，确定基础解系向量的个数 $\displaystyle n-R(A)$
1. 非齐次方程的多个解代入方程相减可以推出齐次方程的一个解，进而搞出基础解系
2. 利用系数矩阵中列向量已知的线性表示，拆出一个解(因为解本身就是线性表示)  $\displaystyle Ax=0\to(\alpha_{1},\alpha_{2},\dots \alpha_{m})\left (\begin{matrix} x_{1}\\x_{2}\\ \vdots\\x_{m} \end{matrix}\right)=0$ 所以 $\displaystyle k_{1}\alpha_{1}+k_{2}\alpha_{2}+\dots+k_{m}\alpha_{m}=0$ 的 $\displaystyle \left (\begin{matrix} k_{1}\\k_{2}\\ \vdots\\k_{m} \end{matrix}\right)$ 就算是方程的一个解
最后非齐次方程组的通解 = 齐次方程组的通解 (由基础解系线性表示) + 非齐次方程组的一个特解


|                                                                                                                                                 |                                                                                                                                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| 设 $\displaystyle \eta^*$ 是非齐次方程 $\displaystyle Ax=b$ 的一个特解<br><br>$\displaystyle k_{1},k_{2},\dots,k_{n-r}$ 是对应的齐次方程 $\displaystyle Ax=0$ 的基础解系 | $\displaystyle \eta^{*}, k_{1},k_{2},\dots,k_{n-r}$ 线性无关<br><br>$\displaystyle \eta^{*}, \eta^*+ k_{1},\eta^*+k_{2},\dots,\eta^*+k_{n-r}$ 线性无关 |

秩相同的向量组等价，等价的向量组张成的空间相同

如果向量组是基底，那么向量个数就是独立向量个数，基底是满秩的，也就是可逆的

从基底 $\displaystyle A$ 变换到基底 $\displaystyle B$ 的过渡矩阵是满足 $\displaystyle AP=B$ 则 $\displaystyle P=A^{-1}B$ 用初等行变换求逆矩阵
如果一个向量在 $\displaystyle A$ 中的表示是 $\displaystyle X_{A}$ 在 $\displaystyle B$ 中的表示是 $\displaystyle X_{B}$ 那么满足 $\displaystyle AX_{A}=BX_{B}$
实际上 $\displaystyle X_{B}=P^{-1}X_{A}$

特别的，如果有一个基底是单位阵 $\displaystyle E$ 的话，其实这个等式就变成非齐次方程。这里如果是 $\displaystyle B=E$ 那么这个等式就变成 $\displaystyle AX_{A}=X_{B}$ 这个非齐次方程来求解了，解的结果就是在基底 $\displaystyle A$ 中的坐标

如果一个向量在两个基底的坐标都一样，其实这个等式就变成了一个齐次方程 $\displaystyle AX_{A}=BX_{A}\to (A-B)X_{A}=0$ 

遇到矩阵中有未知数，类似这样子 $\displaystyle \left (\begin{matrix} 1& 0\\ a-8&0\end{matrix}\right)$ 其实 $\displaystyle a-8$ 那一行是可以消掉的
进行 $\displaystyle r_{2}-(a-8)r_{1}$ 这样的初等行变换，可以得到 $\displaystyle \left (\begin{matrix} 1& 0\\ 0&0\end{matrix}\right)$

用向量的思路根据矩阵方程 $\displaystyle AB=C$ 解出 $\displaystyle B$ 就根据 $\displaystyle A$ 和 $\displaystyle C$ 的阶数判断出来矩阵 $\displaystyle B$ 是几行几列的形状

知道了矩阵 $\displaystyle B$ 的形状之后，就可以把矩阵方程拆开成几个非齐次方程