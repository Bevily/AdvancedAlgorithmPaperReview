# FLP 证明顺序笔记：从状态机角度理解

阅读对象：Fischer, Lynch, and Paterson, *Impossibility of Distributed Consensus with One Faulty Process*。

日期：2026-07-17

说明：本笔记尽量沿原论文第 2 节和第 3 节的顺序整理，不加入额外类比或扩展解释。

## 1. 协议作为状态机系统

论文先把一个 consensus protocol \(P\) 定义成一个异步系统，里面有 \(N\) 个进程。

每个进程 \(p\) 有：

- 一个一位输入寄存器 \(x_p\)；
- 一个输出寄存器 \(y_p\)，取值为 \(b,0,1\)，其中 \(b\) 表示还没有决定；
- 内部存储；
- 程序计数器；
- 一个确定性的 transition function。

这些东西共同构成进程 \(p\) 的内部状态。

当 \(y_p=0\) 或 \(y_p=1\) 时，进程进入 decision state。论文还要求输出寄存器是 write-once 的，也就是说，一旦进程决定了 \(0\) 或 \(1\)，之后不能再改。

所以从状态机角度看，每个进程就是一个确定性自动机。给定当前内部状态和收到的消息，它的下一步状态是确定的。

## 2. 消息系统和 configuration

进程通过消息通信。消息系统维护一个 message buffer，里面保存已经发送但还没有投递的消息。

消息系统支持两个操作：

```text
send(p, m)
receive(p)
```

`send(p, m)` 把消息放入 buffer。

`receive(p)` 要么从 buffer 中取出某个发给 \(p\) 的消息 \((p,m)\)，要么返回特殊空标记 \(\varnothing\)，并且不改变 buffer。

论文允许消息系统是 nondeterministic 的，但要求：如果一个非故障进程无限次执行 `receive(p)`，那么发给它的消息最终会被投递。

接着论文定义 configuration：

> 一个 configuration 包括所有进程的内部状态，以及 message buffer 的内容。

初始 configuration 是：每个进程处于初始状态，message buffer 为空。

## 3. Step、event 和 schedule

一次 step 是某个单独进程 \(p\) 执行一个 primitive step。

这个 step 分两阶段：

1. 对 message buffer 执行 `receive(p)`，得到某个 \(m\)，其中 \(m\) 可以是真消息，也可以是 \(\varnothing\)；
2. 进程 \(p\) 根据自己的当前内部状态和 \(m\)，进入新内部状态，并发送有限个消息。

因为进程是确定性的，所以一个 step 完全由事件

\[
e=(p,m)
\]

决定。

论文把这个事件理解为“进程 \(p\) 收到 \(m\)”。

如果从 configuration \(C\) 应用事件 \(e\)，得到的新 configuration 记作：

\[
e(C)
\]

一个 schedule 是一串可以依次应用的 events。有限 schedule 从 \(C\) 出发会到达某个 reachable configuration。

## 4. Lemma 1：不相关进程步骤的交换性

Lemma 1 说：

如果从同一个 configuration \(C\) 出发，有两个 schedule \(\sigma_1\) 和 \(\sigma_2\)，分别到达 \(C_1\) 和 \(C_2\)，并且 \(\sigma_1\) 中执行步骤的进程集合与 \(\sigma_2\) 中执行步骤的进程集合不相交，那么：

- \(\sigma_2\) 可以从 \(C_1\) 继续应用；
- \(\sigma_1\) 可以从 \(C_2\) 继续应用；
- 两边最终到达同一个 configuration。

从状态机角度看，这是因为两个 schedule 涉及的进程集合不相交，所以它们的状态转换互不干扰。

## 5. partial correctness 和 total correctness

论文接着定义 decision value。

如果某个进程已经处于 decision state，并且 \(y_p=v\)，那么 configuration \(C\) 有 decision value \(v\)。

协议 partially correct 需要满足：

1. 没有 accessible configuration 同时有两个不同的 decision value；
2. 对每个 \(v \in \{0,1\}\)，都存在某个 accessible configuration 具有 decision value \(v\)。

接着定义 run。

一个进程在某个 run 中如果执行无限多步，就是 nonfaulty；否则是 faulty。

一个 run 是 admissible，如果：

- 最多一个进程 faulty；
- 所有发给 nonfaulty 进程的消息最终都会收到。

一个 run 是 deciding run，如果某个进程在这个 run 中进入 decision state。

协议 totally correct in spite of one fault，如果：

- 它 partially correct；
- 每个 admissible run 都是 deciding run。

主定理要证明的是：不存在这样的协议。

## 6. 主定理的反证开始

Theorem 1：

> No consensus protocol is totally correct in spite of one fault.

论文用反证法。假设存在一个协议 \(P\)，它在一个故障下 totally correct。

接下来证明会导出矛盾。

## 7. bivalent 和 univalent configuration

令 \(C\) 是一个 configuration。

令 \(V\) 是从 \(C\) 可达的所有 configuration 中可能出现的 decision value 集合。

如果：

\[
|V|=2
\]

则 \(C\) 是 bivalent。

也就是说，从 \(C\) 出发，未来既可能到达 decision value 0，也可能到达 decision value 1。

如果：

\[
|V|=1
\]

则 \(C\) 是 univalent。

如果唯一可能的 decision value 是 0，则称为 0-valent；如果唯一可能的 decision value 是 1，则称为 1-valent。

## 8. Lemma 2：存在 bivalent 初始 configuration

Lemma 2：

> \(P\) has a bivalent initial configuration.

证明按论文顺序是这样：

先假设不存在 bivalent initial configuration。

因为协议 partially correct，所以既存在能决定 0 的 accessible configuration，也存在能决定 1 的 accessible configuration。因此初始 configuration 中必须既有 0-valent 的，也有 1-valent 的。

论文定义两个初始 configuration 相邻，如果它们只在某一个进程 \(p\) 的输入值 \(x_p\) 上不同。

任意两个初始 configuration 可以通过一串相邻初始 configuration 连接起来。

因此，必然存在两个相邻初始 configuration：

\[
C_0,\ C_1
\]

其中 \(C_0\) 是 0-valent，\(C_1\) 是 1-valent，并且二者只在某个进程 \(p\) 的初始输入上不同。

然后论文考虑从 \(C_0\) 出发的某个 admissible deciding run，在这个 run 中进程 \(p\) 不执行任何步骤。

因为最多只有一个进程 faulty 是允许的，所以让 \(p\) 不执行步骤仍然符合 one fault 的设定。

令这个 run 的 schedule 为 \(\sigma\)。

由于 \(C_0\) 和 \(C_1\) 只在 \(p\) 的内部初始状态上不同，而 \(\sigma\) 中 \(p\) 不执行步骤，所以同一个 schedule \(\sigma\) 也可以应用到 \(C_1\)。

两个 run 中，除了进程 \(p\) 的内部状态外，对其他进程来说是相同的。由于 \(p\) 不执行步骤，最终两个 run 会达到相同的 decision value。

如果这个 decision value 是 1，那么 \(C_0\) 不可能是纯 0-valent；如果是 0，那么 \(C_1\) 不可能是纯 1-valent。

无论哪种情况，都和“不存在 bivalent initial configuration”的假设矛盾。

所以存在 bivalent initial configuration。

## 9. Lemma 3：bivalent configuration 可以避开某个事件后仍到达 bivalent

Lemma 3 是证明核心。

设 \(C\) 是 bivalent configuration，设事件

\[
e=(p,m)
\]

可以应用于 \(C\)。

令 \(\mathcal{C}\) 是从 \(C\) 出发、不应用事件 \(e\) 可以到达的 configuration 集合。

令：

\[
\mathcal{D}=e(\mathcal{C})
\]

也就是对 \(\mathcal{C}\) 中那些可以应用 \(e\) 的 configuration 再应用 \(e\) 后得到的集合。

Lemma 3 要证明：

> \(\mathcal{D}\) 中包含一个 bivalent configuration。

证明采用反证法。

假设 \(\mathcal{D}\) 中没有 bivalent configuration，那么 \(\mathcal{D}\) 中每个 configuration 都是 univalent。

因为 \(C\) 是 bivalent，所以从 \(C\) 可以到达某个 0-valent configuration，也可以到达某个 1-valent configuration。

论文由此推出，\(\mathcal{D}\) 中同时存在 0-valent 和 1-valent 的 configuration。

然后论文定义 neighbors：两个 configuration 如果一个可以通过单步到达另一个，则它们是 neighbors。

通过归纳，可以找到两个相邻 configuration：

\[
C_0,\ C_1 \in \mathcal{C}
\]

使得：

\[
D_i=e(C_i)
\]

是 \(i\)-valent，\(i=0,1\)。

不失一般性，设：

\[
C_1=e'(C_0)
\]

其中：

\[
e'=(p',m')
\]

然后分两种情况。

### Case 1: \(p' \neq p\)

也就是说，事件 \(e'\) 和事件 \(e\) 作用在不同进程上。

根据 Lemma 1，这两个事件可以交换顺序。因此：

\[
D_1=e'(D_0)
\]

但是 \(D_0\) 是 0-valent，而 0-valent configuration 的后继仍应只能导向 0。于是 \(D_1\) 不可能是 1-valent。

矛盾。

### Case 2: \(p'=p\)

也就是说，\(e'\) 和 \(e\) 都是同一个进程 \(p\) 的事件。

论文考虑从 \(C_0\) 出发的某个 finite deciding run，在这个 run 中 \(p\) 不执行步骤。令对应 schedule 为 \(\sigma\)，并令：

\[
A=\sigma(C_0)
\]

根据 Lemma 1，\(\sigma\) 可以应用到 \(D_0\) 和 \(D_1\)，并分别得到：

\[
E_i=\sigma(D_i)
\]

且 \(E_i\) 是 \(i\)-valent。

同时，因为 \(p\) 没有参与 \(\sigma\)，事件顺序仍可按 Lemma 1 交换，论文得到图 3 中的关系，并推出：

\[
A
\]

是 bivalent。

但是 \(\sigma\) 是 deciding run，所以到达的 \(A\) 已经有 decision value，因此 \(A\) 必须是 univalent。

矛盾。

两种情况都矛盾，所以假设不成立。因此 \(\mathcal{D}\) 中必须包含 bivalent configuration。

## 10. 构造 admissible 但 nondeciding 的无限 run

有了 Lemma 2 和 Lemma 3，论文开始构造最终矛盾。

由 Lemma 2，存在一个 bivalent initial configuration：

\[
C_0
\]

从 \(C_0\) 开始构造无限 run。

论文维护一个进程队列，初始顺序任意。同时，configuration 中的 message buffer 按消息发送时间排序。

每一个 stage 包含一个或多个进程步骤。

每个 stage 的结束条件是：队首进程执行一步。如果该进程在 stage 开始时有消息队列，那么它收到自己最早的那条消息。然后这个进程被移动到队尾。

这样，在无限多个 stage 中：

- 每个进程都会执行无限多步；
- 每条发给它的消息最终都会收到。

所以构造出来的 run 是 admissible。

现在还要保证它永远不决定。

假设某个 stage 开始时 configuration \(C\) 是 bivalent，队首进程是 \(p\)。

令 \(m\) 是当前 buffer 中发给 \(p\) 的最早消息；如果没有这样的消息，则令 \(m=\varnothing\)。

令：

\[
e=(p,m)
\]

根据 Lemma 3，存在一个 bivalent configuration \(C'\)，它可以从 \(C\) 通过某个 schedule 到达，并且这个 schedule 的最后一个事件是 \(e\)。

于是论文把这段 schedule 作为当前 stage。

这样每个 stage 都从 bivalent configuration 开始，并以 bivalent configuration 结束。

因此整个无限 run 中，每个 stage 结束时仍然是 bivalent。

如果某个进程进入 decision state，那么 configuration 就已经有 decision value，不能继续作为 bivalent configuration 出现在这个构造中。

所以这条 run 永远没有进程决定。

但这条 run 又是 admissible。

这和假设“每个 admissible run 都是 deciding run”矛盾。

因此协议 \(P\) 不可能 totally correct in spite of one fault。

主定理成立。
