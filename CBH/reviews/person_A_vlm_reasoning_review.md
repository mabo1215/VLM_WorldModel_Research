### Review #1

**Paper Title:** A Survey on Hallucination in Large Vision-Language Models

**Venue / Year:** arXiv 2024

**Main Problem:** LVLM 在实际应用中存在严重的“幻觉”问题，即生成的文本与图像事实内容不一致，影响其可靠性。

**Core Method:** 这是一篇综述论文，系统性地梳理了 LVLM 幻觉的概念定义、评估方法、成因分析（数据、视觉编码器、模态对齐、LLM）和缓解策略。

**Model Architecture:** 综述涵盖的 LVLM 通用架构：视觉编码器（CLIP ViT）+ 连接模块（Q-Former/MLP/Linear）+ LLM（Vicuna/LLaMA）

**Dataset:** POPE (3000样本), NOPE (17983), CIEM (72941), M-HalDetect (4000), GAVIE (1000), FAITHScore (2000), HaELM (5000), MMHal-Bench (96), AMBER (15202)

**Evaluation Metric:** Accuracy, FAITHScore, Reward Model Score, AMBER Score, 人工评分

**Main Result:** 幻觉在 LVLM 中普遍存在；主要原因包括数据偏差、视觉编码器分辨率限制、模态对齐不足、LLM 上下文注意力不足；现有评估方法分为判别类（是/否问答）和生成类（自由描述分析）。

**Limitation:** 
1. 主要聚焦对象幻觉，对关系幻觉（空间推理）分析较少
2. 对时序变化、动作后果等动态场景的幻觉分析缺失
3. 是综述而非方法论文，没有提出新模型或新数据

**Relevance to our paper:** 
与候选方向 A（VLM 世界状态理解）高度相关。论文明确指出 LVLM 存在**关系幻觉**（如物体间空间位置判断错误），这正是我们要研究的空间关系理解问题。论文中提到的 AMBER 基准同时评估对象/属性/关系，可作为实验设计的参考。

**Can be used as baseline?** No

### Review #2

**Paper Title:** A Comprehensive Survey and Guide to Multimodal Large Language Models in Vision–Language Tasks

**Venue / Year:** Computation Journal (MDPI), 2026

**Paper Link:** https://www.mdpi.com/2079-3197/14/6/125

**Main Problem:** MLLM 领域发展迅速但缺乏系统性教程，新人难以上手；现有综述缺少定量模型对比、设计空间分析和系统化的失败模式文档。

**Core Method:** 这是一篇教学导向的综述，提供从 NLP 基础到 MLLM 架构、训练、应用的全流程指南；包含 15 个模型的统一基准对比（Table 4）、设计空间覆盖矩阵（Table 19）、失败模式分析（Table 18）和开放问题讨论。

**Model Architecture:** 综述覆盖的 MLLM 架构：视觉编码器（CLIP/SigLIP/DINOv2/InternViT）+ 连接器（Q-Former/MLP/Cross-Attention）+ LLM（LLaMA/Qwen/InternLM）

**Dataset:** VQAv2, GQA, TextVQA, MMBench, MMMU, POPE, COCO, Flickr30K, MS COCO 等

**Evaluation Metric:** Accuracy, CHAIR, POPE precision/recall, MMBench score, MMMU score

**Main Result:** 2024 年开源模型在感知基准上接近/超越部分闭源模型；MLP 连接器在准确性关键任务上优于 Q-Former；MMMU 等推理任务上开源与闭源仍有差距；空间推理、物体幻觉、多图比较、长视频时序理解是当前主要失败模式。

**Limitation:** 
1. 是综述而非方法论文，没有提出新模型
2. 基准对比不是受控实验（闭源模型未披露训练数据/架构细节）
3. 部分分析（如效率对比）缺乏标准化报告框架

**Relevance to our paper:** 
与候选方向 A（VLM 世界状态理解）高度相关。论文第 5.2.4 节专门分析了空间推理错误，指出：ViT 的 1D 位置嵌入 + 连接器 token 压缩 + 空间监督数据不足 是三大架构原因。这直接支持我们要研究的**空间关系理解**问题。论文还提供了模型对比基准（Table 4）和失败模式分类（Table 18），可作为我们实验设计和 gap 分析的参考。

**Can be used as baseline?** No

### Review #3

**Paper Title:** Data Selection Matters: Towards Robust Instruction Tuning of Large Multimodal Models (ARDS)

**Venue / Year:** NeurIPS 2025 (arXiv 2025)

**Paper Link:** https://proceedings.neurips.cc/paper_files/paper/2025/hash/0d77ccb50a558035f19089096f933e8e-Abstract-Conference.html

**Main Problem:** LMMs 在视觉指令微调后存在严重的脆弱性——当输入受到轻微扰动（如选项顺序打乱、符号替换）时，准确率大幅下降（ScienceQA 下降 32%），原因是数据集存在**位置偏差**和**符号-内容虚假关联**。

**Core Method:** 提出 ARDS（Adversarial Representation-based Data Selection），一种无需梯度的鲁棒数据选择框架。步骤：(1) 提取对话向量（注意力加权聚合）建立向量数据库；(2) 通过层次聚类 + 双扰动（图像扩散噪声 + 文本符号/排列攻击）构建最差情况评估子群；(3) 选择与这些子群语义最相似的训练样本，构成鲁棒训练混合集。

**Model Architecture:** LLaVA-1.5 (7B/13B)：CLIP ViT-L/336 + MLP 投影器 + Vicuna v1.5

**Dataset:** LLaVA-665K 训练集；评估：ScienceQA, SEED-Bench, MMBench, GQA, A-OKVQA, MMMU, TextVQA, SocialIQA, MathVista

**Evaluation Metric:** Clean accuracy + Robust accuracy（符号攻击 SA + 排列攻击 PA）

**Main Result:** 仅用 30% 训练数据，ARDS 在 ScienceQA 上鲁棒准确率比 LESS 高 20.62%，比 Full-data 高 10.33%；在 GQA-OOD（视觉子群体偏移）上达到最高准确率（58.84%）；选择的数据子集可迁移到 LLaVA-Mistral 和 Qwen2.5-VL。

**Limitation:** 
1. 主要针对位置偏差和符号-内容虚假关联，未覆盖所有偏差类型
2. 需要代理模型构建向量数据库，仍有一定计算成本
3. 对视频任务未验证

**Relevance to our paper:** 
与候选方向 A（VLM 世界状态理解）有间接但重要的关联。论文揭示了 VLM 的**位置偏差**——模型可能依赖选项顺序而非真正理解问题。这对空间关系理解有启发：如果模型依赖“通常左边的是较小的物体”这类虚假关联，而非真正理解空间关系，那么评估时需要设计消除位置偏差的测试协议。论文的**扰动评估方法**（排列攻击、符号攻击）可迁移到空间推理任务的鲁棒性测试。

**Can be used as baseline?** Yes
**If Yes, which task/scenario?** ARDS 本身是数据选择方法，不是模型。但其**评估协议**（Permutation Attack + Symbol Attack）可作为你的鲁棒性测试基线；论文使用的 LLaVA-1.5 可作为模型基线。