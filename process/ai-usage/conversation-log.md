# AI Conversation Log

本文件用于持续记录我与 AI 助手围绕本课程论文 review 作业的所有对话。记录内容包括：我问了什么、AI 回答了什么、AI 修改或创建了哪些文件、哪些内容需要我自己验证。

说明：

- 本日志不代表最终论文内容。
- AI 生成的解释、结构和建议都需要我自己阅读原论文、核对来源并改写。
- 之后如果继续使用 AI，本文件应继续追加新条目。

## 2026-07-08 / 2026-07-09: 创建项目结构

### User Prompt

我提供了课程作业要求，核心要求包括：

- 至多四人一组，从给定论文中选择一篇写 review，中英文不限。
- 需要尽可能详细解释论文解决了什么问题、问题为什么重要、相关研究、提出的技术、技术直觉、厉害之处、实现方式、正确性和优越性证明。
- 过程分占 70%，必须使用 Notion / Overleaf / Google Docs / Word Online / GitHub 等有版本历史的在线工具。
- 教师和助教应能看到资料收集、理解、观点形成、草稿撰写和反复修改过程。
- AI 可以用于辅助理解或查询，但不接受 AI 代写；AI 使用记录必须放入版本历史中。
- 我说明准备在当前文件夹中写作，并且项目已经连接 GitHub 仓库，请 AI 帮我组织项目结构，方便后续写作。

### AI Response / Action

AI 先检查了当前仓库，发现主要只有 `README.md` 和 Git 仓库文件。AI 建议使用 GitHub 作为过程记录工具，将 Markdown 用于阅读笔记、AI 使用记录、相关研究和过程材料。

AI 起初建议确认写作方式，包括：

- GitHub Markdown
- Overleaf + GitHub
- Word/Google Docs + GitHub

当时尚未直接创建完整结构，而是询问我希望采用哪种主写作方式。

### Files Changed

无实质文件修改。

### Verification / Follow-up

我随后明确选择了 Markdown + LaTeX 的方式。

## 2026-07-09: 确定 Markdown + LaTeX 双轨结构

### User Prompt

我说：

> 我想把除论文外的内容用markdown写，然后论文用latex写

### AI Response / Action

AI 根据我的选择创建了项目结构：

- `notes/`: 阅读笔记、相关研究、证明理解、小例子验证
- `process/`: 时间线、任务板、会议记录、AI 使用记录、贡献记录
- `paper/`: LaTeX 最终论文
- `refs/`: 参考资料索引
- `docs/`: 提交检查清单

AI 创建了 LaTeX 论文骨架，并验证 `paper/main.tex` 可以用 `latexmk -xelatex` 编译。AI 也添加了 `.gitignore`，忽略 LaTeX 构建产物。

AI 在最终回复中建议执行：

```bash
git add .
git commit -m "docs: initialize paper review project structure"
git push
```

### Files Created / Modified

- `README.md`
- `.gitignore`
- `docs/submission-checklist.md`
- `notes/paper-selection.md`
- `notes/review-outline.md`
- `notes/reading/README.md`
- `notes/related-work/related-work-matrix.md`
- `notes/proofs/proof-notes.md`
- `notes/experiments/verification-examples.md`
- `process/timeline.md`
- `process/task-board.md`
- `process/contribution-log.md`
- `process/meetings/README.md`
- `process/ai-usage/README.md`
- `refs/README.md`
- `paper/main.tex`
- `paper/references.bib`
- `paper/sections/introduction.tex`
- `paper/sections/problem.tex`
- `paper/sections/related-work.tex`
- `paper/sections/technique.tex`
- `paper/sections/implementation.tex`
- `paper/sections/proof.tex`
- `paper/sections/comparison.tex`
- `paper/sections/limitations.tex`
- `paper/sections/conclusion.tex`

### Verification / Follow-up

AI 编译验证了 LaTeX 模板，提示模板可正常编译。后续我指出项目只有我一个成员，因此多人分工结构需要删除。

## 2026-07-09: 理解 FLP 论文解决的问题

### User Prompt

我问：

> 首先先解决第一个问题 这篇论文解决了什么问题

### AI Response / Action

AI 检查项目，发现 `docs/flp.pdf`，判断当前论文是 FLP 经典论文 *Impossibility of Distributed Consensus with One Faulty Process*。

AI 使用 PDF 文本提取查看了论文标题、摘要、引言、共识协议定义和主定理。AI 总结该论文解决的问题是：

> 在一个完全异步、消息可靠、最多只有一个进程会崩溃停止的分布式系统中，是否存在一个确定性共识协议，能够保证所有允许的执行都最终达成一致？

AI 的回答要点：

- 论文研究分布式系统中的 consensus problem。
- 每个进程初始有二进制输入 `0` 或 `1`。
- 协议希望所有非故障进程最终决定同一个值。
- 系统是完全异步的，没有统一时钟，没有消息延迟上界，不能区分进程崩溃和进程运行很慢。
- 消息系统可靠，不丢失、不损坏、不重复。
- 故障不是 Byzantine failure，而是最多一个 crash failure。
- FLP 的结论是：不存在这样的确定性协议能保证所有合法执行都终止。
- 一句话理解：FLP 解决的不是“怎么做共识”，而是证明在完全异步系统中，只要允许一个进程崩溃，确定性共识就不可能保证总是终止。

AI 同时强调：这不是最终论文正文，而是阅读理解笔记，后续需要我用自己的话重写并核对论文原文。

### Files Created / Modified

- `notes/reading/problem-understanding.md`
- `process/ai-usage/2026-07-09-flp-problem-understanding.md`

### Verification / Follow-up

AI 在日志中标注需要进一步核对：

- partial correctness
- total correctness
- admissible run
- bivalence
- 主定理的形式化表述

## 2026-07-09: 改成单人项目结构

### User Prompt

我说：

> 请你稍微更改一下项目结构 ，整个项目只有我一个成员 ，所以关于分工等内容 可以删去

### AI Response / Action

AI 扫描项目中的多人痕迹，包括：

- 成员
- 小组
- Member
- Participants
- contribution
- Owner
- 分工

AI 将项目从多人小组结构改为个人项目结构。

主要修改：

- 删除多人贡献文件 `process/contribution-log.md`
- 删除小组会议模板 `process/meetings/README.md`
- 新增个人工作记录目录 `process/work-sessions/README.md`
- 将 `process/task-board.md` 改为个人任务板，删除 `Owner` 列
- 将 `paper/main.tex` 作者改为 `TODO: Your Name`
- 更新 README，使其描述个人写作流程
- 更新提交检查清单，将“每位成员贡献”改为“个人持续阅读、理解和修改过程”
- 修改 AI 使用日志中“User/member”为“User”

AI 最后重新编译 LaTeX，确认模板仍可用。

### Files Created / Modified / Deleted

Created:

- `process/work-sessions/README.md`

Modified:

- `README.md`
- `docs/submission-checklist.md`
- `notes/paper-selection.md`
- `notes/reading/README.md`
- `notes/review-outline.md`
- `paper/main.tex`
- `process/ai-usage/2026-07-09-flp-problem-understanding.md`
- `process/task-board.md`

Deleted:

- `process/contribution-log.md`
- `process/meetings/README.md`

### Verification / Follow-up

AI 扫描后确认没有残留多人相关字段，并验证 LaTeX 模板可编译。

## 2026-07-09: 要求记录所有 AI 对话

### User Prompt

我说：

> 请你把我之前包括以后 和你对话的所有内容 包括我问你什么 你回答了什么 都做成日志 放到项目中的ai使用日志里

### AI Response / Action

AI 决定创建一个持续维护的对话日志，专门记录与 AI 的所有作业相关对话，包括：

- 用户问题
- AI 回答或操作
- AI 创建、修改或删除的文件
- 需要人工验证的内容

AI 同时更新 `process/ai-usage/README.md`，说明 `conversation-log.md` 是持续日志，后续对话应继续追加。

### Files Created / Modified

- `process/ai-usage/conversation-log.md`
- `process/ai-usage/README.md`

### Verification / Follow-up

后续继续使用 AI 时，应把新的问答追加到本文件。AI 记录不能替代我自己的理解、核对和改写。

## 2026-07-09: 进一步解释 FLP 解决的问题并要求举例

### User Prompt

我引用了前面对 FLP 问题的概括：

> 完全异步分布式系统中的容错共识问题：多个进程各自有一个初始二进制输入值 0/1，希望所有没有故障的进程最终对同一个值达成一致；但系统中最多允许有一个进程发生 crash failure，也就是静默停止运行。

然后要求 AI：

> 请你再给我详细的讲一讲 比如举个例子啥的

### AI Response / Action

AI 将用具体例子解释 FLP 中的共识问题，重点说明：

- 为什么每个进程有初始值；
- “达成一致”意味着什么；
- crash failure 为什么看似温和但仍然困难；
- 完全异步为什么使慢进程和死进程不可区分；
- FLP 证明的不可能性到底是不保证终止，而不是一定产生错误决定。

### Files Created / Modified

- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

正式写入 review 前，需要回到 `docs/flp.pdf` 的 Abstract、Introduction 和 Section 2 核对共识定义与系统模型。

## 2026-07-09: 追问一个 crash、多个 crash、零个 crash 的区别

### User Prompt

我问：

> 请问为什么有“只要允许一个进程crash”，一个和多个和零个有什么区别

### AI Response / Action

AI 将解释 FLP 中 “one faulty process” 的边界意义：

- 零个 crash 时，异步共识可以通过等待所有进程消息来完成；
- 允许一个 crash 后，协议无法区分“某进程死了”和“某进程很慢”，因此不能同时保证安全性和所有执行中的终止；
- 允许多个 crash 不会让问题更容易，一个 crash 已经足够导致不可能性；
- FLP 的强度在于证明最弱的非零故障容忍目标已经不可能。

### Files Created / Modified

- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

正式写入 review 前，需要结合 FLP 论文 Section 1 对 “even with only one faulty process” 的表述，以及 Section 2 中 admissible run 和 faulty process 的定义进行核对。

## 2026-07-09: 追问协议定义和一个会因 crash 失败的协议例子

### User Prompt

我问：

> 请问在这个问题中协议是什么，请给我举一个协议的例子，以及协议可以在零个进程crash下成功 但是在一个crash下失败的例子

### AI Response / Action

AI 将解释在 FLP/共识问题中，协议指每个进程遵循的一套确定性本地规则，包括：

- 初始状态如何设置；
- 收到消息后如何更新本地状态；
- 发送什么消息；
- 何时决定 `0` 或 `1`。

AI 将用一个简单的 “broadcast-all-and-wait-all” 协议举例：所有进程广播自己的输入，等待收到所有进程的输入后按固定规则决定。该协议在 0 个 crash 时可以成功，但如果一个进程在发送消息前 crash，其他进程会永远等待它的消息，因此无法终止。

### Files Created / Modified

- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

正式写入 review 前，需要核对 FLP Section 2 中协议由进程自动机、输入/输出寄存器、消息缓冲区和 transition function 定义的形式化模型。

## 2026-07-09: 澄清零 crash 与一个 crash 下确定性协议的含义

### User Prompt

我尝试总结：

> 所以意味着 当只有零个crash的时候 就会有确定性的协议 让所有的进程同步（对于整个系统只会产生一个确定的执行结果），但是如果有一个crash的时候 不存在确定性的协议让所有的进程一定同步（也就是可能产生不同的执行结果）

### AI Response / Action

AI 将澄清这个总结中接近正确和需要修正的部分：

- 零 crash 时确实存在确定性协议保证共识最终完成；
- 但异步系统即使协议确定，执行顺序仍由调度和消息延迟决定，因此不是“整个系统只有一个确定执行轨迹”；
- 一个 crash 时，FLP 不是说正确协议会产生不同决定值，而是说不存在确定性协议能同时保证 agreement、validity/nontriviality 和 termination；
- 对于保持一致性的协议，失败通常表现为某些合法执行中永远不决定。

### Files Created / Modified

- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

正式写入 review 时，应避免把 FLP 表述成“会产生不同结果”，而应表述成“无法保证所有 admissible runs 都是 deciding runs”。

## 2026-07-09: 解释不决定、一致性、非平凡性、必然终止

### User Prompt

我问：

> 会永远不决定，一致性，非平凡性，必然终止分别是什么意思

### AI Response / Action

AI 将解释共识问题中的四个概念：

- 永远不决定：进程持续运行和收消息，但没有进入决定状态；
- 一致性：不能有两个非故障进程决定不同值；
- 非平凡性：协议不能永远固定输出同一个值，`0` 和 `1` 都应在某些初始配置下可能被决定；
- 必然终止：在满足故障假设的每个合法执行中，最终至少有进程做出决定，通常更强地要求所有非故障进程最终决定。

### Files Created / Modified

- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

正式写入 review 前，应将这些术语与 FLP Section 1 和 Section 2 中的 partial correctness、decision value、deciding run、totally correct in spite of one fault 对应起来。

## 2026-07-09: 总结“论文解决了什么问题”并写入项目

### User Prompt

我说：

> 好了 我觉得我知道这篇论文解决了什么问题了 ，请你总结以上我与你对话的内容，把整个项目中有关解决了什么问题的部分写进去，注意对于一些名词要给出解释，就像我刚刚问你非平凡性是什么等解释那样

### AI Response / Action

AI 将此前关于 FLP 论文问题定义的对话整理为项目材料：

- 在 Markdown 阅读笔记中扩写“这篇论文解决了什么问题”；
- 在 LaTeX 正文章节 `Problem and Motivation` 中写入正文草稿；
- 对协议、完全异步、crash failure、一致性、非平凡性、终止、永远不决定等术语给出解释；
- 使用三进程分布式提交例子说明：零 crash 时朴素协议可以成功，一个 crash 时等待所有人的协议可能永远不终止；
- 强调 FLP 证明的失败点主要是不能保证终止，而不是允许正确协议决定出互相矛盾的值。

### Files Created / Modified

- `notes/reading/problem-understanding.md`
- `paper/sections/problem.tex`
- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

后续需要继续核对 FLP 原文中的形式化定义，并在最终正文中加入引用。
