# 人工智能（AI）简介

## 究竟什么是AI？

“AI”这个词我们听了太多遍，以至于可能都忘了去探究它的真正含义。到底什么是AI？要解释清楚，我们得先回顾一下历史。

过去，要让计算机完成任务——无论是显示一条朋友圈，让我们在购物网站上买东西，还是计算税款——我们都必须通过编写明确的指令来“编程”。也就是说，我们人类首先要自己解决问题，然后把一步步的解法告诉计算机。

而有了AI，情况就不同了。我们不再是先自己解决问题，而是通过给机器提供大量的范例或数据，来“教”会机器如何解决问题。

我经常举一个简单的例子：假设我们要开发一个根据房屋面积预测房价的应用。按照传统方式，我们可以自己写一个公式，比如 `房价 = 房屋面积 × 30000`。但如果换成AI的方式呢？我们不再自己编写公式，而是给计算机展示数百万个房屋面积和对应价格的数据点。计算机会从这些数据中自行寻找规律，它可能会发现：“哦，原来房价不只是面积乘以3万，可能更接近于面积乘以1.5万，并且还要考虑楼层、地段等其他因素。” 即使是人类专家，在看过数百万个案例后，也能提出比简单猜测更精准的公式，但问题是，人脑无法高效处理如此海量的数据。

所以，**AI的核心就是教会计算机从大规模数据或范例中学习**。在那些需要分析数据来解决问题的场景中，AI表现得异常出色。再以社交媒体的内容推荐为例：没有AI，我们可以让运营人员手动决定给你看什么内容；而有了AI，系统可以分析你的浏览历史，并基于这些数据，智能地为你推荐你可能感兴趣的内容。

## 什么是AI模型？

我想进一步明确“AI模型”的定义。到目前为止，我们只是宏观地讲，AI是教会计算机从大数据中解决问题。

那么，这背后到底发生了什么？**纯粹是数学**。别被“数学”吓到，因为一个数学函数，无非就是接收一个输入，然后产生一个输出。比如，有的函数能将输入值翻倍，输入2，就输出4（y = 2x）；有的函数能将输入值取反，输入2，就输出-2（y = -x）。这些都是数学函数。

回到我们预测房价的例子：我们不再自己编写 `房价 = f(面积)` 这个数学函数，而是给计算机展示数百万个“房价”与“面积”的配对数据，让它自己找出这个数学公式。

**这个由计算机自己找到的数学公式，就是AI模型。**

所以：
-   一个预测房价的AI模型，就是一个以房屋面积为输入、房价为输出的数学函数。
-   一个为你的社交动态推荐内容的AI模型，是另一个以你的浏览行为为输入、推荐内容为输出的数学函数，它专门在海量的用户浏览数据上训练而成。

这些函数不是由人预先写死的，而是计算机通过分析数百万个数据样本自己“悟”出来的。

在实践中，过程会更精确一些：通常是人类先选择一个基础的函数形式。例如，我们知道房价和面积大致成正比（房子越大，通常越贵），所以我们可以选择一个线性函数 `y = ax + b` 作为基础模型。然后，计算机通过分析所有“房价-面积”的数据对，来确定 `a` 和 `b` 的最佳取值（这两个值被称为**参数**）。之前我们可能只是猜测 `a=3, b=0`，但有了AI，我们可以让它通过学习海量数据，找出最精准、最合适的 `a` 和 `b`。

## AI的主要类型

从大数据中“教”计算机学习，主要有三种方法：

**监督学习（Supervised Learning）、无监督学习（Unsupervised Learning）和强化学习（Reinforcement Learning）。**

### 1. 监督学习

监督学习是训练AI模型最常见、最基础的方法。我们通过给机器提供一系列**带有标签的数据**来教它解决问题。注意，是“带有标签的”数据。

以房价预测器为例，为了训练模型，我们提供给计算机一系列的“输入/输出”对，即“房屋面积”和对应的“房屋价格”。所有数据都是标记好的，比如：“对于这个面积，价格是这么多”。我们拥有数百万条这样的数据。计算机会不断尝试不同的公式，并将结果与这些带标签的数据进行比对，直到找到一个最完美的公式。这个过程就是我们所说的“训练”AI或机器学习模型——这也是为什么训练AI模型成本高昂的原因，因为它需要在海量数据集上不断进行尝试和修正。一旦训练完成，我们就拥有了一个能根据面积预测房价的AI模型。

同理，如果我们想构建一个能识别猫和狗图片的AI模型（一个数学函数），我们就需要给它提供数百万张已经明确标好“这是猫”或“这是狗”的图片。因为所有数据都经过了人工标注，所以这种学习方式被称为**监督学习**。我们人类完成了标记数据的艰苦工作，从而像导师一样“监督”着机器从这些数据中学习。

![image](https://github.com/user-attachments/assets/4c1b4c17-b1f6-4070-b528-689c6b681668)

### 2. 无监督学习

无监督学习的数据则只有输入，没有预设的输出或标签。我们不再试图预测某个结果（比如一封邮件是否为垃圾邮件），而是单纯地在数据中**发现隐藏的模式或结构**。

购物数据是理解无监督学习最直观的例子。计算机可以从海量的购物记录中发现规律，并得出结论：购买面包的顾客通常也会购买牛奶。这些数据没有标签，仅仅是用户的购买行为记录，例如“顾客A买了鸡蛋，顾客B买了燕麦，顾客C买了牛奶”。模型从这些原始数据中自行发现了潜在的关联。

所以，两者的区别可以总结为：
-   **监督学习**：“这里有房屋的面积和对应的价格，请学习如何根据面积预测价格。”
-   **无监督学习**：“这里有所有的购物数据，请从中找出有趣的规律。”

在电商网站上，“购买了X商品的顾客也购买了Y商品”这样的产品推荐，就是无监督学习模型的典型应用。下次你在视频网站上看到“观看此影片的用户也喜欢……”时，你就知道，你正被无-监督学习模型“包围”着。

![image](https://github.com/user-attachments/assets/c5538fab-6499-4179-be97-b74a1761fd0e)

### 3. 强化学习

强化学习既不依赖有标签的数据，也不只是在无标签数据中寻找模式，而是通过**试错（Trial and Error）** 来学习。这个过程非常像训练宠物。当狗狗做对了某个动作，你就给它一点零食作为奖励。久而久之，狗狗就学会了哪些行为能带来奖励，哪些不能。

在强化学习中，AI模型（我们称之为“智能体”或Agent）会采取一系列行动，并根据这些行动的结果获得“奖励”或“惩罚”。例如，一个AI象棋程序就是通过强化学习训练的。它下了数百万盘棋，对于导致胜利的棋步，它会获得正向奖励；对于导致失败的棋步，则获得负向惩罚。经过长期训练，机器甚至能发现许多人类棋手从未考虑过的策略。

自动驾驶是另一个例子。当自动驾驶汽车成功保持在车道内、维持安全车距或顺利到达目的地时，它就会获得奖励。

当然，我们说的“奖励”，并不是真的给计算机一块糖。奖励只是一个数值，它告诉算法某个行为是好是坏。正数代表好的行为，负数代表坏的行为。

通过强化学习，我们不是预先给AI一个数据集去研究，而是将它置于一个真实或模拟的环境中。它在这个环境中采取行动，观察结果，并根据结果获得奖励或惩罚，从而不断优化自己的行为策略。

像ChatGPT这样的大语言模型，其训练过程也融合了多种方法。它首先通过监督学习，在数百万计的带有标签的文本数据（如书籍、网站）上进行预训练。然后，它会利用强化学习进一步优化：模型针对一个给定的提示生成多个可能的回答，然后由人类评估员或另一个AI系统对这些回答的帮助性或准确性进行评分。模型将这些评分作为奖励信号，来学习如何生成更优质的回答。

![image](https://github.com/user-attachments/assets/d72c893d-1390-4583-8007-50aa0c6bf6d1)

## AI为何在今天成为主流？

你知道吗？AI的概念自20世纪50年代就已存在。问题是，为什么它直到最近才突然成为主流？我们早就有了谷歌翻译（这是AI），信用卡欺诈检测（这也是AI），社交媒体的内容推荐算法也存在已久。为什么AI现在才迎来爆发？

当然，像ChatGPT这样的产品的病毒式传播是原因之一，但更深层次的原因在于，AI的成功依赖于三大支柱的成熟：

1.  **算法（Algorithms）**
2.  **算力（Computers）**
3.  **数据（Data）**

像OpenAI这样的公司所做的，是创造出**通用目的模型（General-Purpose Model）**，并将其开放给公众使用。这些模型对自然语言的理解能力达到了前所未有的高度。事实证明，能够深刻理解自然语言的模型具有极强的通用性，可以被用来解决各种日常问题，比如编写代码、提供客户支持等等。

在过去，你需要组建一支由机器学习工程师组成的庞大团队，并投入巨额资金购买庞大的计算基础设施，才能训练出属于你自己的AI模型。

而现在，情况完全变了。**几乎每家公司都有机会成为一家AI公司**。
