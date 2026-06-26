### Review #1

**Paper Title:** A Survey on Hallucination in Large Vision-Language Models

**Venue / Year:** arXiv 2024

**Paper Link:** https://arxiv.org/abs/2402.00253

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

### Review #4

**Paper Title:** LLaDA-V: Large Language Diffusion Models with Visual Instruction Tuning

**Venue / Year:** CVPR 2026

**Paper Link:** https://doi.org/10.48550/arXiv.2505.16933

**Main Problem:** 现有的 MLLM 几乎都基于自回归（AR）模型，而**因果注意力**可能不适合处理视觉输入中的**空间关系**。能否用一个**纯扩散模型**（双向注意力）来实现有竞争力的多模态理解？

**Core Method:** 基于 LLaDA（大规模语言扩散模型），集成 SigLIP 视觉编码器 + MLP 连接器，采用**双向注意力**机制。三阶段训练：(1) 语言-图像对齐（LLaVA-Pretrain）；(2) 大规模指令微调（MAmoth-VL 10M 单图 + 2M 多图/视频）；(3) 推理增强（VisualWebInstruct）。

**Model Architecture:** SigLIP-400M/384px（视觉编码器）+ 两层 MLP（投影器）+ LLaDA-8B（扩散语言模型，双向注意力）

**Dataset:** LLaVA-Pretrain（对齐）；MAmoth-VL（SI-10M + OV-2M，指令微调）；VisualWebInstruct（推理增强）

**Evaluation Metric:** MMMU, MME, MMBench, SeedBench, MathVista, AI2D, ChartQA, DocVQA, RealworldQA, MuirBench, MLVU, VideoMME 等

**Main Result:** 
1. LLaDA-V 在纯扩散 MLLM 中达到 SOTA，超过 LLaMA3-V 在 11 个基准上的表现（即使语言塔更弱）
2. 数据缩放实验显示 LLaDA-V 从数据增加中获益
3. **双向注意力**比因果注意力在视觉任务上表现更好（Ablation：No Mask 优于对话因果掩码）
4. attention pattern 分析显示 LLaDA-V 更全局/双向，更擅长捕捉空间依赖

**Limitation:** 
1. 语言塔（LLaDA）本身弱于 LLaMA3-8B 和 Qwen2-7B
2. 在图表/文档理解（AI2D, DocVQA）和真实场景理解（RealworldQA）上落后于 LLaMA3-V
3. 未做偏好对齐（RLHF/DPO），可能影响对话能力

**Relevance to our paper:** 
与候选方向 A（VLM 空间关系理解）**高度相关**。论文核心论点是：**双向注意力比因果注意力更适合捕捉空间关系**，这正是研究方向的理论支持。论文的 attention pattern 分析提供了“为什么 VLM 可能空间推理不足”的架构层面解释——自回归模型的因果注意力限制了全局空间信息整合。LLaDA-V 可作为研究注意力机制对空间推理影响的对比基线。

**Can be used as baseline?** Yes
**If Yes, which task/scenario?** 可作为空间关系理解、视觉推理任务的**非自回归基线**，用于对比自回归模型（如 LLaMA3-V）在空间任务上的表现差异。

### Review #5

**Paper Title:** ITA: Image-Text Alignments for Multi-Modal Named Entity Recognition

**Venue / Year:** NAACL 2022

**Paper Link:** https://aclanthology.org/2022.naacl-main.232/

**Main Problem:** 多模态 NER 中，图像特征（来自 ResNet）和文本特征（来自 BERT）未对齐，注意力机制难以建模跨模态交互；现有方法未充分挖掘文本表示的力量。

**Core Method:** 提出 ITA（Image-Text Alignments），将图像转换为文本空间：(1) 局部对齐：物体检测器提取物体标签+属性；(2) 全局对齐：图像标题模型生成 5 个描述；(3) OCR 对齐：提取图像中的文字。三者拼接为跨模态输入，喂入 BERT/XLMR-CRF。额外提出跨视图对齐（CVA），最小化有/无图像输入的输出分布 KL 散度，提升纯文本场景的鲁棒性。

**Model Architecture:** BERT / XLM-RoBERTa（文本编码器）+ VinVL（物体检测+标题生成）+ Tesseract/PaddleOCR（OCR）+ 线性链 CRF（解码）

**Dataset:** Twitter-15, Twitter-17, SNAP（多模态 NER 数据集）

**Evaluation Metric:** F1 分数（实体级别）

**Main Result:** ITA-All+CVA 在 Twitter-15 上 F1=76.01，Twitter-17 上 86.45，SNAP 上 87.44，超越之前所有 SOTA（UMT, RpBERT, UMGF 等）。CVA 显著提升纯文本输入视图的精度（从 74.79 → 76.01）。使用 XLMR 比 BERT 更强（Twitter-15: 78.25）。

**Limitation:** 
1. 依赖多个外部模型（检测、标题、OCR），推理速度慢
2. 仅针对 NER 任务，不涉及空间推理或通用 VLM 理解
3. 在图像与文本无关时可能引入噪声（CVA 部分缓解）

**Relevance to our paper:** 
间接相关但有一定价值。论文的核心是**图像-文本对齐**问题——将图像信息转换为文本空间以利用 BERT 的注意力机制。这与研究 VLM 的**空间关系理解**有概念上的联系：VLM 也面临视觉-文本对齐问题，而双向注意力（LLaDA-V）vs 因果注意力（LLaMA3-V）的讨论与此类似。论文对**纯文本 vs 多模态输入**的对比（CVA 模块）可为实验设计提供参考。

**Can be used as baseline?** No
**If Yes, which task/scenario?** 不适用（任务领域不同，MNER vs MLLM 视觉推理）

### Review #6

**Paper Title:** SpatialVLM: Endowing Vision-Language Models with Spatial Reasoning Capabilities

**Venue / Year:** CVPR 2024

**Paper Link:** https://arxiv.org/abs/2401.12168

**Main Problem:** 当前 VLM 在 3D 空间推理（如距离估计、大小比较）上能力有限，根本原因是**训练数据中缺乏 3D 空间知识**。

**Core Method:** 设计自动化 3D 空间 VQA 数据生成框架：(1) 用 CLIP 过滤场景图像；(2) 用开源模型提取物体分割、深度、描述；(3) 将 2D 图像提升到 3D 点云并规范化坐标；(4) 用模板生成 38 种定性/定量空间 QA 对。最终在 1000 万张图像上生成 20 亿个 VQA 对，用于训练 VLM（PaLM 2-S）。

**Model Architecture:** ViT（视觉编码器）+ PaLM 2-S（语言模型），基于 PaLM-E 架构

**Dataset:** 合成数据：WebLI 和 VQA 数据集中的 1000 万张图像 → 20 亿空间 VQA 对；评估：人工标注的 546 个空间 VQA 对

**Evaluation Metric:** 定性：人类评估成功率；定量：有效格式输出率 + 半到两倍范围内准确率

**Main Result:** 
1. 定性空间 VQA 准确率显著超越 GPT-4V、PaLI、PaLM-E、LLaVA-1.5 等基线
2. 定量距离估计：99% 输出有效数值，约 50% 在半到两倍真值范围内
3. 解冻 ViT 比冻结效果更好（细粒度估计 +2.8%）
4. 模型能从噪声数据中学习空间常识
5. 解锁链式思维空间推理和机器人密集奖励标注应用

**Limitation:** 
1. 依赖单目深度估计器（ZoeDepth）的精度，在远距离和大场景上误差较大
2. 合成数据基于有限的问题模板，多样性受限
3. 仅针对单图像，未扩展到视频

**Relevance to our paper:** 
**与候选方向 A 完美匹配**。论文直接研究 VLM 的空间关系理解（定性）和物体状态/距离的定量估计，正是"VLM 是否理解世界状态"的核心问题。论文的**数据生成框架**和**训练策略**可作为后续实验设计的参考；**解冻 ViT 提升空间推理**的发现可支持分析结果；论文指出"训练数据缺乏 3D 空间知识"是根本原因，可作为研究 gap 的直接论据。

**Can be used as baseline?** Yes
**If Yes, which task/scenario?** 可作为**空间关系推理**和**定量距离估计**任务的强基线；其数据生成方法可作为你构建自己评估数据的参考。

### Review #7

**Paper Title:** When More Is Less: A Systematic Analysis of Spatial and Commonsense Information for Visual Spatial Reasoning

**Venue / Year:** arXiv 2026 (Under Review)

**Paper Link:** https://arxiv.org/abs/2602.21619

**Main Problem:** 当前研究普遍认为向 VLM 注入额外信息（空间线索、常识知识、思维链）能提升空间推理能力，但**何种信息、何种形式、何种数量最有效**尚不清楚。本文系统探究：注入不同类型和形式的信息时，VLM 的空间推理能力究竟如何变化？是否“越多越好”？

**Core Method:** 基于假设的实证分析。将三种 VLM（Qwen-2-VL-7B/72B, LLaVA-NeXT-34B, BLIP-3-8B）作为固定黑盒，在 VSR 和 EmbSpatial 两个空间推理基准上，系统地改变注入信息的**类型**（空间线索 SC / 常识知识 CK / 思维链 CoT）、**形式**（自然语言 vs 数值/坐标）、**数量**和**相关性**，观察性能变化。

**Model Architecture:** 不提出新模型；测试对象为 Qwen-2-VL-7B/72B, LLaVA-NeXT-34B, BLIP-3-8B

**Dataset:** VSR（Visual Spatial Reasoning，判断物体间空间关系）和 EmbSpatial（具身空间推理，涉及三维空间、物体位置和路径规划）

**Evaluation Metric:** 准确率（Accuracy），重点比较注入不同信息后的准确率变化

**Main Result:** 
1. **单一、有针对性的空间线索最有效**：堆砌多个空间线索会导致“认知过载”，反而降低性能。
2. **自然语言描述优于精确数值**：描述性的空间语言（如“在...左前方”）比坐标、深度值等精确数值更有效。
3. **常识知识是双刃剑**：过多或弱相关的常识会成为噪声，损害性能。
4. **思维链依赖空间定位精度**：CoT 仅在空间定位足够精确时才有帮助；存在模糊性时会放大错误。

**Limitation:** 
1. 仅基于闭源/开源 API 模型，未涉及模型内部架构的修改
2. 基准（VSR, EmbSpatial）的规模和多样性有限
3. 分析维度有限，未探索多模态融合或训练阶段干预

**Relevance to our paper:** 
与候选方向 A 高度相关。论文直接回应了“VLM 空间推理能力如何提升”的问题，核心发现是：**“更多并不等于更好”——注入信息的精准度和形式比数量更重要**。这与“图像-文本对齐”质量直接相关：对齐质量决定了注入的空间信息是否被 VLM 正确理解和利用。论文的实证分析可作为你研究“对齐质量与空间推理能力关系”的理论支撑。此外，论文使用的 VSR 基准和实验设计可作为后续评估空间推理能力的参考。

**Can be used as baseline?** No（这是分析论文，本身不是模型，但其发现可作为设计实验的理论依据）