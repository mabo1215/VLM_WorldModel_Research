## 基本信息
- **Paper Title:** A Survey on Hallucination in Large Vision-Language Models
- **Venue / Year:** arXiv 2024 (华为内部技术报告)
- **Paper Link:** https://arxiv.org/abs/2402.00253

## 核心内容
- **Main Problem:** LVLM 在实际应用中存在严重的“幻觉”问题，即生成的文本与图像事实内容不一致
- **Core Method:** 这是一篇**综述论文**，系统性梳理了 LVLM 幻觉的概念、评估方法、成因和缓解策略
- **Model Architecture:** 综述涵盖的 LVLM 架构包括：视觉编码器（CLIP ViT）+ 连接模块（Q-Former/MLP/Linear）+ LLM（Vicuna/LLaMA）
- **Task Type:** Survey / 综合综述

## 实验设置
- **Dataset(s):** 综述中提及的评估基准：POPE, NOPE, CIEM, M-HalDetect, GAVIE, FAITHScore, HaELM, MMHal-Bench, AMBER
- **Evaluation Metric(s):** Accuracy, FAITHScore, Reward Model Score, AMBER Score, Rating Score
- **Main Result:** 幻觉在 LVLM 中普遍存在；主要原因包括：数据偏差、视觉编码器分辨率限制、模态对齐不足、LLM 上下文注意力不足等

## 关键分析
- **Limitation (至少2点):** 
  1. 这是一篇综述，没有提出新的方法或模型
  2. 主要聚焦于**对象幻觉**，对空间关系、时序变化等复杂推理任务的幻觉分析较少
  3. 发布时间较新（2024），可能未覆盖最新的模型
- **Relevance to our paper:** 
  与候选方向 A（VLM 世界状态理解）高度相关。综述指出 LVLM 在**关系幻觉**（relation hallucination）方面存在问题，这正是我们研究**空间关系理解**的切入点。综述中提到的评估方法（如 POPE、AMBER）可以作为我们设计实验的参考。

## 基线潜力
- **Can be used as baseline?** No（这是一篇综述，不是模型）
- **If Yes, which task/scenario?** 不适用，但综述中提到的 POPE、AMBER 等评估框架可作为我们实验的参考基线

## 附加笔记
- **Research Gap 线索:** 
  - 综述指出当前研究主要关注对象幻觉，对**关系幻觉**（如空间关系、动作后果）的研究不足
  - 这正是候选方向 A 可以切入的点：评估 VLM 对物体间**空间关系**和**因果变化**的理解
- **Key Insight:** 
  - LVLM 的幻觉根源之一是**模态对齐不足**（视觉 token 和文本 token 存在差距）
  - 评估方法分为两类：生成质量评估（non-hallucinatory generation）和判别能力评估（hallucination discrimination）