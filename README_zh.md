# UniGen-MOE：视频扩散模型中混合专家路由失效的诊断与边界标定
### 一人 · 一卡 · 一种执念：当 MoE 遇上视频生成，我把所有的“坑”都踩了一遍。

<p align="center">
  <a href="https://arxiv.org/"><img src="https://img.shields.io/badge/arXiv-XXXX.XXXXX-b31b1b.svg" alt="arXiv"></a>
  <a href="https://github.com/Shybert-AI/UniGen-MOE"><img src="https://img.shields.io/github/stars/Shybert-AI/UniGen-MOE?style=social" alt="GitHub stars"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="License"></a>
</p>

> **"工业界展示了 MoE 能跑多远，而我标定了这条路每一步的失效边界。"**

这里是一份聚焦于**底层机理**的**深度诊断报告**，而非仅仅追逐性能极限的模型仓库。它源于一位独立研究者对视觉扩散模型混合专家路由的极限探索。

2025 到 2026 年间，各巨头纷纷将 DiT 架构推向 MoE 化时，我作为个人开发者，在一块 **NVIDIA A100（80GB）GPU** 和 **20,000 条开源数据** 的极限资源下，系统性地发现了 MoE 路由器在视频生成训练中的**"选择性锁死"病理**。

---

## 🔭 为什么这很重要

主流叙事告诉我们，混合专家模型是扩展扩散 Transformer 的答案。然而，在工业界基准测试的表象之下，一种系统性的失效模式一直未被诊断——直到现在。

本工作提供：
- **首个完整的路由锁死现象学**，覆盖三种路由器架构和 5,000 个训练步。
- **在真实单 GPU 约束下对 Token-Choice 范式边界的精确标定**。
- **一种全新的理论视角**——功能冗余假说——将锁死重新定义为一种阶段性的、理性的系统行为，而非一个 bug。

---

## 🔬 核心发现

追踪 **30 层 Transformer** 中超过 **6500 万 token** 的每一次 MoE 路由决策，三项发现尤为突出：

### 1. 选择性锁死
在消除全局锁死之后，大约**三分之一的层**将超过 90% 的 token 永久路由给同一个专家。**将辅助损失增大到 0.2 也无法阻止这一现象。**

### 2. U 型分布
锁死并非随机发生。它高度集中于**浅层视觉处理层（L1–L8）** 和**深层语义整合层（L22–L29）**，中间层则保持近乎完美的均衡。

### 3. bfloat16 精度陷阱
在极小初始化和大数值权重区域，优化器的更新量低于 bfloat16 的硬件最小可分辨步长，导致权重被**静默截断为零**——一个极易被忽视的数值陷阱。

<img width="1305" height="468" alt="image" src="https://github.com/user-attachments/assets/0db7f50f-9f49-49bd-813e-6ae6ef3589af" />


---

## 📖 系列故事：《当 MoE 遇上视频生成：一次路由坍塌的完整诊断与修复之旅》

本项目记录一个完整的**"踩坑实录"技术系列**，详细记录了我从 Dense-to-MoE 转换开始，到最终标定 Token-Choice 范式边界的全过程。

| 章节 | 标题 | 核心内容 |
|------|------|----------|
| **第一篇** | [重磅开源！一个模型搞定九种 AI 创作任务](https://zhuanlan.zhihu.com/p/2033547974647739944) | 一切的起点：在 9 合 1 统一框架中首次提出**"MoE 是解决多任务冲突的潜在方向"**。 |
| **第二篇** | [从零开始 Dense-to-MoE 模型权重转换：一个小白的摸索之路](https://zhuanlan.zhihu.com/p/2034196108247814223) | 三种路由器架构的完整尸检：线性门控全局锁死 → MLP 门控短暂蜜月 → 交叉注意力门控自我恢复假象。 |
| **第三篇** | [当 MoE 遇上视频生成：一次路由坍塌的完整诊断与修复之旅](https://zhuanlan.zhihu.com/p/2036215377823257490) | 如何利用混合精度训练与动态专家组策略突破单卡极限，并规划了大规模扩展的完整路线图。 |

> 如需技术发现的英文概述，请参阅我们的 **[arXiv 论文](https://arxiv.org/)**。

---

## 📂 仓库状态：研究预览

> ⚠️ **代码目前暂不公开。** 本项目处于活跃研究阶段，代码库正在为下一阶段（双 GPU 全专家协同训练）进行大幅度重构。重构完成后，代码将整理并开源。

当前你可以：
- **阅读论文**，获取完整的方法论、实验设计和功能冗余假说。
- **关注文章系列**，了解每一个被踩到的坑以及如何填平它们的幕后故事。
- **关注本仓库**——代码和模型权重将在下一阶段实验完成后发布。

---

## 🤝 合作与交流

我目前正在以下方向推进这项工作，欢迎任何形式的交流：

- **资源合作**：寻求算力支持，将诊断方案规模化应用到 7B+ 参数模型，实证定位功能冗余假说所预言的临界点。
- **学术合作**：联合撰写技术报告，与新兴工业范式进行基准对标。

如果你也对视觉 MoE 的底层机理充满好奇，或愿意支持独立研究者的探索，请随时联系我。

---

## 📜 引用

如果您的研究使用了本仓库对应的论文，请引用：

```bibtex
@misc{unigenmoe2026,
  title = {Sparse Mixture-of-Experts Routing in Visual Diffusion Transformers: 
           Diagnosis, Boundary Calibration and Evolutionary Roadmap 
           from Routing Collapse to Selective Deadlock},
  author = {Haiying Sha and Yan Zheng},
  year = {2026},
  howpublished = {\url{https://github.com/Shybert-AI/UniGen-MOE}},
  note = {arXiv preprint}
}
```

同时，也请引用底层基座模型：

```bibtex
@misc{kiwi2026,
  title = {Kiwi-Edit: Versatile Video Editing via Instruction and Reference Guidance},
  author = {Y. Lin and others},
  year = {2026},
  eprint = {arXiv:2603.02175},
}
```

---

## 📜 许可证
本项目基于 MIT 许可证开源。模型权重遵循其原始基座模型（Wan2.2）的开源协议。

---

<p align="center">
  <i>坑填得多了，路就出来了。</i>
</p>
```
