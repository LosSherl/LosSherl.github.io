---
title: 论文阅读笔记-Deep learning based RecSys utilizing visual content(1)
date: 2019-04-04 10:02:03
tags:
    - RecSys
    - User preference modeling 
---

### 参考文献
[1] Zhou J, Albatal R, Gurrin C. Applying Visual User Interest Profiles for Recommendation and Personalisation.[J]. 2016.
[2] Lei C , Liu D , Li W , et al. Comparative Deep Learning of Hybrid Representations for Image Recommendations[J]. 2016.


### Applying Visual User Interest Profiles for Recommendation and Personalisation

#### 介绍
文献[1]认为用户的兴趣偏好可以从其曾经发布的照片中获取，通过使用深度学习中的技术从图片集中获取用户的视觉偏好信息，并用于酒店推荐及页面个性化定制。

#### 视觉特征提取
在基于内容的图片检索领域中，一些诸如颜色、文本等低级语义信息被用于表示图像以及计算图像相似度，但是这些低级语义并不能很好地表示用户偏好。文献[1]假设通过结合深度学习，对用户特征进行更高语义层面的提取将使得基于内容的推荐更加有效。
文章中的方法利用AlexNet对图像作特征提取，将分类结果（1000个类别的概率分布）作为图像特征。用户偏好档案由多个图像的特征表示组成。

<!-- more -->

#### 利用用户偏好
文章使用余弦距离作图片间的相似度估计，如公式(1)所示。因此，通过计算用户偏好档案中的图像特征向量与其余图像的特征向量的相似度，获得与用户偏好相近的图片。
![.](论文阅读笔记-Deep-learning-based-RecSys-utilizing-visual-content-1/Formula1.png)

#### 应用
应用于酒店推荐以及酒店页面个性化定制。