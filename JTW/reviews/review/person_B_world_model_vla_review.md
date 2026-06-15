# review 1
## Paper Title:
- Reasoning with Language Model is Planning with World Model
## Venue / Year:
- Venve ：EMNLP 2023
- Year：2023
## Main Problem:
- 现有的大语言模型虽然具备一定的推理能力，但缺乏内部的“世界模型”，导致无法像人类一样进行有意识的规划，从而在复杂推理任务中表现不佳。
- 1、缺乏世界模型\
  2、没有奖励评估\
  3、无法平衡探索和利用 
## Core Method:
- 提出了新的推理方法RAP（Reasoning via Planning）,核心思想是：将大语言模型同时用作“推理智能体”和“世界模型”，并引入蒙特卡洛树搜索（MCTS）进行原则性的规划，从而在广阔的推理空间中进行策略性探索，找到高奖励的推理路径。
## Model Architecture:
- LLaMA-33B（作为智能体）\
  LLaMA-33B（作为世界模型）\
  LLaMA-2\
  GPT-4
## Dataset:
- 1、计划生成	Blocksworld\
  2、数学推理	GSM8K\
  3、逻辑推理	PrOntoQA
## Evaluation Metric:
### 计划生成任务（Blocksworld）
- 成功率
- pass@k（采样 k 条计划，只要有一条正确就算成功）
### 数学推理任务（GSM8K）
- 准确率
### 逻辑推理任务（PrOntoQA）
- 预测准确率
- 证明准确率
## Main Result:
| 任务类型 | 数据集 | 评估指标 | RAP 最佳结果 | CoT 基线 | 提升幅度 |
| :--- | :--- | :--- | :---: | :---: | :---: |
| 计划生成 | Blocksworld (6-step) | 成功率 | **42%** | 0% | +42% |
| 计划生成 | Blocksworld (6-step, GPT-4对比) | 成功率 | **42%** (LLaMA-33B + RAP) | 40% (GPT-4 + CoT) | +5% (相对) |
| 计划生成 | Blocksworld (2-step) | 成功率 | **100%** | 17% | +83% |
| 计划生成 | Blocksworld (4-step) | 成功率 | **88%** | 2% | +86% |
| 数学推理 | GSM8K | 准确率 | **51.6%** | 29.4% | +22.2% |
| 逻辑推理 | PrOntoQA | 预测准确率 | **94.2%** | 87.8% | +6.4% |
| 逻辑推理 | PrOntoQA | 证明准确率 | **78.8%** | 64.8% | +14.0% | 
## Limitation:
- 在实验时使用了冻结的预训练模型，没有进行微调，也没有导入外部工具，如计算器，数据库等工具
## Relevance to our paper:
- 展示了大语言模型能够通过特定算法与特定的框架，形成自己的世界模型，对任务处理形成自己的规划，从而去提高复杂任务的推理成功率。
# review 2
## Paper Title:
- WorldVLA: Towards Autoregressive Action World Model
## Venue / Year:
- Venue：arXiv preprint (cs.RO, cs.AI)
- Year：2025
## Main Problem:
- 现有的视觉-语言-动作（VLA）模型与世界模型是分离的，缺乏统一的框架，导致无法充分利用两者的互补优势。
- 1、缺乏统一框架：世界模型预测未来图像时未充分利用动作信息，动作模型生成动作时未利用对未来状态的预测能力
- 2、自回归误差累积：在自回归方式下生成连续动作序列时，模型泛化能力有限，早期动作的误差会逐步传播到后续动作，导致整体性能下降
- 3、在复杂长时序任务中表现不佳
## Core Method:
- 提出了WorldVLA，一种自回归动作世界模型，将VLA模型和世界模型融合在单一框架中。核心思想是：使用一个统一的LLM架构同时建模动作生成和未来图像预测。世界模型利用动作和图像理解来预测未来图像，学习环境的底层物理规律以改进动作生成；动作模型基于图像观测生成后续动作，帮助视觉理解并反过来促进世界模型的视觉生成。针对自回归动作生成中的误差传播问题，提出了动作注意力掩码策略（Action Attention Mask），在生成当前动作时有选择性地遮蔽先前动作的注意力，防止误差传播。
## Model Architecture:
- 基于LLM的统一自回归框架（7B参数）
- 三个独立的Tokenizer：图像（VQ-GAN）、文本、动作（共享同一词汇表）
- 支持256×256和512×512分辨率
## Dataset:
- 1、机器人操作	LIBERO（LIBERO-Spatial / Object / Goal / Long）
- 2、泛化测试	LIBERO-plus（10,030个任务，7个扰动维度）
## Evaluation Metric:
### 动作模型（LIBERO任务）
- 成功率（Success Rate）
- 按四个任务套件分别评估：Spatial、Object、Goal、Long
### 世界模型（视频预测）
- Fréchet Video Distance（FVD，越低越好）
- Peak Signal-to-Noise Ratio（PSNR）
- Structural Similarity Index（SSIM）
- Learned Perceptual Image Patch Similarity（LPIPS）
## Main Result:
| 任务/指标 | WorldVLA 结果 | 对比基线 | 提升幅度 |
| :--- | :---: | :---: | :---: |
| LIBERO-Spatial 成功率 | **87.6%**（512×512） | 独立动作模型 | +4%（平均） |
| LIBERO-Object 成功率 | **96.2%**（512×512） | 独立动作模型 | +4%（平均） |
| LIBERO-Goal 成功率 | **83.4%**（512×512） | 独立动作模型 | +4%（平均）|
| LIBERO-Long 成功率 | **60.0%**（512×512）| 独立动作模型 | +4%（平均）|
| 四任务平均成功率 | **81.8%**（512×512）| 独立动作模型 | +4% |
| 世界模型 FVD | **降低10%** | 传统世界模型 | -10% FVD |
| LIBERO-Goal 5步动作块（无掩码） | 36.7% | — | — |
| LIBERO-Goal 5步动作块（有掩码）| **81.8%** | 无掩码 | **+120%** |
| 抓取成功率提升 | **4–23%** | 独立动作模型 | 4%-23% |
## Limitation:
- 在LIBERO-plus泛化基准上表现有限（25.3%），远低于SOTA模型（如OpenVLA-OFT+ 79.6%）
- 与2025年顶级VLA模型（如VLA-Adapter-Pro 98.5%、OpenVLA-OFT 97.1%）相比仍有差距
- 在长时序任务（LIBERO-Long）上成功率仅60%，长程规划能力有待提升
## Relevance to our paper:
- 展示了VLA模型与世界模型可以统一到一个自回归框架中，通过动作注意力掩码策略缓解自回归误差累积问题，验证了动作生成与视觉预测之间的相互增强关系，为具身智能中的统一建模提供了新的思路。
# review 3
## Paper Title:
- Sora as an AGI World Model? A Complete Survey on Text-to-Video Generation
## Venue / Year:
- Venue：arXiv preprint
- Year：2024
## Main Problem:
- 尽管Sora等文本到视频生成模型展现了接近真实的视频生成能力，但在Sora生成视频缺点的补充审查指出了在数据集、评估指标、高效架构和人类可控生成等视频生成支撑方面需要更深入研究的方向，从技术角度探索文生视频模型如何更接近世界模型。
## Core Method:
- 采用PRISMA系统综述框架，从IEEE Xplorer、ACM Library、Scopus和arXiv等数据库中筛选出97篇高相关度论文。核心贡献在于从四个技术维度系统解构文本到视频生成模型：
- (1) 核心构建模块（语言解释器、视觉处理器、时序处理器）
- (2) 辅助技术（帧序列化、高效学习）
- (3) 数据集与评估指标
- (4) 应用与伦理影响。
## Model Architecture:
- 综述涵盖四大类生成架构：\
  VQ-VAE（GODIVA等）\
  GAN（StoryGAN, TGANs-C等）\
  Autoregressive Transformer（Phenaki, VideoPoet, W.A.L.T.等）\
  Diffusion（Stable Diffusion系列, Make-A-Video, Sora等）\
- 语言解释器：RNN, BERT/T5, CLIP\
- 视觉处理器：VQ-VAE, GAN, VQ-GAN, Diffusion\
- 时序处理器：Temporal Attention, RNN, Pseudo-3D Convolution/Attention, LLM
## Dataset:
- 调研数据集：\
  1、WebVid-10M/2M（34篇论文使用）\
  2、MSR-VTT（11篇，评估基准）\
  3、LAION-5B（大规模图文对）\
  4、UCF-101（10篇，动作分类基准）\
  5、PororoSV（故事可视化）
## Evaluation Metric:
### 视觉质量
- Inception Score（IS）
- Fréchet Inception Distance（FID）
- Fréchet Video Distance（FVD）
- Generative Adversarial Metric（GAM）
### 文本-视觉对齐
- CLIP R-Precision
- CLIP Score / CLIPSIM / CLIP RM
### 人类感知评估
- DrawBench（11个评估类别，200个提示词）
- 人工评估四维度：视觉质量、文本忠实度、运动真实感、时序一致性
## Main Result:
| 维度 | 关键发现 |
| :--- | :--- |
| 技术架构趋势 | 80%以上的论文采用基于Stable Diffusion的伪3D卷积/注意力扩展，已成为视频扩散模型的事实标准 |
| 语言解释器 | CLIP文本编码器使用最广泛；T5系列在强生成模型（Phenaki等）中更受青睐 |
| 时序处理 | 伪3D卷积+时序注意力是最主流方案；LLM作为时序编码器是新兴方向 |
| 高效学习策略 | 图像-视频联合训练（Phenaki开创）、Adapter/运动模块插入、一致性模型蒸馏、解耦学习、模块化生成为五大主流策略 |
| 评估指标分布 | UCF101-FVD（26%）和UCF101-IS（23%）使用最多，其次是MSRVTT-CLIPSIM（20%） |
| Sora局限性 | 物理交互失败（液体流向、物体穿透）、尺度比例失真（相机运动导致）、物体幻觉（遮挡后消失/克隆）、因果效应缺失（动作-反应不匹配） |
## Limitation:
- 调查截止日期为2024年3月18日，未覆盖2024年下半年以来的最新进展（如Sora技术报告的详细技术细节仍未公开）
- 虽然分析了Sora的局限性，但由于Sora本身未开源，分析主要基于公开演示和视觉观察，缺乏对模型内部机制的深入验证
- 论文主要聚焦于技术综述，未提出新的模型或方法

## Relevance to our paper:
- 提供了从世界模型视角审视文本到视频生成模型的系统框架，明确了"世界模型"应具备的可扩展性（Scalability）和泛化性（Generalizability）两个核心要求。
# review 4
## Paper Title:
- VerseCrafter: Dynamic Realistic Video World Model with 4D Geometric Control
## Venue / Year:
- Venue：CVPR 2026
- Year：2026
## Main Problem:
- 现有的视频世界模型在提供统一的相机运动和多个物体运动控制方面存在根本性困难。视频本质上是2D图像平面的投影，导致：\
(1) 2D控制信号（边界框、光流、分割掩码）缺乏3D感知，在大视角变化下容易失效；\
(2) 现有3D控制方法（深度图、稀疏3D轨迹、3D边界框、SMPL-X人体模型）要么是类别特定的，要么是刚性的，无法灵活统一地建模多物体动态；\
(3) 缺乏一个紧凑、可编辑、共享世界坐标系的4D几何场景状态表示。

## Core Method:
- 提出了VerseCrafter，一个基于几何驱动的视频世界模型，从显式的4D几何场景状态生成动态逼真的视频，同时实现对相机和多物体运动的解耦控制。我们的框架包含两个关键组件：\
(i)统一的4D几何控制表示，在共享世界坐标系中表示4D几何场景状态；\
(ii)轻量级的GeoAdapter，将编码后的4D控制图注入冻结的Wan2.1-14B骨干网络，同时保留其强大的视觉先验。
## Model Architecture:
- 基础骨干：Wan2.1 T2V-14B（冻结，含Wan Encoder、Wan-DiT降噪器、Wan Decoder）
- 控制适配器：GeoAdapter（轻量DiT风格分支，每5个Wan-DiT块配对1个GeoAdapter块，输出线性投影后作为残差调制加入）
- 4D Geometric Control：静态背景点云 + 逐物体3D高斯轨迹 {μᵗₒ, Σᵗₒ}
- 渲染输出：4通道控制图（背景RGB/深度、3D高斯轨迹RGB/深度、软融合掩码）
- 文本编码器：umT5
- 训练分辨率：480P → 720P两阶段训练
- 推理：50步去噪，CFG scale=5.0，81帧720P视频约1152秒（8×96GB GPU）
## Dataset:
- 1、VerseControl4D
- 2、26%来自Sekai-Real-HQ，74%来自SpatialVID-HQ
- 3、20%为静态场景样本，用于相机控制评估
- 数据处理流程：场景切割（PySceneDetect，81帧子片段）→ 质量过滤（Grounded-SAM2 + 美学/亮度评分）→ 自动标注（Qwen2.5-VL-72B生成描述，MoGe-2深度估计，MegaSAM相机轨迹）
## Evaluation Metric:
### 联合相机与物体运动控制
- VBench-I2V（Overall Score, Imaging Quality, Aesthetic Quality, Dynamic Degree, Motion Smoothness, Background/Subject Consistency, I2V Background/Subject）
- RotErr（旋转误差，越低越好）
- TransErr（平移误差，越低越好）
- ObjMC（物体运动控制误差，平均欧氏距离，越低越好）
### 相机-only运动控制（静态场景）
- 同上VBench-I2V指标
- RotErr, TransErr
## Main Result:
| 任务 | 指标 | VerseCrafter | 最佳基线 | 提升幅度 |
| :--- | :--- | :---: | :---: | :---: |
| 联合控制 | Overall Score | **88.10** | 85.47（Yume） | +2.63 |
| 联合控制 | RotErr ↓ | **0.890** | 1.361（Uni3C） | -34.6% |
| 联合控制 | TransErr ↓ | **3.103** | 7.731（Uni3C） | -59.9% |
| 联合控制 | ObjMC ↓ | **2.507** | 5.883（Uni3C） | -57.4% |
| 相机-only | Overall Score | **86.80** | 85.33（FlashWorld） | +1.47 |
| 相机-only | RotErr ↓ | **0.650** | 1.792（FlashWorld） | -63.7% |
| 相机-only | TransErr ↓ | **2.587** | 3.257（FlashWorld） | -20.6% |
| 消融-3D高斯vs点轨迹 | ObjMC | **2.507**（高斯） | 6.896（点轨迹） | -63.6% |
| 消融-有/无深度 | RotErr | **0.890**（有深度） | 1.177（无深度） | -24.4% |
| 消融-解耦/合并控制 | ObjMC | **2.507**（解耦） | 3.726（合并） | -32.7% |
## Limitation:
- 在泛化基准（LIBERO-plus等）上的表现未报告，真实世界泛化能力有待进一步验证
- 训练需要大量计算资源：16×96GB GPU，约380小时（两阶段训练）
- 推理耗时较长：81帧720P视频需约1152秒（8×96GB GPU），难以满足实时应用需求
- 依赖输入单张图像进行3D重建，对于运动模糊或遮挡严重的场景，深度估计和点云重建质量可能下降
- 3D高斯轨迹的编辑仍需人工在Blender等3D编辑器中进行关键帧操作，自动化程度有限
## Relevance to our paper:
- 展示了将显式3D几何信息（4D Geometric Control）与视频扩散模型深度融合的技术路线，验证了通过显式几何表示可以实现精确的相机和多物体运动控制。