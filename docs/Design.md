# Design of Concept

## 阶段职责单一

- 检测，精读，综合，再拓展，明确拆分.
- 每个stage只做一类主要工作，避免混成一个大杂烩.

## 松耦合

- 下一个stage不依赖聊天上下文记忆。
- 只依赖上一个stage的交付包，状态文件，目录说明。
- 这样可以换session，换调用者，换执行环境。

## 契约优先

- 每一个stage都有明确输入和输出
- 不是`看着目录猜`，而是靠状态文件，manifest, directory index来衔接。

## 人类可读 + 机器可读 双规

- 一方面又Markdown报告，方便人看。
- 一方面有json / manifest / routing status, 方便后续 stage 自动消费。

## 可审计

- 每一步都尽量保留来源，状态，用途，是否有效。
- 这样后面出问题能追溯，不会变成黑盒流水线。

## 可恢复

- 任一stage结束时，交付应该足够完整。
- 即使重新开一个新sission, 也能从 bundle 继续， 而不是必然依赖之前对话。

# pipeline 分工

## paper-source-collection

- 负责"找材料"
- 把目标方向相关论文尽量高召回的收集下载下来。
- 下载到本地可用的 pdf /latex.
- 如果是ICLR, 同时拉取 OpenReview 的审稿，回复, decision.
- 产出的是"可交付的论文集合"，不是深度分析。

## paper-deep-reading

- 负责"读论文"。
- 对已下载论文做逐篇精读，生成详细报告和结构化中间结果。
- 核心是把论文里面的问题，方法，实验，局限，review context读透。
- 不再负责继续外部检索。

## report-innovation-graph

- 负责"从深读结果里抽象结构和方向".
- 把多篇深读结果汇总成方向图，问题层级，创新点，候选研究方向。
- 它做的是综合，归纳，图谱化，而不是重新精读单篇论文。

## select-direction-literature-expansion

- 负责"围绕一个已选方向继续扩文献".
- 用户从候选方向里选一个，或者直接给科学问题。
- 然后围绕这个方向继续补找2025/2026 顶会顶刊论文，可借鉴但未关联的论文。
- 它是一次定向扩展，而不是无界搜索。

## specified-direction-deepread-graph-refresh

- 负责"把新补进来的论文重新吸收到方向图里".
- 先对新论文做定向精度，再基于同一指定方向刷新图和报告.
- 最后判断是否还需要再回去补文献。

## selected-direction-literature-expansion <-> specified-direction-deepread-graph-refresh

- 这两个阶段构成一个loop.
- 前者做"广度拓展"，后者做"深度吸收与重构"。
- 一轮轮迭代，直到方向足够稳定，文献缺口足够少。

## 进入loop的时间点

- 已经完成 report-innovation-graph 
- 已经拿到候选方向。
- 用户指定了一个方向，或者指定了一个/ 多个科学问题。
- 然后开始执行 select-direction-literature-expansion。
- 从这一刻起，就进入到了 elected-direction-literature-expansion <-> specified-direction-deepread-graph-refresh。
- 也就是先补文献，再吸收新文献并刷新图，再看是否需要补新文献。

## Q & A 

- 为什么最后两个阶段是loop
- 已经拿到候选方向。
- 用户指定了一个方向，或者指定了一个/ 多个科学问题。
- 然后开始执行 select-direction-literature-expansion。
- 从这一刻起，就进入到了 elected-direction-literature-expansion <-> specified-direction-deepread-graph-refresh。
- 也就是先补文献，再吸收新文献并刷新图，再看是否需要补新文献。