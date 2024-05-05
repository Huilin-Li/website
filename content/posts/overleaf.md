+++
title = "All about Latex and Overleaf"
description = ""
tags = ["overleaf",]
categories = ["Latex",]
math = true
+++ 

```
\documentclass{article}
\usepackage[utf8]{inputenc}
\usepackage{tcolorbox}
\newtcbox{\inlinecode}{on line, boxrule=0pt, boxsep=0pt, top=2pt, left=2pt, bottom=2pt, right=2pt, colback=gray!15, colframe=white, fontupper={\ttfamily \footnotesize}}
\begin{document}
This is a sample of some inline code: \inlinecode{int x = 0;}.
\end{document}
```

- [Remove the space on the left hand of the beginning of a paragraph](https://tex.stackexchange.com/questions/25983/how-and-when-to-remove-the-space-on-the-left-hand-of-the-beginning-of-a-paragrap)
```
\noindent
```
- [Typeseeting programming packages/libraries in LaTex](https://tex.stackexchange.com/questions/156383/typesetting-programming-packages-libraries-in-latex)
```
\textsc{}
```