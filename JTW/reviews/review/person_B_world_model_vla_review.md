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
