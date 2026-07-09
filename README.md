# Advanced Algorithm Paper Review

本仓库用于完成高级算法课程论文 review 作业。

写作方式：

- 除最终论文外，所有过程材料使用 Markdown 记录。
- 最终论文使用 LaTeX，位于 `paper/`。
- GitHub commit history 用于保留资料收集、阅读理解、观点形成、草稿撰写、反复修改和 AI 使用记录。

## 目录结构

```text
.
├── README.md
├── paper/                  # 最终 LaTeX 论文
│   ├── main.tex
│   ├── references.bib
│   ├── sections/
│   ├── figures/
│   └── tables/
├── notes/                  # 阅读、理解、证明和相关研究笔记
│   ├── paper-selection.md
│   ├── review-outline.md
│   ├── reading/
│   ├── related-work/
│   ├── proofs/
│   └── experiments/
├── process/                # 过程证据链
│   ├── timeline.md
│   ├── task-board.md
│   ├── work-sessions/
│   └── ai-usage/
├── refs/                   # 参考资料索引，不建议提交大体积 PDF
├── assets/                 # 截图、手绘图、推导草稿扫描等
└── docs/                   # 提交说明、检查清单等
```

## 推荐工作流

1. 在 `notes/paper-selection.md` 记录候选论文比较和最终选择理由。
2. 在 `notes/reading/` 下持续记录自己的阅读笔记和疑问。
3. 每次集中阅读、推导或修改后，在 `process/work-sessions/` 新建工作记录。
4. 每次使用 AI 后，在 `process/ai-usage/` 记录问题、回答摘要、验证方式和采纳情况。
5. 先在 `notes/review-outline.md` 逐步形成观点，再迁移到 `paper/sections/`。
6. 每完成一个小阶段就提交一次 commit，commit message 写清楚本次推进内容。

## Commit 建议

推荐小步提交，避免截止日前一次性上传完整论文。

示例：

```text
docs: record paper selection criteria
notes: add initial reading notes for section 2
notes: compare related work on randomized rounding
paper: draft problem background section
paper: revise correctness proof explanation
process: document AI-assisted explanation verification
```

## 学术诚信记录要求

本仓库应保留：

- 论文选择过程
- 阅读笔记和疑问
- 相关研究收集过程
- 大纲形成过程
- 草稿和修改过程
- 个人持续推进的痕迹
- AI 使用记录及人工验证过程

AI 可以用于帮助理解、查资料和解释概念，但最终内容必须由你自己理解、验证并独立表述。
