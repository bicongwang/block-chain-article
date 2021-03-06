![以太坊.jpeg][1]

# 共识机制

在之前的文章中，我讲述了比特币挖矿的原理——**工作量证明**。为了叙述的方便性，我并没有提到**共识机制**这个概念。其实，**工作量证明只是共识机制的一种**。在聊到什么是共识机制之前，我们先看一个著名的分布式系统中的信息一致性问题：**拜占庭问题**。

## 拜占庭问题

维基百科中关于拜占庭问题的描述是这样的：

> 一组拜占庭将军分别各率领一支军队共同围困一座城市。为了简化问题，将各支军队的行动策略限定为进攻或撤离两种。因为部分军队进攻部分军队撤离可能会造成灾难性后果，因此各位将军必须通过投票来达成一致策略，即所有军队一起进攻或所有军队一起撤离。因为各位将军分处城市不同方向，他们只能通过信使互相联系。在投票过程中每位将军都将自己投票给进攻还是撤退的信息通过信使分别通知其他所有将军，这样一来每位将军根据自己的投票和其他所有将军送来的信息就可以知道共同的投票结果而决定行动策略。

![拜占庭问题][2]

这个问题存在的根本原因是没有一个能发号施令的大将军（中心节点），因此每个将军（分布式节点）都需要通过一定的策略来同步信息。在信息同步中，有三个关键问题需要解决：

1. **谁来发送信息（谁负责充当发送方）**：尽管这样的分布式系统中不存在一个发号施令的中心大将军，但是在单次战斗中肯定需要某个（或者某些）将军用来指定策略（进攻或撤退 + 时间），然后发送给其他人。
2. **相信谁的信息（接收方对信息的筛选）**：当某个将军接收到多个信使发来的作战策略，他应该挑选其中一个执行。
3. **如何处理叛徒（虚假信息）**：如果将军中出了一个（或者少量个）叛徒，应该也尽量保证军队行动策略的正确性。

区块链中的共识机制解决了拜占庭问题，**让遵从共识算法的节点们可以在区块链这个分布式网络中达成信息一致**。

## 工作量证明（PoW）与拜占庭问题

让我们看看比特币的挖矿算法（工作量证明）是如何解决拜占庭问题的：

1. **谁来发送信息**：挖到矿的人负责发送信息。本质上，全网上10分钟内只需要一个信息（区块），过多的区块只会让信息一致性的解决更加复杂，因此工作量证明采用了暴力计算的方式提高了信息发送的难度，进而降低了信息发送者的数量。
2. **相信谁的信息**：如果接收到多个区块，节点会存储所有的区块信息，但是会保证自己在最长链上挖矿，使得最终只会有一个信息被接受。
3. **如何处理叛徒**：事实上，节点对于接收到的区块都会进行哈希值校验，来保证基本的信息安全。另外，即便叛徒发送的是可通过校验的区块，最长链挖矿机制也会保证这些区块最终会被淘汰，除非区块链网络里大部分节点都是叛徒（这就是著名的**51%算力攻击**）。

## 新的共识算法：权益证明（Power of Stake）

PoS挖矿最早是2012年8日发布的点点币（PPC）实现的，白皮书是[《一种P2P（点对点）的权益证明（Proof of Stake）密码学货币》](https://peercoin.net/assets/paper/peercoin-paper-cn.pdf)。

PoW的工作原理是让矿工们计算出一个小于X的哈希值，因此计算能力更强的矿工有更大的概率挖到矿，但是这种计算能力的竞争也造成了很大的资源浪费。与PoW用工作量证明挖矿权不同，**PoS使用资产证明自己的挖矿权**。

先讲个重要概念：**币龄**。而**币龄=持币总量\*持币时间**。也就是说，如果李明从韩梅那里收到了10个币，并且持有90天，那么李明就收集到了900币天的币龄。此外，如果李明使用了从韩梅收到的这10个币，我们就认为李明从这10个币上积累的币龄被消耗了。

在PPC的PoS挖矿中，我们**需要通过主动消耗币龄（自己发送给自己一个交易，这等同于有币龄的旧币被销毁，产生了一个无币龄新币）来降低挖矿难度**。

![PoS挖矿][3]

权益输入->权益输出即为主动消耗币龄的过程，而核心输入为挖矿所得。当然，想成功获得这个核心输入，也要像比特币的PoW完成一个产生目标哈希值的任务。

权益消耗地越多，目标哈希值越大，也就越容易产生。消耗币龄多的人仅仅需要完成一个简单任务（如在1~10000里随机产生一个小于9990的数字），而消耗币龄少的人需要完成一个困难任务（在1~10000里随机产生一个小于100的数字）。但是在PoW中所有矿工在某一时刻挖矿难度是相同的。

![有钱人][4]

![穷人][5]

这样的机制用一句话表述就是：**有钱人过着开挂的人生，付出少量努力就能赚到某笔钱，而穷人需要付出更多的努力才能赚到这笔钱**。这种机制最大的好处是降低了挖矿的门槛。在PoW这个算力很足的世界里，没有专业矿机根本无法挖矿，但是在PoS里，挖矿不需要额外消耗计算资源（因为PoS的哈希计算任务远比PoW要简单，不然又和PoW有什么区别呢？），所以可以真正人人挖矿。

但是这样的机制也存在一定的问题：

1. PoS挖矿本质上是让富人拥有了更大的挖矿权，这样富人们自然就不会攻击货币系统（货币价格降低首先损害自身利益）。但是这样的机制导致穷人的攻击成本变得很低，穷人完全可以不断产生序号相同的区块，如果挖到矿正是好事，如果没有挖到也不会消耗币龄，但是造成了大量小概率硬分叉的可能性，对货币系统的安全性提出了挑战。
2. 币龄大的人有更大的挖矿权，这在一定程度上激励大家屯币而非交易。主动的交易会导致自己丧失币龄，用币龄挖矿才能获得更大的收益。

我之所以多次强调PPC，是因为**PoS算法本质上指的是一类算法**。**消耗自身币龄也只是证明权益手段的一种**。事实上，PPC的PoS算法由于提到上述存在的问题，也经常被人认为是一种不可用的PoS算法。但是PPC算法第一次将PoS算法应用于数字货币中，就值得被首次介绍。

## 以太坊：PoW -> PoW + PoS

不同币种所使用的PoW、PoS等共识算法在细节实现上是存在一定差异的。对于以太坊：

- **Ethash：以太坊的PoW算法。**比特币使用的PoW采用的哈希函数是SHA-256，这样的哈希函数需要**纯算力破解**，在全网总算力飙升的时代**人们需要购买昂贵的专用集成电路(ASICs)进行挖矿**。但是Ethash算法使用的是**基于内存的算法**，减轻了算力负担。内存本身价格也比CPU、GPU等低廉很多，
- **Casper：以太坊的PoS算法。**Casper不采用币龄作为权益证明，而是使用**持币量**作为权益证明。这样的机制减少了用户对屯币的渴求，一定程度上促进了交易。另外，Casper采用了一种保证金+下注的机制来进行挖矿，原理较为复杂，这里不过多讨论。

目前以太坊仍然是PoW挖矿机制，原计划在2017年底推进1%区块实现PoS挖矿，如今又有所推迟，应该需要在19年才能看到Casper的真正落地了。

其实，以太坊之前是打算直接从PoW转为PoS的，而非这样的混合机制。但是完全抛弃PoW，矿工们不满意，因为他们手中的计算型矿机将会变成废铁。“精明”的矿工们会从ETH转向众多PoW机制的币去，没有人挖的以太币将会面临危机。这就是信仰与现实的战争。


  [1]: http://blockchain8.tech/usr/uploads/2018/04/506210745.jpeg
  [2]: http://blockchain8.tech/usr/uploads/2018/04/1809860871.png
  [3]: http://blockchain8.tech/usr/uploads/2018/04/473518971.jpg
  [4]: http://blockchain8.tech/usr/uploads/2018/04/3571889520.jpg
  [5]: http://blockchain8.tech/usr/uploads/2018/04/2011783744.jpg  