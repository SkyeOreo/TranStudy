原文：[https://blog.daglabs.com/an-introduction-to-the-blockdag-paradigm-50027f44facb](https://blog.daglabs.com/an-introduction-to-the-blockdag-paradigm-50027f44facb)

## An Introduction to the BlockDAG Paradigm

## BlockDAG 范式入门

*Contrary to popular belief, using a DAG (directed acyclic graph) as a distributed ledger is not about removing proof-of-work mining, blocks, or transaction fees. It is about leveraging the structural properties of DAGs to potentially solve blockchain’s orphan rate problem. The ability of a DAG to withstand this problem and thus improve on scalability is contingent on the additional rules implemented to deal with transaction consistency, and any other design choices made.*

*与大众的普遍观点相反，将 DAG（有向无环图）用作分布式账本并不会消除工作量证明式的挖矿，也不会消除区块和交易手续费。它的目的是利用 DAG 的结构特性帮助解决区块链的孤儿率问题。DAG 解决这个问题并且改进可扩展性的能力取决于为了处理交易一致性而实现的额外规则，以及其它设计上所作的选择。*

### Directed acyclic graphs

### 有向无环图

A DAG is not a novel concept or technology, and it is definitely not a consensus mechanism; it is purely a mathematical structure originating from centuries ago. Technically, a DAG is a graph with directed edges and no cycles (i.e., there is no path from a vertex back to itself).

DAG 并不是一个新概念或是技术，并且它当然也不是一个共识机制；它纯粹是起源于几个世纪前的一种数学结构。在技术上，DAG 是一种包含有向边但不含环（即没有一条可以从一个顶点出发然后又回到它自己的路径）的图。

![DAG](https://cdn-images-1.medium.com/max/1600/1*aYmhytXcO6yxNpatmPwIIA.png)

In the context of distributed ledgers, a blockDAG is a DAG whose vertices represent blocks and whose edges represent references from blocks to their predecessors. Evidently, in a blockDAG, blocks may have several predecessors instead of just one; this will be described in more detail below. First, let us recall the orphan rate problem.

在分布式账本的环境中，blockDAG 一种有特殊含义的 DAG。在这种 DAG 中，顶点代表区块，边代表区块对它们父辈的引用。显然，在 blockDAG 中，区块可能有多个父辈，而不是只有一个；下文会对此进行详细描述。首先，让我们回顾一下孤儿率问题。

### Blockchain’s orphan rate problem

### 区块链的孤儿率问题

There are many barriers to blockchain scalability, including processing speeds, disk I/O, RAM, bandwidth, and syncing new nodes. While these limitations can be addressed with improved hardware and technology, a major barrier exists on the protocol level: the orphan rate. Orphans are blocks that are created outside the longest chain due to unavoidable network propagation delays.

有许多因素制约着区块链的可扩展性，包括处理速度、磁盘 I/O、RAM、带宽，以及新节点的同步（译注：即新节点刚加入网络时将整条区块链下载同步到自己本地的过程）。虽说可以通过改善硬件和技术来突破这些限制，但一个最主要的瓶颈是在协议层面：孤儿率。孤儿是指由于不可避免的网络延迟而在最长链以外创建的区块。

Accelerating block creation and/or increasing block sizes increases the orphan rate: by the time a new block propagates throughout the network, other new blocks which do not reference it are likely to be created. It is well established that a high orphan rate amounts to compromised security; the more honest blocks that end up outside the longest chain due to spontaneous forks, the less secure is the chain. ^1

加快区块创建速度以及／或者增大区块大小会增大孤儿率：在一个新区块传播遍网络之前，很可能会有其它新的不引用它的区块被创建出来。孤儿率增大会导致安全性降低，这是公认的；如果有越多的诚实区块因为自然分叉的原因导致自己不在最长链中，那么这条链就越不安全。^1 （译注：这里的自然分叉是指由于协议本身的特性和网络延迟导致的分叉，而非攻击导致的分叉。在最长链规则下，分叉越多，攻击者为了使自己成为最长链而需要的算力就越小，因此最长链就越不安全。比如在有三条分叉的情况下，攻击者只需超过全网三分之一的算力即可攻占最长链。因此从协议本身的角度来讲，自然分叉越少越好。）

Blockchain protocols typically impose a maximum block size and constant block creation rate to accommodate the network propagation and minimize orphans. This artificial limit of transaction throughput and lower bound on latency (in Bitcoin’s case, to 3–7 transactions per second and tens of minutes confirmation times) is a hard pill for blockchains to swallow — while it impedes on-chain scaling, it guarantees that spontaneous forks and orphans are rare and, therefore, that the main chain is secure. DAG protocols, however, may deal with orphans in other ways.

区块链协议一般会规定一个最大区块大小和一个常数的出块率从而应对网络延时并且将孤儿的数量降到最低。这种对交易吞吐量和等待时间下限的人工限制（在比特币的情况下，这个限制是每秒3至7个交易和数十分钟的确认时间）对区块链来说是一副苦药丸——它阻止了链上扩容，但保证了自然分叉和孤儿极少出现，因此主链是安全的。然而 DAG 可以通过其它方式处理孤儿问题。

### The blockDAG paradigm

### blockDAG 范式

The notion of a fork is organically absorbed in the DAG framework, so it seems worthwhile to consider if a DAG could do better than the chain/linked list structure of blockchains. Accordingly, with Satoshi’s proof-of-work system as the starting point, we need to make one change to the mining protocol in order to yield a blockDAG: blocks may reference multiple predecessors instead of a single parent. A canonical way to extend the ledger is to have blocks reference all tips of the graph (that their miners observe locally) instead of referencing the tip of the single longest chain, as in Satoshi’s original protocol.

DAG 框架有机地吸收了分叉这一概念，所以看上去 DAG 是否可以比链式／链表结构的区块链做得更好这件事情是值得我们考虑的。于是，以中本聪的工作量证明系统为基础，为了生成一个 blockDAG，我们需要对挖矿协议做出一个改变：区块可以引用多个父辈，而非一个单一的父亲。一种典型的扩展账本的方式是让区块引用（产生区块的挖矿者在本地能看到的）图中的所有末端，而非依照中本聪的原始协议只引用最长链的末端。

![blockDAG](https://cdn-images-1.medium.com/max/2000/1*YJgJTzHlnrXrDU_ddsWtAA.png)

In a canonical blockDAG ledger, new blocks reference all tips of the graph (blocks that have not yet been referenced) that their miners see locally. As in a blockchain, blocks are published immediately.

在一个典型的 blockDAG 账本中，新区块引用它们的挖矿者在本地看到的图中的所有末端（即还未被引用的区块）。和在区块链中一样，区块会被立即发布。

However, unlike a blockchain which, by construction, preserves consistency (every block in the chain adds transactions that are consistent with its predecessors in the chain), a blockDAG incorporates blocks from different “branches” and so may contain many conflicting transactions. Because of this, a DAG, or blockDAG, cannot be considered a “solution” or “novel approach” or “new protocol” in and of itself. Instead, a blockDAG is a framework for devising consensus protocols that may (or may not) be as secure as and more scalable than chain-based protocols.²

然而，和区块链不同的是，区块链在构建时会一直维护着一致性（链中的每个区块添加的交易都与链中的父辈一致），而 blockDAG 包含了来自不同“分支”的区块，所以可能包含许多冲突交易。因此，DAG 或是 blockDAG 本身并不能被认为是一个“解决方案”或是“新方法“或是”新协议“。blockDAG 只是一个用来设计比链式协议扩展性更高的共识协议的框架，而所设计出的协议可能（也可能不）具有链式协议同等的安全性。

We therefore need a method to recover consistency; in other words, a blockDAG system requires replacing Satoshi’s longest chain rule with a new consensus protocol.

因此我们需要一种方法来恢复一致性；换句话说，一个 blockDAG 系统需要用一个新的共识协议来取代中本聪的最长链规则。

#### Consensus via ordering

#### 通过排序达成共识

Observe that if a distributed system achieves consensus on the order of all events in it, then we can easily extend this agreement to achieve consensus on the state — we simply iterate over all transactions according to the agreed order, and accept each transaction that is consistent with those accepted so far. This method preserves consistency by construction.

如果一个分布式系统对系统中所有事件的顺序达成共识，我们就可以很容易地将这个共识扩展到状态的共识——只需要简单地按照达成共识的顺序遍历所有交易，并且接受那些与已经被接受的交易一致的交易。这种方法在构建区块链的过程中就一致维护着一致性。

We are left with the task of defining an ordering protocol on all events in the system — in our context, on all blocks in the DAG — in a way that will be agreed upon, eventually, by all nodes.

我们现在要做的就是为系统中的所有事件——在我们的上下文中，就是 DAG 中的所有区块——定义一个排序协议。所有节点都会遵从这个协议从而在区块顺序上最重达成共识。

The natural topology of a DAG already induces a partial ordering over the blocks: if there is a path from block X to block Y in the DAG, then block Y was provably created before block X and so should precede X in the global order, and vice versa. Thus, we only need to define an order over blocks created in parallel, i.e. any set of blocks such that there is no path that connects them.

DAG 的自然拓扑本身就已经为区块引入了偏序排序：若在 DAG 中有一条路径从区块 X 到区块 Y，即可证明区块 Y 是在区块 X 之前被创建的，因此在全局排序中应该排在 X 前面，反之亦然。因此，我们只需要为并行创建的区块，也就是相互之间没有路径相连的区块所组成的集定义一种顺序。

This paradigm began with the blockDAG-based protocols developed out of the Hebrew University ([Inclusive](http://www.cs.huji.ac.il/~yoni_sompo/pubs/15/inclusive_full.pdf), [SPECTRE](http://www.cs.huji.ac.il/~yoni_sompo/pubs/17/SPECTRE.pdf), and [PHANTOM](https://eprint.iacr.org/2018/104.pdf)); these protocols each define an algorithm that outputs an order over the DAG’s blocks, iterates the DAG by that order, and eliminates transactions that conflict with previous ones. (Actually, SPECTRE does something slightly weaker, but that’s a topic for a separate blog post.)

这种范式起源于耶路撒冷希伯来大学开发的基于 blockDAG 的协议（[Inclusive](http://www.cs.huji.ac.il/~yoni_sompo/pubs/15/inclusive_full.pdf)、[SPECTRE](http://www.cs.huji.ac.il/~yoni_sompo/pubs/17/SPECTRE.pdf)，和 [PHANTOM](https://eprint.iacr.org/2018/104.pdf)）；这些协议分别定义了自己的算法，每个算法都对 DAG 中的区块进行排序，并按顺序遍历 DAG，消除与已经被遍历过的交易相冲突的交易。（实际上，SPECTRE 做的事情弱一些，不过这需要另写一篇单独的博文来论述。）
