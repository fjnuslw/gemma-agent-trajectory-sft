# 公开版本说明

本版本于2026-11-16整理，发布范围为技术文档、脱敏证据摘录、机器可读指标和CPU教学Notebook。GitHub与Kaggle共享同一个Notebook；新增代码仅作合成例子的教学演算。

## 阅读入口

- [技术总览](technical-overview.zh-CN.md)：架构、数据资格、训练与交付设计。
- [证据与指标](evidence-and-metrics.zh-CN.md)：42项主张、对应状态和限制。
- [技术追问与表达示例](interview-guide.zh-CN.md)：解释设计取舍，不证明个人独立完成范围。
- [脱敏源摘录](source-excerpts.md)：项目相对文件名、原行号和有限摘录。
- [实验指标](../evidence/metrics.json)：逐版本分母、来源和验证边界。
- [教学Notebook](../notebooks/gemma-agent-trajectory-sft.ipynb)：同时镜像到Kaggle。

## 可复现范围

Notebook的四个代码单元使用Python标准库演示题/阶段均权、当前目标mask、selected CE与共享K/V键布局；输入均为合成数据，不读取任务、模型或凭据。已在本地逐单元检查。平台发布使用Quick Save，公开版本没有启动Kaggle计算任务。

完整31B训练、官方任务执行和历史HTTP审计需要对应资源与环境，不能用教学单元代替。历史训练和任务指标来自既有回传报告。本仓库保留负面结果，不声称LoRA稳定提分、隐藏泛化改善、奖牌或达到0.22。

## 实验状态

C03已冻结990个目标回合、160个任务ID。当前最后接纳的进度信息仅为用户报告23/124步；完整训练回传尚待核实。base/step62/step93/step124各T0.2/T1.0的12题开发面板，共96题次，目前仍按待执行/待回传记录。外部留出环境与精确resume入口也未完成。

这些状态限定于本次证据整理截点；后续进展须附新回传后更新，不能从旧计划推断完成。

## 来源与发布边界

官方SDK/ADK工具、独立判分、模型模板、LoRA、FSDP2和vLLM属于上游能力。query分块与selected CE等内存策略参考公开训练kit；项目工作是适配、组合、约束和验证。来源见[NOTICE](../NOTICE.md)。

官方规则的公开代码条款可从[本竞赛规则页](https://www.kaggle.com/competitions/gemma-4-developer-agent/rules)检索到，要求在本竞赛关联的论坛或Notebook分享。因此同一份教学代码与技术材料也通过关联本竞赛的Kaggle Notebook发布。该条款不替代具体第三方材料的授权。

只发布明确列出的文档与教学材料。真实部署身份、地址、账号凭据、原始HTTP、任务数据、权重和optimizer均不属于本次附件。源摘录经脱敏，不主张与原文件全字节等价；它用于解释实现依据，不是完整可运行源码发行。
