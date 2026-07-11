# AGENTS.md

本文件是给所有 AI 助手/代理的项目协作规则。进入本仓库后，请先阅读本文件，再继续处理用户请求。

## AI 使用日志是必做事项

本项目是高级算法课程论文 review 项目，过程证据非常重要。所有与 AI 助手围绕本项目发生的对话，都必须记录到：

```text
process/ai-usage/conversation-log.md
```

记录范围包括：

- 用户输入了什么问题、请求或修正意见；
- AI 如何回答、解释、建议或采取行动；
- AI 创建、修改、删除或检查了哪些文件；
- 哪些内容需要用户自己继续核对、验证或改写；
- 如果 AI 没有修改文件，也要说明没有实质文件修改。

## 日志格式

追加日志时，参考当前 `process/ai-usage/conversation-log.md` 中已有条目的格式。推荐格式如下：

```md
## YYYY-MM-DD: 简短标题

### User Prompt

记录用户本次输入。较短的输入可以直接引用原话；较长的输入可以先摘要，再保留关键原文。

### AI Response / Action

记录 AI 本次回答了什么、解释了什么、决定做什么，以及实际采取了哪些操作。

### Files Created / Modified / Deleted

- `path/to/file`

如果没有文件变化，写“无实质文件修改”。

### Verification / Follow-up

记录已经做过的验证，以及后续仍需人工核对、补充或改写的内容。
```

## 写作与学术诚信提醒

- AI 生成的解释、结构、草稿和建议都只是辅助材料。
- 正式论文内容必须由用户自己理解、核对原论文和相关资料后独立表述。
- 如果 AI 根据论文、PDF、笔记或外部资料给出解释，要在日志里写清楚使用了哪些来源，以及哪些地方还需要用户验证。
- 不要把 AI 日志当作最终论文正文；日志只用于保留过程证据。

## 项目内容约定

- 过程材料主要写在 `notes/` 和 `process/`。
- 最终论文写在 `paper/`，主文件是 `paper/main.tex`。
- 源论文和提交检查材料在 `docs/`。
- 当前主要 review 对象是 FLP 论文：*Impossibility of Distributed Consensus with One Faulty Process*。
