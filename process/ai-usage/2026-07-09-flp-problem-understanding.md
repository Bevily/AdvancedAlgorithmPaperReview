# AI Usage - 2026-07-09 - FLP Problem Understanding

- Tool: Codex / ChatGPT-style AI assistant
- User: TODO: fill in your name
- Purpose: 帮助理解 FLP 论文解决的核心问题，并形成阅读笔记。

## Prompt Summary

用户要求先解决 review 中的第一个问题：这篇论文解决了什么问题。

## AI Response Summary

AI 阅读了本仓库中的 `docs/flp.pdf`，重点查看了论文标题、摘要、引言、共识协议定义和主定理部分。AI 总结认为该论文研究完全异步消息传递系统中的确定性容错共识问题，并证明在最多一个 crash failure 的情况下，不存在能保证所有合法执行都终止的共识协议。

## Verification

- Sources checked:
  - `docs/flp.pdf`
- Paper sections checked:
  - Abstract
  - Section 1 Introduction
  - Section 2 Consensus Protocols
  - Section 3 Main Result
- Which parts were correct:
  - 论文确实研究 asynchronous consensus。
  - 论文确实假设消息可靠，但延迟可任意长。
  - 论文确实考虑最多一个 faulty process。
  - 主定理确实为 no consensus protocol is totally correct in spite of one fault。
- Which parts need further verification:
  - 后续写 review 时，应结合论文原文进一步核对 partial correctness、total correctness、admissible run、bivalence 等形式化定义。

## How It Was Used

- Adopted:
  - 将“慢进程和死进程不可区分”作为理解该问题的核心直觉。
  - 将问题表述为“完全异步、可靠消息、最多一个 crash failure 下，确定性共识是否能保证终止”。
- Modified:
  - 后续正式正文需要由我用自己的话重写，不能直接复制笔记。
- Rejected:
  - 暂无。

## Related Files / Commits

- `notes/reading/problem-understanding.md`
