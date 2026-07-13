# Related Work Matrix

Target paper: *Impossibility of Distributed Consensus with One Faulty Process*.

本文件记录与 FLP 共识不可能性相关的研究脉络。这里先按 review 需要分成三类：

1. 原论文明确提到或引用的相关研究；
2. 原论文没有重点展开、但对理解问题很有帮助的相关研究；
3. FLP 发表之后发展出的新方向。

## High-Level Map

FLP 相关研究可以按问题意识分成几条线：

- **事务提交与分布式数据库**：说明共识为什么重要，典型问题是 commit / abort 必须一致。
- **拜占庭一致与早期容错协议**：研究更强故障模型下的 agreement，通常依赖同步或额外假设。
- **随机化协议**：绕开 FLP 的确定性限制，将终止保证改成概率意义上的终止。
- **部分同步与故障检测器**：在完全异步和完全同步之间加入更现实的时间或检测假设。
- **实际共识与复制系统**：Paxos、PBFT、Raft 等把共识思想推进到系统构建和工程实现。

## Related Work Matrix

| Work | Year | Relation to Selected Paper | What Problem It Solves | Key Technique | Difference / Improvement | Should Cite? |
|---|---:|---|---|---|---|---|
| Pease, Shostak, and Lamport, *Reaching Agreement in the Presence of Faults* | 1980 | Mentioned by original paper | 多处理器在存在故障时达成一致 | Byzantine agreement 模型 | 关注 Byzantine faults，通常比 crash failure 更强；为 agreement 问题提供早期形式化基础 | Yes |
| Lamport, Shostak, and Pease, *The Byzantine Generals Problem* | 1982 | Mentioned by original paper | 将拜占庭故障下的一致性问题经典化 | 口头消息 / 签名消息模型 | 与 FLP 同属 agreement 研究，但故障模型和同步假设不同 | Yes |
| Dolev and Strong, *Distributed Commit with Bounded Waiting* | 1982 | Mentioned by original paper | 分布式 commit 中如何避免无限等待 | bounded waiting / commit protocol | FLP 指出这类方案依赖额外时序或模型假设，不能直接适用于完全异步模型 | Maybe |
| Skeen, *A Decentralized Termination Protocol* | 1982 | Mentioned by original paper | 分布式事务终止与提交协调 | decentralized termination | 帮助说明 FLP 的实际动机：事务系统中的等待和终止问题 | Yes |
| Skeen and Stonebraker, *A Formal Model of Crash Recovery in a Distributed System* | 1983 | Mentioned by original paper | 分布式系统 crash recovery 的形式化模型 | crash recovery model | 与 FLP 的 crash failure 背景相关，但重点是恢复与事务系统 | Maybe |
| Ben-Or, *Another Advantage of Free Choice: Completely Asynchronous Agreement Protocols* | 1983 | Mentioned by original paper | 完全异步系统中的 agreement | randomized consensus | 通过随机化绕开确定性 FLP 不可能性，只保证概率意义上的终止 | Yes |
| Bracha and Toueg, *Resilient Consensus Protocols* | 1983 | Mentioned by original paper | 异步/容错共识协议 | resilient agreement protocols | 与 Ben-Or 一样代表 FLP 后随机化或更弱要求方向 | Yes |
| Dwork, Lynch, and Stockmeyer, *Consensus in the Presence of Partial Synchrony* | 1988 | Mentioned by original paper as subsequent progress | 在部分同步系统中解决 consensus | partial synchrony | 引入介于同步和异步之间的模型，是回应 FLP 的核心路线之一 | Yes |
| Two-Phase Commit / Three-Phase Commit literature | 1970s-1980s | Not emphasized as a single theory line in original paper | 分布式数据库中 commit / abort 一致决定 | coordinator, logging, timeout, nonblocking variants | 帮助把 FLP 和数据库事务联系起来；但不是 FLP 的核心证明对象 | Maybe |
| Schneider, *Implementing Fault-Tolerant Services Using the State Machine Approach* | 1990 | Later work / not mentioned by original paper | 如何用复制状态机实现容错服务 | state machine replication | 将共识作为实现复制服务的核心子问题，连接理论 consensus 与系统服务 | Yes |
| Chandra and Toueg, *Unreliable Failure Detectors for Reliable Distributed Systems* | 1996 | Later work | 异步 crash failure 系统中如何用不可靠故障检测器解决 consensus | failure detector classes | 明确把“额外故障检测信息”作为绕开 FLP 的模型假设 | Yes |
| Chandra, Hadzilacos, and Toueg, *The Weakest Failure Detector for Solving Consensus* | 1996 | Later work | 求解 consensus 所需的最弱故障检测能力 | weakest failure detector | 进一步刻画绕开 FLP 需要多少故障检测信息 | Yes |
| Lamport, *The Part-Time Parliament* | 1998 | Later work | crash fault 下的实用共识协议族 Paxos | ballots, quorums, majority intersection | Paxos 成为之后 crash-fault-tolerant consensus 的基础，但依赖 leader、超时和多数派等工程机制 | Yes |
| Lamport, *Paxos Made Simple* | 2001 | Later work | 更清晰地解释 Paxos consensus | proposer / acceptor / learner, quorum | 适合在 review 中作为 Paxos 的易懂引用 | Yes |
| Castro and Liskov, *Practical Byzantine Fault Tolerance* | 1999 | Later work | 实用 Byzantine fault-tolerant replication | PBFT, state machine replication | 将 Byzantine fault tolerance 推向可运行系统；故障模型比 FLP crash failure 更强 | Maybe |
| Ongaro and Ousterhout, *In Search of an Understandable Consensus Algorithm* | 2014 | Later work | 设计更易理解的 replicated log consensus | leader-based Raft, decomposition | 与 Paxos 等价目标相近，但强调可理解性和工程实现 | Maybe |

## How To Use This In The Review

正文中可以按三段组织，而不是逐篇罗列：

1. **Before / Around FLP: why consensus was already important.**  
   事务提交和拜占庭一致说明 agreement 是分布式系统中的基础问题。可以引用 Pease-Shostak-Lamport、Lamport-Shostak-Pease、Skeen，以及分布式 commit 相关工作。

2. **FLP's immediate escape routes.**  
   FLP 之后的研究主要沿两条路线继续：一是随机化，代表是 Ben-Or、Bracha-Toueg；二是加入时间假设，代表是 Dwork-Lynch-Stockmeyer 的 partial synchrony。

3. **Long-term system impact.**  
   后续 failure detectors、Paxos、state machine replication、PBFT、Raft 等工作，都可以看作是在明确额外假设或工程机制的前提下，把 consensus 变成可构建系统的核心模块。

## Search Log

| Date | Search Query / Source | Findings | Next Step |
|---|---|---|---|
| 2026-07-13 | `docs/flp.pdf` references | 原论文引用了 Byzantine agreement、distributed commit、randomized agreement、partial synchrony 等方向 | 选出代表性文献写入矩阵 |
| 2026-07-13 | `Chandra Toueg unreliable failure detectors consensus 1996` | failure detector 是 FLP 之后绕开完全异步不可能性的核心模型 | 写入 later work |
| 2026-07-13 | `Paxos Made Simple Lamport 2001 consensus` | Paxos 是 crash fault 共识在工程系统中的核心路线 | 写入 later work |
| 2026-07-13 | `Raft In Search of an Understandable Consensus Algorithm 2014` | Raft 将 replicated log consensus 组织成更易理解的工程协议 | 写入 later work |
| 2026-07-13 | `Practical Byzantine Fault Tolerance Castro Liskov 1999` | PBFT 展示 Byzantine fault tolerance 的实用系统化路线 | 写入 later work |
