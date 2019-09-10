---
title: latex排版工具生成pdf
date: 2019-08-26 14:08:40
tags:
 - tex
 - latex
categories:
 - latex
---

使用latex排版软件，方便的生成格式化的文档，比word的各种编辑方便很多

```
sudo pacman -S texlive-most texlive-lang
```

tex的安装很大，``texlive-most``大约600m, ``texlive-lang``大约300m

新建文本文件``test.tex``, '%' is comment
```
\documentclass[UTF8]{article}
% 这里是导言区
\begin{document}
Hello, world!
\end{document}
```
```
latex test.tex     # generate test.dvi
xdvi test.dvi      # open window to overview
pdflatex test.tex  # generate test.pdf
```
![latex预览](test.png)
``test2.tex``, 试试中文支持
```
\documentclass{article}

\usepackage{fontspec}
\setmainfont[Ligatures=TeX]{WenQuanYi Micro Hei Mono}

\title{你好，world!}
\author{Liam}
\date{\today}
\begin{document}
\maketitle
\section{你好中国}
中国在 East Asia.
\subsection{Hello Beijing}
北京是 capital of China.
\subsubsection{Hello Dongcheng District}
\paragraph{Tian'anmen Square}
is in the center of Beijing
\subparagraph{Chairman Mao}
is in the center of 天安门广场。
\subsection{Hello 北京}
\paragraph{北京} is an international city。
\end{document}
```
```
xelatex test2.tex # test2.pdf
```
![latex预览](test2.png)
注意使用中文字体，``fc-list``

如果在``pdflatex``或``xelatex``过程出现错误，在``?``后面输入``x``然后回车退出