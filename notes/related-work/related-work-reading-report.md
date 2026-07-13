# Related Work Reading Report

Target paper: *Impossibility of Distributed Consensus with One Faulty Process*.

本报告基于 `refs/papers/` 中已下载的相关论文 PDF 初读整理。阅读目标不是逐字复述每篇论文，而是服务最终 review：弄清每篇论文解决什么问题、使用什么思想、它和 FLP 的关系是什么，以及哪些内容适合写进相关工作章节。

说明：Bracha and Toueg, *Resilient Consensus Protocols* 目前没有下载到 PDF，因此本报告暂不单独展开该篇。Skeen 的部分早期扫描件文本提取质量较差，本报告结合可读片段、标题、摘要和相关工作矩阵进行概括，后续正式写作前建议人工再核对原 PDF。

## Overall Picture

围绕 FLP 的相关研究可以分成五条主线：

1. **早期 agreement / Byzantine agreement**  
   这条线说明“多个进程在故障下达成一致”本身已经是分布式计算中的核心问题。代表论文包括 Pease-Shostak-Lamport 1980 和 Lamport-Shostak-Pease 1982。

2. **分布式事务提交与 crash recovery**  
   这条线解释 FLP 为什么选择 transaction commit 作为动机。commit / abort 必须一致，否则数据库状态会分裂。代表论文包括 Dolev-Strong 1982、Skeen 1981/1982、Skeen-Stonebraker 1983。

3. **随机化绕开 FLP**  
   FLP 排除的是确定性异步共识协议的保证终止。Ben-Or 1983 说明，如果允许随机化，可以在概率意义上重新获得 agreement。

4. **加入额外时序或故障检测假设**  
   Dwork-Lynch-Stockmeyer 的 partial synchrony 和 Chandra-Toueg 的 failure detectors 都可以看作对 FLP 的直接回应：既然完全异步下信息不够，就明确加入额外信息。

5. **共识走向系统实现**  
   Schneider 的 state machine approach、Lamport 的 Paxos、Castro-Liskov 的 PBFT、Ongaro-Ousterhout 的 Raft 说明，FLP 没有终结共识研究，而是迫使实际系统清楚说明自己依赖的多数派、leader、超时、故障检测或同步假设。

## 1. Early Agreement and Byzantine Agreement

### Pease, Shostak, and Lamport, *Reaching Agreement in the Presence of Faults* (1980)

**Problem.**  
这篇论文研究一组处理器中有部分处理器可能故障时，非故障处理器如何对信息形成一致认识。论文使用 interactive consistency 的表述：每个非故障处理器最终应得到同一个向量，并且向量中对应非故障处理器的值必须等于该处理器真实持有的值。

**Key idea.**  
论文证明，在一般 Byzantine 式故障下，若最多有 `m` 个故障处理器，总处理器数需要满足 `n >= 3m + 1` 才能解决问题。也就是说，故障容忍不是“多数投票”这么简单；如果故障节点可以对不同接收者撒不同的谎，系统需要更强的冗余比例。

**Relation to FLP.**  
这篇论文和 FLP 都研究 agreement，但二者的困难来源不同。Pease-Shostak-Lamport 关注 Byzantine fault 下的信息不可信问题；FLP 关注完全异步系统中无法区分慢进程和崩溃进程的问题。它可以作为相关工作章节中“agreement 在 FLP 之前已经是核心问题”的代表。

**Use in review.**  
可以用来说明：FLP 不是凭空提出 consensus，而是在已有 Byzantine agreement 传统上，转向了更弱的 crash failure 和更强的不可能性边界。

### Lamport, Shostak, and Pease, *The Byzantine Generals Problem* (1982)

**Problem.**  
这篇论文用“拜占庭将军问题”将 agreement 的困难抽象成一个经典故事：忠诚将军必须达成同一行动计划，而叛徒可以任意发送误导信息。

**Key idea.**  
论文区分 oral messages 和 signed messages 两类模型。在 oral messages 中，叛徒可以向不同将军发送不同内容，因此要达成一致需要足够多的冗余；在 signed messages 中，签名能限制叛徒伪造他人消息的能力。

**Relation to FLP.**  
Byzantine Generals 强调恶意或任意故障；FLP 则强调即使没有恶意、只有一个进程静默 crash，完全异步模型下也无法保证确定性共识终止。因此，FLP 的结果显得更惊人：它不需要 Byzantine 那么强的故障模型。

**Use in review.**  
相关工作中可以把它放在“更强故障模型下的 agreement”部分，用来对比 FLP 的 crash failure。

## 2. Distributed Commit and Crash Recovery

### Dolev and Strong, *Distributed Commit with Bounded Waiting* (1982)

**Problem.**  
分布式 commit 协议要让多个参与者一致地 commit 或 abort，同时避免某些站点因为等待其他站点而无限阻塞。论文标题中的 bounded waiting 正是针对等待时间问题。

**Key idea.**  
论文试图将 Byzantine agreement 思想和 transaction commit 结合起来，让 commit 协议在某些故障情况下仍能给出有界等待保证。

**Relation to FLP.**  
FLP 的重要背景正是：现实 commit 协议常常存在“window of vulnerability”，即某个关键时刻一个进程不可达会导致系统等待。Dolev-Strong 代表了 FLP 之前试图通过协议设计减轻 commit 阻塞问题的方向。

**Use in review.**  
可用来说明 FLP 不是只针对抽象模型；它回应了数据库系统中真实存在的 commit waiting 问题。

### Skeen, *Nonblocking Commit Protocols* (1981)

**Problem.**  
这篇论文研究 nonblocking commit protocols，即在站点故障出现时，仍允许正常站点继续处理事务，而不是被迫无限等待。

**Key idea.**  
论文分析 commit protocol 中哪些状态会造成 blocking，并给出设计 nonblocking protocol 的必要和充分条件。它把 commit 协议的状态划分、commit point、committable / noncommittable 等概念形式化。

**Relation to FLP.**  
Nonblocking commit 反映了 FLP 动机中的核心痛点：分布式事务系统不能只保证一致性，还希望能终止。FLP 则指出，在完全异步且允许 crash 的模型下，想要普遍避免 blocking 需要额外假设。

**Use in review.**  
可作为“原论文没有重点展开但对问题动机有帮助”的背景材料。

### Skeen, *A Decentralized Termination Protocol* (1982)

**Problem.**  
该论文关注当 commit 协议执行中出现站点故障时，剩余站点如何通过 termination protocol 推动事务进入 commit 或 abort，而不是一直停留在不确定状态。

**Key idea.**  
与中心化协调者不同，decentralized termination 试图让多个站点协作完成终止决策。论文讨论 nonblocking commit protocol 的属性，并给出分散式终止协议。

**Relation to FLP.**  
这篇论文和 FLP 共享同一个工程问题背景：分布式事务需要一致决定，但故障可能造成等待。FLP 的结论为这些 protocol 的适用边界提供理论解释。

**Use in review.**  
可用于说明 FLP 中 transaction commit 例子并不是随意选择，而是当时数据库研究中的实际问题。

### Skeen and Stonebraker, *A Formal Model of Crash Recovery in a Distributed System* (1983)

**Problem.**  
该论文试图形式化分布式系统中 crash recovery 的问题，即站点崩溃和恢复时，如何保证系统状态和事务结果一致。

**Key idea.**  
重点在于把 crash recovery 作为系统模型来刻画，讨论事务、恢复、日志和一致性之间的关系。

**Relation to FLP.**  
FLP 假设的是 crash failure，但并不处理恢复；Skeen-Stonebraker 更接近数据库系统工程语境。二者结合起来可以说明：crash failure 看似温和，但在分布式事务中已经足以造成复杂问题。

**Use in review.**  
适合放在 related work 的事务提交与 crash recovery 背景中，不必作为主要技术线展开。

## 3. Randomized Agreement

### Ben-Or, *Another Advantage of Free Choice: Completely Asynchronous Agreement Protocols* (1983)

**Problem.**  
这篇论文直接回应 FLP：FLP 证明完全异步系统中没有确定性共识协议能容忍一个未宣布的进程死亡；Ben-Or 则问，如果允许概率选择，是否可以解决。

**Key idea.**  
Ben-Or 给出 probabilistic solution，保证在多数进程继续运行时，以概率 1 最终作出决定。论文还将类似思想扩展到 Byzantine failure，说明随机化能让某些确定性不可解问题重新变得可解。

**Relation to FLP.**  
这是最直接的“绕开 FLP”路线之一。FLP 排除 deterministic total correctness；Ben-Or 改变终止保证的性质，从确定性终止改为概率 1 终止。

**Use in review.**  
可以在相关工作中写成：FLP 并不意味着异步共识完全没有希望，而是迫使后续研究改变模型或目标。Ben-Or 代表“改变目标”的方向。

## 4. Partial Synchrony and Failure Detectors

### Dwork, Lynch, and Stockmeyer, *Consensus in the Presence of Partial Synchrony* (1988)

**Problem.**  
完全同步模型太强，完全异步模型又有 FLP 不可能性。DLS 研究介于两者之间的 partial synchrony：系统可能存在消息延迟和处理速度的界，但这个界可能未知，或只在某个 global stabilization time 之后成立。

**Key idea.**  
论文提出 partial synchrony 模型，并在该模型下给出容错 consensus 协议及可容忍故障数边界。它将现实系统中“最终网络会稳定一些”的直觉形式化。

**Relation to FLP.**  
DLS 是对 FLP 的经典回应：既然完全异步不够，就加入更现实但比完全同步更弱的时间假设。它解释了为什么 timeout 在实践中有用：timeout 本身并不是纯异步模型中的可靠事实，而是依赖某种最终同步性。

**Use in review.**  
应作为 FLP 后续发展中的核心论文重点引用。

### Chandra and Toueg, *Unreliable Failure Detectors for Reliable Distributed Systems* (1996)

**Problem.**  
FLP 的困难来自无法判断一个进程是崩溃还是很慢。Chandra-Toueg 研究如果给异步系统增加 failure detector，即使这个 detector 可能犯错，能否解决 consensus。

**Key idea.**  
论文用 completeness 和 accuracy 描述 failure detector 的性质，并证明某些不可靠 failure detector 足以解决 consensus。它还证明 consensus 和 atomic broadcast 在异步 crash failure 系统中可以互相归约。

**Relation to FLP.**  
这篇论文把 FLP 的核心直觉形式化：异步系统缺少关于故障的信息；如果提供某种额外但不完美的信息，就可能恢复可解性。

**Use in review.**  
非常适合用于“FLP 后，研究者如何补足模型信息”的部分。

### Chandra, Hadzilacos, and Toueg, *The Weakest Failure Detector for Solving Consensus* (1996)

**Problem.**  
如果 failure detector 能帮助解决 consensus，那么自然问题是：最少需要多少故障检测能力？

**Key idea.**  
论文证明某个很弱的 failure detector 已经足以解决 consensus，并且任何能解决 consensus 的 failure detector 至少要提供这么多信息。也就是说，它给出了 solving consensus 所需 failure detector 的下界和精确刻画。

**Relation to FLP.**  
FLP 说明完全异步 crash failure 系统中信息不足；这篇论文进一步问：到底缺的是什么信息，最少补多少才够？

**Use in review.**  
可以作为 Chandra-Toueg failure detector 路线的深化，不一定在正文中长篇展开，但值得列为后续重要工作。

## 5. Consensus as a System-Building Primitive

### Schneider, *Implementing Fault-Tolerant Services Using the State Machine Approach* (1990)

**Problem.**  
分布式服务如果只有一个服务器，容错能力受限。Schneider 讨论如何用多个副本实现容错服务。

**Key idea.**  
State machine approach 的核心是：如果多个副本从同一初始状态开始，并按同一顺序执行同一组确定性命令，它们就会保持一致。因此，系统需要解决的关键问题是如何让副本对请求顺序达成一致。

**Relation to FLP.**  
该论文把 consensus 从抽象问题转化成构建容错服务的基础模块。FLP 说明在完全异步 crash 模型下这个模块不可能免费获得；state machine replication 则说明为什么实际系统仍然必须想办法实现它。

**Use in review.**  
可用于连接理论 consensus 和现实系统。

### Lamport, *The Part-Time Parliament* (1998)

**Problem.**  
这篇论文提出 Paxos，用带有故事化的方式描述一组不可靠参与者如何对一系列值达成一致。

**Key idea.**  
Paxos 使用 ballots、quorums 和多数派交集保证安全性。即使消息延迟、丢失或进程故障，只要有足够参与者继续工作并最终有稳定进展条件，就能选择值。

**Relation to FLP.**  
Paxos 并不违反 FLP，因为它在实际运行中依赖 leader、重试、超时和多数派等机制来获得活性；其安全性和活性所依赖的假设需要分开理解。

**Use in review.**  
适合作为 FLP 后 crash-fault-tolerant consensus 走向实际系统的代表。

### Lamport, *Paxos Made Simple* (2001)

**Problem.**  
原始 Paxos 论文难懂，Lamport 用更直接的方式解释 Paxos consensus algorithm。

**Key idea.**  
论文将 Paxos 分为 proposer、acceptor、learner 等角色，通过多数派 acceptor 的交集性质保证最多只有一个值被 chosen。随后将单次 consensus 扩展到 state machine replication。

**Relation to FLP.**  
Paxos 是 FLP 后最重要的实践路线之一：它承认完全异步下无法保证无条件终止，但在工程系统中通过 leader 和超时等机制获得可用性。

**Use in review.**  
如果正文篇幅有限，引用 *Paxos Made Simple* 比原始 Paxos 更适合解释给读者。

### Castro and Liskov, *Practical Byzantine Fault Tolerance* (1999)

**Problem.**  
早期 Byzantine fault tolerant 协议往往过于理论化或依赖同步假设。PBFT 试图让 Byzantine fault tolerance 在真实系统中可用。

**Key idea.**  
PBFT 使用 state machine replication，在 `3f + 1` 个副本中容忍 `f` 个 Byzantine faulty replicas。正常路径包含 pre-prepare、prepare、commit 等阶段，并用认证机制降低开销。

**Relation to FLP.**  
PBFT 的故障模型比 FLP 更强，但系统目标不同。它展示了 agreement / replication 问题如何在更强攻击模型下走向工程实践。和 FLP 的关系主要是：现实协议必须明确假设和安全/活性条件。

**Use in review.**  
可作为 later work 的补充，不一定是核心，除非 review 想强调 Byzantine fault tolerance 的发展。

### Ongaro and Ousterhout, *In Search of an Understandable Consensus Algorithm* (2014)

**Problem.**  
Paxos 影响很大，但难以理解和实现。Raft 的目标是提供一个与 Paxos 等价目标相近、但更容易理解的 replicated log consensus algorithm。

**Key idea.**  
Raft 将共识分解为 leader election、log replication、safety 和 membership changes。它使用强 leader 模型和随机化 election timeout 来减少状态空间，让协议更容易学习和实现。

**Relation to FLP.**  
Raft 是工程化 consensus 的后续代表。它并不绕开 FLP 的理论边界，而是在实际系统假设下提供更清晰的实现结构。

**Use in review.**  
适合作为长期影响的例子：FLP 之后，共识研究不仅发展理论模型，也发展了可教学、可实现的系统协议。

## What To Write In The Review

最终正文不宜逐篇堆叠。可以组织为如下三段：

1. **FLP 之前：agreement 已经是分布式计算核心问题。**  
   引用 Byzantine agreement 和 distributed commit，说明 FLP 继承了已有问题意识，但选择了完全异步和 crash failure 这个更精确的边界。

2. **FLP 之后：研究者通过改变假设绕开不可能性。**  
   随机化、partial synchrony、failure detectors 是三条直接回应 FLP 的理论路线。

3. **长期影响：consensus 成为容错系统构建模块。**  
   State machine replication、Paxos、PBFT、Raft 说明，现实系统可以实现共识服务，但必须明确依赖多数派、leader、超时、故障检测或更强同步假设。

## Citation Priority

如果正文篇幅有限，建议优先引用：

1. Lamport, Shostak, and Pease 1982：Byzantine Generals 作为早期 agreement 代表。
2. Skeen 1981/1982：transaction commit 动机。
3. Ben-Or 1983：随机化绕开 FLP。
4. Dwork, Lynch, Stockmeyer 1988：partial synchrony。
5. Chandra and Toueg 1996：failure detectors。
6. Lamport 2001：Paxos 作为系统化 consensus 代表。

PBFT 和 Raft 可以在“后续系统影响”中一笔带过；如果篇幅紧张，不必详细展开。
