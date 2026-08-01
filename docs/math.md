# 数学原理推导

> 结论速查：每节只给出关键公式与结论，供展示与答疑使用，详细推导见所引文献。

## A. 差分隐私 ε 界证明

- 机制：对本地类原型注入拉普拉斯噪声（见 [3.2 跨客户端原型隐私保护](./fedpkda.md#32-模块一-跨客户端原型隐私保护)）

  $\tilde{p}\_i^k = p\_i^k + \xi, \quad \xi \sim \text{Lap}(0, b)$

- 拉普拉斯分布的概率密度：

  $\text{Lap}(a|b) = \frac{1}{2b} e^{-|a|/b}$

- 特征裁剪（Clipping）把函数敏感度限制在 $\Delta f$ 内；取噪声尺度 $b = \Delta f / \varepsilon$ 时，拉普拉斯机制满足 **ε-差分隐私（ε-DP）**
- 结论：即使带噪原型被截获，也无法据此还原单个样本，提供**可量化**的隐私保障
- 参考：完整证明见差分隐私经典文献（Dwork et al., 2006）

## B. 马氏距离滤波的数学推导

- 工具页：[Mahalanobis Distance](./utils/ma_distance.md)（公式 + 直觉 + 演示代码）
- K-Means 聚类得到类中心（最小化欧氏距离）：

  $\mu\_k = \arg\min\_\mu \sum\_{i \in I\_k} \|\tilde{p}\_i^k - \mu\|^2$

- 马氏距离衡量各原型对中心的偏离程度：

  $d\_i^k = \sqrt{(\tilde{p}\_i^k - \mu\_k)^\top \Sigma\_k^{-1} (\tilde{p}\_i^k - \mu\_k)}$

  其中 $\Sigma\_k = \text{Cov}(\tilde{P}\_k) + \epsilon I$，$\epsilon = 10^{-6}$ 为数值稳定常数

- 反距离加权聚合：

  $w\_i^k = \frac{1/(d\_i^k + \epsilon)}{\sum\_{j \in I\_k} 1/(d\_j^k + \epsilon)}, \quad p\_G^k = \sum\_{i \in I\_k} w\_i^k \tilde{p}\_i^k$

- 结论：偏离中心越远（噪声越大）→ 距离越大 → 权重越小 → 自动滤除离群/噪声原型

## C. φ(t) 严格单调递增性证明

- 定义：

  $\varphi(t) = \frac{\log(1 + e^t) - \log(1 + e^{-t})}{\log(1 + e^t) + \log(1 + e^{-t})}$

- 证明思路：令 $h = \log(1 + e^t)$，$q = \log(1 + e^{-t})$，求导得

  $\varphi'(t) = \frac{2(h'q - hq')}{(h + q)^2} > 0 \quad (t > 0)$

- 结论：φ(t) 在 $(0, 1]$ 上**严格单调递增**；$t \to 0$ 时 $\varphi(t) \to 0$（偏向本地），$t$ 增大时 $\varphi(t) \to 1$（偏向全局）
- 图像与绘制代码：[时间激活函数](./utils/function.md)

---

[Previous: 总结](./summary.md)
