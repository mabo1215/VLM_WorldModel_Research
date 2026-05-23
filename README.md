# VLM / 世界模型新论文任务计划

## 总体目标

完成一篇围绕 **Vision-Language Models / Multimodal World Models / Vision-Language-Action Models** 的新论文。

核心方向限定为：

```text
1. VLM 多模态理解
2. VLM 视觉推理
3. VLM 空间关系理解
4. VLM 视频理解
5. 世界模型中的状态建模
6. 世界模型中的时序预测
7. VLM 与世界模型结合
8. VLA / Embodied AI 中的感知-推理-行动
```

不做：

```text
隐私
安全
攻击
防御
jailbreak
privacy leakage
membership inference
```

---

# 建议论文主线

可以先保留 3 个候选方向，Phase 1 review 后再决定最终选题。

## 候选方向 A：VLM 的世界状态理解能力

### 题目方向

**Do Vision-Language Models Understand World States?**

核心问题：

```text
VLM 是否真正理解图像或视频中的物体状态、空间关系、动作过程和因果变化？
```

可研究内容：

```text
object state
spatial relation
temporal change
physical relation
action consequence
scene graph consistency
```

---

## 候选方向 B：面向世界模型的多模态状态一致性评估

### 题目方向

**Evaluating World-State Consistency in Multimodal World Models**

核心问题：

```text
世界模型生成或预测的未来状态，是否和真实物理世界、场景关系、动作结果一致？
```

可研究内容：

```text
video prediction
future frame understanding
object permanence
motion consistency
causal transition
state-change reasoning
```

---

## 候选方向 C：VLM + World Model 的统一评估框架

### 题目方向

**A Benchmark for Evaluating World-State Reasoning in Vision-Language and World Models**

核心问题：

```text
能否构建一个 benchmark，统一评估 VLM 和世界模型对场景状态、空间关系、时序变化和动作结果的理解？
```

这个方向比较适合写成一篇完整论文，因为可以包含：

```text
benchmark
metric
baseline
model comparison
error taxonomy
case study
```

---

# 4 人角色分工

| 人员       | 主要方向              | 长期职责                                                                 |
| -------- | ----------------- | -------------------------------------------------------------------- |
| Person A | VLM 理解与推理         | VLM 论文 review、视觉问答、空间推理、grounding                                    |
| Person B | 世界模型 / 视频模型 / VLA | world model、video prediction、embodied AI、action-conditioned modeling |
| Person C | 数据集与实验            | benchmark、dataset、metrics、baseline、ablation                          |
| Person D | 方法整合与论文写作         | research gap、论文结构、related work、最终整合                                  |

---

# Phase 0：项目初始化

## 阶段目标

建立项目结构、任务追踪表和 review 模板。

## 4 人并行任务

| 人员       | 子任务                           | 交付物                                                   |
| -------- | ----------------------------- | ----------------------------------------------------- |
| Person A | 建立 VLM review 模板              | `reviews/templates/vlm_review_template.md`            |
| Person B | 建立 world model review 模板      | `reviews/templates/world_model_review_template.md`    |
| Person C | 建立 dataset / metric review 模板 | `reviews/templates/dataset_metric_review_template.md` |
| Person D | 建立 repo 和论文目录结构               | `docs/`, `paper/`, `reviews/`, `src/`, `experiments/` |

## 接受标准

```text
[ ] 项目目录建立完成
[ ] review 模板建立完成
[ ] 任务追踪表建立完成
[ ] 每个任务有 owner
[ ] 每个任务有 deliverable
[ ] 每个任务有 reviewer
```

---

# Phase 1：论文收集与系统 Review

## 阶段目标

4 个人先完成论文收集和 review，明确当前 VLM / 世界模型领域的研究空白。

这是第一阶段最重要的任务。

---

## Person A：VLM 理解与推理方向

### Review 范围

```text
1. Large Vision-Language Models
2. Multimodal instruction tuning
3. Image-text alignment
4. Visual question answering
5. Visual grounding
6. Spatial reasoning in VLM
7. Compositional reasoning in VLM
8. Video VLM
```

### 最低要求

至少 review **15 篇论文**。

### 交付物

```text
reviews/person_A_vlm_reasoning_review.md
```

### 每篇论文 review 模板

```text
Paper Title:
Venue / Year:
Main Problem:
Core Method:
Model Architecture:
Dataset:
Evaluation Metric:
Main Result:
Limitation:
Relevance to our paper:
Can be used as baseline? Yes / No:
```

### 接受标准

```text
[ ] 至少 15 篇论文
[ ] 每篇都有 limitation
[ ] 至少总结 5 个 VLM reasoning research gaps
[ ] 至少列出 3 个可用 baseline
[ ] 至少推荐 2 个适合新论文的切入点
```

---

## Person B：世界模型 / 视频模型 / VLA 方向

### Review 范围

```text
1. World model
2. Video world model
3. Latent dynamics model
4. Action-conditioned video prediction
5. Vision-Language-Action model
6. Embodied AI
7. Robot foundation model
8. Spatial-temporal prediction
```

### 最低要求

至少 review **15 篇论文**。

### 交付物

```text
reviews/person_B_world_model_vla_review.md
```

### 必须输出表格

```text
Paper | Model Type | Input | Output | Task | Dataset | Metric | Limitation | Relevance
```

### 接受标准

```text
[ ] 至少 15 篇论文
[ ] 明确 world model 的主要技术路线
[ ] 明确 video model 和 world model 的区别
[ ] 明确 VLM 和 world model 的交叉点
[ ] 至少总结 5 个 world model research gaps
[ ] 至少提出 2 个可实验的新论文方向
```

---

## Person C：数据集、Benchmark、指标方向

### Review 范围

```text
1. VQA datasets
2. Visual grounding datasets
3. Spatial reasoning datasets
4. Video understanding datasets
5. Embodied AI datasets
6. Robot manipulation datasets
7. World model evaluation benchmarks
8. Scene graph / state-change datasets
```

### 最低要求

至少 review **12 个 dataset / benchmark / metric paper**。

### 交付物

```text
reviews/person_C_dataset_metric_review.md
```

### 必须输出表格

```text
Dataset | Input Type | Task | Annotation | Metric | Size | Suitable for VLM | Suitable for World Model
```

### 接受标准

```text
[ ] 至少 12 个 benchmark 或 metric
[ ] 至少筛选出 3 个可直接使用的数据集
[ ] 至少提出 1 个自建 mini benchmark 方案
[ ] 明确每个数据集适合 VLM 还是 world model
[ ] 给出 500 / 1000 / 5000 samples 三档实验规模建议
```

---

## Person D：Related Work、选题策略、论文结构方向

### Review 范围

```text
1. VLM survey
2. World model survey
3. Video generation / video understanding survey
4. Multimodal reasoning survey
5. Embodied AI / VLA survey
6. TCSVT / TOMM / CVPR / ICCV / ICLR / NeurIPS 相关论文
```

### 最低要求

至少 review **15 篇论文**。

### 交付物

```text
reviews/person_D_related_work_strategy.md
```

### 接受标准

```text
[ ] 至少 15 篇论文
[ ] 给出 Related Work 初步结构
[ ] 给出 3 个候选论文题目
[ ] 给出 3 个候选投稿 venue
[ ] 给出 novelty statement 初稿
[ ] 给出 reviewer 可能质疑点列表
```

---

## Phase 1 总体验收标准

Phase 1 完成后，必须生成：

```text
docs/literature_review_summary.md
```

该文件必须包括：

```text
1. 4 个人 review 的论文列表
2. VLM 方向 research gaps
3. World model 方向 research gaps
4. VLM + world model 交叉方向 research gaps
5. 可用数据集
6. 可用 baseline
7. 推荐主线方向
8. 不推荐方向及原因
9. 下一阶段需要解决的问题
```

如果有人未完成，必须记录：

```text
Incomplete Item:
Owner:
Missing Deliverable:
Current Status:
Impact on Next Phase:
Required Fix:
New Deadline:
```

---

# Phase 2：选题收敛与 Research Gap 定义

## 阶段目标

从 Phase 1 的 review 中确定最终论文方向。

## 4 人并行任务

| 人员       | 子任务                        | 交付物                                 |
| -------- | -------------------------- | ----------------------------------- |
| Person A | 评估 VLM reasoning 方向是否有创新空间 | `docs/topic_A_vlm_reasoning.md`     |
| Person B | 评估 world model 方向是否有创新空间   | `docs/topic_B_world_model.md`       |
| Person C | 评估三个方向的数据和实验成本             | `docs/topic_dataset_feasibility.md` |
| Person D | 整合最终选题决策                   | `docs/topic_decision_v1.md`         |

## 接受标准

```text
[ ] 每个候选方向都有 problem statement
[ ] 每个方向都有至少 3 篇核心相关论文
[ ] 每个方向都有可用 dataset
[ ] 每个方向都有可实现 baseline
[ ] 明确最终主方向
[ ] 明确备用方向
[ ] 明确不做哪些方向
```

---

# Phase 3：方法设计

## 阶段目标

把选题变成可以实验验证的方法。

---

## 如果选择 VLM 方向

### 方法模块建议

```text
1. Scene representation extraction
2. Object-state parsing
3. Spatial relation reasoning
4. Visual question answering
5. Consistency scoring
6. Error type classification
```

### 论文可能贡献

```text
1. 提出一个面向 VLM 的 world-state reasoning benchmark
2. 设计 object-state / spatial-relation / temporal-change 三类任务
3. 评估多个主流 VLM 的世界状态理解能力
4. 提出一个 consistency-based evaluation metric
```

---

## 如果选择 World Model 方向

### 方法模块建议

```text
1. Observation encoding
2. Latent world-state construction
3. Future state prediction
4. State transition consistency check
5. Physical plausibility scoring
```

### 论文可能贡献

```text
1. 提出一个评估 world model 状态一致性的框架
2. 对比 video prediction model、video VLM、VLA model
3. 设计 object permanence、motion consistency、action consequence 任务
4. 分析世界模型的错误类型
```

---

## 如果选择 VLM + World Model 交叉方向

### 方法模块建议

```text
1. Image / video observation input
2. World-state representation extraction
3. Scene graph construction
4. Future state question answering
5. State consistency evaluation
6. Failure mode taxonomy
```

### 论文可能贡献

```text
1. 统一评估 VLM 和 world model 的世界状态理解
2. 构建 world-state reasoning benchmark
3. 设计空间、时序、因果、动作结果四类任务
4. 对主流 VLM / video model / VLA model 进行系统比较
```

---

## 4 人并行任务

| 人员       | 子任务                                 | 交付物                                     |
| -------- | ----------------------------------- | --------------------------------------- |
| Person A | 写 formal problem definition         | `paper/sections/problem_definition.tex` |
| Person B | 设计 model pipeline                   | `docs/model_pipeline_design.md`         |
| Person C | 设计 evaluation metrics               | `docs/metrics_definition.md`            |
| Person D | 写 method overview 和 contribution 初稿 | `paper/sections/method_overview.tex`    |

## 接受标准

```text
[ ] 有完整 problem definition
[ ] 有 method pipeline
[ ] 有至少 3 个核心指标
[ ] 有 framework figure 草图
[ ] 有符号表
[ ] 明确输入和输出
[ ] 明确哪些 claim 可以实验验证
```

---

# Phase 4：数据集与实验协议设计

## 阶段目标

确定实验怎么做，避免后面结果支撑不了论文。

---

## 4 人并行任务

| 人员       | 子任务                                | 交付物                                       |
| -------- | ---------------------------------- | ----------------------------------------- |
| Person A | 设计核心假设和预期结果                        | `docs/hypotheses.md`                      |
| Person B | 设计模型运行配置                           | `configs/model_config_plan.md`            |
| Person C | 建立 dataset manifest 和 question set | `data/benchmark_manifest_v1.json`         |
| Person D | 写 experiment section skeleton      | `paper/sections/experiments_skeleton.tex` |

## 推荐实验任务

```text
Task 1: Object State Recognition
模型是否能识别物体当前状态，例如 open / closed, full / empty, broken / intact。

Task 2: Spatial Relation Reasoning
模型是否能判断 left / right, inside / outside, above / below, occluded / visible。

Task 3: Temporal Change Understanding
模型是否能理解前后帧中的状态变化。

Task 4: Action Consequence Prediction
给定一个动作，模型是否能预测合理结果。

Task 5: Future State QA
给定视频或多帧图像，问模型下一步会发生什么。
```

## 接受标准

```text
[ ] 至少 2 个 public datasets
[ ] 至少 3 个模型
[ ] 至少 4 个 baselines
[ ] 至少 3 个 evaluation metrics
[ ] 至少 5 个 random seeds 或重复实验设置
[ ] 每个实验都有 expected table format
```

---

# Phase 5：代码实现与小规模验证

## 阶段目标

先跑通小规模实验，确认技术路线可行。

## 4 人并行任务

| 人员       | 子任务                           | 交付物                             |
| -------- | ----------------------------- | ------------------------------- |
| Person A | 检查方法和代码输出是否一致                 | `docs/method_code_alignment.md` |
| Person B | 实现模型推理和结果保存                   | `src/run_models.py`             |
| Person C | 准备 debug subset               | `data/debug_subset.json`        |
| Person D | 整理 pilot results 和 case study | `docs/pilot_results.md`         |

## 小规模验证要求

```text
[ ] 至少 50 个样本
[ ] 至少 2 个模型
[ ] 至少 2 个 baseline
[ ] 至少 2 个 metrics
[ ] 至少 5 个成功案例
[ ] 至少 5 个失败案例
```

## 接受标准

```text
[ ] 代码可以通过 config 运行
[ ] 输出结果保存为 csv/json
[ ] 每个样本有 input、model output、metric result
[ ] 失败案例有原因分析
[ ] 结果足够支持进入主实验
```

---

# Phase 6：主实验与 Ablation

## 阶段目标

完成论文核心结果。

## 4 人并行任务

| 人员       | 子任务                               | 交付物                                         |
| -------- | --------------------------------- | ------------------------------------------- |
| Person A | 负责 VLM reasoning 结果解释             | `experiments/vlm_reasoning_analysis.md`     |
| Person B | 负责 world model / video model 批量运行 | `experiments/world_model_outputs/`          |
| Person C | 负责 baseline 和 ablation            | `experiments/baseline_ablation_results.csv` |
| Person D | 负责表格、图和结果文字                       | `paper/sections/results.tex`                |

## 主实验列表

```text
Experiment 1: Model Comparison
比较不同 VLM / video model / world model。

Experiment 2: Task Difficulty Analysis
比较 object state、spatial relation、temporal change、action consequence。

Experiment 3: Robustness to Input Type
比较 single image、multi-image、video、text instruction。

Experiment 4: Ablation Study
去掉 scene graph、去掉 temporal context、去掉 object-state prompt 等。

Experiment 5: Failure Mode Taxonomy
统计模型常见错误类型。
```

## 接受标准

```text
[ ] 主方法结果完成
[ ] baseline comparison 完成
[ ] ablation study 完成
[ ] task difficulty analysis 完成
[ ] qualitative examples 完成
[ ] 所有表格有 mean/std 或 confidence interval
[ ] 所有结果可以复现
```

---

# Phase 7：论文初稿整合

## 阶段目标

形成完整论文初稿。

## 4 人并行任务

| 人员       | 子任务                                              | 交付物                                                                  |
| -------- | ------------------------------------------------ | -------------------------------------------------------------------- |
| Person A | 完成 Problem Definition 和 Method                   | `paper/sections/problem_definition.tex`, `paper/sections/method.tex` |
| Person B | 完成 Implementation Details                        | `paper/sections/implementation.tex`                                  |
| Person C | 完成 Experiments 和 Appendix                        | `paper/sections/experiments.tex`, `paper/appendix.tex`               |
| Person D | 完成 Abstract、Introduction、Related Work、Conclusion | `paper/main.tex`                                                     |

## 初稿接受标准

```text
[ ] Abstract 完成
[ ] Introduction 完成
[ ] Related Work 完成
[ ] Problem Definition 完成
[ ] Method 完成
[ ] Experiments 完成
[ ] Results 完成
[ ] Ablation 完成
[ ] Limitation 完成
[ ] Conclusion 完成
[ ] Appendix 完成
[ ] References 完成
```

---

# Phase 8：内部 Review 与投稿准备

## 阶段目标

模拟审稿人，提前发现问题。

## 4 人并行任务

| 人员       | Review 角度                    | 交付物                                        |
| -------- | ---------------------------- | ------------------------------------------ |
| Person A | VLM 方法和 reasoning claim 是否成立 | `reviews/internal_review_A_vlm.md`         |
| Person B | world model 相关表述是否准确         | `reviews/internal_review_B_world_model.md` |
| Person C | 实验设计和指标是否充分                  | `reviews/internal_review_C_experiment.md`  |
| Person D | 写作、结构和 venue fit             | `reviews/internal_review_D_writing.md`     |

## 内部 review 模板

```text
Major Weaknesses:
Minor Weaknesses:
Missing Experiments:
Unclear Claims:
Potential Reviewer Criticism:
Required Fix Before Submission:
Score:
Recommendation:
```

## 接受标准

```text
[ ] 每个人至少提出 5 条 comments
[ ] 所有 comments 进入 revision tracker
[ ] 每条 comment 有 owner
[ ] 每条 comment 有 deadline
[ ] 每条 comment 有状态
```

状态格式：

```text
OPEN / FIXING / FIXED / WON'T FIX
```

---

# 统一任务追踪模板

```text
Task ID:
Phase:
Owner:
Task Description:
Deliverable:
Deadline:
Status:
Reviewer:
Acceptance Criteria:
Blocking Issues:
Evidence of Completion:
```

示例：

```text
Task ID: P1-A-01
Phase: Phase 1 Literature Review
Owner: Person A
Task Description: Review 15 papers on VLM reasoning, grounding, spatial reasoning, and visual QA
Deliverable: reviews/person_A_vlm_reasoning_review.md
Deadline: Week 1 Friday
Status: IN PROGRESS
Reviewer: Person D
Acceptance Criteria:
- At least 15 papers reviewed
- Each paper includes limitation and relevance
- At least 3 baselines identified
Blocking Issues: None
Evidence of Completion: File path or pull request link
```

---

# 每周检查格式

每个人每周必须回答：

```text
1. 本周完成了什么？
2. 交付物在哪里？
3. 当前卡在哪里？
4. 下周准备交付什么？
```

项目负责人汇总：

```text
Completed this week:
Blocked tasks:
Delayed tasks:
Tasks requiring decision:
Next week target:
```

---

# 最终完成标准

```text
[ ] Phase 1 literature review complete
[ ] 选题方向确定
[ ] Research gap 明确
[ ] Method fully specified
[ ] Dataset protocol fixed
[ ] Code can run from config
[ ] Pilot experiment complete
[ ] Main experiment complete
[ ] Baseline comparison complete
[ ] Ablation complete
[ ] Figures complete
[ ] Tables complete
[ ] Full paper draft complete
[ ] Internal review complete
[ ] Major issues fixed
[ ] Limitation section written
[ ] Reproducibility checklist complete
```

---

# 最重要管理原则

```text
一个任务 = 一个 owner
一个 owner = 一个 deliverable
一个 deliverable = 一个 reviewer
一个 reviewer = 一个 acceptance decision
```

4个人每个人都有各自的文件夹和branch，每个人都merge 到main brach, 4 个人可以每个阶段同时推进；但谁没完成、哪个任务卡住、哪个人交付物不能进入下一阶段，可以马上查出来。
