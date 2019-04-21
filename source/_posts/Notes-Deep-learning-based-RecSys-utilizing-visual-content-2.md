---
title: Notes-Deep learning based RecSys utilizing visual content(2)
date: 2019-04-09 16:10:25
tags:
    - RecSys
    - User preference modeling 
---

### 参考文献
[1] Li Z, Zhao H, Liu Q, et al. Learning from history and present: Next-item recommendation via discriminatively exploiting user behaviors[C]//Proceedings of the 24th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining. ACM, 2018: 1734-1743.
[2] Grbovic M, Cheng H. Real-time personalization using embeddings for search ranking at Airbnb[C]//Proceedings of the 24th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining. ACM, 2018: 311-320.

## Learning from History and Present: Next-item Recommendation via Discriminatively Exploiting User Behaviors

#### 介绍

电子商务中，顾客的行为包含了大量信息，包括消费习惯，变化的偏好等，session-based的推荐越来越流行。文章指出现有研究工作仅关注了短期内的行为，忽略了用户的长期偏好和演化过程。提出了Behavior-Intensive Neural Network(BINN)，通过结合用户的长期偏好和当前消费动机对下一件访问的物品进行推荐。模型包含两个组件，其一是基于用户交互用于获得物品统一表示的嵌入方法，其二是基于嵌入物品以及交互序列的判别行为学习，学习目标用户历史偏好和当前动机。图1展示了文章的思想。
![Figure1](https://i.loli.net/2019/04/09/5cac62432b607.png)
与传统基于文本或图片的嵌入方法不同，文章中提出的嵌入方法直接利用用户与物品的交互序列。对于第二个组件，文章中用两个深层网络共同地学习用户的当前动机与历史偏好。最终，候选物品也映射到同一隐空间中，由BINN为用户生成推荐，整体结构如图2所示。
![Figure2](https://i.loli.net/2019/04/10/5cada5c5c1fd3.png)
<!-- more -->
#### 物品嵌入方法(w-item2vec)

w-item2vec通过物品访问序列习得物品间的相似度，从而得到每个物品的向量表示，核心思想是表现出相似吸引性（访问序列相似）的物品往往很相似。受Item2vec启发，w-item2vec也使用了Skip-gram model with Negative Sampling，w-item2vec中的Skip-gram最大化的目标函数如公式1，加入负采样，并将物品在序列中的频度作为采样权重后，最终的目标函数如公式5所示，其中w为物品的向量表示，v为上下文物品的向量表示。
![F1](https://i.loli.net/2019/04/10/5cad9b789b6e5.png) ![F5](https://i.loli.net/2019/04/10/5cad9b78aceb6.png)

#### Discriminative Behaviors Learning

该组件包含两个基于LSTM的网络，用于学习用户的短期动机和长期偏好。两个网络的输出经过一个全连接层后生成一个d维向量，为下件物品进行推荐。

##### Session Behaviors Learning(SBL)

如图2所示，SBL中的每一个隐状态由上一时刻隐状态、当前物品向量、交互动作(one-hot向量)更新，ts指示session的长度，默认为10，最终SBL的输出为t-1时刻的隐状态。

##### Preference Behaviors Learning(PBL)

该网络试图对用户长期的偏好进行建模，考虑用户历史偏好物品（添加购物车、购买、收藏等）。由于长期偏好波动不大，因此SBL的网络结果不适用于PBL，PBL采用了类似双向LSTM的结构，更新方式与SBL类似。每个时刻的隐状态由前向和后向的隐状态向量连接得到，PBL最终输出为各时刻的隐状态均值。

##### 模型训练

训练采用均方误差作为损失函数，如公式10所示。生成k维向量v后，在嵌入空间中计算v与其他物品表示的相似度，将最相似的前k个物品作为推荐。

![F10](https://i.loli.net/2019/04/10/5cadae7f86d2f.png)

#### 实验

实验采用了京东和天池数据集。由于加入了频度作为负采样权重，w-item2vec的表现优于item2vec。评测尺度采用Recall@20和MRR@20，根据与其他模型的比较实验结果，强调了对用户长期偏好进行建模的有效性。对于冷启动问题，实验中用预训练的模型fit新用户，从第二个交互开始预测，实验中发现深度学习模型能够较好地面对冷启动现象。此外，在对用户历史偏好建模分析中，利用的历史交互次数越多模型表现越好。

天池数据集：https://tianchi.aliyun.com/competition/entrance/231522/information

## Real-time Personalization using Embeddings for Search Ranking at Airbnb

#### 介绍

文章中的应用场景是双向的，例如在预定民宿时，算法需要同时优化租户与房东的要求。作者同时考虑了用户的短期浏览行为和长期偏好，在个体与类型两个层面做嵌入，分别是利用用户短期的浏览行为对民宿做向量化，以及利用用户的历史预定序列学习用户类型和民宿类型在同一隐空间的向量表示。

#### 方法

##### 民宿嵌入

![Figure3](https://i.loli.net/2019/04/20/5cbad8a48fe17.png)

给定由N个用户的短期浏览行为序列组成的集合S，基于skip-gram模型学习每个民宿的向量表示，由于民宿集合过大，应用负采样方法降低计算复杂度，正向样本对包括用户访问过的民宿和对应民宿在访问序列中的相邻民宿，负向样本对包含用户访问过的民宿以及n个随机采样的民宿。进一步将浏览序列分为两类，一类为以预定结束的浏览序列，另一类是未以预定做结尾的浏览序列。最终预定的民宿可作为全局context，从而第一类浏览序列里的每个民宿不仅预测相邻民宿，还对最终预定的民宿做预测，过程如图1所示。由于用户每次浏览的通常都是位于同一区域的民宿，因此正向样本中相邻的民宿往往在同一区域中，而负向样本序列是随机采样的，不具有这一性质。该不平衡现象将导致次优的局域民宿相似度，为解决该问题，为序列中国的每个民宿添加一个局域随机采样的负样本集合。最终，目标函数如公式5所示。为解决冷启动问题，作者选择3所与新加入的民宿距离最近且类型相同租金相近的有向量表示的民宿，对这三所民宿的向量化表示取平均值作为新民宿的向量表示，此举解决了98%的新民宿表示问题。

![F5'](https://i.loli.net/2019/04/20/5cbadd2e7da2b.png)

##### 民宿类型与用户类型嵌入

![Figure5](https://i.loli.net/2019/04/20/5cbaed84a318d.png)

除短期浏览行为意外，用户的长期偏好对个性化推荐也具有重要意义，但是，用户的订单数据十分稀疏，因此，作者对用户类型和民宿类型进行建模。对于民宿类型划分，根据民宿类型、租金、面积等属性，通过基于规则的方法将每个民宿多对一地映射到某个民宿类型。用户类型的划分也使用类似民宿的基于规则的方法，值得注意的是，民宿类型与用户类型可能随属性改变而变化。为在同一空间表示用户类型和民宿类型，作者将用户类型加入了预定序列，训练过程如图5所示，预定序列由多个预定事件按时间顺序连接组成，预定事件为（用户类型、民宿类型）二元组。训练方法与民宿嵌入类似，同样基于skip-gram模型，目标函数取决于位于滑动窗口中心的元素为用户类型或民宿类型。与民宿嵌入不同的是，该序列已包含跨区域的预定事件，因此无需添加跨区域负采样。

不同于浏览行为仅反映用户偏好，预定事件还包含了房东偏好信息，在训练过程中利用房东的拒绝事件可以在嵌入向量中反映房东的偏好。由于一些类型的民宿对信息不全，历史订单少的用户类型不敏感，作者希望这类民宿与用户类型更近，从而提高未来的预定成功率。因此，训练时添加了一类拒绝事件集合，包含（用户类型，民宿类型）二元组，主要关注预定失败后预定成功的情况。最终用户类型和民宿类型的目标函数如公式8，9所示。

![F8](https://i.loli.net/2019/04/20/5cbaf85bc5477.pngg) ![F9](https://i.loli.net/2019/04/20/5cbaf85bd654e.png)


#### 实验

作者利用得到的民宿向量、用户类型向量和民宿类型向量，与用户近期浏览的民宿（根据不同喜好程度分多个类别）计算相似度，得到多个相似度特征表示，最终将特征加入GBDT搜索排序模型中完成排序。