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
# review 5
## Paper Title:
- Learning from Reward-Free Offline Data: A Case for Planning with Latent Dynamics Models
## Venue / Year:
- Venue：NeurIPS 2025（Conference）
- Year：2025
## Main Problem:
- 人工智能的一个长期目标是开发能够解决各种环境中多样化任务的智能体，包括那些在训练期间从未见过的环境。在离线设置中的相对优势（智能体必须从无奖励轨迹中学习）仍未得到充分探索。主要研究如何构建一个在未见过的任务和环境组合中表现良好的系统。

## Core Method:
- 提出了PLDM（Planning with a Latent Dynamics Model），一种基于学习的潜动力学模型的规划方法。\
核心方法包括：
GCIQL[52]——隐式Q学习[33]的目标条件版本，是一种强大且广泛使用的离线RL方法；\
 HIQL[52]——一种分层GCRL方法，训练两个策略：一个生成子目标，另一个到达子目标。值得注意的是，两个策略使用相同的值函数；\
 HILP[53]——一种从离线数据中学习状态表示的方法，使得所学表示空间中的距离与两个状态之间的步数成正比。然后学习一个方向条件策略，使其能够在潜在空间中沿着任意指定方向移动\
 CRL[19]——使用对比学习学习状态与可能可达目标之间的兼容性。学到的表示（已被证明与目标条件Q函数直接相关）随后用于训练目标条件策略；\
 GCBC[23, 43]——目标条件行为克隆，是到达目标问题的最简单基线方法。

## Model Architecture:
- 编码器 hθ：将观测映射到潜表示 z = hθ(s)
- 预测器集成 fᵏθ：K个独立的潜空间动力学预测器，ˆzᵏₜ = fᵏθ(ˆzᵏₜ₋₁, aₜ₋₁)
- 训练目标：VICReg防坍缩正则化 + 逆动力学建模 + 预测-编码距离最小化
- 规划机制：MPPI（Model Predictive Path Integral），每一步重新规划（i=1）
- 对比的基线模型：\
  HIQL（分层GCRL，双策略+共享值函数）\
  GCIQL（目标条件IQL）\
  HILP（潜空间距离保持+方向条件策略）\
  CRL（对比表征学习+目标条件Q函数）\
  GCBC（目标条件行为克隆）
## Dataset:
- 自建23个不同质量的导航数据集：\
  1、Two-Rooms环境（点智能体，64×64顶视图，3M条转移）\
  2、PointMaze变体（多布局，随机墙体排列）\
  3、Ant-U-Maze（29维状态+8维动作，5M条转移，四足机器人）\
  数据质量维度：不同轨迹长度（16/32/64/91）、不同策略质量（Von Mises采样 vs 均匀随机）、不同布局覆盖（5/10/20/40训练布局）
## Evaluation Metric:
### 主要指标
- 成功率（Success Rate）
### 评估维度（6个泛化压力测试）
- 数据效率（不同数据集规模：千级到百万级）
- 轨迹拼接（不同轨迹长度，从不通过门的约束数据）
- 随机策略学习（均匀随机动作轨迹）
- 新任务泛化（目标到达→状态避免的零样本迁移）
- 高维控制（Ant-U-Maze四足机器人）
- 新布局泛化（不同数量的训练布局 + 分布外布局距离分析）
## Main Result:
| 评估维度 | 条件 | PLDM | 最佳对比方法 | 关键发现 |
| :--- | :--- | :---: | :---: | :--- |
| 充分数据下性能 | 3M高质量转移 | **97.8%** | 100%（HILP） | 充足数据下所有方法都接近完美 |
| 数据效率 | 千级转移 | **~80%**（3K） | ~80%（GCIQL） | PLDM和GCIQL最数据高效 |
| 轨迹拼接（短轨迹） | 16步轨迹 | **>75%** | ~100%（HILP, GCIQL） | GCRL方法在短轨迹下失败，PLDM和HILP保持性能 |
| 轨迹拼接（不通过门） | 无门穿越数据 | **34.4%** | 100%（HILP, GCIQL） | 仅有HILP和GCIQL完美拼接） |
| 随机策略学习 | 均匀随机数据 | **>75%** | >75%（HILP, GCIQL） | GCRL长程任务失败，PLDM/HILP/GCIQL更鲁棒 |
| 新任务泛化（状态避免） | 躲避追逐者 | **~65%**（速度1.0） | ~25%（HILP） | PLDM通过反转规划目标即可零样本迁移 |
| 高维控制（Ant-U-Maze） | 50步轨迹 | **100%** | 100%（HIQL, HILP） | PLDM在四足机器人任务上依然出色 |
| 新布局泛化 | 5训练布局→新布局 | **~40%** | ~0%（所有基线） | PLDM是唯一能在未见布局上成功的方法 |
| 新布局泛化（分布外程度） | 高编辑距离布局 | **~35%** | ~0% | 所有基线随分布偏移急剧退化，PLDM最鲁棒 |
## Limitation:
- 实验主要集中在导航任务上，未在机器人操作（manipulation）等更复杂领域进行验证
- PLDM在轨迹拼接（不通过门）测试中表现不如HILP和GCIQL，存在拼接能力的局限
- PLDM每一步都重新规划，推理速度约为模型无关方法的1/4（∼4x slower），部署效率有待提升
- 依赖潜空间中的距离度量作为规划目标，对于需要精确末端执行器控制的任务可能不够精细
- 预测误差在长程规划中会累积，虽然使用了集成不确定性惩罚，但极长程任务的表现未充分评估
## Relevance to our paper:
- 展示了基于潜动力学模型进行规划（而非显式学习策略）在泛化性和数据效率上的显著优势，PLDM采用JEPA架构进行无重建的潜空间表示学习，避免了重建的计算开销和特征次优性问题,为我们在世界模型研究中如何利用无标签离线数据提供了重要的方法论参考。
# review 6
## Paper Title:
- Embed to Control: A Locally Linear Latent Dynamics Model for Control from Raw Images
## Venue / Year:
- Venue：NIPS 2015
- Year：2015
## Main Problem:
- 具有连续状态和动作空间的非线性动态系统的控制是机器人学中的关键问题之一，在更广泛的背景下，也是自主智能体强化学习中的关键问题。它通过局部线性化来逼近一般的非线性控制问题。当与滚动时域控制以及用于学习近似系统模型的机器学习方法相结合时，这类算法成为解决复杂控制问题的强大工具；然而，它们要么依赖于已知的系统模型，要么需要设计相对低维的状态表示。要使真正的自主智能体取得成功，我们最终需要能够仅从原始感官输入（例如图像）控制复杂动态系统的算法。
## Core Method:
- 提出了E2C模型，一种深度生成模型（VAE家族），核心创新在于将潜空间动力学**显式约束为局部线性**。具体而言：(1) 编码网络将图像映射为高斯分布 N(μₜ, diag(σ²ₜ))；(2) 变换网络，从潜变量 zₜ 预测局部线性化矩阵 Aₜ、Bₜ 和偏移 oₜ，使得 zₜ₊₁ = Aₜzₜ + Bₜuₜ + oₜ + 噪声；(3) 解码网络从潜变量重建图像。训练时最小化变分下界（VAE目标）加上一个额外的KL散度项，强制过渡模型^Q与编码模型Q一致，确保长程预测不漂移。规划时，在潜空间中使用iLQR或AICO等SOC算法求解最优控制序列。
## Model Architecture:
- 编码器网络：图像 x → 高斯参数 (μ, σₜ) → 采样 zₜ
- 变换网络 h(zₜ) → (A, B, o)，其中 Aₜ可参数化为 I + vₜrᵀₜ 降维
- 解码器网络：zₜ → 重建图像 
- 潜空间维度：平面系统 2D，倒立摆 3D，cart-pole 8D，机械臂 8D
- 对比变体：全局线性E2C（参数全局共享）、非线性E2C（使用Jacobian线性化）
- 对比基线：标准VAE、深度自编码器AE、带慢速项的VAE（均单独训练动力学）
## Dataset:
- 自建4个视觉控制数据集：\
  1、平面障碍物导航（40×40黑白图，3K样本）\
  2、倒立摆摆起（48×48图，15K样本，2帧堆叠恢复马尔可夫性）\
  3、Cart-pole平衡（80×80图，15K样本，2帧堆叠）\
  4、三连杆机器人臂（128×128图，30K样本，2帧堆叠）
## Evaluation Metric:
### 预测质量
- 状态重建损失 log p(xt| x)
- 下一状态预测损失 log p(xt+1| x, u)
### 控制性能
- 潜空间轨迹成本
- 真实环境轨迹成本（在模拟器中执行规划的动作后累积的cost）
- 成功率（Success Rate）
## Main Result:
| 任务 | 指标 | E2C结果 | 最佳基线 | 对比方法表现 |
| :--- | :--- | :---: | :---: | :--- |
| 平面系统 | 成功率 | **100%** | 0%（AE/VAE） | 所有AE/VAE基线均完全失败（0%） |
| 平面系统 | 真实轨迹成本 | **25.1±5.3** | 273.3±16.4（AE） | 近真实模型水平（真实：20.2±4.15） |
| 倒立摆摆起 | 成功率 | **90%** | 0%（AE/VAE） | 仅E2C实现稳定平衡控制 |
| 倒立摆摆起 | 真实轨迹成本 | **15.4±3.4** | 194.7±44.8（AE） | 远低于所有基线（真实：9.8±2.4） |
| 倒立摆摆起 | 非线性E2C | 63.33% | — | 非线性变体性能下降，说明局部线性约束的重要性 |
| 倒立摆摆起 | 全局线性E2C | 0% | — | 全局线性无法捕捉摆起这种高度非线性动力学 |
| Cart-pole | 轨迹成本 | **11.13** | — | 接近真实模型控制（7.28） |
| 三连杆臂 | 轨迹成本 | **85.12** | — | 接近真实模型控制（60.74） |
## Limitation:
- 局部线性假设限制了模型对高度非线性动力学的表达能力（实验中全局线性E2C在倒立摆上完全失败）
- 需要多帧堆叠（2帧）来恢复马尔可夫性，对高速动态系统可能需要更多历史帧
- 训练数据量相对较小（3K-30K条转移），在更大规模、更复杂场景下的表现未知
- 生成图像质量有限（展示的是低分辨率的黑白/简单渲染图），难以推广到真实机器人视觉输入
- 规划依赖潜空间中的二次型成本函数（LQG假设），对于非欧几里得距离的目标表示可能不适用
## Relevance to our paper:
- 作为潜空间动力学模型的奠基性工作，E2C首次展示了通过变分推断将最优控制公式融入表示学习的技术路线。其核心洞见——在潜空间中显式约束动力学为局部线性形式，使得SOC算法可以直接在学到的特征空间中进行规划——对后续世界模型研究（如Dreamer、PLDM）产生了深远影响。其提出的"编码器-动力学-规划器"三阶段框架和训练时强制过渡与编码一致性的KL正则化策略，至今仍是潜空间世界模型设计的核心范式之一。
# review 7
## Paper Title:
- RealWonder: Real-Time Physical Action-Conditioned Video Generation
## Venue / Year:
- Venue：arXiv preprint (cs.CV)
- Year：2026（March）
## Main Problem:
当前的视频生成模型无法模拟3D动作（如力和机器人操作）的物理后果，因为它们缺乏对动作如何影响3D场景的结构性理解,本质上仍局限于被动生成或简单的2D控制。
## Core Method:
- 提出了RealWonder，首个支持实时物理动作条件视频生成的系统。核心洞察：使用物理仿真作为中间表示桥梁——将3D物理动作通过物理仿真转化为视频模型能够自然处理的视觉表示。系统由三个精心设计的组件构成：\
(1) 输入图像重建可仿真的3D场景表示，估计适合实时物理的几何和材料属性\
(2) 应用物理仿真计算场景对输入动作的动态响应，将结果渲染为编码运动模式同时保留动作因果关系的光流F_t和粗糙RGB预览Ṽ_t（\
(3) 基于物理的视觉表示与原始图像一起，条件化一个蒸馏视频生成器，在4步扩散中产生逼真结果
## Model Architecture:
- 对于物理仿真，我们采用Genesis作为仿真器。我们的仿真使用0.01s的时间步长，每个仿真步最多20个子步以保证数值稳定性。
- 对于机器人动作，我们使用Genesis提供的Franka机器人模型，支持机器人与多种材料的交互。
- 对于视频模型训练，我们采用VideoXFun wan2.1-1.3B-InP模型作为I2V基础模型。我们冻结其所有权重，并在每个注意力块中注入秩为2048的LoRA模块。
- 应用自强制风格训练，获得蒸馏的、实时的、流条件视频生成器。
## Dataset:
- 自建200K"光流-视频"对：\
  1、180K真实视频片段（来自OpenVid，80-120帧，使用RAFT提取光流）\
  2、20K合成视频（Wan2.1-14B-T2V生成，来自VidProM提示词）\
  评估集：30张图像（真实+合成），覆盖多种材质（布料、刚体、弹性体、液体、气体、沙、雪）及相应物理动作
## Evaluation Metric:
### 自动指标
- VBench Visuals（成像质量）
- VBench Aesthetics（美学质量）
- VBench Consistency（时序一致性）
- GPT-4o-based PhysReal（物理真实感）
### 人工评估（2AFC协议，400名参与者，4个维度）
- Action Following（动作跟随）
- Motion Fidelity（运动保真度）
- Visual Quality（视觉质量）
- Physical Plausibility（物理合理性）
### 速度指标
- FPS（帧率）
- Latency（延迟）
## Main Result:
| 评估维度 | 条件 | RealWonder | 最佳基线 | 提升幅度 |
| :--- | :--- | :---: | :---: | :--- |
| 自动-Visuals | VBench | **0.708** | 0.700（Tora） | +0.008 |
| 自动-Aesthetics | VBench | **0.593** | 0.603（CogVideoX） | -0.010（略低） |
| 自动-Consistency | VBench | **0.265** | 0.234（CogVideoX） | +0.031 |
| 自动-PhysReal | GPT-4o | **0.705** | 0.624（CogVideoX） | +0.081 |
| 人工-动作跟随 | vs PhysGaussian | **88.4%** | 11.6% | 显著偏好RealWonder |
| 人工-物理合理性 | vs PhysGaussian | **87.1%** | 12.9% | 显著偏好RealWonder |
| 人工-动作跟随 | vs CogVideoX-I2V | **89.6%** | 10.4% | 显著偏好RealWonder |
| 人工-动作跟随 | vs Tora | **83.9%** | 16.1% | 显著偏好RealWonder |
| 生成速度 | FPS | **13.2** | 0.225（CogVideoX） | **58.7×** |
| 延迟 | 首帧延迟 | **0.73s** | 4.84s（PhysGaussian） | **-84.9%** |
## Limitation:
- 3D场景重建依赖单张图像的深度估计，在深度估计不准确时会导致物理仿真和视频生成结果次优
- 训练数据中20K为合成视频，可能与真实世界域有分布差异
- 视频生成器基于1.3B参数模型，视觉质量和分辨率（480×832）不及更大模型
- 对于极其复杂的长程物理交互（如多物体长时间相互碰撞），物理仿真和视频生成的累积误差可能增加
## Relevance to our paper:
- 展示了利用物理仿真作为中间桥梁来实现3D物理动作条件视频生成的技术范式。在世界模型的领域，此结论也有助于我们研究如何让世界模型更为全能，实用。
# review 8
## Paper Title:
- Generating Action-conditioned Prompts for Open-vocabulary Video Action Recognition
## Venue / Year:
- Venue：ACM MM 2024
- Year：2024
## Main Problem:
- 开放词汇视频动作识别面临的根本挑战是：现有方法虽然通过时序建模增强了视频编码器对已见过动作的识别能力，但在面对从未见过的新动作时表现不佳。
## Core Method:
-  适应CLIP进行视频动作识别 (Adapt CLIP for Video Action Recognition)\
- 动作条件提示生成 (Action-conditioned Prompts Generation)
- 多模态动作知识对齐 (Multi-modal Action Knowledge Alignment - MAKA)
## Model Architecture:
- 视频编码器：CLIP ViT-B/16（默认）或ViT-L/14
- 文本编码器：CLIP文本编码器
- 提示词生成：GPT-4（LLM，不参与推理）
- MAKA对齐：视频帧嵌入 v∈Rⁿᵛ×ᵈ × 提示词嵌入 c∈Rⁿᵗ×ᵈ → 双向最大相似度平均
  sim(v,c) = ½(simᵥ₂ₜ(v,c) + simₜ₂ᵥ(v,c))
- 多视图推理：8帧输入，2个空间裁剪×2个时间视图（全监督时：16帧，4空间裁剪×3时间视图）
- 与基线对比方法：Vanilla CLIP, ActionCLIP, XCLIP, ViFi-CLIP, Open-VCLIP, BIKE, Text4Vis, DiST
## Dataset:
- Kinetics-400（训练集，~240K视频，400类动作）
- Kinetics-600（~390K视频，600类，零-shot评估）
- HMDB-51（~7K视频，51类，零-shot评估）
- UCF-101（~13K视频，101类，零-shot评估）
- SSv2（Something-Something v2，~220K视频，174类，base-to-novel评估）
## Evaluation Metric:
### 主要指标
- Top-1准确率
### 评估设置
- 零-shot（Zero-shot）：K400训练 → HMDB-51 / UCF-101 / K600评估
- Base-to-novel泛化：在base类上训练 → novel类评估（调和平均HM）
- Few-shot：每类1/2/4/8/16样本
- 全监督（Fully-supervised）
## Main Result:
| 设置 | 模型/基线 | HMDB-51 | UCF-101 | K600 | 提升 |
| :--- | :--- | :---: | :---: | :---: | :--- |
| 零-shot (ViT-B/16) | AP-CLIP（Ours） | **55.4%** | **82.4%** | **73.4%** | — |
| 零-shot (ViT-B/16) | ViFi-CLIP | 51.3% | 76.8% | 71.2% | +4.1/+5.6/+2.2 |
| 零-shot (ViT-B/16) | XCLIP | 44.6% | 72.0% | 65.2% | +10.8/+10.4/+8.2 |
| 零-shot (ViT-B/16) | Open-VCLIP+Action Prompts | **57.0%** | **85.1%** | **74.4%** | +3.1/+1.7/+1.4 |
| 零-shot (ViT-L/14) | Open-VCLIP+Action Prompts | **60.0%** | **90.2%** | **81.9%** | **新SOTA** |
| Base-to-novel (K400) | AP-CLIP 调和平均HM | **最高** | — | — | — |
| Base-to-novel (HMDB) | AP-CLIP 调和平均HM | **最高** | — | — | — |
| Base-to-novel (UCF) | AP-CLIP 调和平均HM | **最高** | — | — | — |
| Base-to-novel (SSv2) | AP-CLIP 调和平均HM | **最高** | — | 时序复杂数据集提升有限 | — |
## Limitation:
- 对于时序复杂的数据集（如SSv2），生成的知识提示词带来的提升有限，说明纯文本属性描述在捕捉细粒度时序模式方面存在局限
- 提示词生成依赖于GPT-4，虽然无需手动标注，但存在潜在的生成偏差和不可靠内容
- MAKA机制需要计算每帧与所有提示词的相似度，推理时计算开销随视频帧数和提示词数量增加
- CLIP模型本身在细粒度和时序建模上的固有限制未被突破，方法更多是发掘了CLIP已有的表征能力
## Relevance to our paper:
- 展示了利用大语言模型的先验知识来增强视频动作识别中文本表征的技术路线。其核心洞见——为不同动作生成知识丰富的多属性描述提示词而非使用统一的标准提示词——对于提升开放词汇识别中的泛化能力具有重要的方法论意义。在世界模型的设计里，使用LLA生成提示词，是训练世界模型自我思考能力的一个好方法。
# review 9
## Paper Title:
- OpenVLA: An Open-Source Vision-Language-Action Model
## Venue / Year:
- Venue：arXiv preprint (cs.RO)
- Year：2024
## Main Problem:
- 机器人操作学习策略的一个关键弱点是缺乏在训练数据之外泛化的能力：虽然为单个技能或语言指令训练的策略有能力将行为外推到新的初始条件（如物体位置或光照），但它们缺乏对场景干扰物或新物体的鲁棒性，并且难以执行未见过的任务指令。
## Core Method:
- 提出OpenVLA（7B），通过对预训练的Prismatic-7B VLM进行动作预测微调得到。核心逻辑三步走：\
(1) 融合视觉编码（SigLIP语义特征 + DinoV2空间特征，通道拼接）\
(2) 2层MLP投影到Llama 2 7B的输入空间 \
(3) LLM以next-token prediction预测动作token。动作离散化：每维256 bins（基于1st-99th分位数的均匀分箱），覆盖Llama tokenizer末尾256个最不常用token。训练数据：Open X-Embodiment 97万条轨迹，筛选后仅含第三人称单臂操作数据。训练配置：64×A100 GPU × 14天（21,500 A100-hours），batch=2048，27个epochs直到动作token准确率>95%。
## Model Architecture:
- 视觉编码器：SigLIP（语义/类别级特征）+ DinoV2（空间/低级特征），双骨干逐通道拼接
- 投影器：2层MLP
- 语言骨干：Llama 2 7B
- 动作离散化：每维256 bins（1st-99th分位数均匀分箱），使用词汇表末尾256个token
- 输入分辨率：224×224px（384×384无提升但慢3x）
- 训练发现：微调视觉编码器（较冻结）更优；需多epochs（27）至准确率>95%
- 推理配置：bfloat16: 15GB显存 / int4: 7.0GB / RTX 4090: ~6Hz
- 对比基线：RT-1-X（35M）、Octo（93M）、RT-2-X（55B闭源）、Diffusion Policy
## Dataset:
- 训练：Open X-Embodiment，筛选后约97万条轨迹（单臂+第三人称），Octo加权策略平衡多样性
- 零-shot评估：Bridge V2 WidowX（17任务×10=170次）、Google Robot（12任务×5=60次）
- 微调评估：Franka-Tabletop（6任务/5Hz）+ Franka-DROID（1任务/15Hz），每任务10-150条示范
- 仿真评估：LIBERO四个套件（Spatial/Object/Goal/Long），各10任务×50条示范
## Evaluation Metric:
### 零-shot直接评估
- 绝对成功率（A/B评估，相同初始状态）
- 泛化维度：视觉（未见背景/干扰物/颜色/外观）、运动（未见位置/朝向）、物理（未见尺寸/形状）、语义（未见目标/指令/互联网概念）、语言条件能力（多物体场景按指令操作正确目标）
### 微调评估
- 成功率（含ID + OOD评估）
- 参数高效微调：全微调 / 仅最后一层 / 冻结视觉编码器 / Sandwich / LoRA r=32/64
- 量化推理：bfloat16 vs int8 vs int4（成功率+显存+速度）
- LIBERO仿真：500 trials/套件 × 3 seeds
## Main Result:
| 评估维度 | 条件 | OpenVLA 结果 | 最佳对比方法 | 关键发现 |
| :--- | :--- | :---: | :---: | :--- |
| **零-shot：Bridge V2** | 17任务平均成功率 | **70.6±3.2%** | RT-2-X 50.6% | 7x更少参数下超越55B闭源模型16.5% |
| **零-shot：Google Robot** | 12任务平均成功率 | **85.0±4.6%** | RT-2-X 78.3% | 在Taylor Swift等语义泛化上仍弱于RT-2-X |
| **微调：Franka-Tabletop** | 6任务平均成功率 | **67.2±4.0%** | DP 48.5% / Octo 43.4% | 唯一所有任务>50%的方法 |
| **微调：Franka-DROID** | Wipe Table平均 | **58.3±7.2%** | Octo 38.3% / DP 35.0% | OOD场景下优势更大 |
| **LoRA参数高效微调** | r=32 | **68.2±7.5%** | 全微调 69.7% | 仅1.4%参数，59.7GB VRAM，匹配全微调 |
| **4-bit量化推理** | int4 | **71.9±4.7%** | bfloat16 71.3% | 显存16.8GB→7.0GB，性能无损 |
| **LIBERO仿真平均** | 4套件 × 500 trials | **76.5% (Rank 1.5)** | Octo 75.1% (R2) / DP 72.4% (R2.5) | 真实数据预训练可有效迁移到仿真域 |
| **消融：OpenX训练** | 仅Bridge训练 | 76.3→**45.6%**（-30%） | — | 大规模预训练数据多样性是关键 |
| **消融：DinoV2** | 移除DinoV2 | 45.6→**40.6%**（-5%） | — | 双编码器融合有增益但不如OpenX数据重要 |
| **消融：微调视觉编码器** | 冻结vs微调 | 冻结局成功率损失>30% | — | VLA中微调视觉编码器是必需的 |
## Limitation:
- 仅支持单图像观测，不支持多摄像头/本体感知/观测历史——但真实机器人系统通常是异构传感的
- 推理速度有限（RTX 4090: ~6Hz），无法满足ALOHA（50Hz）等高频控制需求
- 在窄领域精确操作任务上不及Diffusion Policy——论文提出引入action chunking作为未来方向
- 在互联网概念语义泛化上弱于RT-2-X——原因：未使用联合微调（机器人数据+互联网数据同时训练）
- 所有测试任务的成功率均未超过90%，可靠性有显著提升空间
- VLM骨干规模效应、是否联合训练视觉-语言数据等VLA核心设计问题尚未探索
## Relevance to our paper:
- 作为首个开源的大规模VLA模型，OpenVLA提供了从VLM微调VLA的完整技术栈。LoRA和4-bit量化为VLA在消费级GPU上的部署提供了可直接复用的最佳实践。对于世界模型研究,开源VLM模型给我们对于世界模型的训练在如何解决此类问题，如何实现世界模型的自主意识形成提出了一种可行的方法。
# review 10
## Paper Title:
- Fine-Tuning Vision-Language-Action Models: Optimizing Speed and Success
## Venue / Year:
- Venue：arXiv preprint (cs.RO)
- Year：2025
## Main Problem:
- 现有的VLA模型虽然展现了强大的任务执行和语言跟随能力，但在新机器人平台上必须通过微调才能达到可用性能。VLA在新型机器人设置上仍面临困难，需要通过微调才能达到良好性能，然而面对众多可能的策略，如何最有效地微调VLA尚不明确。微调对于VLA在新机器人和新任务上的令人满意的部署来说至关重要，然而考虑到巨大的设计空间，最有效的适配方法是哪一个尚不清楚
## Core Method:
- 关于本文所使用的基础模型：openVLA，研究团队提出了三个关键设计选择：\
1、动作解码方案（自回归 vs. 并行生成）\
2、动作表示（离散 vs. 连续）\
3、学习目标（下一个token预测 vs. L1回归 vs. 扩散）
- 研究揭示了几个相互叠加的关键洞察：\
（1）带有动作分块的并行解码不仅提升了推理效率，还提高了下游任务的成功率，同时使模型的输入输出规格具有更大灵活性；\
（2）相比离散表示，连续动作表示进一步提高了模型质量；\
（3）使用L1回归目标微调VLA可获得与基于扩散的微调相当的性能，同时提供更快的训练收敛和推理速度。

- 基于这些洞察，提出了OpenVLA-OFT：这是优化微调（OFT）方案的一个实例化，集成了并行解码与动作分块、连续动作表示和L1回归目标，以增强推理效率、任务性能和模型输入输出灵活性，同时保持算法简洁性。
## Model Architecture:
- 基础模型：OpenVLA（7B，Prismatic VLM + Llama 2骨干，SigLIP+DinoV2视觉编码器）
- 微调方式：LoRA（参数高效，适应小数据集）
- 并行解码：将因果注意力mask替换为双向注意力，一次性预测所有动作token
- 动作分块：预测K个未来动作（K=8 for LIBERO, K=25 for ALOHA），完整执行后重新规划
- 连续动作头：MLP直接将解码器最终隐状态映射为归一化连续动作值（替代softmax离散分类）
- L1回归目标：最小化预测动作与真实动作之间的平均L1距离
- FiLM（OFT+）：语言嵌入→缩放γ+偏移β → 逐通道调制视觉Transformer每层的补丁嵌入
- 多模态输入：通过共享投影器支持多视角图像（256 patch embeddings/视角）+ 本体感知状态（额外投影网络）
- 对比基线：ACT（84M）、Diffusion Policy（157M）、RDT-1B（1.2B）、π0（3.3B）
## Dataset:
- 1、LIBERO仿真基准：四个任务套件（Spatial/Object/Goal/Long），各10任务×50条示范=500条，7维动作
- 2、ALOHA双臂机器人（真实世界）：4个任务——叠短裤（20条示范）、叠T恤（30条）、用勺子舀指定配料入碗（45条，3种配料）、将指定物品放入锅中（300条，3种物体），14维关节角动作
- 3、各任务独立微调，50K-150K梯度步（非扩散）/ 100K-250K步（扩散），batch size 64-128，8×A100/H100-80GB GPU
## Evaluation Metric:
### LIBERO仿真
- 成功率（500 trials/套件×3 seeds=1500 trials/统计量）
- 推理吞吐量（Throughput, Hz）和延迟（Latency, sec）——100次查询平均，NVIDIA A100
### ALOHA双臂机器人
- 任务完成百分比（预定义评分rubric，部分完成给部分分）
- 语言跟随能力：在语言依赖任务中接近正确指定物体的成功率
- 推理效率：吞吐量和延迟（3×224×224图像 + 14-D本体状态 + 任务指令）
## Main Result:
| 评估维度 | 条件 | OpenVLA-OFT 结果 | 最佳对比方法 | 提升幅度/关键发现 |
| :--- | :--- | :---: | :---: | :--- |
| **LIBERO平均**（1图像+L1） | 4套件平均SR | **95.3%** | π0 94.2% | +1.1%；较原始OpenVLA 76.5%提升+18.8% |
| **LIBERO平均**（2图像+本体） | 4套件平均SR | **97.1%** | π0 94.2% | **新SOTA**；较原始OpenVLA提升+20.6% |
| **推理速度** | 吞吐量（1图像） | **109.7 Hz** | 原始OpenVLA 4.2 Hz | **26× 加速**；延迟0.073s vs 0.240s |
| **ALOHA双臂平均** | 4任务平均 | **最高**（定量图表） | π0 > RDT-1B > DP > ACT | 超越所有微调VLA和从零训练策略，提升最高15% |
| **ALOHA语言跟随** | 语言依赖任务 | **最高** | RDT-1B次之 | FiLM消融后降至33%（随机水平） |
| **ALOHA推理速度** | 吞吐量（3图像+本体） | **77.9 Hz** | ACT 432.8 / π0 291.6 | 7B参数下接近1.2B RDT-1B（84.1 Hz） |
| **消融：并行解码+分块** | vs 自回归 | **+14%绝对成功率** | — | LIBERO-Long提升最显著 |
| **消融：连续动作** | vs 离散256-bin | **+5%绝对成功率** | — | 动作精度提升是主因 |
| **消融：L1 vs 扩散** | 同等条件 | **性能相当** | — | L1训练更快、推理1步 vs 扩散50步 |
| **消融：OpenVLA预训练** | 移除预训练 | **-5.2%**（绝对） | — | OpenX预训练表征仍有价值 |
## Limitation:
- **处理多模态演示：** 实验使用统一策略的聚焦演示数据集。L1回归虽可通过学习演示动作的中位数模式来平滑噪声，但可能在真实多模态动作分布（同一输入对应多个有效动作）下表现不佳。扩散方法可能更好地捕获此类多模态性，但有过拟合次优模式的风险。

- **预训练 vs 微调：** 本研究仅针对下游任务的VLA微调。OFT的收益是否能有效扩展到预训练阶段，或者更复杂的算法（如扩散）对于大规模训练是否必要，需要进一步研究。

- **不一致的语言grounding：** OpenVLA无FiLM时在ALOHA中语言跟随差但在LIBERO仿真中无此问题，根源尚不明确（可能与预训练中缺少双臂数据或其他因素有关）。
## Relevance to our paper:
- 提供了VLA高效微调的系统性实证研究范式，三条关键设计维度（解码策略、动作表示、学习目标）的消融实验为VLA适应新任务提供了可复现的最佳实践。对于世界模型而言，此openVLA OFT+模型是对openVLA模型进行微调训练去使用与真实世界的机器人使用，世界模型的目标也是为了针对现实世界的情况去研发的，为我们去研究世界模型时，如何调整模型，使其能够更为适用于现实场景，提供了一条清晰，可行的方法。
# review 11
## Paper Title:
- A Survey of Embodied AI: From Simulators to Research Tasks
## Venue / Year:
- Venue：IEEE Transactions on Emerging Topics in Computational Intelligence (IEEE TETCI)
- Year：2022
## Main Problem:
- 1、缺乏对具身AI领域的当代全面综述：之前的综述大多发表于现代深度学习时代（2009年起）之前，已经过时
- 2、具身AI模拟器的评估标准不统一：需要一套系统性的特征框架来比较不同模拟器的优劣
- 3、模拟器、数据集与研究任务之间的关联不清晰：研究者难以根据自身任务选择合适的模拟器
## Core Method:
- 提出了一个系统性的具身AI综述框架，从模拟器到研究任务两个维度展开。核心贡献包括：\
(1) 提出了7项技术特征（Environment, Physics, Object Type, Object Property, Controller, Action, Multi-Agent）用于评估9个具身AI模拟器，并进一步归纳为3个二级评估维度——Realism（真实感）、Scalability（可扩展性）和Interactivity（交互性）；\
(2) 构建了具身AI研究任务的金字塔层次结构，从视觉探索（Visual Exploration）、视觉导航（Visual Navigation）到具身问答（Embodied QA），复杂度逐步递增；\
(3) 建立了模拟器→数据集→研究任务之间的关联映射，为研究者根据研究任务选择合适的具身AI模拟器提供了系统指导。
## Model Architecture:
- 综述论文，对9个具身AI模拟器进行7+3特征对比，进一步归纳为三个二级评估维度——Realism（真实感）、Scalability（可扩展性）和Interactivity（交互性）：

| Simulator | Year | Environment | Physics | Object Type | Object Property | Controller | Action | Multi-Agent | Engine |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| DeepMind Lab | 2016 | G | - | - | - | P, R | N | - | Quake II Arena |
| AI2-THOR | 2017 | G | B | O | I, M | P, R | A, N | U | Unity 3D |
| CHALET | 2018 | G | B | O | I, M | P | A, N | - | Unity 3D |
| VirtualHome | 2018 | G | - | O | I, M | R | A, N | - | Unity 3D |
| VRKitchen | 2019 | G | B | O | I, M | P, V | A, N, H | - | Unreal Engine 4 |
| Habitat-Sim | 2019 | W | - | D | - | - | N | - | — |
| iGibson | 2019 | W | B | D | I | P, R | A, N | U | — |
| SAPIEN | 2020 | G | B | D | I, M | P, R | A, N | - | PhysX + ROS |
| ThreeDWorld | 2020 | G | B, A | O | I | P, R, V | A, N, H | AT | Unity 3D |

**符号说明**：Environment: G=Game-based, W=World-based | Physics: B=Basic, A=Advanced | Object Type: D=Dataset-driven, O=Asset-driven | Object Property: I=Interact-able, M=Multi-state | Controller: P=Python API, R=Virtual Robot, V=VR | Action: N=Navigation, A=Atomic Action, H=Human-Computer Interaction | Multi-Agent: AT=Avatar-based, U=User-based

## Dataset:
- 视觉探索 / 视觉导航数据集：\
  1、Matterport3D（90张真实室内3D扫描，61/11/18训练/验证/测试划分）\
  2、Gibson V1（高质量3D场景，支持iGibson交互增强）\
  3、AI2-THOR（120个房间，4个类别，交互式环境）
- 视觉-语言导航（VLN）数据集：\
  1、Room-to-Room (R2R)（21,567条导航指令，平均29词）\
  2、Cooperative Vision-and-Dialog Navigation (CVDN)（2,050段人机对话，7,000+轨迹）
- 具身问答数据集：\
  1、EQA数据集（5,000个问题，750个环境，45个物体，7种房间类型，基于SUNCG/House3D）\
  2、MT-EQA数据集（6种组合式比较问题）\
  3、IQUAD V1（75,000个多选题，基于AI2-THOR）
## Evaluation Metric:
### 模拟器评估（7+3特征框架）
- Environment（游戏场景G vs 真实世界场景W）
- Physics（基础B vs 高级A）
- Object Type（数据集驱动D vs 资产驱动O）
- Object Property（可交互I vs 多状态变化M）
- Controller（Python API P / 机器人R / VR控制器V）
- Action（导航N / 原子动作A / 人机交互H）
- Multi-Agent（虚拟化身AT / 用户多智能体U）
- 二级评估维度：Realism（真实感）、Scalability（可扩展性）、Interactivity（交互性）
### 视觉探索
- Amount of Targets Visited (ATV)：访问目标数量（如覆盖面积m²、探索百分比）
- Impact on Downstream Tasks (D)：对下游导航任务的影响
### 视觉导航
- Success Weighted by Path Length (SPL)：路径长度加权成功率（主要指标）
- Success Rate (SR)：成功率
- Path Length Ratio (PLR)：路径长度比
- Distance to Success / Navigation Error (DTS/NE)：导航误差距离
- VLN额外：Oracle Success Rate (OSR), Trajectory Length (TL)
- 视觉对话导航额外：Goal Progress (GP/d∆), Oracle Path Success Rate (OPSR)
### 具身问答（EQA）
- 导航性能：dT（终止距离）、d∆（目标进展）、dmin（最近距离）、%stop（提前终止率）、%rT（正确房间终止率）、%re（目标房间进入率）、IoU（目标交并比）、hT（命中准确率）、Episode Length
- QA性能：Mean Rank (MR), Accuracy (Acc)
## Main Result:
| 维度 | 关键发现 |
| :--- | :--- |
| 模拟器全面性排名 | AI2-THOR、iGibson和Habitat-Sim在三个二级特征上均表现优异，是应用最广泛的三大模拟器 |
| 渲染性能 | Habitat-Sim（10,000 fps/thread）和iGibson（1,000 fps/thread）显著领先于其他模拟器 |
| 任务-模拟器映射 | 视觉探索和导航主要使用真实世界场景模拟器（Habitat-Sim/iGibson，高保真优势）；具身QA和带先验导航需多状态对象属性，AI2-THOR为首选；VLN目前不使用具身AI模拟器而是Matterport3D模拟器 |
| 金字塔层次结构 | 视觉探索→视觉导航→具身QA，每层为上一层提供基础模块，复杂度递增 |
| 点导航近完美结果 | DD-PPO在2.5B步训练后达到接近最短路径oracle的性能（差距3-5%），辅助任务可5.5×加速训练 |
| 挑战赛推动发展 | iGibson Sim2Real Challenge / Habitat Challenge / RoboTHOR Challenge成为标准化评估平台 |
| 预测性发展方向 | 提出任务型交互问答（TIQA）作为金字塔下一阶段任务——要求智能体先完成具体任务以获取信息再回答问题 |
## Limitation:
- 记忆架构： 长轨迹和多模态输入凸显了鲁棒记忆架构的重要性。RNN已知在捕获长期依赖方面有限，但哪种记忆类型最优尚无定论
- 复杂性管理：每个新组件（如VLN加入语言理解，EQA加入QA）导致训练难度和成本指数增长。两个有希望的方向：混合方法（经典+学习）和先验知识注入
- 多智能体设置： 目前缺乏支持多智能体的仿真器，该领域受到的关注相对较少
## Relevance to our paper:
- 作为具身AI领域的重要综述文献，系统梳理了从模拟器到研究任务的完整技术栈。其提出的7+3特征评估框架为理解和选择具身AI平台提供了结构化方法论，金字塔层次结构（探索→导航→问答）清晰展示了具身AI任务复杂度的递进关系。对于世界模型研究，该综述揭示了当前模拟器在真实感和物理准确性方面的核心瓶颈——高级物理特性的缺乏直接限制了世界模型学习真实物理规律的能力，这为我们设计兼顾视觉保真度与物理真实性的世界模型训练环境提供了重要的需求分析参考。
# review 12
## Paper Title:
- Aligning Cyber Space with Physical World: A Comprehensive Survey on Embodied AI
## Venue / Year:
- Venue：IEEE/ASME Transactions on Mechatronics
- Year：2025（arXiv首次提交2024年7月，最新修订2025年8月）
## Main Problem:
研究界对从MLM中获取强大感知和推理能力兴趣浓厚，但社区缺少一篇能帮助梳理现有具身AI研究、面临的挑战以及未来研究方向的全面综述。在MLM时代，作者团队旨在通过执行一项从赛博空间到物理世界的具身AI系统综述来填补这一空白。从不同视角进行综述，包括具身机器人、仿真器、四个代表性具身任务（视觉主动感知、具身交互、多模态智能体和虚实迁移适应）以及未来研究方向。
## Core Method:
- 提出了一个面向MLM时代的具身AI系统综述框架，核心贡献包括：\
(1) 提出了基于MLM和WM的具身智能体ABC模型架构——AI Brain（A模型，具身世界模型负责环境理解）、Body（B模型，物理实体执行动作）、Cross-modal Sensors（C模型，多模态主动感知），系统刻画了具身智能体从赛博空间到物理世界的完整信息流；\
(2) 将具身AI划分为六个核心组成部分——具身机器人、通用/真实场景模拟器、具身感知（主动视觉感知+视觉语言导航）、具身交互（具身问答+具身抓取）、具身智能体（任务规划+动作规划）、Sim-to-Real适应（具身世界模型+数据采集+控制），并分别进行了sota方法、核心范式和数据集的系统性梳理；\
(3) 首次提出了ARIO（All Robots In One）数据集标准和统一大规模数据集（约300万片段，258个系列，321,064个任务），解决了多机器人平台数据格式不统一的痛点问题。
## Model Architecture:
- **具身智能体ABC架构**（综述提出的核心概念框架）：
  - A模型（AI Brain）：具身世界模型，使智能体理解虚拟-物理环境，实现状态预测与决策推理
  - B模型（Body）：物理实体（固定基座/轮式/履带式/四足/人形/仿生机器人），赋予智能体动作执行能力
  - C模型（Cross-modal Sensors）：多模态传感器，使智能体主动感知多模态元素（视觉、3D点云、触觉、音频等），增强情境感知
- **世界模型三范式**：
  - 生成式方法（Generation-based）：如Sora、Pandora、3D-VLA、DWM，通过大规模生成模型内化世界知识
  - 预测式方法（Prediction-based）：如I-JEPA、MC-JEPA、iVideoGPT、MuDreamer，在潜空间中学习预测表征，避免像素级重建
  - 知识驱动方法（Knowledge-driven）：如ElastoGen、Holodeck、GRUtopia，将人工构建的物理规则或常识知识注入模型
- **具身任务规划方法**：
  - LLM涌现能力驱动（Translated LM, Inner Monologue, ReAd, Code as Policies）
  - 视觉信息驱动（SayPlan使用3D场景图, ConceptGraphs, RoboGPT带重规划）
  - VLM驱动（EmbodiedGPT的Embodied-Former, LEO的2D+3D视觉编码, RT系列, PaLM-E, Matcha的VLA模型）
- **Sim-to-Real五范式**：Real2Sim2Real（数字孪生+RL微调）、TRANSIC（人在回路纠正+残差策略）、Domain Randomization（仿真参数随机化）、System Identification（高精度场景重建）、Lang4Sim2Real（自然语言桥接跨域图像表示）
## Dataset:
### 视觉语言导航（VLN）数据集
- R2R（21,567条逐步指令，Matterport3D），R4R（200,000+条更长路径）
- VLN-CE（连续环境扩展），REVERIE（21,702条远程物体指代），SOON（3,848条由粗到细指令）
- ALFRED（25,743条交互式导航+操作，AI2-THOR），BEHAVIOR-1K（1,000个长序列日常任务，OmniGibson）
- CVDN（2,050段对话导航），DialFRED（53,000段交互式对话导航）
### 具身问答（EQA）数据集
- EQA v1（5,000+问题，SUNCG/House3D），MT-EQA（19,000+多目标比较问题）
- IQUAD V1（75,000+多选交互QA，AI2-THOR），SQA3D（33,400+知识密集QA，ScanNet）
- OpenEQA（1,600+开放词汇QA，ScanNet/HM3D-Habitat），HM-EQA（500多选QA，HM3D-Habitat-VLM）
- S-EQA（二值情境QA，VirtualHome-LLM），EXPRESS-Bench（2,044样本，探索感知QA，HM3D-Habitat）
### 具身抓取数据集
- 传统：Cornell（280物体/8K抓取），Jacquard（11K物体/1.1M抓取），6-DOF GraspNet（206物体/7.07M），ACRONYM（8,872物体/17.7M）
- 语言引导：OCID-VLG（89物体/75K，空间推理），ReasoningGrasp（264物体/9.3M，逻辑推理），CapGrasp（51物体/50K，语义灵巧手）
### ARIO数据集（该综述提出）
- 约300万片段，来自258个机器人系列，321,064个任务，统一格式支持多形态机器人、多模态感官数据
## Evaluation Metric:
### 主动视觉感知
- vSLAM：绝对轨迹误差（ATE）、相对位姿误差（RPE）、定位精度与建图完整性
- 3D场景理解：3D目标检测mAP、语义分割mIoU、实例分割AP
- 主动探索：信息增益（Information Gain）、覆盖率（Coverage）、探索效率（步数/面积）
### 视觉语言导航（VLN）
- Success Rate (SR)、Success Weighted by Path Length (SPL)、Oracle Success Rate (OSR)
- Navigation Error (NE)、Trajectory Length (TL)
### 具身问答（EQA）
- 导航性能：距离目标终止距离（dT）、目标进展（d∆）、最近距离（dmin）、提前终止率（%stop）、IoU、命中准确率（hT）
- QA性能：准确率（Accuracy）、平均排名（MR）
### 具身抓取
- 抓取成功率（Grasp Success Rate）、覆盖率（Declutter Rate）、接触丰富度
### 具身智能体
- 任务规划准确率、动作执行成功率、端到端任务完成率、重规划次数
### 世界模型/Sim-to-Real
- 预测误差（MSE/MAE）、生成质量（FVD/PSNR/SSIM/LPIPS）、策略迁移成功率
## Main Result:
| 维度 | 关键发现 |
| :--- | :--- |
| ABC模型框架 | 首次系统定义A（AI Brain/世界模型）、B（Body/物理实体）、C（Cross-modal Sensors/多模态感知）三级架构，为具身智能体的设计提供了统一参考模型 |
| MLM时代vs前MLM时代 | 2023年后的MLM（LLM/VLM）为具身智能体注入了强大的感知、推理和规划能力——从规则驱动（PDDL/MCTS）转向数据驱动+涌现能力（LLM零样本规划、VLA端到端模型） |
| 世界模型三范式 | 生成式（Sora等→内化世界知识）、预测式（JEPA系列→潜空间高效表征）、知识驱动（ElastoGen等→注入物理规则）——三种范式在保真度、效率、可解释性上各有取舍 |
| RT-2（VLA）里程碑 | 将网络知识迁移到机器人控制，验证了VLM→VLA的技术路线可行性，但任务规划准确率96%而端到端完成率仅60%，说明动作执行是瓶颈 |
| Sim-to-Real五范式 | Real2Sim2Real、TRANSIC、Domain Randomization、System Identification、Lang4Sim2Real——从数字孪生到自然语言桥接，方法从重仿真保真度向轻量化数据高效迁移演进 |
| ARIO数据集 | 首个针对多形态机器人的统一数据集标准，覆盖258系列/321K任务/3M片段，解决了多机器人平台数据格式不统一的核心痛点 |
| Cosmos平台 | NVIDIA Cosmos（2025）集成了自回归和扩散模型用于Text-to-World和Video-to-World生成，可能成为构建具身世界模型的重要基础设施 |
## Limitation:
- 综述为纯文献调研，未提供实验验证或基准测试结果，所有结论均基于对已有文献的归纳和推演
- 提出的ARIO数据集标准和ABC模型架构目前仍处于概念阶段，其实用性和可扩展性有待实际部署验证
- 具身世界模型的核心瓶颈仍在：复杂环境的高维感知、动态随机性建模、长期依赖关系处理、跨场景泛化能力不足——这些在该综述中被指出但未被实质性解决
- Sim-to-Real适应仍然严重依赖大量高质量仿真数据，仿真与真实世界之间的域差距（传感器噪声、物理复杂度、环境多样性）是当前具身AI的最大障碍
- 长序任务执行（如"清理厨房"）的端到端成功率尚未被系统评估——现有高层任务规划虽有初步成功，但在多样化场景中仍显不足

## Relevance to our paper:
- 作为MLM时代的具身AI综述文献，系统梳理了多模态大模型和世界模型如何重塑具身AI全技术栈。其提出的ABC模型架构为世界模型在具身智能体中的定位提供了清晰的概念框架。同时，该综述明确指出"缺乏世界模型的行动规划器无法仅凭LLM内部知识模拟物理规律"这一核心限制，有力论证了世界模型在弥合赛博空间与物理世界差距中的不可替代性。
# review 13
## Paper Title:
- FAM-HRI: Foundation-Model Assisted Multimodal Human-Robot Interaction Combining Gaze and Speech
## Venue / Year:
- Venue：arXiv preprint (cs.RO, cs.AI)
- Year：2025
## Main Problem:
- 有效的人机交互（HRI）对于增强现实世界机器人应用的可及性和可用性至关重要
- 现有的人机交互（HRI）解决方案通常依赖仅手势或仅语言的指令，导致交互效率低下且存在歧义，尤其对于身体功能受损的用户。
- 提出的系统集成了来自人类视角和机器人视角的环境感知，以促进有效协作。为了结构化多模态信号以供基础模型推理，形式化了几何框架和控制参数的集合。\
定义分类：\
A. 框架与变换的定义\
B. 人类视角的意图融合\
C. 机器人视角表示\
D. 控制系统形式化
## Core Method:
- 提出了FAM-HRI框架，一种基于基础模型的多模态HRI方法，融合注视和语音输入实现直观高效的机器人操控。核心包含三个组件：\
(1) 人类视角意图融合——通过LLM Agent处理来自ARIA眼镜的实时注视轨迹和语音命令，提出注视轨迹滤波函数（基于OPTICS聚类和加权策略）精确确定意图目标；\
(2) 多视角意图对齐——利用SuperGlue特征匹配和深度信息实现人类自我中心视角与机器人外部视角的空间对齐，解决视角差异导致的物体指代歧义；\
(3) 规划策略生成——LLM基于场景上下文和机器人工作空间约束生成参数化动作原语。同时提出确定性冲突解决策略处理多视角对齐中的匹配冲突。
## Model Architecture:
- 感知硬件：Meta ARIA眼镜（采集人类视角图像+语音）、Intel Realsense D435i RGBD摄像头（机器人视角）
- 机器人平台：Franka Emika Panda 7DOF机械臂
- 基础模型：GPT-4o（主要LLM Agent，对比Gemini 1.5 Pro、Qwen2-VL-72B）
- 视觉模型：Grounding DINO（物体检测）、SAM2（分割）、SuperGlue（特征匹配）
- 工具链：Whisper（语音识别）、OPTICS聚类（注视轨迹分析）
## Dataset:
- 自建真实世界HRI评估数据集：
- 4个评估场景：S1相似物体选择（10个棋子中选特定目标）、S2物体操作（拾取放置）、S3多步动作（顺序动作序列）、S4因果动作（需理解因果关系的复杂任务）
- 每场景6个不同任务，每任务5次重复试验（N=30/场景）
- 用户研究：12名参与者（8名正常+4名残疾），含系统可用性量表（SUS）问卷
## Evaluation Metric:
### 主要指标
- 成功率（Success Rate, %）
- 交互时间（Interaction Time, s，从用户发出指令到机器人开始执行）
### 用户研究指标
- 系统可用性量表（SUS）分数
- 用户满意度（7点李克特量表，10个维度）
- 方差分析（ANOVA）验证用户组间差异
### 消融分析指标
- 正确指代概率（Correct Referred Probability）
- 旋转误差（RotErr）、平移误差（TransErr）
- 命令处理延迟、策略生成延迟
## Main Result:
| 维度 | 条件 | FAM-HRI结果 | 最佳基线 | 提升幅度 |
| :--- | :--- | :---: | :---: | :---: |
| S1相似物体选择 | 成功率 | **83.3%** | 33.3%（ProgPrompt语言-only） | +50% |
| S2物体操作 | 成功率 | **80.0%** | 30.0%（ProgPrompt） | +50% |
| S3多步动作 | 成功率 | **86.7%** | 26.7%（ProgPrompt） | +60% |
| S4因果动作 | 成功率 | **83.3%** | 16.7%（GesSentence手势-only） | +66.6% |
| S1交互时间 | 秒 | **16.7s** | 50.3s（ProgPrompt） | -66.8% |
| S2交互时间 | 秒 | **20.9s** | 53.1s（ProgPrompt） | -60.6% |
| S3交互时间 | 秒 | **9.3s** | 86.0s（GesSentence） | -89.2% |
| S4交互时间 | 秒 | **14.7s** | 89.7s（GesSentence） | -83.6% |
| 注视滤波-短间隔 | 正确指代概率 | **0.96** | 0.89（均匀加权） | +7.9% |
| 注视滤波-长间隔 | 正确指代概率 | **0.98** | 0.92（均匀加权） | +6.5% |
| 多视角对齐消融 | 移除机器人视角后成功率下降 | **-27.5%（平均）** | — | — |
| LLM Agent比较 | GPT-4o命令处理 | **100%** | 73.3%（Gemini 1.5 Pro） | +26.7% |
| LLM Agent比较 | GPT-4o策略生成 | **87.5%** | 66.7%（Gemini 1.5 Pro） | +20.8% |
| 模态消融 | 移除注视 | 成功率下降 | — | — |
| 模态消融 | 移除语音 | 成功率下降 | — | — |
| 用户研究SUS | 正常参与者 | **87.3** | — | — |
| 用户研究SUS | 残疾参与者 | **84.2** | — | — |
## Limitation:
- LLM/VLM推理延迟影响实时响应，在边缘设备上部署面临挑战；注视-语言融合系统在松配ARIA眼镜和室外环境（光照变化、遮挡、背景运动）下稳定性下降
- 多视角对齐依赖准确的深度传感器，在远距离或透明物体上可能失效
- 实验在受控环境下进行，复杂真实场景（多物体长期交互、动态变化环境）的泛化能力有待验证
- 系统依赖闭源商用大模型（GPT-4o），在无互联网连接的环境中可用性受限；开源模型（Qwen2-VL-72B）的性能尚有一定差距
- 注视轨迹分析需要用户发出语音指令时注视目标，对于无法说话的严重残障用户，纯注视交互模式未覆盖
## Relevance to our paper:
- 展示了基础模型（LLM/VLM）在多模态人机交互中的实际应用范式，验证了大模型作为"智能推理核心"将多模态感知（注视+语音+视觉）融合为可执行机器人动作的技术路线。其核心洞见——利用注视轨迹滤波函数从动态噪声中精确提取用户意图，以及通过多视角对齐解决自我中心与外部视角的物体指代歧义——对世界模型研究中如何融合多视角感知、消除感知不确定性具有直接的方法论参考价值。该工作同时也揭示了当前大模型在HRI中面临的核心瓶颈（延迟、鲁棒性、开放场景泛化），为世界模型设计时需平衡性能与实时性提供了实证依据。
# review 14
## Paper Title:
- DiffusionVLA: Scaling Robot Foundation Models via Unified Diffusion and Autoregression
## Venue / Year:
- Venue：ICML 2025 (International Conference on Machine Learning)
- Year: 2025
## Main Problem:
- 现有VLA（Vision-Language-Action）模型存在两种范式的不对称缺陷：\
(1) 自回归VLA模型（如RT-2、OpenVLA）将动作预测映射为next-token prediction任务，但由于将连续动作离散化为固定大小token会导致动作一致性和精度受损，且自回归生成效率低下；\
(2) 扩散策略模型（Diffusion Policy）在动作生成上表现出色（多模态、高效），但天生缺乏推理能力。核心问题是如何将自回归模型的推理能力与扩散模型的高质量动作生成统一起来。
## Core Method:
- DiffusionVLA（DiVLA）提出了一种统一自回归和扩散模型的VLA框架：
 方法的核心是自回归推理——由预训练的视觉语言模型实现的子任务分解与解释过程——用以指导基于扩散的动作策略。为将推理与动作生成紧密耦合，引入了推理注入模块，该模块将自生成的推理短语直接嵌入到策略学习过程中。同时，提出了推理注入模块，该模块重用推理输出并将其直接嵌入到策略头中，从而利用显式的推理信号丰富策略学习过程。
 - 最终目标是创建一个统一框架，将擅长预测语言序列以进行推理的自回归模型与高效生成机器人动作的扩散模型相结合。开发这样一个集成模型面临着巨大挑战，关键问题集中在：\
 (i) 设计一种能够无缝且高效地集成自回归机制和扩散机制的架构；\
 (ii) 利用自生成的推理来增强动作生成，且不增加推理计算开销。
## Model Architecture:
- 自回归模型（Autoregression models）。预测下一个标记（next-token prediction）已被视为实现通用人工智能的关键途径。
- 扩散模型（Diffusion models）。扩散模型已在视觉生成领域占据主导地位。扩散策略（Diffusion Policy）（Chi et al., 2023）将扩散模型的应用扩展至机器人学习，证明了其在处理多模态动作分布方面的有效性。
- 机器人基础模型（Robot foundation models）
- 统一自回归模型与图像生成（Unified auto-regressive model and image generation）。聚焦于统一多模态理解与图像生成。
## Dataset:
| 数据类型 | 数据集/任务 | 规模 | 用途 |
|:---|:---|:---:|:---|
| 预训练 | Droid | — | DiVLA-2B/7B预训练 |
| 预训练 | OXE + Droid | — | DiVLA-72B预训练 |
| 微调 | Multi-Task Learning（5个任务） | 580条轨迹 | 多任务学习微调 |
| 微调 | Factory Sorting | 500条轨迹 | 工业分拣微调 |
| 微调 | Table Bussing（双臂） | 400条轨迹 | 双臂机器人微调 |
| 零样本 | Zero-Shot Bin Picking | 102个未见物体 | 无需训练直接评估 |
## Evaluation Metric:
- **成功率**（Success Rate, %）：所有场景的主要评估指标
- **控制频率**（Control Frequency, Hz）：实时性评估

### 实验任务定义
| 任务类别 | 任务名称 | 任务描述 | 评估条件 |
|:---|:---|:---|:---|
| 多任务学习 | 物体选择（Object Selection） | 根据用户意图从多个物体中选取指定目标 | ID / 视觉泛化（干扰物/背景/光照） |
| 多任务学习 | 翻转立放锅（Flip Pot） | 识别平底锅左右朝向并翻转至正确方向 | ID / 视觉泛化 |
| 多任务学习 | 方块入盒（Cube in Box） | 识别关盖盒子→开盖→将方块放入 | ID / 视觉泛化 |
| 多任务学习 | 杯子放盘（Cup on Plate） | 识别盘子所在的层架，将杯子放在盘上 | ID / 视觉泛化 |
| 多任务学习 | 方块入指定色盒（Cube into Box） | 按指令将方块放入指定颜色（黄/蓝）的盒子 | ID / 视觉泛化 |
| 工业分拣 | Factory Sorting | 将桌面物品按类别分拣至四格分类盒中 | Seen / Mixed / Cluttered Seen / Cluttered Mixed |
| 零样本抓取 | Zero-Shot Bin Picking | 将右侧面板上的任意未知物体移至左侧篮子 | 102个零样本未见物体 |
| 双臂清洁 | Table Bussing | 双手臂机器人分类：餐具放左盘、垃圾丢右桶 | Seen / Mixed |
## Main Result:
### 多任务学习成功率（%）
| 模型 | 预训练数据量 | Task1 | Task2 | Task3 | Task4 | Task5 | ID平均 | OOD平均 |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Diffusion Policy | — | 66.7 | 36.4 | 0 | 36.4 | 0 | **27.9** | **8.9** |
| TinyVLA | — | 72.7 | 45.5 | 36.4 | 45.5 | 27.3 | **45.5** | **28.9** |
| Octo | 970K | 57.6 | 27.3 | 9.1 | 0 | 27.3 | **24.3** | **17.8** |
| OpenVLA-7B | 970K | 69.7 | 18.2 | 18.2 | 36.4 | 54.5 | **39.4** | **26.7** |
| **DiVLA-2B** | **39K** | **100** | **100** | **63.6** | **63.6** | **90.9** | **83.6** | **57.8** |

### Factory Sorting 成功率（%）
| 模型 | Seen | Mixed | Cluttered Seen | Cluttered Mixed | **平均** |
|:---|:---:|:---:|:---:|:---:|:---:|
| Diffusion Policy | 33.8 | 24.6 | 13.8 | 9.2 | **20.4** |
| Octo | 41.3 | 33.8 | 26.3 | 23.1 | **31.1** |
| TinyVLA | 56.2 | 46.2 | 45.0 | 33.8 | **45.3** |
| OpenVLA-7B | 48.8 | 32.3 | 31.2 | 21.5 | **33.4** |
| **DiVLA-2B** | **76.2** | **66.2** | **62.5** | **60.0** | **66.2** |

### Zero-Shot Bin Picking 与 Table Bussing 成功率（%）
| 模型 | Bin Picking（102未见物体） | Table Bussing Seen | Table Bussing Mixed |
|:---|:---:|:---:|:---:|
| Diffusion Policy | 8.9 | 45.8 | 31.2 |
| Octo | 19.6 | — | — |
| TinyVLA | 23.5 | — | — |
| OpenVLA-7B | 28.4 | 0 | 0 |
| **DiVLA-2B** | **63.7** | **72.9** | **70.8** |

### 模型扩展性 Scaling（%）
| 模型规格 | Factory Sorting Avg | Zero-Shot Bin Picking |
|:---:|:---:|:---:|
| DiVLA-2B | 66.2 | 63.7 |
| DiVLA-7B | 74.5 | 67.3 |
| DiVLA-72B | **82.4** | **75.9** |

### 消融研究与泛化分析
| 分析维度 | DiVLA-2B | 对比方法 | 关键发现 |
|:---|:---:|:---|:---|
| View Shifting泛化 | **60.0%** | OpenVLA 0% / DP 0% | 视角变化下鲁棒性显著优于基线 |
| 新颖指令执行 | ✓ | OpenVLA ✗ | 能执行多步序列指令（如"先捡西瓜→蓝色垃圾→柠檬水"） |
| VQA能力保留 | ✓ | 不适用 | 未协同训练仍保留基本对话和视觉问答能力 |
| 推理注入消融 | 移除后性能下降 | — | 推理注入模块对泛化性能至关重要 |
## Limitation:
- **推理注入的信息瓶颈**：仅使用最终嵌入（FiLM）注入推理信号，将多步推理压缩为单一向量，在长序复杂推理任务中可能丢失细粒度中间信息
- **VLM质量依赖**：推理能力完全源于Qwen2-VL预训练，VLM的视觉理解错误（如玩具龙误识为老虎）会直接传播至动作生成
- **自动标注偏差**：GPT-4o自动为Droid数据生成的推理链注释未经过人类评估或消融验证，标注质量与真实机器人推理之间可能存在分布偏差
- **评估规模限制**：Zero-Shot Bin Picking仅102个物体，Table Bussing仅12次试验，在更广泛多样性下的泛化能力需进一步验证
- **长序任务未评估**：主要评估单步或短序推理→动作，未系统研究长推理链的累积误差及纠错机制
- **低精度量化退化**：8-bit/4-bit低精度下性能显著下降，嵌入式部署需专门量化方法
## Relevance to our paper:
- 推理-行动解耦范式：DiVLA的"自回归推理 + 扩散动作生成 + 推理注入桥接"架构，与世界模型中"状态预测 + 规划执行 + 语义引导"的分层设计高度呼应，为设计具身世界模型的推理-控制接口提供了直接架构参考。
- 自生成推理增强泛化：实验证明显式推理链路（如screwdriver→hex key的类比推理）能显著提升对未见物体的零样本泛化能力，提示世界模型应引入可解释推理机制来增强对未知物理场景的模拟质量。
- 量化对比论证范式转换必要性：DiVLA以统一模型形式同时证明了自回归VLA的精度效率瓶颈（动作离散化损失 + 5Hz低推理速度）与扩散策略的推理缺失，有力论证了机器人控制需要超越"token prediction"范式——世界模型以其紧凑、物理一致的状态空间预测能力，天然规避了自回归离散化带来的精度损失。