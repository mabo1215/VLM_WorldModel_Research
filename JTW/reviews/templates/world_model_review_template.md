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
