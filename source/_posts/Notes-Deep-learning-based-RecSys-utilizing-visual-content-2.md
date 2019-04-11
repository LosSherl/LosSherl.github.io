---
title: Notes-Deep learning based RecSys utilizing visual content(2)
date: 2019-04-09 16:10:25
tags:
    - RecSys
    - User preference modeling 
---

### 参考文献
[1] Li Z , Zhao H , Liu Q , et al. Learning from History and Present: Next-item Recommendation via Discriminatively Exploiting User Behaviors[J]. 2018. (KDD)
[2] 

## Learning from History and Present: Next-item Recommendation via Discriminatively Exploiting User Behaviors

#### 介绍

电子商务中，顾客的行为包含了大量信息，包括消费习惯，变化的偏好等，session-based的推荐越来越流行。文章指出现有研究工作仅关注了短期内的行为，忽略了用户的长期偏好和演化过程。提出了Behavior-Intensive Neural Network(BINN)，通过结合用户的长期偏好和当前消费动机对下一件访问的物品进行推荐。模型包含两个组件，其一是基于用户交互用于获得物品统一表示的嵌入方法，其二是基于嵌入物品以及交互序列的判别行为学习，学习目标用户历史偏好和当前动机。图1展示了文章的思想。
![Figure1](https://i.loli.net/2019/04/09/5cac62432b607.png)
与传统基于文本或图片的嵌入方法不同，文章中提出的嵌入方法直接利用用户与物品的交互序列。对于第二个组件，文章中用两个深层网络共同地学习用户的当前动机与历史偏好。最终，候选物品也映射到同一隐空间中，由BINN为用户生成推荐，整体结构如图2所示。
![Figure2](https://i.loli.net/2019/04/10/5cada5c5c1fd3.png)

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

实验采用了京东和天池(https://tianchi.aliyun.com/competition/entrance/231522/information)数据集。由于加入了频度作为负采样权重，w-item2vec的表现优于item2vec。评测尺度采用Recall@20和MRR@20，根据与其他模型的比较实验结果，强调了对用户长期偏好进行建模的有效性。对于冷启动问题，实验中用预训练的模型fit新用户，从第二个交互开始预测，实验中发现深度学习模型能够较好地面对冷启动现象。此外，在对用户历史偏好建模分析中，利用的历史交互次数越多模型表现越好。
