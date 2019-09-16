---
title: latex常用的数学公式生成
date: 2019-09-10 10:33:02
tags:
 - 数学公式
categories:
 - latex
---

下面是数学符合列表，很多，看看常用的就行
[List of LaTeX mathematical symbols](https://oeis.org/wiki/List_of_LaTeX_mathematical_symbols)

[LaTeX/Mathematics--wiki](https://en.wikibooks.org/wiki/LaTeX/Mathematics)

[数学公式--wiki](https://zh.wikipedia.org/wiki/Help:%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F)

在线网页查看tex公式生成

[Online LaTeX formula editor--hostmath](http://hostmath.com/)

[Online LaTeX formula editor--codecogs](https://www.codecogs.com/latex/eqneditor.php?lang=en-us)

``http://latex.codecogs.com/png.latex?`` 后面加下面的tex代码【直接】生成的公式图片

有的符号需要使用urlencode转义

空格 : %20


``\frac{1}{2}`` : 是分数``fraction``的缩写 ![数学tex](http://latex.codecogs.com/png.latex?\frac{1}{2})

``\sqrt[n]{3}`` ![数学tex](http://latex.codecogs.com/png.latex?\sqrt[n]{3})

``\lim_{t\to n}T`` ![数学tex](http://latex.codecogs.com/png.latex?\lim_{t\to%20n}T)

``\begin{bmatrix}1&2&3&\\4&5&6&\\9&8&7&\end{bmatrix}`` ![数学tex](http://latex.codecogs.com/png.latex?%5cbegin%7bbmatrix%7d1%262%263%26%5c%5c4%265%266%26%5c%5c9%268%267%26%5cend%7bbmatrix%7d)

``bmatrix`` 就是 ``bracket`` 方括号
``pmatrix`` 就是 ``parenthesis`` 圆括号

``\partial y`` 偏微分

``\prod_{t\to n}`` 连乘积

``\sum_{x=0}^{n}``  连加和

``\sim`` 等价号

``\approx`` 约等于

``\equiv``  三线恒等于

``\iint_{-N}^{N}\int_{-N}^{N}\frac{x}{R^3}dx=\frac{2}{(2n-1)(4ac-b^2)}[\frac{2ax+b}{r^{2n-1}}+4a(n-1)\int\frac{\left| \frac{a}{b} \right|}{6\sqrt[3]{x}R}]`` 只是演示，不是公式 ![数学tex](http://latex.codecogs.com/png.latex?\iint_{-N}^{N}\int_{-N}^{N}\frac{x}{R^3}dx=\frac{2}{(2n-1)(4ac-b^2)}[\frac{2ax+b}{r^{2n-1}}+4a(n-1)\int\frac{\left|%20\frac{a}{b}%20\right|}{6\sqrt[3]{x}R}])

``\iint`` 二重积分, ``_{-N}^{N}``积分上下限, ``\frac{x}{R^3}``分数，分子分母, ``^2``指数上标, ``^{2n-1}``指数含有多个, ``\left| ``,`` \right|``左右绝对值符号

基本都可以从上面这个复杂的公式得到相应公式写法，其他用到了再查看文档