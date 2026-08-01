# FedPKDA 论文分享

Personalized Federated Learning with Privacy-Preserving Knowledge Dynamic Alignment

---

## 📑 目录

### 正文

- [基础背景：联邦学习基础概念](/docs/basic_background.md#联邦学习基础概念)
  - [1.1 联邦学习的定义与背景](/docs/basic_background.md#11-联邦学习的定义与背景)
  - [1.2 联邦学习的基本工作流程](/docs/basic_background.md#12-联邦学习的基本工作流程)
  - [1.3 联邦学习的典型架构](/docs/basic_background.md#13-联邦学习的典型架构)
- [现有问题：现有问题与痛点](/docs/problems.md#现有问题与痛点)
  - [2.1 数据异构性问题（Non-IID）](/docs/problems.md#21-数据异构性问题（non-iid）)
  - [2.2 隐私泄露风险](/docs/problems.md#22-隐私泄露风险)
  - [2.3 本地表示偏差](/docs/problems.md#23-本地表示偏差)
  - [2.4 现有方法的局限](/docs/problems.md#24-现有方法的局限)
- [FedPKDA 方法全流程](/docs/fedpkda.md#fedpkda-方法全流程)
  - [3.1 总体框架概览](/docs/fedpkda.md#31-总体框架概览)
  - [3.2 模块一：跨客户端原型隐私保护](/docs/fedpkda.md#32-模块一-跨客户端原型隐私保护)
  - [3.3 模块二：服务器端几何滤波](/docs/fedpkda.md#33-模块二-服务器端几何滤波)
  - [3.4 模块三：客户端知识动态对齐](/docs/fedpkda.md#34-模块三-客户端知识动态对齐)
  - [3.5 模型聚合](/docs/fedpkda.md#35-模型聚合)
  - [伪代码](/docs/fedpkda.md#伪代码)
- [总结](/docs/summary.md#总结)
  - [背景](/docs/summary.md#背景)
  - [挑战](/docs/summary.md#挑战)
  - [方案](/docs/summary.md#方案)
- [数学原理](/docs/math.md#数学原理推导)
  - [A. 差分隐私 ε 界证明](/docs/math.md#a-差分隐私-ε-界证明)
  - [B. 马氏距离滤波的数学推导](/docs/math.md#b-马氏距离滤波的数学推导)
  - [C. φ(t) 严格单调递增性证明](/docs/math.md#c-φt-严格单调递增性证明)

### 附录（工具页）

- [时间激活函数 φ(t)](/docs/utils/function.md#时间激活函数)
- [Mahalanobis Distance](/docs/utils/ma_distance.md#mahalanobis-distance)

### 论文

- [论文原文](./FedPKDA.pdf)
