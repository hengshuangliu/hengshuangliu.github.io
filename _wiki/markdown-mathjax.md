---
layout: wiki
title: Markdown-mathjax
categories: Markdown
description: Markdown 公式编辑示例
keywords: Markdown
mermaid: true
sequence: true
flow: true
mathjax: true
---

**目录**
* TOC
{:toc}

### 插入公式

When $(a \ne 0)$, there are two solutions to $(ax^2 + bx + c = 0)$ and they are
$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}$$

### 上下标
\^表示上标，\_表示下标
```
$$ x^{y^z}=(1+{\rm e}^x)^{-2xy^w} $$
```
$$ x^{y^z}=(1+{\rm e}^x)^{-2xy^w} $$

### 括号、分隔符和分数
`()`和`[]`和`|`直接表示符号本身, 使用 `\{\}` 来表示 `{}`, 当要显示大号的括号或分隔符时，要用 `\left` 和 `\right` 命令, 通常使用 `\frac {分子} {分母}`表示分数.
```
$$ f(x,y,z) = 3y^2z \left( 3+\frac{7x+5}{1+y^2} \right) $$
```
$$ f(x,y,z) = 3y^2z \left( 3+\frac{7x+5}{1+y^2} \right) $$

### 开方
使用 `\sqrt [根指数，省略时为2] {被开方数}`命令输入开方.
```
$$\sqrt{2} \quad and \quad \sqrt[n]{3}$$
```
$$\sqrt{2} \quad and \quad \sqrt[n]{3}$$

### 省略号
数学公式中常见的省略号有两种，`\ldots` 表示与文本底线对齐的省略号，`\cdots `表示与文本中线对齐的省略号.
```
$$f(x_1,x_2,\underbrace{\ldots}_{\rm ldots} ,x_n) = x_1^2 + x_2^2 + \underbrace{\cdots}_{\rm cdots} + x_n^2$$
```
$$f(x_1,x_2,\underbrace{\ldots}_{\rm ldots} ,x_n) = x_1^2 + x_2^2 + \underbrace{\cdots}_{\rm cdots} + x_n^2$$

### 矢量
使用 `\vec{矢量}`来自动产生一个矢量.
```
$$\vec{a} \cdot \vec{b}=0$$
```
$$\vec{a} \cdot \vec{b}=0$$

### 积分
使用`\int_` 积分下限`^`积分上限`{被积表达式}`  来输入一个积分.
```
$$\int_0^1 {x^2} \,{\rm d}x$$
```
$$\int_0^1 {x^2} \,{\rm d}x$$

### 极限运算
使用`\lim_{变量 \to 表达式} 表达式` 来输入一个极限.
```
$$ \lim_{n \to +\infty} \frac{1}{n(n+1)} \quad and \quad \lim_{x\leftarrow{示例}} \frac{1}{n(n+1)} $$
```
$$ \lim_{n \to +\infty} \frac{1}{n(n+1)} \quad and \quad \lim_{x\leftarrow{示例}} \frac{1}{n(n+1)} $$

### 希腊字母

| 显示    | Markdown |  显示    | Markdown |
| -----: | ------  | ------: | -------  |
| $\alpha$  | \$\alpha$ | $\beta$ | \$\beta$ |
| $\gamma$ | \$\gamma$ | $\delta$  | \$\delta$  |
| $\epsilon$ | \$\epsilon$ | $\zeta$ |\$\zeta$ |
|$\eta$ | \$\eta$ | $\theta$  |  \$\theta$ |
| $\iota$ | \$\iota$ | $\kappa$  | \$\kappa$ |
|$\lambda$| \$\lambda$ |$\nu$  |  \$\nu$  |
|$\mu$ | \$\mu$ | $\xi$ |\$\xi$ |
|$o$| \$o$   | $\pi$  |  \$\pi$|
|$\rho$| \$\rho$  | $\sigma$  |  \$\sigma$   |
|$\tau$| \$\tau$  | $\upsilon$ | \$\upsilon$ |
|$\phi$| \$\phi$ | $\chi$  |  \$\chi$|
|$\psi$|  \$\psi$ | $\omega$ | \$\omega$  |
|$A$| \$A$ |  $B$  |  \$B$ |
|$\Gamma$| \$\Gamma$ | $\Delta$ |\$\Delta$ |
|$E$ | \$E$ |$Z$  |  \$Z$ |
|$H$|  \$H$| $\Theta$  | \$\Theta$ |
|$I$| \$I$  | $K$  |  \$K$ |
|$\Lambda$| \$\Lambda$ |$N$  | \$N$ |
|$M$| \$M$ | $\Xi$  |  \$\Xi$ |
|$O$|  \$O$ |  $\Pi$ |  \$\Pi$ |
|$P$|  \$P$  |  $\Sigma$  | \$\Sigma$ |
|$T$|  \$T$  | $\Upsilon$  |  \$\Upsilon$ |
|$\Phi$|  \$\Phi$ | $X$  | \$X$ |
|$\Psi$|  \$\Psi$ | $\Omega$| \$\Omega$ |


### 大括号和行标
使用 `\left`和 `\right`来创建自动匹配高度的 (圆括号)，[方括号] 和 {花括号}, 在每个公式末尾前使用`\tag{行标}`来实现行标.
```
$$ f\left( \left[ \frac{ 1+\left\{x,y\right\} }{ \left( \frac{x}{y}+\frac{y}{x} \right) \left(u+1\right) }+a \right]^{3/2} \right) \tag{行标} $$
```
$$ f\left( \left[ \frac{ 1+\left\{x,y\right\} }{ \left( \frac{x}{y}+\frac{y}{x} \right) \left(u+1\right) }+a \right]^{3/2} \right) \tag{行标} $$

### 偏导

```
$$\frac{\partial^{2}y}{\partial x^{2}}$$
```
$$\frac{\partial^{2}y}{\partial x^{2}}$$

### 常见运算符

| 运算符    | Markdown |  运算符   | Markdown |
| ------: | -------- | --------: | ------- |
| $\pm$    | \$\pm$   | $\times$  | \$\times$|
| $\div$   | \$\div$  | $\mid$    | \$\mid$  |
| $\leq$  | \$\leq$  | $\geq$    | \$\geq$  |
| $\neq$   | \$\neq$  | $\approx$ |\$\approx$|
| $\sum$   | \$\sum$  | $\prod$   |\$\prod$ |
| $\in$  | \$\in$  | $\emptyset$ |\$\emptyset$|
| $\notin$  | \$\notin$ | $\subset$ | \$\subset$ |
| $\supset$ |\$\supset$ | $\subseteq$|\$\subseteq$|
| $\supseteq$|\$\supseteq$ | $\log$ |\$\log$ |
|$\lg$     | \$\lg$   |  $\ln$    | \$\ln$   |
| $\hat{y}$ | \$\hat{y}$ |$\check{y}$|\$\check{y}$|
| $\cos$ |  \$\cos$ | $\tan$ |  \$\tan$ |
| $\int$  | \$\int$ | $\iint$  |  \$\iint$  |
|$\iiint$  | \$\iiint$ | $\oint$ | \$\oint$ |
| $\lim$ |  \$\lim$  |  $\infty$  | \$\infty$  |
| $\nabla$  |  \$\nabla$  | $\not=$ |  \$\not=$ |

[More](http://www.cnblogs.com/q735613050/p/7253073.html)