# 中文课程论文排版实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 FLP 论文综述切换为简洁的黑白中文课程论文版式，并统一章节标题与结构性名称。

**Architecture:** 由 `paper/main.tex` 集中负责全局文档类、字体层级、页边距、页眉页脚、标题编号和前置部分；章节文件只保留正文及各自标题。实施不改写论述内容，完成后通过 XeLaTeX 编译、日志扫描和逐页图片检查验证版面。

**Tech Stack:** XeLaTeX、`ctexart`、`geometry`、`fancyhdr`、`setspace`、`latexmk`、Poppler

---

### Task 1: 配置中文课程论文全局版式

**Files:**
- Modify: `paper/main.tex:1-54`

- [ ] **Step 1: 记录当前版式基线**

Run:

```bash
cd paper
latexmk -xelatex -interaction=nonstopmode -halt-on-error main.tex
pdfinfo main.pdf | rg 'Pages|Page size'
```

Expected: 编译成功，输出包含 A4 页面尺寸和当前页数。

- [ ] **Step 2: 切换文档类并添加版式宏包**

将文档类和宏包区域改为：

```tex
\documentclass[UTF8,zihao=-4,a4paper]{ctexart}

\usepackage{amsmath,amssymb,amsthm}
\usepackage{algorithm}
\usepackage{algorithmic}
\usepackage{booktabs}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{geometry}
\usepackage{xcolor}
\usepackage{fancyhdr}
\usepackage{setspace}

\geometry{
  top=2.6cm,
  bottom=2.5cm,
  left=2.8cm,
  right=2.8cm,
  headheight=15pt
}

\setstretch{1.25}
\setlength{\parindent}{2em}
\setlength{\parskip}{0pt}
```

- [ ] **Step 3: 设置纯黑中文标题层级**

在导言区加入：

```tex
\ctexset{
  section = {
    name = {,、},
    number = \chinese{section},
    format = \heiti\zihao{-3}\bfseries,
    beforeskip = 2.2ex plus 0.5ex minus 0.2ex,
    afterskip = 1.2ex plus 0.2ex
  },
  subsection = {
    name = {（,）},
    number = \chinese{subsection},
    format = \heiti\zihao{4}\bfseries,
    beforeskip = 1.8ex plus 0.4ex minus 0.2ex,
    afterskip = 0.8ex plus 0.2ex
  }
}
```

Expected: 一级标题显示为“一、引言”，二级标题显示为“（一）预备定义”，且没有横线、色块或边框。

- [ ] **Step 4: 设置无装饰页眉页脚和黑色链接**

在导言区加入：

```tex
\hypersetup{
  colorlinks=true,
  linkcolor=black,
  citecolor=black,
  urlcolor=black
}

\pagestyle{fancy}
\fancyhf{}
\fancyhead[C]{\zihao{-5} 高级算法课程论文综述}
\fancyfoot[C]{\zihao{-5}\thepage}
\renewcommand{\headrulewidth}{0pt}
\renewcommand{\footrulewidth}{0pt}
```

Expected: 正文页有居中短页眉和页码，不出现页眉线或页脚线。

- [ ] **Step 5: 中文化定理环境和参考文献标题**

将结构名称定义改为：

```tex
\newtheorem{theorem}{定理}
\newtheorem{lemma}{引理}
\newtheorem{definition}{定义}
\renewcommand{\refname}{参考文献}
```

- [ ] **Step 6: 规范首页、摘要、关键词和目录**

将标题与前置部分改为：

```tex
\title{\heiti\zihao{2} 一个故障进程下分布式共识不可能性\\[0.8em]
  \zihao{4}\songti FLP 论文综述}
\author{姓名：\underline{\hspace{5em}}\qquad 学号：\underline{\hspace{7em}}}
\date{\today}

\begin{document}

\maketitle
\thispagestyle{plain}

\begin{abstract}
本文综述 Fisher、Lynch 和 Paterson 的论文《一个故障进程下分布式共识的不可能性》。全文依次介绍论文所研究的问题、FLP 之前的相关工作、不可能性证明过程、初始失效进程情形，以及 FLP 之后的相关研究。

\noindent\textbf{关键词：}分布式共识；异步系统；故障容错；不可能性证明
\end{abstract}

\clearpage
\tableofcontents
\clearpage
```

Expected: 首页为中文标题、中文摘要和关键词；目录单独成页，目录标题由 `ctexart` 显示为“目录”。

- [ ] **Step 7: 编译全局版式并处理语法问题**

Run:

```bash
cd paper
latexmk -xelatex -interaction=nonstopmode -halt-on-error main.tex
```

Expected: 命令退出状态为 0，并生成 `paper/main.pdf`。

### Task 2: 统一章节标题语言

**Files:**
- Modify: `paper/sections/introduction.tex:1`

- [ ] **Step 1: 查找所有章节标题中的英文残留**

Run:

```bash
rg -n '^\\(section|subsection|subsubsection)\{[^}]*[A-Za-z]' paper/sections
```

Expected: 仅报告 `paper/sections/introduction.tex` 中的 `Introduction`，以及按设计允许保留的 FLP 名称。

- [ ] **Step 2: 将引言标题改为中文**

将：

```tex
\section{Introduction}
```

改为：

```tex
\section{引言}
```

- [ ] **Step 3: 复查章节标题**

Run:

```bash
rg -n '^\\(section|subsection|subsubsection)\{' paper/sections
```

Expected: 所有通用章节名称均为中文；FLP 作为论文代称继续保留。

### Task 3: 验证编译结果和 PDF 版面

**Files:**
- Generated: `paper/main.pdf`
- Create temporary renderings under: `tmp/pdfs/chinese-layout/`

- [ ] **Step 1: 完整重新编译**

Run:

```bash
cd paper
latexmk -xelatex -interaction=nonstopmode -halt-on-error -gg main.tex
```

Expected: 完整编译成功，PDF、目录和文献引用全部更新。

- [ ] **Step 2: 检查关键警告**

Run:

```bash
rg -n "undefined|Citation .* undefined|Warning--I didn't find|LaTeX Warning|Overfull|Underfull" paper/main.log
```

Expected: 不出现未定义引用、缺失文献或影响版面的溢出警告；如有警告，先定位并修正再继续。

- [ ] **Step 3: 检查 PDF 元数据和中文结构文字**

Run:

```bash
pdfinfo paper/main.pdf | rg 'Pages|Page size'
pdftotext paper/main.pdf - | rg '摘要|关键词|目录|一、引言|参考文献'
```

Expected: 页面为 A4，文本提取结果包含全部指定的中文结构名称。

- [ ] **Step 4: 渲染全部页面**

Run:

```bash
mkdir -p tmp/pdfs/chinese-layout
pdftoppm -png -r 110 paper/main.pdf tmp/pdfs/chinese-layout/page
```

Expected: 每一页都生成对应 PNG 文件。

- [ ] **Step 5: 逐页目视检查**

检查首页、目录、章节转换页、证明图片页和参考文献页，确认：

- 中文标题编号和层级一致；
- 标题、正文、图片和页眉页脚无重叠或裁切；
- 首页不显示正文页眉；
- 页面中没有装饰横线；
- 目录与正文页码对应；
- 参考文献标题为中文。

- [ ] **Step 6: 记录 AI 使用日志**

在 `process/ai-usage/conversation-log.md` 追加本次实施内容、修改文件、编译命令、警告检查和人工版面检查结果。

- [ ] **Step 7: 提交排版修改**

Run:

```bash
git add paper/main.tex paper/sections/introduction.tex process/ai-usage/conversation-log.md
git commit -m "统一中文论文排版"
```

Expected: 仅提交排版源文件、中文标题修改和 AI 使用日志，不包含用户在 `notes/` 中的修改或 `.superpowers/` 预览文件。
