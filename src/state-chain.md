状态通道也算是一种比较热门的扩容方案，状态通道解决方案通过将链下交互和链上清算隔离开，能够在保障一定程度的非中心化和资产安全性的同时，实现速度更快、费用更低的交易。状态通道作为一种链下扩容方案，从一般到特殊分为通用状态通道、状态通道和支付通道三个层次。状态通道面临的主要问题包括路由问题、通道平衡问题、节点离线问题以及保证金锁定问题。

通过设计合理的路由策略可以将状态通道扩展为网络结构，目前的方案有哈希时间锁合约、虚拟通道和元通道。通道平衡问题的主要解决思路有背压路由算法和链下重新平衡方案。节点离线问题的主要解决思路是引入第三方来监督链下状态，并加以经济激励。保证金锁定问题的解决思路是从流动性资金供应商那里吸收闲散资金、共享资金池。

相比于链上扩容方案，状态通道巧妙地将链上和链下的职能分开，采用该方案无需改变公链结构，结合链上的安全性和非中心化特性以及链下的扩展性，但从整体生态来说，由于链上链下通信过程中可能存在的问题，依然属于安全性、非中心化和扩展性的一种新平衡。相比于其他链下扩容方案，状态通道的隐私性较好，可做到即时性，尤其适合于固定双方的高频互动，其劣势在于需要中间节点“垫付”资金、要求节点实时在线或需引入第三方来监督

状态通道（State Channel）是区块链链下（off-chain）扩容方案之一，目前有众多知名项目都采用了状态通道技术方案。

### 一. 状态通道概述

状态通道中的状态指的是区块链账本当前的样子，包括账户名称（或 UTXO 模型的地址）、余额（或 UTXO 模型下的未花费交易输出）、合约数据等。状态通道是在两方或多方之间开辟一条链下通道来进行状态交换，以实现较低的手续费和即时到账等特性。

利用状态通道交易的流程一般为：

- 交易方在链上锁定一定量的资产，在区块链上记录并开辟状态通道；
- 在通道内进行相互交易和状态更新，但不提交到链上；
- 当任一方想要关闭通道时，提交最终状态到区块链上进行清算。若另一方有异议，可在规定时间内申请链上仲裁

状态通道的扩容原理主要是链下交互、链上清算，避免将每一笔小额交易都放在链上进行，只需要把最终的状态提交到链上即可，减轻了链上的工作量。当双方均无异议可以很快完成清算，实现即时终结性（Instant Finality），此外由于在通道内进行链下交易，可实现较快的交易速度、较低的手续费，以及较好的隐私性。

当某一方试图欺诈，提交有利于自己的非最终状态上链时，另一方可以通过提交带有时间戳的最新状态向链上申诉，坐实后欺诈者会面临被没收抵押物的惩罚。但这要求状态通道的用户实时在线，

如果某一方长时间离线，链上清算会强制执行，从而可能造成损失。这种情况下一般会引入第三方来监督是否有欺诈现象发生，但这需要设计出合理的第三方经济激励机制。

状态通道方案比较适合参与方在相对较长的时间里需多次交换状态、对单次转账的交易手续费比较敏感等场景

### 二. 状态通道的三个层级

状态通道作为一种链下扩容方案，从一般到特殊主要分为通用状态通道（Generalized State Channel）、状态通道（State Channel）和支付通道（Payment Channel）三个层级。

![](https://raw.githubusercontent.com/the-web3/layer2/79839bb1ee4b3ca0a345fca240678b111dd64efd/images/5.png)

#### 1 通用状态通道

通用状态通道是模块化、标准化的状态通道开发框架，实例化之后即成为状态通道或支付通道。开发人员无需很熟悉状态通道的相关技术细节也可以很容易地开发基于状态通道的应用，用户也可在不执行任何链上操作的情况下在通道内安装、运行和终止 DApp。

典型的项目有 Counterfactual（基于链下状态通道建立应用的开发架构）、Celer Network（layer-2 扩展平台，帮助开发者快速构建和运行非中心化应用。）。

#### 2 状态通道

状态通道是在链下进行状态交互的通道。通过将链上的图灵完备智能合约放到链下执行，来实现状态交换和链下扩容的目的。例如基于状态通道开发的围棋游戏，就是在链下不断进行状态的交换。

典型的项目有 Perun Network（支持链下智能合约的执行）、FunFair（基于状态通道的对赌游戏）。

#### 3.支付通道

支付通道属于状态通道在支付领域内的具体应用。交易双方在状态通道内可多次进行账务往来，直到通道关闭时上链清算。链下转账快速无手续费，当一方尝试在上链清算时进行欺诈，会有仲裁和惩罚机制来确保双方的资产安全。

典型的项目有闪电网络（BTC 的状态通道支付网络）、Raiden（以太坊版的闪电网络）、Trinity（Neo 版的闪电网络）Liquidity Network（基于 NOCUST 协议的支付通道网络）

### 三. 状态通道存在的问题

#### 1. 路由问题
![](https://raw.githubusercontent.com/the-web3/layer2/79839bb1ee4b3ca0a345fca240678b111dd64efd/images/6.png)

为了将支付通道扩展为支付网络，在已开辟的状态通道上通过路由节点建立两个陌生节点的连接无疑是比较经济的。正如六度人脉理论阐释的那样，我们可以借助六度人脉和世界上其他任何人建立联系。目前主要有如下三种路由方案：

##### 1.1. 哈希时间锁合约（HTLC，Hashed-Time Lock Contract）

代表项目：闪电网络

发送方向接收方发送 BTC 到 1/2 多重签名地址，如果接收方不能在合约规定的时间内通过“签名+哈希密文”的方式将 BTC 解锁，那么 BTC 就会退还给发送方。

![](https://raw.githubusercontent.com/the-web3/layer2/79839bb1ee4b3ca0a345fca240678b111dd64efd/images/7.jpeg)

如图所示，A 想通过 B、C 向 D 发送 1 个 BTC。D 作为接收方，首先生成一个随机私钥 r（相当于钥匙）和公钥 R（相当于锁），然后将 R 发送给 A。

A 和 B 首先搭建了哈希时间锁合约，R 就是其中的“锁”，这里的“时间”是 3 天，“哈希密文”就是只有 D 才知道的私钥 r。这个合约意味着只要 B 能在 3 天之内获得私钥 r 来解锁 R，就可以得到A 在智能合约里发送的 1 个 BTC，否则这 1 个 BTC 就会退还给 A。

B 需要获得私钥 r 来解锁 R，按照同样的思路，B 和 C 的智能合约规定，只要 C 能在 2 天之内获得私钥 r 来解锁 R，就可以得到B 在智能合约里发送的 1 个 BTC，否则这 1 个 BTC 就会退还给 B。

最后 C 和 D 的智能合约也是如此，只要 D 能在 1 天之内用私钥 r 来解锁 R 就可以获得这 1 个 BTC。相当于 D 是用私钥 r 换取了C“垫付”的 1 个 BTC，C 拿到私钥 r 后再用它解锁并换取了 B“垫付”的 1 个 BTC，之后 B 用 r 解锁了 A 的 1 个 BTC。

也就是说，D 首先生成了“锁”和对应的“钥匙”，然后锁依次传到 A、B、C；D 收到 BTC 后，“钥匙”依次传到 C、B、A。这样几个节点无需相互信任就实现了从 A 到 D 的安全转账。实际操作中，中间节点会收到额外的服务费作为经济激励（这也意味着经过的中间节点越多，手续费越高）。

##### 1.2. 虚拟通道（Virtual Channel）

代表项目：Perun Network

![](https://raw.githubusercontent.com/the-web3/layer2/79839bb1ee4b3ca0a345fca240678b111dd64efd/images/8.jpeg)

如图所示，A、B 都已经与 I 开辟了状态通道，分别为通道 X 和通道 Y。现在 A、B 向中间节点 I 通过智能合约发出申请，建立 A 和 B 的虚拟通道 Z（虚线所示）。


A 在 X 通道锁定了 Z（A）个通证，对应的 I 在 Y 通道也锁定了 Z（A）个通证；B 在 Y 通道锁定了 Z（B）个通证，对应的 I 在 X 通道也锁定了 Z(B)个通证。

锁定之后相当于在 A、B 之间建立了一个虚拟通道 Z，A、B 在该通道内对应的可用额度分别是 Z（A）、（B）。A 如果想向 B 转账1 个通证，I 的左边会增加 1 个通证，与此同时右边会减少 1 个通证，减少的 1 个通证转给了 B，达成的效果类似于武侠小说里的“隔山打牛”，A 通过 I 作为“跳板”向 B 转账了 1 个通证。虚拟通道也可以进一步扩展形成更大的网络。

与哈希时间锁合约方案相比，该方案的中间节点无需牵涉到每笔交易中来，只需通过智能合约自动执行，因此更具隐私性和低延迟性，但是需要中间节点锁定足够数量的通证，以为虚拟通道两端提供“中介服务”。比如在上面的例子中，I 节点至少需要锁定 Z（A）+（B）个通证。

##### 1.3 元通道（Meta Channel）

代表项目：Counterfactual

该方案与虚拟通道类似 ， 只是结构稍有差异 。 比如 Counterfactual，是将通用状态通道进行实例化后分别建立 A 和 I、
B 和 I 的代理状态通道，在此基础上建立 A、B 的支付通道，实现和虚拟通道相同的效果。

![](https://raw.githubusercontent.com/the-web3/layer2/79839bb1ee4b3ca0a345fca240678b111dd64efd/images/9.png)

在状态通道网络中，采用传统计算机网络的最短路径的路由策略，会造成通道的不平衡状态，反过来又会使得路由策略失效，寻路遇到“死胡同”便无法有效传递价值。如图表 8 所示 A、B、C 三个节点两两组成的双向状态通道，采用最短路径使得节点在某一通道的可用通证变为 0，双向通道变成了单向通道。左上图是平衡状态，对应左下图所示双向通道。中上图和右上图的通证集中到了一边，分别对应中下和右下的单向通道。

![](https://raw.githubusercontent.com/the-web3/layer2/79839bb1ee4b3ca0a345fca240678b111dd64efd/images/10.png)

Celer Network 提出了背压（BackPressure）路由算法，在每个时间切片内衡量局部网络的拥堵程度和通道失衡度，然后按照背压权重最大的方向进行路由转移，减少网络拥堵的同时实现通道平衡。通过对 77 个节点组成的 254 个状态通道组成的支付网络进行测试后发现，背压路由算法和闪电网络的最短路径路由算法相比，性能提高了 15 倍，通道利用率提高了 20 倍。

Liquidity Network 提出了一个允许在链下重新平衡支付通道的Revive 协议。该协议通过使一组支付通道节点执行一组交易，来重新平衡每个节点在多个通道的通证资产量，防止单向通道形成。如下左图所示的树形网络结构无法做到重新平衡，而 Revive 所采用的右图所示的网络包含环状结构，可以实现节点在多个通道的通证分布从集中重新变得均匀。例如，B 在和 D 组成的通道里有 200 个通证，而在和 E 组成的通道里有 0 个通证，对于右图可以通过（B，D，E，B）环状结构进行重新调配，使得 B 和 D、B 和 E 组成的两个通道里各有 100 个通证。

![](https://raw.githubusercontent.com/the-web3/layer2/79839bb1ee4b3ca0a345fca240678b111dd64efd/images/11.png)

#### 2. 节点离线问题

前文提到状态通道是在链下交互、链上清算，任何一方都可以关闭状态通道实现最终的清算和交割。当双方同时在线且达成一致，可实现即时终结性。但是当一方处于离线状态，或者另一方发起攻击（如 DDoS 攻击、破坏网线等）使其离线，并提交对自己有利的非最终状态给链上，离线方一旦错过仲裁期，最终的清算结果将会对离线方造成资产损失，损害状态通道的公平性、安全性。

这种情况下人们想到通过把监测任务外包给第三方加以解决,例如闪电网络的监督者（Monitors）、Pisa 的看管者（Custodians）、Celer Network 的状态保卫网络（State Guardian Network）。

Monitors 通过举证获得奖励，Custodians 通过质押安全保证金获得监管机会，State Guardian Network 是由状态守护者组成的一个类似于 Plasma 的侧链，CELR 通证（Celer Network 网络中的通证）持有者通过抵押通证成为链下状态守护者，抵押通证越多则被委派以守护链下状态任务的概率越大，从而获得更多收益的概率也越大。

#### 3. 保证金锁定问题

上面提到的路由方案，无论是哈希锁定合约，还是虚拟通道、元通道，都需要中间节点“垫付”资金，或锁定保证金。网络规模越大、平均的转账金额越大，整个网络锁定的保证金也越多，由此产生的机会成本也越高昂。

Liquidity Network 和 Celer Network 都提出了类似于银行的机制来共享流动性，试图通过智能合约从通证持有者处租借通证或支付中心相互“拆借”和共享的方式，减少保证金的锁定。

下图为 Celer Network 的经济模型，主要由流动性资金支持拍卖机制（LiBA，Liquidity Backing Auction）、流动性资金担保挖矿机制（PoLC Mining，Proof of Liquidity Commitment Mining）和链下状态守护者网络三部分构成。其中 PoLC Mining 机制是 Celer 网络流动性资金的关键源头，简单来说就是通过质押担保合约从流动性资金提供商那里收集闲散资金，类似于银行从储户那里获得资金，再通过 LiBA 机制为状态通道服务供应商提供流动性资金，类似于银行放贷。

![](https://raw.githubusercontent.com/the-web3/layer2/79839bb1ee4b3ca0a345fca240678b111dd64efd/images/12.png)

Liquidity Network 基于 NOCUST 的多方支付中心，通过汇聚和共享抵押资金的方式来减少资金锁定。而闪电网络需要对所有的交易进行 100%的抵押，且抵押金相互隔离，流动性较差。

原文链接：http://www.wenwoha.com/20/course_article?act_id=249