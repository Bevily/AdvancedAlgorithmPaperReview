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

## 2026-07-10: 询问新 vLLM 实验项目结构中各部分职责

### User Prompt

我准备在一台 Spark 机器上重新写一个基于 vLLM 的实验项目，主要实验包括：

- 三档部署，以及 FP4 / FP8 / BF16 分开部署时的开销对比；
- 混合精度请求到来时如何调度。

AI 先给出了一个新的项目结构。随后我追问：

> 请你详细给我讲讲 整个项目结构中 每一部分分别是做什么的

### AI Response / Action

AI 将逐层解释新项目结构中各目录和文件的职责，包括：

- 项目根目录文件；
- `docs/` 文档；
- `configs/` 实验配置；
- `src/mp_vllm/` 核心代码；
- `experiments/` 具体实验入口；
- `scripts/` 环境和运维脚本；
- `benchmarks/` 压测入口；
- `workloads/` 请求数据；
- `results/` 实验输出；
- `notebooks/` 分析笔记本；
- `tests/` 单元测试。

### Files Created / Modified

- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

这是新实验项目的结构设计说明，尚未在文件系统中创建该项目。

## 2026-07-11: 为项目新增 AGENTS.md 记录 AI 日志规则

### User Prompt

用户要求：

> 请你为这个项目写一份AGENTS.md，主要内容就是 我所有和你对话的内容，包括我的输入和你的输入 都记录进ai使用日志 ，使用格式参考当前的ai日志中的格式

### AI Response / Action

AI 检查了项目根目录，确认当前不存在 `AGENTS.md`。随后参考 `process/ai-usage/conversation-log.md` 中已有日志格式，为项目新增 `AGENTS.md`，要求所有后续 AI 助手/代理进入本仓库后，都必须把与本项目相关的用户输入、AI 回答或操作、文件变化、验证和后续事项追加记录到 AI 使用日志中。

AI 同时将本次创建 `AGENTS.md` 的对话追加到当前 AI 使用日志，立即按照新规则执行。

### Files Created / Modified

- `AGENTS.md`
- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

后续所有与本项目相关的 AI 对话都应继续追加到 `process/ai-usage/conversation-log.md`，格式参考已有条目。

## 2026-07-11: 提交并推送新增 AGENTS.md

### User Prompt

用户要求：

> 好的 请将当前更改推送到github，项目描述为“新增AGENTS.md”

### AI Response / Action

AI 按照 `AGENTS.md` 中的要求，先将本次用户请求和后续操作追加到 AI 使用日志。随后检查 git 状态和远端配置，确认当前分支为 `main`，远端为 `origin`，待提交内容包括新增的 `AGENTS.md` 和更新后的 `process/ai-usage/conversation-log.md`。

AI 将使用提交信息“新增AGENTS.md”提交当前更改，并推送到 GitHub 远端仓库。

### Files Created / Modified

- `AGENTS.md`
- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

提交和推送完成后，需要通过 git 状态确认工作区干净，并确认本地分支已推送到远端。

## 2026-07-11: 咨询下一步工作安排

### User Prompt

用户问：

> 好的 我接下来应该做什么呢

### AI Response / Action

AI 根据当前项目状态建议下一步应优先从 FLP 论文的主证明入手，而不是继续扩写正文。原因是项目已经完成“论文解决了什么问题”的初步理解和 LaTeX 问题章节草稿，但证明笔记、相关工作、大纲和任务板仍然空缺或未同步。

AI 建议的近期路线是：先读 Section 2 的形式化模型和 Section 3 的主证明，整理 bivalent / univalent、Lemma 2、Lemma 3、admissible nondeciding run 等关键概念；再把理解写入 `notes/proofs/proof-notes.md` 和 `notes/review-outline.md`；最后再迁移到 LaTeX 正文。

### Files Created / Modified

- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

后续可以继续让 AI 陪读 FLP Section 2 和 Section 3，但正式论文内容仍需用户核对原文并用自己的话改写。

## 2026-07-11: 解释 FLP 研究的问题为什么重要

### User Prompt

用户说：

> 我还需要解决一个问题 就是 这个论文研究的问题为什么重要

### AI Response / Action

AI 根据 `AGENTS.md` 的要求先阅读项目规则，并从 `docs/flp.pdf` 的 Introduction 和 Conclusion 中提取与问题重要性相关的内容。AI 总结认为，FLP 研究的问题重要，主要因为共识是分布式数据处理、分布式文件管理、容错应用和分布式数据库事务提交的基础；如果进程对 commit / abort 等决定不一致，系统状态会分裂。

AI 进一步解释，FLP 的贡献不是指出某个具体协议失败，而是证明在完全异步、可靠消息、最多一个 crash failure 的模型下，确定性共识协议无法保证所有合法执行都终止。这一结论迫使后续系统和理论工作显式引入额外假设，例如同步或部分同步、故障检测、随机化协议或较弱的终止要求。

AI 将这部分理解整理为新的阅读笔记，并把相应内容补入 LaTeX 的 `Problem and Motivation` 章节。

### Files Created / Modified

- `notes/reading/problem-importance.md`
- `paper/sections/problem.tex`
- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

本次解释依据 `docs/flp.pdf` 的 Introduction 和 Conclusion。正式论文中仍需要用户核对原文表述，并用自己的语言改写或调整引用。

## 2026-07-11: 重组 Problem and Motivation 章节措辞

### User Prompt

用户要求：

> 请你重新组织一下problem and motivation章节的措辞 使语言更严谨规范，更符合一篇review的要求，更加的有条理，有逻辑

### AI Response / Action

AI 阅读了 `AGENTS.md`、当前 `paper/sections/problem.tex` 和 `notes/reading/problem-importance.md`，判断原章节的主要问题是叙述顺序较松散：问题定义、示例、重要性和最终问题表述之间存在回跳。AI 将该章节重组为更适合 review 的逻辑顺序：先定义 FLP 研究对象，再说明共识问题的重要性，然后介绍协议要求和系统模型，接着用事务提交例子解释异步模型下的困难，最后总结 FLP 的不可能性结论及其对后续系统设计的边界意义。

### Files Created / Modified

- `paper/sections/problem.tex`
- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

AI 已运行 `latexmk -xelatex -interaction=nonstopmode -halt-on-error main.tex`，LaTeX 编译通过。当前仍有 bibliography 为空的正常提醒，因为正文尚未加入 `\cite{...}`。用户后续仍应根据原论文检查术语和表述是否符合自己的理解。

## 2026-07-11: 收束 Problem and Motivation 章节范围

### User Prompt

用户反馈：

> 我觉得在问题这一章节不需要说这篇论文的结果 以及这个结果有什么重要意义 只需要有这篇论文解决了什么问题 以及这个问题的重要性就行了

### AI Response / Action

AI 采纳该反馈，认为 `Problem and Motivation` 章节应聚焦于“论文研究什么问题”和“为什么这个问题重要”，不应提前展开 FLP 的否定结论或该结论对后续系统设计的理论边界意义。AI 因此删除了章节末尾关于“FLP 的答案是否定的”和“不可能性结果的重要意义”的总结段落，仅保留清晰的问题表述、问题背景、系统模型和事务提交动机示例。

### Files Created / Modified

- `paper/sections/problem.tex`
- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

AI 已运行 `latexmk -xelatex -interaction=nonstopmode -halt-on-error main.tex`，LaTeX 编译通过。后续可在 proof、comparison 或 limitations 章节中再讨论 FLP 结论及其重要意义。

## 2026-07-13: 整理 FLP 相关研究

### User Prompt

用户要求：

> 接下来该解决该问题有什么相关研究（包括该论文提到的，和没提到的，以及在该论文发表之后的新的相关研究）

### AI Response / Action

AI 根据 `AGENTS.md` 的要求记录本次对话，并围绕 FLP 共识不可能性整理相关研究。AI 首先从 `docs/flp.pdf` 的参考文献和结论中抽取原论文提到的方向，包括 Byzantine agreement、distributed commit、randomized agreement、partial synchrony 等。随后检索并补充 FLP 之后的重要方向，包括 unreliable failure detectors、weakest failure detector、state machine replication、Paxos、PBFT 和 Raft。

AI 将相关研究分为三类：原论文提到的相关研究、原论文没有重点展开但相关的研究、FLP 发表之后的新进展。AI 更新了 `notes/related-work/related-work-matrix.md`，并将代表性内容整理成 LaTeX 正文草稿 `paper/sections/related-work.tex`。为了让正文引用能够解析，AI 同时补充了 `paper/references.bib` 中的代表性文献条目。

### Files Created / Modified

- `notes/related-work/related-work-matrix.md`
- `paper/sections/related-work.tex`
- `paper/references.bib`
- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

AI 已运行 `latexmk -xelatex -interaction=nonstopmode -halt-on-error main.tex`，LaTeX 编译通过，新增引用最终均已解析。用户后续应检查每篇相关工作的取舍是否符合课程 review 篇幅，并可进一步补充原论文参考文献中的 commit protocol 细节。

## 2026-07-13: 下载 Related Work Matrix 中的论文 PDF

### User Prompt

用户要求：

> 请把Related Work Matrix 中提到的论文下载到refs/papers文件夹中，我已经下载了三篇

### AI Response / Action

AI 检查了 `refs/papers/`，发现用户已经下载了 `byz.pdf`、`dist-commit.pdf`、`reaching.pdf` 三篇。AI 随后根据 `notes/related-work/related-work-matrix.md` 下载了其余可公开获取的代表性 PDF，包括 Skeen 的 decentralized termination protocol、Skeen 和 Stonebraker 的 crash recovery、Ben-Or 的 randomized agreement、Dwork-Lynch-Stockmeyer 的 partial synchrony、Skeen 的 nonblocking commit protocols、Schneider 的 state machine approach、Chandra-Toueg 的 unreliable failure detectors、Chandra-Hadzilacos-Toueg 的 weakest failure detector、Lamport 的 Paxos 两篇、Castro-Liskov 的 PBFT 和 Ongaro-Ousterhout 的 Raft。

Bracha and Toueg, *Resilient Consensus Protocols* 的 ACM PDF 直链返回 403，Cornell eCommons 直链返回 405/404，因此本次没有成功下载该篇，已在 `refs/README.md` 中标注需要通过 ACM 或图书馆访问。

AI 还使用 `file` 和 `pdfinfo` 验证了已下载 PDF 均为可读取 PDF，并更新 `refs/README.md` 建立 PDF 索引。

### Files Created / Modified

- `refs/papers/*.pdf`
- `refs/README.md`
- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

已下载的 PDF 均通过 `pdfinfo` 页数检查。后续若需要 Bracha-Toueg 原文，可通过学校图书馆、ACM Digital Library 或其他合法渠道手动下载后放入 `refs/papers/` 并补充索引。

## 2026-07-13: 统一用户下载的三篇论文文件名

### User Prompt

用户要求：

> 请把我下载的那三篇论文的名字也改成和其他论文一样的格式

### AI Response / Action

AI 检查了 `refs/papers/` 中用户原先下载的三篇论文：`reaching.pdf`、`byz.pdf`、`dist-commit.pdf`，并将它们改名为与其他 PDF 一致的作者-年份-标题格式：

- `pease-shostak-lamport-1980-reaching-agreement.pdf`
- `lamport-shostak-pease-1982-byzantine-generals-problem.pdf`
- `dolev-strong-1982-distributed-commit-bounded-waiting.pdf`

AI 同步更新了 `refs/README.md` 中的本地文件路径索引。

### Files Created / Modified / Deleted

- Renamed `refs/papers/reaching.pdf` to `refs/papers/pease-shostak-lamport-1980-reaching-agreement.pdf`
- Renamed `refs/papers/byz.pdf` to `refs/papers/lamport-shostak-pease-1982-byzantine-generals-problem.pdf`
- Renamed `refs/papers/dist-commit.pdf` to `refs/papers/dolev-strong-1982-distributed-commit-bounded-waiting.pdf`
- `refs/README.md`
- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

需要检查 `refs/papers/` 文件清单和 `refs/README.md` 索引，确认旧文件名不再作为当前索引出现。

## 2026-07-13: 阅读相关研究论文并撰写报告

### User Prompt

用户要求：

> 请你阅读这些论文 然后写一个阅读报告放在笔记的相关工作文件夹中

### AI Response / Action

AI 使用 `pdf` 技能处理 `refs/papers/` 中的相关研究 PDF。AI 先用 `pdftotext` 将 15 篇 PDF 提取为临时文本，重点阅读标题、摘要、引言、结论和与 consensus / agreement / commit / failure detector / Paxos / Raft 等关键词相关的段落。个别老扫描件文本提取质量较差，AI 在报告中注明该限制。

AI 随后在 `notes/related-work/` 下新增阅读报告，按五条研究脉络组织：早期 agreement / Byzantine agreement、分布式事务提交与 crash recovery、随机化 agreement、partial synchrony 与 failure detectors、consensus 作为系统构建模块。报告逐篇总结每篇论文解决的问题、核心思想、与 FLP 的关系，以及可在 review 中使用的角度。

### Files Created / Modified

- `notes/related-work/related-work-reading-report.md`
- `process/ai-usage/conversation-log.md`
- Temporary extracted text files under `tmp/related-work-text/` were created for reading and then deleted.

### Verification / Follow-up

报告是面向课程 review 的初读整理，不是逐页逐字翻译。后续正式写作前，用户应结合原 PDF 再核对重点引用和具体技术细节。

## 2026-07-13: 咨询相关研究论文 PDF 存放位置

### User Prompt

用户问：

> 如果我想把相关研究的论文下载下来应该放到哪里

### AI Response / Action

AI 建议不要把相关研究 PDF 放进 `paper/` 或 `notes/`，而是放到 `refs/papers/` 这类专门的参考资料目录中，并在 `refs/README.md` 或 `notes/related-work/related-work-matrix.md` 中记录文件名、论文题目、来源链接和用途。对于体积较大的 PDF，如果担心 GitHub 仓库过大，也可以只记录链接、DOI 或本地路径，不提交 PDF 本体。

### Files Created / Modified

- `process/ai-usage/conversation-log.md`

### Verification / Follow-up

如果后续实际下载 PDF，可以创建 `refs/papers/` 并补充 `refs/README.md` 的索引表。
