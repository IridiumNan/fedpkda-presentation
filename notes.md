# note

## 裁剪 (Clipping)

> 原文: the extracted feature vectors are clipped to a fixed range to reduce the risk of sensitive information leakage

裁剪指的是将超过指定膜长的向量化为方向不变， 大小为最大膜长限制的向量。
用于减少信息泄漏

---

## Client-Server 架构

- cilent: 数据在本地, 训练在本地, 传输本地模型的权重或者梯度
- server: 接收多个 client 的权重或者梯度, 更新全局模型, 广播模型

---

## 通信轮数

进行下面这些循环的次数, 每经过一次算一轮

1. 分发模型至各客户端
2. 客户端使用本地数据训练
3. 上传模型更新（梯度/权重）
4. 服务器聚合更新，生成新全局模型(并广播)

---

## 参与率

- 本轮选中的客户端数量 / 参与联邦学习的全部客户端数量

---

## Class Prototype (类原型)

- 什么是类原型？
  简单说，就是把同一类别的所有样本（比如“猫”的所有图片），在模型的特征空间里取一个平均向量。这个原型代表了该类别的核心语义特征，而不是某一张具体的图片。

- 为什么这能保护隐私？
  
  - 信息被高度压缩：一张高清图片可能有上百万个像素点，而一个原型只是一个几百维的向量。你无法从“平均脸”还原出任何一张具体的脸。
  
  - 信息被抽象化：原型提取的是“什么是猫”的共性知识，丢弃了“这只猫耳朵上有块黑斑”这种个体细节。攻击者拿到原型，最多知道“这个客户端有猫”，但不知道具体是哪只猫、长什么样。

- 所以，特征提取本身就已经完成了一次信息泛化和脱敏，大幅降低了原始数据被还原的风险。

- 如何使用 Prototype ?

用于计算 本地的样本和原型之间计算距离
得到 $\mathcal{L}_{LA}$ 本地的损失
以及 $\mathcal{L}_{GA}$ 对于全局损失

---

## Laplacian Noise

- 提供可量化的ϵϵ-DP数学保障，即使原型被截获也无法还原. 是数学上更合理的噪声分布选择

---

## Loss

- step1

$\mathcal{L}_{align} = \omega_{local} \cdot (1 - \varphi(t)) \cdot \mathcal{L}_{LA} + \omega_{global} \cdot \varphi(t) \cdot \mathcal{L}_{GA}$

计算**双对齐**的损失

- step2

$\mathcal{L}_{CE} = - E_{(x,y) \sim D_i}[log {}{p(y|x)}]$

计算**标准的有监督学习的交叉熵损失**

- step3

$\mathcal{L} = \mathcal{L}_{CE} + \mathcal{L}_{align}$

将两个损失相加得到总的损失

---

## FIM (Fisher Information Matrix)

通常通过本地损失函数对模型参数的二阶导数来近似

主要作用是保护 **高密度/稀有** 的本地知识不被平均掉
