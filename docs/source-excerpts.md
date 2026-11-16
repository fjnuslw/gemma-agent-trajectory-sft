# 脱敏源证据摘录

本页来自2026-11-16的定向源码与既有报告阅读。路径为项目相对路径；行号指原始本地文件。
摘录经过路径、运维身份和无关账号脱敏，长行可能省略后半部分；它不是原文件的字节等价副本，也不能替代完整执行证据。
历史“已验证”归属于相应原报告，本次没有重新运行模型、训练、任务评测或全量token审计。

## s001

来源：`learning_pipeline/specs/FRESH_SFT_C03.md:77`

````text
77: ## D096：独立检查点面板（2026-11-16）
78: 
79: 用户报当前训练step23/124、loss1.25→0.35、LR9.6e-5，四卡健康；此为用户进度转述，尚未收到本轮训练原件，不冒称本地已接纳完成。原990行/124步/保存31-62-93-124配方和训练发行零修改。
80: 
81: 用户新增明确授权：训练正常收尾并完成原封包后，以原12题开发面板对底座、50%、75%、100%各跑T0.2/T1.0。GPU0/1/2/3依次对应四候选，四worker并行，卡内先T0.2后T1.0，每臂独立原快照，共8臂96题次。覆盖此前只首选base/50/100与T0.2的建议；12题身份和顺序未变，旧预声明保存在development_panel_initial_20261116.json。25%仅保留，未扩大为八轮129。
82: 
83: 统一入口新增`python -m learning_pipeline checkpoint-panel-package`，轻量发行`dist/harness_k_a800/checkpoint_panel_c03_r01`，服务器仅新目录展开。复用成熟官方SDK/裁判runner及原258控制证据；全129校准和12题执行范围分别核，基座4成员/无adapter与三checkpoint6成员/真实注册路由均覆盖。原生820 FP32用原导出函数转换840 BF16、r16/a16不变；所有Agent文本与预算只允许温度及adapter声明差异。短IPC、UUID→index、同卡清理后续温、卡间失败隔离保持。
84: 
85: 默认analysis回传全部轨迹和原始逐题判分，50/75输入权重保留服务器，manifest明确本地尚无外部引用，不能用final补中间点；--full可自含。根据8臂逐题矩阵、空patch/额度/耗时和必要轨迹选候选，不根据最低训练loss或一题微差自动判胜。全部为暴露开发题，无隐藏泛化/线上分数保证。当前交接已准备，实际面板/新129/Kaggle均NOT_RUN；训练原包与所有历史不变。
````

## s002

来源：`reports/fresh_lora_implementation_20261116/status.json:1`

````text
1: {
2:   "schema": "fresh-sft-execution-progress-v1",
3:   "date": "2026-11-16",
4:   "decision": "TRAINING_USER_REPORTED_IN_PROGRESS_PANEL_HANDOFF_READY",
5:   "spec": "learning_pipeline/specs/FRESH_SFT_C03.md",
6:   "research": "COMPLETE_6_INDEPENDENT_REPORTS",
7:   "local_selection": {
8:     "status": "COMPLETE",
9:     "reviewed_tasks": 129,
````

## s003

来源：`reports/checkpoint_panel_c03_20261116/overview.md:3`

````text
3: 2026-11-16。用户最新授权为**四候选×两温度×12题，共96题次**，覆盖先前三候选/单温度建议。原C03训练和124步/四快照配方不动；用户报step23/124、loss1.25→0.35、LR9.6e-5，尚未收到训练原件，本地没有宣布训练完成或提分。
4: 
5: |GPU|候选|顺序|
6: |---|---|---|
7: |0|官方QAT底座，无LoRA|T0.2 → 清理 → T1.0|
8: |1|step62 / 50%|T0.2 → 清理 → T1.0|
9: |2|step93 / 75%|T0.2 → 清理 → T1.0|
10: |3|step124 / 100%|T0.2 → 清理 → T1.0|
11: 
````

## s004

来源：`learning_pipeline/server/requirements_train.txt:1`

````text
1: torch==2.10.0+cu128
2: transformers==5.13.1
3: peft==0.21.2
4: accelerate==1.15.0
5: compressed-tensors==0.15.0.1
6: tokenizers==0.22.2
7: safetensors==0.8.0
8: Jinja2==3.1.6
````

## s005

来源：`baseline_settings.py:109`

````text
109: EXPECTED_VERSIONS = {
110:     "swegemma": "0.2.11",
111:     "adk-submission": "0.2.13",
112:     "adk-eval-core": "0.1.0",
113:     "google-adk": "1.36.1",
114:     "google-genai": "2.11.0",
115:     "fastapi": "0.136.3",
116:     "starlette": "0.52.1",
117:     # Existing host dependencies used by both the service and visible tests.
````

## s006

来源：`learning_pipeline/configs/r04_cycle01.json:14`

````text
14:   "expected_evaluation": {
15:     "verification_timeout_seconds": 300,
16:     "timeout_seconds": 240,
17:     "max_time_minutes": 8,
18:     "max_tool_calls": 28,
19:     "max_turns": 80
20:   },
21:   "expected_sampling": {
22:     "temperature": 0.2,
````

## s007

来源：`runtime/baseline.py:302`

````text
302:         evaluator = Evaluator(EvalConfig(
303:             tasks_path=settings.DATA_DIR / "tasks.jsonl", snapshots_dir=settings.DATA_DIR / "snapshots",
304:             results_dir=settings.OUTPUT_DIR / "results", submission_dir=agent_dir, models=models,
305:             sandbox="subprocess",
306:             graph_dir=str(settings.DATA_DIR / "graphs"), embeddings_dir=str(settings.DATA_DIR / "embeddings"),
307:             wheels_dir=settings.DATA_DIR / "wheels", limits=limits,
308:             generation_constraints=generation_constraints, adapter_manifest=adapters,
309:             **evaluation, display_mode="quiet", enable_sandbox_testing=True,
310:             context_cache_config=ContextCacheConfig(min_tokens=2048, ttl_seconds=1800, cache_intervals=10),
````

## s008

来源：`runtime/baseline.py:343`

````text
343:                 logger.info("[%d/%d] Evaluating %s", index, len(tasks), task.instance_id)
344:                 task_started_at = time.monotonic()
345:                 result = await watch_evaluation(
346:                     evaluator.evaluate_task(task=task, task_index=index, total_tasks=len(tasks), context=context),
347:                     server, recorder, settings.OUTPUT_DIR / "vllm.log", logger,
348:                     check_evidence=check_evidence,
349:                     preserve_interrupted=lambda: evaluator.sandbox.preserve_current("interrupted"),
350:                 )
351:                 summary = {
````

## s009

来源：`runtime/baseline.py:310`

````text
310:             context_cache_config=ContextCacheConfig(min_tokens=2048, ttl_seconds=1800, cache_intervals=10),
311:             events_compaction_config=EventsCompactionConfig(
312:                 compaction_interval=15, overlap_size=2, token_threshold=14336, event_retention_size=5,
313:             ),
314:         ))
315:         # The official manager is still empty here. The subclass only saves
316:         # Agent evidence before deletion; it never reads the verification workspace.
317:         if evaluator.sandbox.sandboxes:
318:             raise RuntimeError("Cannot replace an already active sandbox manager.")
````

## s010

来源：`reports/trajectory_cycle01_pair_r02_review_20261115/OVERVIEW.md:24`

````text
24: 两臂phase/cleanup闭合、not_run为空；包装器exit0，实际main python_exit1表示批内未解。全部请求使用同adapter别名、六工具及真实系统提示/采样核对通过，无落回底座证据。已返回可解析摘要167/141全部进入后续主请求；不能归为compact普遍丢失。无本轮OOM/IPC中断。
25: 
26: ## 值得保留与需要纠正
27: 
28: 1. **保留调用/恢复改善，补语义闭环。** 7205/T02的`if any(_netrc)`破坏None路径；5624把兄弟scope混合；7328删除中间history。这些需要旧行为和新行为一起成立的目标，单次“编辑成功”回执不足。
29: 2. **把已知证据落实为源码。** 12942/T02到第29次才编辑，额度已用完；9425/T1、14430/T1已见关键事实仍没有修复。同题另一臂能做，不代表当前状态已会稳定采取该动作。
30: 3. **学会选择能击中问题的检查。** 11194/T1只测UploadFile，遗漏bytes转换；3521/T02多做均匀字符探针，没覆盖混宽；3468回避原pipe触发，普通输出通过不能说明修好。
31: 4. **JSON合法不等于操作正确。** 15800/T1的sed `w`本想读源码却清空4694行；9753/T1通过调用用户函数来分类，真实运行错误后撤回合理。后者应学习正确替代，而不是硬保留已证明有错的实现。
32: 5. **不强行补未知原因。** 13713/T1已修改完整公开调用链，但正式仍missing且自测未运行，确切剩余根因未知；3052返回规范化、14791输出结构都须从公开可见来源建立期望，不能把裁判后验灌进教师。
````

## s011

来源：`reports/trajectory_cycle02_pair_r01_review_20261116/overview.md:14`

````text
14: 主要信号：低温重复相同搜索并忽略预算拒绝、空补丁和失实终答；高温产生补丁却修错接口/写坏语法；少数响应在文本/推理里写伪工具标记而未执行。39摘要均送达，但个别内容对编辑状态判断有误。不存在“工具提醒/摘要完全没送达”的解释。D04相关11题188目标的有限核查未找到坏短句直接复制，且有正确编辑目标；不能据此证明整个监督混合最优。
15: 
16: 记录523调用尝试＝447计费+32免费status+15免费submit+29额度拒绝；581HTTP元数据/路由已核，动作参数与ADK及16正式audit回执逐值绑定。可见reasoning全文深读8条T02与T1/11194，其余295条明确UNKNOWN/masked；未假称全文读完所有重复前缀或重审全部2090训练行。
17: 
18: 简报校正：14077仅T02通过，T1未运行；“精确172秒”是相近墙钟值，不是共同超时；13537才是两臂都过。恢复后原表确认GPU2 PID<OPERATION_ID>、GPU3 PID<OPERATION_ID>，UID987/目标UUID/脚本/驻留内存/holder日志一致；<OPERATION_ID>无原件支持。两臂safe_to_pack/无残余自有进程闭合，保持原STOPPED/failed状态，不回写为completed。
19: 
20: 本地判断：接纳这一份真实的部分评测证据；C02暂不自动晋级，不从本轮推断全129或隐藏集分数。训练产物有效性接纳D090保持，但有限能力测试出现回退，后续应采用短训练、checkpoint和单变量行为对照。
21: 
22: [独立结论](independent/review.md) · [逐题表](independent/cases.md) · [机器结论](independent/review.json) · [训练方法与最新公开源码调研](../lora_method_reassessment_20261116/overview.md) · [待讨论实验](../lora_method_reassessment_20261116/PROPOSED_NEXT_EXPERIMENTS.md)。没有新训练、模型/SDK/工具运行、采集、服务器或Kaggle操作，无新增hash，提示和预算保持。
````

## s012

来源：`runtime/request_log.py:1`

````text
1: """Passive localhost OpenAI relay for local diagnostics, not submission code.
2: 
3: Bodies are forwarded unchanged. ``response.raw`` contains HTTP entity bytes
4: (including SSE), after HTTP transfer framing, not a tokenizer/internal trace.
5: No HTTP headers are recorded. Call ``check()`` at task boundaries; a normal
6: context-manager exit also checks recorder integrity. HTTP/API failures and
7: cancelled or interrupted requests belong to the official evaluator's task
8: lifecycle, not this recorder's run-level policy. An unwritable disk can prevent
9: a terminal metadata update, but that recorder failure remains sticky in memory.
````

## s013

来源：`runtime/evidence.py:1`

````text
1: """Local observation of official events and Agent-workspace cleanup.
2: 
3: These helpers neither run an Agent loop nor change official tools or scoring.
4: Snapshots are bounded, non-atomic observations, never submission predictions.
5: """
6: 
7: import json
8: import os
9: import re
````

## s014

来源：`reports/trajectory_cycle01_pair_r02_review_20261115/OVERVIEW.md:5`

````text
5: 一个干净GPT-6.1 Sol/max完成语义独审，root核输入、统计和回传内容。所有258题有处置，6464原始工具动作与ADK/ATIF逐值绑定，7031请求核配置与路由。重点深审20温度分歧及共同丢解14485；不声称逐字阅读所有重复HTTP前缀、所有长stdout及全部reasoning。完整范围与逐题证据见[独审报告](independent/report.md)、[逐题案例](independent/cases.md)。
6: 
7: ## 结果与可比性
8: 
9: |指标|原r04无adapter T0.2|同r04+LoRA T0.2|同r04+LoRA T1.0|
10: |---|---:|---:|---:|
11: |resolved/129|51|48|52|
12: |缺必填字段的调用/涉及题|107/16|1/1|8/6|
13: |实际会话截止|5|1|1|
````

## s015

来源：`docs/BASELINE.md:155`

````text
155: [数据计划](../experiments/trajectory_dataset_r01/README.md)和[34项核验](../reports/trajectory_training_sampling_20261112/verification.json)仅检查129分区/任务槽/预声明触发/源包直字节及无掉题、无限制题误入正例的反例，不是新SDK/模型/训练结果。129开发评测保留，115候选/2空基线/12已知限制复用原控制，不重新启动CPU校准链。r04基线尚无结果；r03审查只用于选题，缺真实中间工作区不能继续旧会话。当前新采集0、可训练token导出0、已有两个原型不自动晋级。
156: 
157: 接手合同要求实际请求边界/工作区/工具状态/剩余预算一致；终局patch或摘要不能替代现场。窗口只监督当前目标，前缀中的先前正确教师动作也mask，避免重复训练。真实摘要触发不要求已有源码编辑，以覆盖未写入的错误进展摘要。后续采集实现及native模板标签验收另做，当前冻结发行和GPU2单臂交接不受影响。
158: 
159: ## 2026-11-12　r04-T02训练前基线交接：GPU2单臂
160: 
161: D064原SELF-v17-r04-T02 6842B ZIP整字节进入新交接，四成员与冻结件相等。继承r03已验证A800执行器/官方判分，单臂只允许t02，129序/任务资源/293包/98官方源及已知117/115/12校准口径保持；无新模型成绩。新run_single保存真实main退出、monitor停止和owned_server_cleanup证明；完整exit1允许，部分/源漂移/清理未闭合不产生正式baseline_record。CPU只短来源与实际SDK声明核对，不扩完整门禁。
162: 
163: 用户最终GPU2而非前案GPU3或旧GPU5。新device_binding只读核物理index2/PCI65:00.0/A80080G并保存真实UUID，后续复核不可改绑；原vLLM UUID→物理index适配和逻辑CUDA0的UUID验证保持。两次空闲快照/2秒GPU与60秒磁盘记录，3/5号不操作。预留按实际状态PRESERVED/RESTORED/RESERVED_NEW/NOT_STARTED_EMPTY/NOT_RESTORED如实记录，不能把原空闲说成恢复服务。
````

## s016

来源：`learning_pipeline/evaluation/panel_package.py:58`

````text
58:     # Verify the entire original calibration; project its descriptive view only afterwards.
59:     def gate(s):
60:         a=s.index('def verify_reference_binding()');b=s.index('def require_acceptance()',a)
61:         block=s[a:b].replace('settings.TASK_IDS','settings.CALIBRATION_TASK_IDS')
62:         block=replace(block,'    return coverage','    require(set(settings.TASK_IDS) <= set(coverage["target_discriminated_ids"]), "Panel includes a non-discriminating task")\n    return coverage')
63:         return s[:a]+block+s[b:]
64:     edit('runtime/alignment_gate.py',gate)
65:     edit('check_resources.py',lambda s:s.replace('!=settings.TASK_IDS','!=settings.CALIBRATION_TASK_IDS'))
66:     edit('runtime/device_binding.py',lambda s:replace(s,'INDEX = {"t02":2,"t10":3}[os.environ.get("HARNESS_K_VARIANT","t02")]',
````

## s017

来源：`docs/BASELINE.md:131`

````text
131: 原129/51与78未解不变，12校准限制单列。66非限制未解中A调用/编辑表示24、B动作推进/恢复/验证50、C实现语义覆盖22；全78分别28/51/22，可重叠且不是主要失败原因占比。严格SDK缺参/范围拒绝仅9/78，宽A还含静默字段错误、路径及编辑表示。C另44/66不确定；known-unfinished不等同理解错误。详细覆盖是完整动作序列索引和分段材料、关键原行，不是全HTTP/thinking全文。
132: 
133: root核129计费与summary相等、3666原HTTP可见调用与ADK参数一致、1063 budget_warning全部计数正确且进入后续主请求；正式/输出四成员与活动提示绑定。没有parser前原始文本，不证明格式错误只能由模型引起。58未解用满28，其中45在Agent300秒内；成功题也有14用满，不能单凭额度判流程错。[统一报告](../reports/self_v17_r04_failure_taxonomy_20261113/root_review.zh-CN.txt)与[统计核验](../reports/self_v17_r04_failure_taxonomy_20261113/root_summary.js … [长行省略]
134: 
135: ## 2026-11-13　r04基线已完成，训练数据未放行
136: 
137: 原r04-T02服务器129/51、phase/cleanup completed、not_run空，main exit1为完整批次含未解题；驱动29743.89s，Agent合计27842.71s。input/output与本地正式6842B四成员一致；用户线上截图.13，线上上传ZIP未下载。对r03 T02 52保持47/新4/丢5。[统计与绑定](../reports/trajectory_correction_r01_review_20261113/baseline_check.json)保存原口径，不推同题单轮为因果或预测隐藏分。
138: 
139: 数据包11训练行全部submit，238正标签含128 thought渲染token及110动作/handoff（含标记）；root和单Terra独立原生重编码相同，无编辑监督，0经验证教师纠正。5作业真实重放与官方时钟存在，但主要在教师队列等待时到截止，并非教师完成动作后解题失败。378项验收不覆盖动作mask与语义资格。[本地不放行记录](../reports/trajectory_correction_r01_review_20261113/training_gate.json)不改原包eligibility。仅只读数据/tokenizer检查，无新工具、裁判、模型或训练；下一采集方式与预算尚待明确，旧交接不重入。
````

## s018

来源：`reports/trajectory_cycle01_pair_r02_review_20261115/OVERVIEW.md:26`

````text
26: ## 值得保留与需要纠正
27: 
28: 1. **保留调用/恢复改善，补语义闭环。** 7205/T02的`if any(_netrc)`破坏None路径；5624把兄弟scope混合；7328删除中间history。这些需要旧行为和新行为一起成立的目标，单次“编辑成功”回执不足。
29: 2. **把已知证据落实为源码。** 12942/T02到第29次才编辑，额度已用完；9425/T1、14430/T1已见关键事实仍没有修复。同题另一臂能做，不代表当前状态已会稳定采取该动作。
30: 3. **学会选择能击中问题的检查。** 11194/T1只测UploadFile，遗漏bytes转换；3521/T02多做均匀字符探针，没覆盖混宽；3468回避原pipe触发，普通输出通过不能说明修好。
31: 4. **JSON合法不等于操作正确。** 15800/T1的sed `w`本想读源码却清空4694行；9753/T1通过调用用户函数来分类，真实运行错误后撤回合理。后者应学习正确替代，而不是硬保留已证明有错的实现。
32: 5. **不强行补未知原因。** 13713/T1已修改完整公开调用链，但正式仍missing且自测未运行，确切剩余根因未知；3052返回规范化、14791输出结构都须从公开可见来源建立期望，不能把裁判后验灌进教师。
33: 
34: D03并非没有源码学习：已有正确源码目标的12942/14485/5077/5624仍失分；同时，旧通过7205/9753/7427的最终核心代码因保护/来源/整call资格并未全进正目标。应按“已教仍失效”和“核心目标未教”分别处理，不能只用1990行或129题覆盖数判断训练充分。
````

## s019

来源：`learning_pipeline/collection/runner.py:69`

````text
69: def bind_execution(job,index,stage,receipt,previous,actual,responses):
70:     """Bind the last added proposal to actual SDK consumption and tool receipts."""
71:     teacher=job/f'teacher_{index:03d}';entry=responses[-1]
72:     request_index=len(responses)
73:     if receipt['replayed_responses']!=request_index:raise RuntimeError('Latest teacher response was not consumed by official replay')
74:     comparison=read(stage/'request_comparisons'/f'{request_index:04d}.json')
75:     state=read(stage/'state_comparisons'/f'{request_index:04d}.json')
76:     if not comparison['equal_under_declared_comparison'] or not state['equal']:
77:         raise RuntimeError('Teacher proposal input/state binding failed')
````

## s020

来源：`learning_pipeline/collection/runner.py:252`

````text
252:             req=read(stage/'next_request.json')
253:             if not (stage/'task_state.zip').is_file() or not (stage/'task_state.json').is_file() or not req.get('task_state'):
254:                 raise RuntimeError('Captured boundary lacks complete task-file state evidence')
255:             if idx==0:
256:                 comparison=compare_requests(original_cut,req['openai_request'])
257:                 write_new(stage/'original_cut_request_comparison.json',comparison)
258:                 if not comparison['equal_under_declared_comparison']:raise RuntimeError('Original cut request drift')
259:                 presentation_variant|=comparison['presentation_order_variant']
260:             if stop_fragment:
````

## s021

来源：`learning_pipeline/collection/equivalence.py:168`

````text
168: def compare_receipts(tool,a,b,args=None):
169:     equal=receipt(tool,a,args)==receipt(tool,b,args)
170:     ga=_presentation_groups(a,args) if tool=='run_command' else None
171:     gb=_presentation_groups(b,args) if tool=='run_command' else None
172:     variant=equal and ga is not None and gb is not None and ga==gb and a['stdout']!=b['stdout']
173:     return {'raw_equal':a==b,'equal_under_declared_comparison':equal,'comparison_version':COMPARISON_VERSION,
174:         'presentation_order_variant':variant,
175:         'reconstruction_condition':'RECONSTRUCTED_PRESENTATION_ORDER_VARIANT' if variant else 'OFFLINE_RECONSTRUCTION_DECLARED_COMPARISON',
176:         'comparison_rules':RULES,'not_a_byte_identical_receipt_claim':a!=b}
````

## s022

来源：`learning_pipeline/contracts/teacher_fragment_review.md:9`

````text
9: ## 先核真实执行
10: 
11: - 只接收 `job_result.status=FRAGMENT_CAPTURED`、`collection_mode=fragment`、`overall_verification=NOT_RUN`。`final_stage` 指最后真实下一请求边界，不是整题终态。该链没有新 TaskResult、独立判分或虚构 submit/final。
12: - `teacher_000` 到末项全部逐项处置，包括中途实际 summary；编号完整且唯一。每个实际提案须有 ACCEPTED 的 `response.json`、原 `proposal.txt`、`response.raw.json`、实际 `teacher_input.txt`、无 native 工具的 CLI 事件，以及下一 stage 的实际请求/状态比较和 ADK 调用及后续回执。`target_binding` 的执行计划不能代替 `execution_binding.status=EXECUTED`。
13: - 所有 stage（包括前缀 stage_000 和末 stage）必须真实 BOUNDARY_CAPTURED、cleanup 完成、`protected_mutation_observed=false`、cleanup 前保护字节相等、无 audit_error，并保留下一请求与 task_state.zip/json。首次原学生工作区未观测仍记 RECONSTRUCTED，不称严格同现场。
14: - 源码写入、验证或捕获结论、正 reasoning、保护/历史访问边界，须由独立审查者或执行协调员实际核实。不得因工具 ok 就认可未知代码。保护污染、历史/私有证据访问、未执行或工具拒绝不能正训。
15: - 非零检查仅在它确实是有效失败诊断时可接纳：lane 必须 VALID_DIAGNOSTIC，另写 `receipt_assessment=EXPECTED_DIAGNOSTIC_FAILURE`，并解释真实失败揭示的缺口。畸形脚本、错误断言或缺参拒绝不属于此例外。
16: 
17: ## 逐目标资格
````

## s023

来源：`learning_pipeline/contracts/teacher_natural_chain_review.md:7`

````text
7: - `overall_verification=SEE_OFFICIAL_TASK_RESULT`；所有实际proposal（包括summary和最后真实text）逐一处置，不为达到计划步数伪造工具动作。
8: - 中间stage保持真实 BOUNDARY_CAPTURED、next_request、task_state及实际请求/状态比较；最终stage必须真实 COMPLETED、task_result_created=true、对应真实boolean resolved，patch/verification与TaskResult直接字节相等。终态没有task_state.zip时，不补造或声称已捕获。
9: - 仍逐proposal核ACCEPTED payload/实际CLI事件/原始公开说明/实际teacher_input/下一stage请求与状态比较/唯一工具调用与后续回执。最后text另外须有REPLAYED_VISIBLE_TEXT绑定，且和最终ADK最后可见model文本一致。
10: - action-mode请求数可能包括真实text终答。分别记录实际工具数、真实终答数、summary数；不能把6个action-mode提案称6个工具动作。
11: - 动作lane可为LOCAL_ACTION_VERIFIED、VALID_DIAGNOSTIC；只有真实TaskResult.resolved=true且审核确认语义/范围的动作才能FULL_REPAIR_VERIFIED。官方通过不能代替逐动作语义审核。未解链仍可有有依据的局部目标。
12: - `final_approved=true`仅限实际最后一个action模式text，action_approved/reasoning_approved必须false，并提供 `qualification.final` 的独立或执行协调员来源。未出现终答不造target；summary不当终答。final的lane可null，表示事实终答资格；FULL字样仍要求resolved=true。
13: 
14: 示例终答target：
15: 
````

## s024

来源：`learning_pipeline/collection/cli_worker.py:27`

````text
27: For mode=action, decision_summary may contain a concise public decision explanation:
28: the already observed evidence and the next intended action/check, usually a few sentences.
29: Give no private chain-of-thought. Do not invent observations, future results or an answer key.
30: This field is explicitly authored training material, not a transcript of internal reasoning.
31: Use an empty decision_summary for compaction summaries and when no explanation is useful.
32: For kind=text use name="", arguments={} and the actual text. For kind=tool_call use
33: text="", the declared tool name and its native JSON arguments object. Do not JSON-encode
34: that object a second time. Supply the nullable line bounds and allow_multiple explicitly.
35: Return only this object. Do not provide private chain-of-thought, an explanation of these
````

## s025

来源：`learning_pipeline/native.py:107`

````text
107:     def complete_example(self,sample_id,request,target,selected_call_ids,metadata,reasoning=False,final_text=False):
108:         """Supervise the complete *current* accepted assistant completion.
109: 
110:         This is deliberately separate from ``example``: old action/joint masks
111:         keep their exact behaviour.  A no-reasoning derivative must remove its
112:         current thought explicitly before calling this method; retaining an
113:         unknown thought as masked answer context is not a completion example.
114:         Every real call in a multi-call completion must be qualified.
115:         """
````

## s026

来源：`learning_pipeline/native.py:32`

````text
32: class Native:
33:     def __init__(self,model=MODEL):
34:         self.model=Path(model);self.tokenizer=Tokenizer.from_file(str(self.model/'tokenizer.json'))
35:         env=ImmutableSandboxedEnvironment(trim_blocks=True,lstrip_blocks=True,extensions=['jinja2.ext.loopcontrols','jinja2.ext.do'])
36:         def fail(s):raise TemplateError(s)
37:         env.globals.update(raise_exception=fail,strftime_now=lambda fmt:datetime.now().strftime(fmt))
38:         env.filters['tojson']=lambda x,ensure_ascii=False,indent=None,separators=None,sort_keys=False:json.dumps(x,ensure_ascii=ensure_ascii,indent=indent,separators=separators,sort_keys=sort_keys)
39:         self.template=env.from_string((self.model/'chat_template.jinja').read_text(encoding='utf-8'))
40:     def render(self,messages,tools,generation):
````

## s027

来源：`learning_pipeline/native.py:116`

````text
116:         calls=target.get('tool_calls') or []
117:         if ({c['id'] for c in calls}!=set(selected_call_ids) or len({c['id'] for c in calls})!=len(calls)
118:                 or len(selected_call_ids)!=len(calls)):
119:             raise ValueError('Complete completion requires every real call to be selected exactly once')
120:         if not reasoning and any(target.get(k) for k in ('reasoning','reasoning_content')):
121:             raise ValueError('Unsupervised current reasoning must be explicitly removed from completion derivative')
122:         row=self.example(sample_id,request,target,selected_call_ids,metadata,reasoning=reasoning,final_text=final_text)
123:         prompt_tokens=row['metadata']['prompt_tokens'];ids=row['input_ids']
124:         reason=row['labels_reasoning']
````

## s028

来源：`reports/fresh_lora_implementation_20261116/data_builder/implementation.md:63`

````text
63: ## 实际 A 中后回合修复差量
64: 
65: root 第一次实际导出保留了 953 行/154 task，但全部 37 个 A 目标因 `A_ORIGINAL_PREFIX_NOT_EXACTLY_RECONSTRUCTED` 被隔离。该同型全层隔离是构建器遗漏源边界分支，不能解释为 37 个目标语义不合格；首次导出未 audit/freeze，root 保全为 attempt1 后重新导出。
66: 
67: 根因是作者 A 源模板的工具回执后 generation prefix 含 `<|channel>thought\n`，长 18 字符；本地当前 r04 归档模板把该入口留给 completion。作者 `render_example` 原码明确以 `prefix.endswith(FORCED)` 选择两种 completion 边界；首次实现仅测初始 index0，漏测工具回执后的分支。原历史本体没有差异。例如 `chatcmpl-fef133a1decb95466f12dc8ca6ec7a5e` 的 index6，当前模板 prefix 长 24410、源保存 24428；index7 为 25406/25424；index1 … [长行省略]
68: 
69: 修复按实际历史末消息 `role=tool`、且规范渲染必须严格以 `<tool_response|>` 结束时，确定性重建源 generation 的 FORCED 入口；没有从任意差异里猜测或忽略内容。之后仍要求完整 source prefix 逐字节相等，按作者原分支构建完整 completion 并核 IDs，另增加 prompt/completion/context token 数全相等检查。当前训练派生仍用真实本地模板渲染，没有往训练输入人工追加源 FORCED 入口。
70: 
71: `check_a_multiturn.py` 对全部 **14 条 A/382 个目标** 重建通过：14 初始入口、368 工具回执后入口；源 prefix、completion、completion IDs、三种 token 数均精确相等。两个 curator selection 的 **37/37 A 目标** 全部重建通过。完整逐目标结果在 `a_multiturn_reconstruction.json`，失败列表为空。此为机械渲染绑定，不是重新语义审查或 SDK 重执行。
````

## s029

来源：`reports/fresh_lora_implementation_20261116/overview.md:13`

````text
13: ## 最终数据不是原2090行重复训练
14: 
15: | 来源 | 目标回合 | 任务ID | 实际目标系数质量 |
16: |---|---:|---:|---:|
17: | 本地Gemma合格行为（含选中旧来源） |245|65|27.9219% |
18: | 本地Sol纠偏（含选中旧来源） |475|76|42.7031% |
19: | 公开Native |233|41|25.6250% |
20: | 公开A转换来源 |37|6|3.7500% |
21: | 合计 |**990**|**160**|**100%** |
````

## s030

来源：`learning_pipeline/fresh.py:226`

````text
226: def _task_stage_weights(groups):
227:     """Uniform rows are traversed once, hence m=N/(T*K*n), never p alone."""
228:     counts=defaultdict(Counter)
229:     for task,stage in groups:counts[task][stage]+=1
230:     total=len(groups);tasks=len(counts)
231:     if not total:raise ValueError('No qualified fresh completion rows')
232:     for task,stages in counts.items():
233:         if not set(stages)&{'edit','verify','recover_finish'}:
234:             raise ValueError('Read-only task reached main-lane weighting: '+task)
````

## s031

来源：`learning_pipeline/training/train_adapter.py:112`

````text
112:     base_cfg=AutoConfig.from_pretrained(a.model,local_files_only=True)
113:     qdict=base_cfg.quantization_config
114:     if qdict.get('quant_method')!='compressed-tensors':raise RuntimeError('Expected archived packed QAT base')
115:     # Pinned Transformers 5.13.1 uses run_compressed=False, not the later
116:     # dequantize=True API. Restore existing packed values, no NF4 re-quantization.
117:     quant=CompressedTensorsConfig.from_dict(qdict);quant.run_compressed=False
118:     base,loading=Gemma4ForConditionalGeneration.from_pretrained(a.model,local_files_only=True,
119:         quantization_config=quant,dtype=torch.bfloat16,device_map={'':local_rank},attn_implementation='action_chunked',output_loading_info=True)
120:     loading={k:sorted(v,key=repr) if isinstance(v,set) else v for k,v in loading.items()}
````

## s032

来源：`learning_pipeline/training/train_adapter.py:134`

````text
134:     pattern=r'.*language_model\.layers\.\d+\.(self_attn\.(q|k|v|o)_proj|mlp\.(gate|up|down)_proj)$'
135:     targets=[n for n,m in base.named_modules() if re.fullmatch(pattern,n) and isinstance(m,torch.nn.Linear)]
136:     if not targets or any('vision' in n or 'audio' in n for n in targets):raise RuntimeError('Invalid text-only LoRA targets')
137:     text_cfg=base.config.get_text_config()
138:     expected_count=sum(6 if typ=='full_attention' and text_cfg.attention_k_eq_v else 7 for typ in text_cfg.layer_types)
139:     if text_cfg.num_hidden_layers!=60 or expected_count!=410 or len(targets)!=expected_count:
140:         raise RuntimeError('Expected exactly 410 existing text projections across 60 QAT-31B layers')
141:     for idx,layer in enumerate(base.model.language_model.layers):
142:         for branch,names in [('self_attn',['q_proj','k_proj','v_proj','o_proj']),('mlp',['gate_proj','up_proj','down_proj'])]:
````

## s033

来源：`learning_pipeline/training/fsdp_runtime.py:18`

````text
18: def shard_frozen_decoders(objective, base, trainable, *, device, expected_layers=60):
19:     from torch.distributed.device_mesh import init_device_mesh
20:     from torch.distributed.fsdp import fully_shard
21:     from torch.distributed.tensor import DTensor
22: 
23:     layers=base.model.language_model.layers
24:     if len(layers)!=expected_layers:raise ValueError('Unexpected decoder layer count')
25:     names=[(n,list(p.shape),str(p.dtype)) for n,p in trainable]
26:     structure={'adapters':names,'base_parameters':parameter_inventory(base)}
````

## s034

来源：`learning_pipeline/training/fsdp_runtime.py:88`

````text
88: def average_adapter_gradients(trainable, bucket_bytes=16*1024**2):
89:     """One collective validation before bounded, deterministically ordered sums."""
90:     device=trainable[0][1].device
91:     invalid=torch.zeros((),dtype=torch.int32,device=device)
92:     for _,p in trainable:
93:         if p.grad is None:invalid.add_(1)
94:         else:invalid.add_((~torch.isfinite(p.grad).all()).to(torch.int32))
95:     dist.all_reduce(invalid,op=dist.ReduceOp.MAX)
96:     if int(invalid):raise RuntimeError('Missing/nonfinite LoRA gradient on at least one rank')
````

## s035

来源：`learning_pipeline/training/fsdp_runtime.py:34`

````text
34:     # Restored compressed-tensors Linear modules can retain integer Parameters
35:     # such as weight_shape. Torch 2.10 FSDP2 first wraps them with requires_grad
36:     # defaulting to True, even though they were frozen. Preserve, do not cast or
37:     # delete this state. Floating restored weights and scales are still sharded.
38:     nonfloat={n:p for n,p in layers.named_parameters() if not p.is_floating_point()}
39:     if any(p.requires_grad for p in nonfloat.values()):raise ValueError('Nonfloating trainable state is not supported')
40:     auxiliary_bytes=sum(p.numel()*p.element_size() for p in nonfloat.values())
41:     auxiliary=[{'name':n,'shape':list(p.shape),'dtype':str(p.dtype),'bytes':p.numel()*p.element_size()} for n,p in nonfloat.items()]
42:     with torch.no_grad():
````

## s036

来源：`learning_pipeline/training/train_core.py:13`

````text
13: def _chunked_mlp_forward(self, hidden_states):
14:     """Chunk the position-wise MLP, not attention or the causal input sequence."""
15:     if hidden_states.ndim!=3 or hidden_states.shape[1]==0:
16:         raise ValueError('MLP chunking expects a nonempty batch/sequence/hidden tensor')
17:     pieces=[]
18:     for block in hidden_states.split(self._trajectory_chunk_tokens,dim=1):
19:         original=self._trajectory_original_forward
20:         if torch.is_grad_enabled():
21:             output=checkpoint(original,block,use_reentrant=False,preserve_rng_state=True)
````

## s037

来源：`learning_pipeline/training/train_core.py:223`

````text
223: def chunked_attention(module,query,key,value,attention_mask,dropout=0.0,scaling=None,sliding_window=None,**kwargs):
224:     """Training-only batch1 causal text attention, including Gemma local windows.
225: 
226:     No caches/padding/multimodal masks are accepted. Key/value GQA is explicit.
227:     Query chunks are recomputed for backward, without an L x L attention matrix.
228:     """
229:     if query.shape[0]!=1 or query.shape[-2]!=key.shape[-2] or key.shape[-2]!=value.shape[-2]:
230:         raise ValueError('Only unpadded batch1 full-sequence training is supported')
231:     if dropout:raise ValueError('This recipe fixes attention dropout to zero')
````

## s038

来源：`learning_pipeline/training/train_core.py:276`

````text
276: def selected_action_loss(model,input_ids,labels,head_chunk=64,token_weights=None,*,diagnostic_masks=None):
277:     """Compute exact causal CE on supplied targets only, without full-vocab L logits."""
278:     if input_ids.ndim!=2 or input_ids.shape[0]!=1 or labels.shape!=input_ids.shape:
279:         raise ValueError('Microbatch must be one intact sequence')
280:     if int(labels[0,0])!=-100:raise ValueError('First token cannot be a causal target')
281:     positions=torch.nonzero(labels[0]!=-100,as_tuple=False).flatten()
282:     if positions.numel()==0:raise ValueError('No action labels')
283:     base=model.get_base_model() if hasattr(model,'get_base_model') else model
284:     output=base.model(input_ids=input_ids,use_cache=False,return_dict=True)
````

## s039

来源：`learning_pipeline/training/SOURCES.md:7`

````text
7: ## 可核作者源码
8: 
9: - [Scott Weeden 的公开训练kit](https://www.kaggle.com/datasets/scottweeden/gemma-4-developer-agent-tool-lora-kit)：本地归档为 `vendor/lora_research_20261112/scott_kit/4/scripts/train.py`（43326字节）及 `0/recipe/gemma4_31b_peft_recipe.md`（6607字节）；原始来源和许可记录见该目录外的 `SOURCE_NOTES.txt`。实际代码提供了按query分块的checkpoint attention、仅目标位置的chunked CE、 … [长行省略]
10: - 上述作者实际脚本还使用NF4，并在注意力注册失败时将长度降至6144、过滤长行；这些行为不进入本框架。我们保留完整32768上限，任何超长行或注册失败都显式失败。原PEFT adapter保持不变，按实际competition wheel另做K→V服务导出，不套作者宽后缀配置。
11: - 作者榜上最佳成绩没有绑定到这一份adapter/脚本，不作为本包效果证据。其他已归档教程存在Gemma2/随机fallback、模拟训练或全token标签问题，未作为运行实现采用。
12: 
13: ## 第一方版本依据
14: 
15: - [Transformers 5.13.1 Gemma4接口](https://huggingface.co/docs/transformers/v5.13.1/en/model_doc/gemma4)：核对实际 `Gemma4ForConditionalGeneration`、原生工具模板与文本配置；不把普通31B或新版接口替代现有QAT父模型。
````

## s040

来源：`learning_pipeline/training/train_adapter.py:300`

````text
300:                     if micro>=len(local_batch):
301:                         communication_only_backward(objective,torch.device('cuda',local_rank))
302:                         continue
303:                     i=local_batch[micro]
304:                     row=rows[i];weight=row_weights[i]
305:                     sync=objective.no_sync() if world>1 and not fsdp and micro<len(local_batch)-1 else nullcontext()
306:                     with sync:
307:                         with torch.autocast('cuda',dtype=torch.bfloat16):
308:                             result=action_loss(row,return_diagnostics=fresh)
````

## s041

来源：`learning_pipeline/training/fsdp_runtime.py:114`

````text
114: def communication_only_backward(objective,device,mixed=True):
115:     """Equal FSDP slots for an uneven tail; no data row/loss/RNG contribution."""
116:     devices=[device.index] if device.type=='cuda' else []
117:     with torch.random.fork_rng(devices=devices):
118:         ids=torch.zeros((1,2),dtype=torch.long,device=device)
119:         with torch.autocast(device.type,dtype=torch.bfloat16,enabled=mixed):
120:             zero=objective(ids,None,communication_only=True)
121:         zero.backward()
122: 
````

## s042

来源：`learning_pipeline/training/train_core.py:324`

````text
324: def selected_completion_loss(model,input_ids,labels_action,labels_reasoning,head_chunk=64,*,return_diagnostics=False):
325:     """One row's mean CE across every qualified current-completion target."""
326:     if labels_action.shape!=input_ids.shape or labels_reasoning.shape!=input_ids.shape:
327:         raise ValueError('Bad completion component dimensions')
328:     action=labels_action!=-100;reason=labels_reasoning!=-100
329:     if (action&reason).any() or not action.any():raise ValueError('Need disjoint completion targets with an accepted action/final')
330:     labels=torch.where(action,labels_action,labels_reasoning)
331:     if not return_diagnostics:return selected_action_loss(model,input_ids,labels,head_chunk)
332:     loss,sums=selected_action_loss(model,input_ids,labels,head_chunk,diagnostic_masks=(action,reason))
````

## s043

来源：`learning_pipeline/server/export_serving.py:16`

````text
16: def expand(tensors,config,text_config):
17:     if len(tensors)!=820 or config['r']!=16 or config['bias']!='none' or config.get('modules_to_save'):
18:         raise ValueError('Unexpected training adapter contract')
19:     global_layers=[i for i,typ in enumerate(text_config['layer_types']) if typ=='full_attention']
20:     if not text_config['attention_k_eq_v'] or global_layers!=list(range(5,60,6)):raise ValueError('Unexpected shared K/V architecture')
21:     wanted=set()
22:     for i in range(60):
23:         for branch,projs in [('self_attn',['q_proj','k_proj','o_proj']+([] if i in global_layers else ['v_proj'])),('mlp',['gate_proj','up_proj','down_proj'])]:
24:             for proj in projs:
````

## s044

来源：`reports/trajectory_cycle02_fsdp4_r01_return_review_20261116/overview.md:7`

````text
7: | 执行链 | 九退出码全0，524更新；每轮2090条各一次，合4180次真实消费 | 完整按配方执行，尾2没有补样/丢样 |
8: | 父权重 | 原820 FP32在同步前后精确继承，新AdamW；最终820/820张量均变化且有限 | 续训真实生效，父件未覆盖 |
9: | 训练损失 | 第1/2轮按真实样本平均0.226930→0.192685，下降15.09% | 对D04训练目标拟合改善；不是验证loss |
10: | 最后20步 | 按真实样本平均0.173166（每步等权平均0.166827） | 比末尾单批更有代表性，仍不能代表解题能力 |
11: | 耗时 | 训练loop13.594h，worker13.669h；四卡总971.6输入token/s | 比C01的10.295h更长，工作量与长度也变大 |
12: | 显存 | FSDP ready后最长+训练allocated峰52.7096GiB；最长reserved59.45–62.85GiB，估算余量15.70–19.10GiB | 这批26979完整长度稳定承载；52.7不是全生命周期总显存峰值 |
13: | 导出/服务 | 820→840 BF16逐值正确，195042419B六成员单adapter ZIP，真实vLLM短加载通过 | 可作为评测候选；短探针不等于解题提分 |
14: 
15: ## 需要纠正的loss口径
````

## s045

来源：`reports/trajectory_cycle01_fsdp4_r05_return_review_20261114/README.md:5`

````text
5: | 项目 | 原件核验结果 |
6: |---|---|
7: | 回传 | 1,038,477,616 B；186 regular=185 payload+summary，安全解包/直接字节归档 |
8: | 训练 | 498正式更新；D03 1990×2，全部3980样本访问符合冻结洗牌/四rank分配 |
9: | 耗时 | 训练循环37062.58秒，约10.30小时 |
10: | loss | 首步0.22633、末步0.05348；前20步均值0.36723、末20步0.09394。不是简报中0.54→0.29口径 |
11: | 显存 | 完整19356-token两累积，四rank峰值allocated43.84 GiB；估算余量28.45/30.24/30.90/29.08 GiB |
12: | 权重 | 820 FP32训练张量有限，转换840 BF16服务张量逐值相符，包含20共享K/V复制 |
13: | 加载 | 最终真实vLLM加载及单token数值差异通过；只证明加载/变化，未证明解题收益 |
````

## s046

来源：`reports/trajectory_cycle02_fsdp4_r01_return_review_20261116/overview.md:5`

````text
5: | 项目 | 原件核对结果 | 评价 |
6: | --- | --- | --- |
7: | 执行链 | 九退出码全0，524更新；每轮2090条各一次，合4180次真实消费 | 完整按配方执行，尾2没有补样/丢样 |
8: | 父权重 | 原820 FP32在同步前后精确继承，新AdamW；最终820/820张量均变化且有限 | 续训真实生效，父件未覆盖 |
9: | 训练损失 | 第1/2轮按真实样本平均0.226930→0.192685，下降15.09% | 对D04训练目标拟合改善；不是验证loss |
10: | 最后20步 | 按真实样本平均0.173166（每步等权平均0.166827） | 比末尾单批更有代表性，仍不能代表解题能力 |
11: | 耗时 | 训练loop13.594h，worker13.669h；四卡总971.6输入token/s | 比C01的10.295h更长，工作量与长度也变大 |
12: | 显存 | FSDP ready后最长+训练allocated峰52.7096GiB；最长reserved59.45–62.85GiB，估算余量15.70–19.10GiB | 这批26979完整长度稳定承载；52.7不是全生命周期总显存峰值 |
13: | 导出/服务 | 820→840 BF16逐值正确，195042419B六成员单adapter ZIP，真实vLLM短加载通过 | 可作为评测候选；短探针不等于解题提分 |
````

## s047

来源：`learning_pipeline/training/schedule.py:20`

````text
20: def _fresh_contract(recipe, sample_count):
21:     if type(sample_count) is not int or sample_count < 1:
22:         raise ValueError('Fresh training requires a nonempty frozen dataset')
23:     required = {'supervision': 'completion', 'initialization': FRESH_INITIALIZATION,
24:                 'optimizer_initialization': 'new_stage_empty_adamw', 'schedule': FRESH_SCHEDULE,
25:                 'rank': 16, 'alpha': 16, 'dropout': .05, 'epochs': 1,
26:                 'world_size': 4, 'microbatch_size': 1, 'gradient_accumulation': 2,
27:                 'global_batch_size': 8, 'parallelism': 'fsdp2',
28:                 'seed': 42, 'max_sequence_length': 32768, 'query_chunk': 512,
````

## s048

来源：`learning_pipeline/training/checkpoint.py:89`

````text
89: def save_final_training_state(directory, optimizer, trainable, *, step, schedule, binding,
90:                               next_cursor, rank_rng, adapter_path):
91:     """Retain one AdamW copy plus exact data cursor; no promise of usable resume."""
92:     import torch
93:     validate_finite_adapter(trainable)
94:     if step != schedule.total_optimizer_steps:raise ValueError('Final optimizer retention must be at the frozen horizon')
95:     if len(optimizer.state) != len(trainable):raise RuntimeError('Final AdamW state is incomplete')
96:     moment_bytes = 0
97:     for name, parameter in trainable:
````

## s049

来源：`reports/trajectory_cycle01_pair_r02_review_20261115/OVERVIEW.md:7`

````text
7: ## 结果与可比性
8: 
9: |指标|原r04无adapter T0.2|同r04+LoRA T0.2|同r04+LoRA T1.0|
10: |---|---:|---:|---:|
11: |resolved/129|51|48|52|
12: |缺必填字段的调用/涉及题|107/16|1/1|8/6|
13: |实际会话截止|5|1|1|
14: |连续相同工具/参数调用|81|12|12|
15: |多次submit_patch的题|30|7|4|
````

## s050

来源：`reports/fresh_lora_implementation_20261116/prior_c01_scores_20261116.json:1`

````text
1: {
2:   "date": "2026-11-16",
3:   "source": "USER_PROVIDED_SCREENSHOT",
4:   "image": "reports/fresh_lora_implementation_20261116/c01_scores_user_screenshot.png",
5:   "image_bytes": 33079,
6:   "observations": [
7:     {
8:       "description_prefix": "SELF-C01-D03-T10-r01",
9:       "status": "Succeeded",
````

## s051

来源：`reports/trajectory_cycle02_pair_r01_review_20261116/overview.md:7`

````text
7: | 臂 | 完成/计划 | resolved | C01同题同温度 | 完成题空patch | 中断活动题 | 未运行 |
8: |---|---|---:|---:|---:|---|---:|
9: | T0.2 | 10/129 | 2 | 4/10 | 6 | fastapi_14186 | 118 |
10: | T1.0 | 6/129 | 1 | 3/6 | 0 | fastapi_13786 | 122 |
11: 
12: 两臂各丢2道C01同窗成功题、0新增。T02相同前10无adapter为5/10。所有18初始消息及六工具声明与C01/base直接相等、同温度采样一致、C02路由符合输入，故不是拿旧9工具/420秒版作对照。但这是主动中断、FastAPI-only、非随机且训练已暴露的窗口，不是完整129成绩，也不足以确诊灾难性遗忘。
13: 
14: 主要信号：低温重复相同搜索并忽略预算拒绝、空补丁和失实终答；高温产生补丁却修错接口/写坏语法；少数响应在文本/推理里写伪工具标记而未执行。39摘要均送达，但个别内容对编辑状态判断有误。不存在“工具提醒/摘要完全没送达”的解释。D04相关11题188目标的有限核查未找到坏短句直接复制，且有正确编辑目标；不能据此证明整个监督混合最优。
15: 
````

## s052

来源：`reports/fresh_lora_research_20261116/overview.md:74`

````text
74: ## 下一步怎样避免又训完才发现不好
75: 
76: 先完成候选SPEC的数据视图与少量必要训练入口改造，再发一个fresh训练作业。主作业只有一次完整pass；按冻结N算U，不为了凑262或524步复制行。保存25/50/75/100%，训练结束后先用固定小面板比较底座、50%、100%权重，其他两个点按需要进入比较。最终选一个候选跑当前同配置129，保留逐题增/丢、首次有效源码修改、空patch和无信息重复；CE仅辅助。公开129早已暴露，外部留出尚未实跑时不宣称泛化提高。
77: 
78: 这是一个联合配方试验，若进步不能把功劳独归alpha或cosine。它比“换一组大参数直接再训两轮”多的是可选择的中间权重、明确的数据目标和与完整修复一致的决策标准。新训练、模型、服务器、教师采集和Kaggle均 **NOT_RUN**；[结构化汇总](synthesis.json)记录各主张及执行边界。
````

## s053

来源：`reports/checkpoint_panel_c03_20261116/overview.md:5`

````text
5: |GPU|候选|顺序|
6: |---|---|---|
7: |0|官方QAT底座，无LoRA|T0.2 → 清理 → T1.0|
8: |1|step62 / 50%|T0.2 → 清理 → T1.0|
9: |2|step93 / 75%|T0.2 → 清理 → T1.0|
10: |3|step124 / 100%|T0.2 → 清理 → T1.0|
11: 
12: 每臂原快照、相同12题及顺序、同提示/六工具/28-480-80/240-300/8192-4096/p.95/k64，GPU/耗时记录保留。12题都是暴露开发题；逐题差异和退化形态用于筛选，不能把一题微差或最低loss当泛化胜负。下一步选择候选再决定完整129及提交，本包不自动执行这两步。
13: 
````

## s054

来源：`learning_pipeline/__main__.py:1`

````text
1: import argparse
2: import json
3: from .core import init,make_plan,status,verify
4: from .shards import prepare as prepare_shards,inspect_reviews
5: 
6: def main():
7:     p=argparse.ArgumentParser(description='Versioned trajectory-learning workflow. Collection is explicit and requires recorded authorisation.')
8:     commands=p.add_subparsers(dest='command',required=True)
9:     commands.add_parser('init').add_argument('--config',required=True)
````

## s055

来源：`learning_pipeline/templates/stage_compact.py:16`

````text
16: def stage(folder):
17:     root=Path(folder).resolve()
18:     manifest=json.loads((root/'manifest.json').read_text(encoding='utf-8'))
19:     members=manifest['members'];names=[r['path'] for r in members]
20:     if len(names)!=len(set(names)) or len(names)!=len({n.casefold() for n in names}):
21:         raise ValueError('Duplicate member names')
22:     created=[];reused=[]
23:     with zipfile.ZipFile(root/'source_bundle.zip') as archive:
24:         if len(archive.namelist())!=len(names) or set(archive.namelist())!=set(names):
````

## s056

来源：`learning_pipeline/server/delivery_contract.py:38`

````text
38: def verify(root,pending=()):
39:     root=Path(root).resolve();manifest=json.loads((root/'manifest.json').read_text(encoding='utf-8'))
40:     pending=set(pending)
41:     if not pending<=HOST_FILES:raise RuntimeError('Only declared environment helpers may be adjusted')
42:     with zipfile.ZipFile(root/'source_bundle.zip') as z:
43:         names=[m['path'] for m in manifest['members']]
44:         if len(names)!=len(set(names)) or len(z.namelist())!=len(names) or set(z.namelist())!=set(names):raise RuntimeError('Member set differs')
45:         if z.testzip():raise RuntimeError('CRC failed')
46:         latest,records=adjustments(root,z,manifest['version'])
````

## s057

来源：`learning_pipeline/evaluation/runtime_ipc.py:1`

````text
1: """Allocate a short private vLLM RPC directory without changing task TMPDIR."""
2: import os
3: from pathlib import Path
4: import socket
5: import tempfile
6: import uuid
7: 
8: 
9: def allocate(resource_dir, gpu_index, *, keep=False):
````

## s058

来源：`README.md:67`

````text
67: r01两臂均在vLLM IPC路径120B超过107B时启动失败，真实0题、清理闭合，原失败证据保留。根因是本地临时路径设计遗漏，后续summary缺失是次生错误。新r02只设置官方支持的短`VLLM_RPC_BASE_PATH`并实测socket；提示/LoRA/预算/SDK裁判保持。
68: 
69: 只上传[5件小差量目录](dist/harness_k_a800/trajectory_cycle01_joint_t02_t10_pair_r02_update/)（80426B），把[完整zcode指令](dist/harness_k_a800/trajectory_cycle01_joint_t02_t10_pair_r02_update/ZCODE.txt)转交。服务器保留r01原件，从旧封存包复用大输入到新r02目录，继续GPU2 T0.2/GPU3 T1.0各129一次并结束后保留本人预留；不清旧排他文件、不重传207MB、不重跑已执行题。[原件分析与验收](reports/trajectory_cycle01_pair … [长行省略]
70: 
71: ## 2026-11-14：D082 首轮LoRA训练已接纳并提交，准备双温度129评测
72: 
73: 四卡FSDP2 r05实际完成498更新/两epoch，最长19356-token两次累积通过；820训练张量与840服务张量逐值核验、真实vLLM挂载检查完成。单干净GPT-6.1 Sol/max已接纳训练与原六成员ZIP，首次失败load和总失败回执保留，按最终续跑证据作新增接纳。训练约10.30h；加载数值变化不等同解题提升。
74: 
75: 用户本轮授权实际Kaggle提交：[SELF-C01-D03-T02-r01包](dist/self_c01_d03_t02_kaggle_r01/submission.zip)194789594B，提交号<OPERATION_ID>，目前PENDING，尚无新成绩。原0.18提交及所有基线保留。[回传审查](reports/trajectory_cycle01_fsdp4_r05_return_review_20261114/)记录原件、独审、完成状态解释和提交回执。
````

## s059

来源：`learning_pipeline/evaluation/panel_driver.py:113`

````text
113: def cleanup_closed(arm_id):
114:     if any(CONTROL.rglob(arm_id+'*.unclosed.json')):return False
115:     p=paths(arm_id);report=p['report']
116:     if (report/'batch_result.json').is_file():return read(report/'batch_result.json').get('safe_to_pack') is True
117:     # A preflight failure has no model path; the wrapper process must have exited.
118:     return not (report/'batch_started.json').exists() and not p['output'].exists() and not p['setup'].exists()
119: 
120: 
121: def worker(candidate):
````

## s060

来源：`reports/trajectory_cycle01_pair_r02_review_20261115/OVERVIEW.md:38`

````text
38: ## 回传为什么大，已经怎样缩小
39: 
40: 原tar.gz **1706923496B**，23836存储成员；恢复5个精确引用后逻辑文件23841（含manifest）。存储层展开体积中，8个ZIP约1.414GB加一个权重247.36MB，占约76%；多数ZIP里面又重复装着同一个adapter。仅做整个文件去重不够。
41: 
42: 本地用全部实际原件制作analysis验证包 **154622101B（154.6MB）**，减少**90.94%**。所有**21093 HTTP文件/7031组**、**1030 ADK/observations文件**、官方trace、patch、验证、GPU与清理记录保持原字节。全内容核对为23834普通/引用逻辑文件及364个ZIP成员通过；少传的是与已接纳冻结输入全字节相等的权重/ZIP重复内容，未知或不同字节照常保留。
43: 
44: 需要保留本地冻结`dist/self_c01_d03_t02_kaggle_r01/submission.zip`才能完整恢复引用。嵌套ZIP只保证全部成员字节，不保留原压缩容器字节；原完整tar、旧发行和服务器原件未改。首次新adapter仍必须传真实权重，未来完整自含需求有`--profile full`。当前源码已接入未来pair打包与统一lean-return/restore-return入口；本轮不重新上传或重新跑服务器。
45: 
46: 验证：[真实全内容直比](lean_return_verification.json)、[独立瘦身审查](independent/lean_return_review.md)。本地13项初检及3组可复用回归覆盖不同权重、Unicode、引用尺寸/key/CRC、原路径恢复和拒绝覆盖。初次unittest被Windows Temp目录ACL拦住，尚未进入内容测试；改用工作区新建隔离测试目录后全过，未改生产验收规则。
````

## s061

来源：`reports/checkpoint_panel_c03_20261116/overview.md:27`

````text
27: ## 交付与回传
28: 
29: 五件压缩上传约12.5MB；source_bundle.zip 12,509,850B /65成员，字节与展开/当前源直接一致。README另按独审澄清CPU序列化fixture存在。只上传该新目录，避免再次同步训练目录。实际大小清单与最后直接绑定见delivery_receipt.json。
30: 
31: analysis回传8臂所有HTTP/ADK/patch/verification/官方trace/日志/配置/SDK路由/耗时/显存/清理/预留；中间权重、八臂精确输入ZIP保留服务器，外部引用明确NOT_RETURNED。本地可完整读行为证据，但不能声称已独立直比中间权重；选定后再取该候选准确温度的输入。--full可自含。原C03训练回传独立保留。
32: 
33: 无新hash、模型下载、依赖安装、服务器操作、教师、训练、129或Kaggle提交。旧训练发行和历史数据/adapter不变。
````

## s062

来源：`learning_pipeline/specs/FRESH_SFT_C03.md:14`

````text
14: | 任务口径 |原SELF-v17-r04六工具/28调用/480秒/80轮，240秒命令/300秒裁判、8192总输出/4096思考保持 |
15: | 服务器范围 |训练沿用0–3，短加载GPU2，结束恢复本人GPU2/3预留；GPU0他人条目不碰，4–7不操作 |
16: 
17: 发布身份：`trajectory_cycle03_fresh_fsdp4_r01`。数据身份：`learning_runs/TC-r04-001/datasets/D05_fresh`，复用该轮无adapter的6842B Agent来源；新数据拥有独立来源/选择/派生/资格账本，原round catalog和原D03/D04不可变。最终N=990、160个任务ID，U=124/W=7，保存31/62/93/124，末批6条按2/2/1/1分配（按真实长度决定rank）。
18: 
19: ## 本次实际冻结结果
20: 
21: | 项目 | 实际值 |
22: |---|---:|
````

## s063

来源：`archive/harness_k_a800/trajectory_cycle02_joint_t02_t10_pair_r01/project/calibration/coverage.json:1`

````text
1: {
2:   "all_task_ids": [
3:     "fastapi_11194",
4:     "fastapi_11355",
5:     "fastapi_12942",
6:     "fastapi_13207",
7:     "fastapi_13537",
8:     "fastapi_13713",
9:     "fastapi_13786",
````

## s064

来源：`runtime/baseline.py:351`

````text
351:                 summary = {
352:                     "instance_id": task.instance_id, "resolved": result.resolved,
353:                     "test_exit_code": result.test_exit_code, "patch_chars": len(result.agent_patch),
354:                     "tool_calls": result.tool_calls, "llm_calls": result.total_llm_calls,
355:                     "duration_seconds": result.duration_seconds, "error": result.error_message,
356:                     "evaluation_wall_seconds": time.monotonic() - task_started_at,
357:                     # The official context freezes this clock when its Agent
358:                     # session stops; TaskResult duration also includes other work.
359:                     "agent_duration_seconds": context.agent_elapsed_seconds,
````

## s065

来源：`learning_pipeline/contracts/teacher_fragment_review.md:11`

````text
11: - 只接收 `job_result.status=FRAGMENT_CAPTURED`、`collection_mode=fragment`、`overall_verification=NOT_RUN`。`final_stage` 指最后真实下一请求边界，不是整题终态。该链没有新 TaskResult、独立判分或虚构 submit/final。
12: - `teacher_000` 到末项全部逐项处置，包括中途实际 summary；编号完整且唯一。每个实际提案须有 ACCEPTED 的 `response.json`、原 `proposal.txt`、`response.raw.json`、实际 `teacher_input.txt`、无 native 工具的 CLI 事件，以及下一 stage 的实际请求/状态比较和 ADK 调用及后续回执。`target_binding` 的执行计划不能代替 `execution_binding.status=EXECUTED`。
13: - 所有 stage（包括前缀 stage_000 和末 stage）必须真实 BOUNDARY_CAPTURED、cleanup 完成、`protected_mutation_observed=false`、cleanup 前保护字节相等、无 audit_error，并保留下一请求与 task_state.zip/json。首次原学生工作区未观测仍记 RECONSTRUCTED，不称严格同现场。
14: - 源码写入、验证或捕获结论、正 reasoning、保护/历史访问边界，须由独立审查者或执行协调员实际核实。不得因工具 ok 就认可未知代码。保护污染、历史/私有证据访问、未执行或工具拒绝不能正训。
15: - 非零检查仅在它确实是有效失败诊断时可接纳：lane 必须 VALID_DIAGNOSTIC，另写 `receipt_assessment=EXPECTED_DIAGNOSTIC_FAILURE`，并解释真实失败揭示的缺口。畸形脚本、错误断言或缺参拒绝不属于此例外。
16: 
17: ## 逐目标资格
18: 
19: 每个 target 的三个 approved 字段都是真 boolean。未选、错误、未知、summary 的三个字段均 false；不要为补齐行数降低标准。短片段动作仅能标 LOCAL_ACTION_VERIFIED 或 VALID_DIAGNOSTIC。永远不标 FULL_REPAIR_VERIFIED，`final_approved` 永远 false。
````

## s066

来源：`learning_pipeline/collection/runner.py:282`

````text
282:             if p['decision_summary']:msg['reasoning_content']=p['decision_summary']
283:             if p['kind']=='tool_call':msg['tool_calls']=[{'id':f'teacher-{task_id}-{cut}-{attempt}-{idx:03d}','type':'function',
284:                 'function':{'name':p['name'],'arguments':json.dumps(args,ensure_ascii=False)}}]
285:             write_new(teacher/'response.raw.json',{'model':'controlled-gpt-6.1-sol/max','choices':[{'index':0,'message':msg,
286:                  'finish_reason':'tool_calls' if p['kind']=='tool_call' else 'stop'}],'usage':None,'teacher_usage_separate':response.get('usage')})
287:             write_new(teacher/'target_binding.json',{'input_request':str(stage/'next_request.json'),'actual_proposal':str(teacher/'proposal.txt'),
288:                 'author':'gpt-6.1-sol/max','executed_in_next_stage':idx+1,'training_eligible':False,
289:                 'reasoning_source':'TEACHER_PUBLIC_DECISION_SUMMARY','private_teacher_thinking_collected':False})
290:             responses.append({'mode':req['mode'],'response_path':linux(teacher/'response.raw.json'),'source':'SOL_TEACHER',
````

## s067

来源：`learning_pipeline/SPEC.md:276`

````text
276: ## 8. thinking与公开决策说明
277: 
278: 本轮用户要求与A–E同时推进推理引导。**129题每个实际可见reasoning回合均审查**，但不预设每段都错或每段都值得正训。主审与另一分片交审均确认该整段仅依据当时输入、无错误事实/未来泄漏/无效重复，并与同回合实际合格动作相符，才可监督原文；混有错误或未知内容则整段mask，不事后删改成“原始正确thinking”。摘要回合只审其状态影响，不作为动作reasoning正例。没有可见reasoning就记ABSENT，不补造。
279: 
280: Sol/Codex私有思维链不请求、不记录。真正新教师在实际动作前另行输出**公开简短决策说明**：已观察证据与下一动作/检查的理由，一般几句。它是教师专门撰写的训练内容，来源固定记`TEACHER_PUBLIC_DECISION_SUMMARY`，不是其内部思维链。输出由干净CLI进程产生，后续真实官方工具执行，资格再独审；分析者看完结果写的rewrite_suggestion永远只是metadata，不进loss。
281: 
282: 原生模板将工具回合的reasoning_content放入thought channel。新导出器仅监督本目标中被接受的thought区段及工具/终答区段，旧学生错误、已有老师前缀、工具观察全部-100。分别导出`labels_action`、`labels_reasoning`和合并labels；同一行的action-only对照保持完全相同的input_ids，仅改mask，不能删掉因果历史制造更容易输入。
283: 
284: 新数据在原生span/token前缀与独立mask审查通过前不冻结。训练建议先比较action-only与联合监督；联合版按组件分别平均再加权，避免长thought淹没短动作。具体混合权重在训练配方明列并可审；当前无GPU训练，不称其必然提分。原学生prompt/推理预算不改变。
````

## s068

来源：`learning_pipeline/native.py:43`

````text
43:     def example(self,sample_id,request,target,selected_call_ids,metadata,reasoning=False,final_text=False):
44:         target=copy.deepcopy(target)
45:         prompt=self.render(request['messages'],request.get('tools'),True)
46:         full=self.render(request['messages']+[target],request.get('tools'),False)
47:         if not full.startswith(prompt):raise ValueError('Native text prefix does not match')
48:         pe=self.tokenizer.encode(prompt,add_special_tokens=False)
49:         fe=self.tokenizer.encode(full,add_special_tokens=False)
50:         if fe.ids[:len(pe.ids)]!=pe.ids:raise ValueError('Strict native token prefix mismatch; no truncation/fallback')
51:         completion=full[len(prompt):]
````

## s069

来源：`reports/fresh_lora_implementation_20261116/overview.md:60`

````text
60: 全行native审计和一个干净Sol/max独审均通过：**990行/12,199,343位置**的源对象、允许的派生、完整正标签、action/reason分解、权重及split全核；无历史/回执监督。六主件快照61,075,049B逐成员核同。独立审查使用自身边界和offset重算，不调用本次builder的complete_example自证。[冻结审查](independent/review.json) · [发行审查](independent/release_review.json)。
61: 
62: 必要CPU检查包括trainer的CE/梯度/尾批/快照、builder多回合源重建及负例、实际服务器合同和旧入口回归。独审自行执行20项及A差量1项，复用实施侧检查结果而不反复扩审。实际本地Torch2.14CPU与目标2.10CUDA的边界保留；本地PASS不等于31B训练/加载已经运行。
63: 
64: ## 旧结果与下一轮边界
65: 
66: 用户截图显示C01-D03-T10-r01 **0.15**，同LoRA T02 **0.17**，均Succeeded；本轮未重新API核提交ID，原截图和[观测记录](prior_c01_scores_20261116.json)已保存。它们没有达到0.20/0.22目标；T1本地52/129没有形成线上温度优势。不能由两个提交单独诊断LR、rank或灾难遗忘，也不能因为新包工程验收通过就承诺新分数。
67: 
68: 本轮实际状态为 **DATA_FROZEN_HANDOFF_READY**。服务器31B训练、加载、候选行为选择、129、Kaggle均未新增运行；GPU与预留由zcode按已授权交接实际处理。下一步是上传当前七件并转交完整指令，收到实际训练与checkpoint证据后继续选择和评测。
````

## s070

来源：`learning_pipeline/fresh.py:159`

````text
159: def _a_source(native,record,index):
160:     """Precisely match one author-converted example to its message and prefix."""
161:     assistants=[i for i,m in enumerate(record['messages']) if m.get('role')=='assistant']
162:     if len(assistants)!=len(record['sft_examples']):raise ValueError('A assistant/example alignment differs')
163:     message_index=assistants[index];history,removed=_served_a_messages(record['messages'][:message_index])
164:     target=copy.deepcopy(record['messages'][message_index]);example=record['sft_examples'][index]
165:     canonical_prefix=native.render(history,record['tools'],True)
166:     # The published A corpus was built with a source generation boundary that
167:     # forces the thought-channel opener after a tool response.  Our deployed
````

## s071

来源：`learning_pipeline/specs/FRESH_SFT_C03.md:21`

````text
21: | 项目 | 实际值 |
22: |---|---:|
23: | 本地D04重选 |720回合/113任务ID；原2090中1370排除、16题不凑只读样本 |
24: | 公开Native |233回合/41任务ID |
25: | 公开A |37回合/6任务ID；source-converted，不冒称本SDK重执行 |
26: | 合计 |990回合/160任务ID；新增外部47任务来自45个训练repo |
27: | 主要阶段 |定位313、编辑271、验证195、恢复收尾211 |
28: | 完整输入位置 |12,199,343 |
29: | 正目标token |199,458＝156,980行动/边界＋42,478合格公开说明 |
````

## s072

来源：`reports/fresh_lora_implementation_20261116/overview.md:15`

````text
15: | 来源 | 目标回合 | 任务ID | 实际目标系数质量 |
16: |---|---:|---:|---:|
17: | 本地Gemma合格行为（含选中旧来源） |245|65|27.9219% |
18: | 本地Sol纠偏（含选中旧来源） |475|76|42.7031% |
19: | 公开Native |233|41|25.6250% |
20: | 公开A转换来源 |37|6|3.7500% |
21: | 合计 |**990**|**160**|**100%** |
22: 
23: 本地两行任务有交集，不能将65+76相加当141道不同题。本地总体113任务ID；外部47任务ID/45训练repo。本地从D04的2090行逐题重选720，排除1370行；16题只有准备性读取/诊断，不为凑129给整题训练权重。旧资格复用，未重审全部旧HTTP。外部train分片读完47题/85attempt/1532可见回合，205段保留的非空说明全文批准。没有本轮新教师生成。
````

## s073

来源：`learning_pipeline/specs/FRESH_SFT_C03.md:28`

````text
28: | 完整输入位置 |12,199,343 |
29: | 正目标token |199,458＝156,980行动/边界＋42,478合格公开说明 |
30: | 完整长度 |最长26,910/32768；最大当前completion2208/8192，无截断 |
31: | 说明及收尾 |721行有原合格说明，7行事实final；0新教师调用 |
32: | 均权 |全行平均loss倍率1，范围0.1546875–3.09375 |
33: | 实际目标系数 |行动72.6095%、说明27.3905%；这是优化目标系数，不是测得的梯度份额 |
34: | 外部隔离 |dev/final各8repo9issue，未入训练；环境尚未重建 |
35: 
36: 独立Sol/max逐源对象核变换、全文位置/两component mask/倍率/来源资格及split：990行、12,199,343位置全过，开放阻断0。六主件快照61,075,049B逐成员直接核同，已由统一freeze接纳。21个既有真实serving渲染fixture继续逐字/逐token相等。语义审查复用已有本地资格，外部两个分片读完85条轨迹1532可见回合；不冒称所有历史reasoning都全文审过。
````

## s074

来源：`learning_runs/TC-r04-001/datasets/D05_fresh/summary.json:7`

````text
7:   "rows": 990,
8:   "tasks_with_targets": 160,
9:   "canonical_tasks_with_targets": 160,
10:   "token_counts": {
11:     "input": 12199343,
12:     "action": 156980,
13:     "reasoning": 42478,
14:     "reasoning_rows": 721,
15:     "final_rows": 7
````

## s075

来源：`reports/fresh_lora_implementation_20261116/overview.md:25`

````text
25: 阶段为定位313、编辑271、验证195、恢复收尾211。`run_command`按真实内容分类，不能全部算探索。完整输入12,199,343位置，正token **199,458**（action/合法边界156,980＋合格公开说明42,478）；721行说明、7行事实final。最长26,910，无截断；最大目标2208，未超过8192输出预算。新视图是完整合格completion CE，不把未合格当前说明留在输入中当答案提示；原件和显式移除记录保留。
26: 
27: 每题/阶段总质量均衡，均匀遍历使用`m=N/(T*K_i*n_i_s)`，平均1，范围0.1546875–3.09375。由真实行长度和倍率算得说明目标系数27.3905%、行动72.6095%，不是简单token比例，也不是测得梯度贡献。详细[计数与系数](training_dataset_statistics.json)。
28: 
29: 数据有价值的筛选实例：
30: 
31: - 本地保留12942的递归Annotated修正及真实端点/兼容检查，11194的字段类型修正链；普通查询、重复成功声明和无实质推进的状态/提交不单独准入。
32: - 外部发现有edit解释错误地声称旧行为完全保留、正确源码调用却夹着错误推理的情况：动作可有资格，错误当前说明在新派生视图移除，不伪造新思考。
33: - speedcopy的abspath改动可能改变返回值接口，没有因源resolved就正训整edit；CrossGL的exit0来自catch但解析实际失败，不标成功验证。
````

## s076

来源：`learning_pipeline/specs/FRESH_SFT_C03.md:44`

````text
44: - 本地D04复用已通过的原生mask、动作语义及公开说明资格；重选阶段价值，避免再次审阅全部历史HTTP。每题保留必要定位、源码编辑、命中真实行为的验证、恢复/事实收尾；既有源身份不是质量分数，不能只选教师或只选成功链。
45: - 公开候选只Native/A。训练split47issue/85轨迹由两个干净Sol/max curator分24/23题阅读其全部动作/回执链；正标签reasoning必须全文已读并接受，历史未读reasoning继续masked。dev/final各9issue/8repo不用于筛选目标；B/B2不扩入首轮。
46: - 每题选主要有效路径，可保留互补真实状态。无信息重复、错误/未知源码、工具格式错、假成功不进正标签。不得把源resolved当本SDK重执行通过；保留SOURCE_NATIVE_CAPTURE_SUPPORTED与SOURCE_CONVERTED_SUPPORTED。
47: - 新派生视图显式替换为当前system和六工具声明，原user题面、过去动作和工具回执保持；当前目标原动作不改。只监督完整合格completion（原已核公开说明＋动作/合法结束），不训练工具回执、后验理由或私人thinking。
48: - 未合格当前说明从新衍生输出移除，保留原件和变换记录；不把其masked后继续作为答案提示。不截断长说明；80token只是研究短说明偏好，不是准入硬门。多工具目标只有全部目标动作获资格才可完整监督，否则隔离原因。
49: - 现有joint/action历史模式原样，新`completion`模式对完整正token逐行平均CE；分别记录action/说明诊断。每题阶段均衡采用`loss_weight=N/(T*K_i*n_i_s)`，平均倍率1。均匀行遍历，不再按同一权重重采样。
50: - 重复只在同源/等价原状态有直接证据时去重；相似状态只标冗余筛选，不假称精确去重。长于32768的输入隔离而不截断。来源、schema异常、隐私替换按实际影响判断；本地采集guard不冒充官方禁止规则。
51: 
52: ## 入口与验证
````

## s077

来源：`reports/fresh_lora_implementation_20261116/overview.md:36`

````text
36: 各分片真实选择/排除和原件锚：[本地](local_selection/report.md)、[外部A分片](external_a/report.md)、[外部B分片](external_b/report.md)。Native原生捕获支持与A转换来源支持明确分开，均未宣称本轮在本SDK重新执行。dev/final各8repo9issue没有进入训练；它们不是已跑通的外部环境，129本地题早已暴露。
37: 
38: ## 实际配方及已实现能力
39: 
40: | 参数 | 本轮固定值 |
41: |---|---|
42: | 底座/初始化 |官方QAT恢复冻结BF16；PEFT随机A/零B、新AdamW，无父adapter |
43: | LoRA |标准r16/alpha16/dropout.05，全60层410投影，122,429,440参数 |
44: | batch与长度 |4卡micro1×accum2=global8；1pass、**124更新**，尾6不补样 |
````

## s078

来源：`learning_pipeline/training/SOURCES.md:13`

````text
13: ## 第一方版本依据
14: 
15: - [Transformers 5.13.1 Gemma4接口](https://huggingface.co/docs/transformers/v5.13.1/en/model_doc/gemma4)：核对实际 `Gemma4ForConditionalGeneration`、原生工具模板与文本配置；不把普通31B或新版接口替代现有QAT父模型。
16: - [Transformers 5.13.1 compressed-tensors量化器](https://github.com/huggingface/transformers/blob/v5.13.1/src/transformers/quantizers/quantizer_compressed_tensors.py)：`run_compressed=False`在加载后走decompress_model。归档同版本wheel源也已检查，沿用此API而不改成后续版本的参数。
17: - [PEFT 0.21.2 LoRAConfig源码](https://github.com/huggingface/peft/blob/v0.21.2/src/peft/tuners/lora/config.py)：核对target_modules、rank/alpha/dropout与bias参数。该版本官方文档URL本次不能访问，因此版本特定判断以实际wheel/源码及已有真实CPU序列化检查为准；不将重定向到0.21.0的文档冒充0.21.2。
18: - [vLLM 0.19.1 LoRA装载资料](https://docs.vllm.ai/en/v0.19.1/features/lora/)：支持独立LoRA请求。实际比赛build仍按 `official_lora_sources.zip` 的14份源文件直接比较；不能仅靠同为0.19.1就宣称一致。
19: - [Google Gemma的Transformers入口](https://ai.google.dev/gemma/docs/core/huggingface_inference)：仅作为官方模型使用来源，不证明本A800训练配方或比赛四卡部署已通过。
20: 
21: ## D073训练交接修复
````

## s079

来源：`learning_pipeline/training/train_core.py:266`

````text
266: def preserve_explicit_lora_scope(model,base,targets):
267:     """Keep saved PEFT scope exact after its optional suffix minimization."""
268:     wanted=set(targets)
269:     actual={name for name,module in base.named_modules() if 'default' in getattr(module,'lora_A',{})}
270:     if actual!=wanted:raise ValueError('Actual LoRA injection differs from the explicit text projection set')
271:     # Serialization only: injection is already complete. Avoid broad suffixes
272:     # expanding into other towers when an adapter is loaded on a full model.
273:     model.peft_config['default'].target_modules=wanted
274:     return sorted(actual)
````

## s080

来源：`learning_pipeline/SPEC.md:148`

````text
148: ## D081：QAT辅助状态与FSDP2兼容（2026-11-14）
149: 
150: r04原包188352B/110文件（109payload+summary）完整归档。四rank同在PyTorch2.10 nn.Parameter(DTensor)默认requires_grad=True时报非浮点错误，无31B longest前向/反向或优化更新。普通小Gemma在真实2.10CUDA全过，不能据此宣称31B显存稳定。原日志不含失败参数名；RoPE/zero_point不是已证根因。
151: 
152: 固定compressed-tensors 0.15.0.1源码及真实小Linear压缩/解压证实：pack-quantized解压删除weight_packed，但保留weight_shape int64 Parameter和浮点weight_scale。官方2.10包装参数的先后顺序与错误栈吻合；没有现场清单前，具体31B触发参数仍标强候选。新版在31B分片前落盘全部参数名称/shape/dtype/bytes/grad供直接定位。
153: 
154: 修复只按属性分类：decoder冻结非浮点Parameter在各layer与root均忽略分片，保留原对象、dtype和同步后值；不转float、不删除、不转buffer、不动依赖库。冻结浮点weight/scale仍分片，820 FP32 LoRA仍复制显式归约。首次广播前全rank名称/shape/dtype顺序一致；记账分开decoder浮点可分片字节、非浮点复制字节与实际local逻辑字节，容量仍看CUDA实测而非参数理论量。
155: 
156: 回归升级现有小Gemma检查，不追加长CPU门禁：真实CT symmetric int4/group32 compress→decompress，出现weight_shape/weight_scale后再PEFT和四rankFSDP；含旧默认Parameter(int)负控、8→6→8、最长两累积、dropout重算、冻权/元数据逐值、PEFT存取。该测试是module lifecycle，不冒称31B原checkpoint加载。通过后沿现有长样本/首步→两epoch训练链直接执行。D03/全部输入及mask/19356/BF16/超参/Agent/GPU0–3与保护策略不变；新execution版本r02，新发行trajector … [长行省略]
````

## s081

来源：`learning_pipeline/training/fsdp_runtime.py:1`

````text
1: """Shard frozen decoder weights; explicitly average replicated LoRA gradients.
2: 
3: The shared embedding/head stay replicated: selected-token CE invokes the head a
4: different number of times on different ranks. Never put that head in an FSDP
5: collective. A parameterless objective root coordinates the decoder FSDP states.
6: """
7: import torch
8: import torch.distributed as dist
9: 
````

## s082

来源：`learning_pipeline/training/fsdp_runtime.py:49`

````text
49:     # All ordinary tensors left outside managed decoder groups stay replicated.
50:     ignored={p for p in objective.parameters() if not isinstance(p,DTensor)}
51:     fully_shard(objective,mesh=mesh,reshard_after_forward=True,ignored_params=ignored)
52:     del ignored,ignored_layer_parameters
53:     if objective._get_fsdp_state()._fsdp_param_group is not None:
54:         raise ValueError('Objective root unexpectedly owns a parameter group')
55:     for layer in layers:
56:         for p in layer.parameters():
57:             if not p.requires_grad and p.is_floating_point() and not isinstance(p,DTensor):raise ValueError('Floating decoder weight left unsharded')
````

## s083

来源：`learning_pipeline/training/SOURCES.md:36`

````text
36: ## D077 MLP分块重算与四rank
37: 
38: 实际失败点来自固定[Transformers 5.13.1 Gemma4TextMLP](https://github.com/huggingface/transformers/blob/v5.13.1/src/transformers/models/gemma4/modeling_gemma4.py)及[PEFT 0.21.2 Linear](https://github.com/huggingface/peft/blob/v0.21.2/src/peft/tuners/lora/layer.py)。MLP按位置独立，直接复用原bound forward，以512-token块运行，不改公式、投影或参数键名；KV共享区的加宽MLP … [长行省略]
39: 
40: 依据[PyTorch 2.10 checkpoint](https://docs.pytorch.org/docs/2.10/checkpoint.html)，采用non-reentrant及preserve_rng_state=True；原decoder外层checkpoint仍在。固定分块的dropout重算必须等价，分块前后随机数分配不保证逐位相同。实际固定TF/PEFT微型数值检查包含普通/加宽MLP、非整除尾块、嵌套重算、LoRA输入/参数梯度与RNG；不是31B CUDA峰值证明。
41: 
42: 四rank每rank累积2，保持global8/498更新；尾6分2/2/1/1，各rank以最后一个真实micro同步，梯度平均后仍sum/8。真实四Gloo进程覆盖full8→tail6→full8及MLP嵌套重算。最长完整序列预检同时保留同形同dtype的两份AdamW状态容量，仅预留容量而非提前optimizer.step；恢复RNG/清梯度后，首个原洗牌批真正更新并验证AdamW状态，记入498步。
43: 
44: 旧双卡r01两次目标机OOM/零更新证据保留。本轮四A800实际长序列前反向、吞吐、adapter加载与成绩全部待服务器回传，不预报速度或提分。
````

## s084

来源：`learning_pipeline/training/SOURCES.md:9`

````text
9: - [Scott Weeden 的公开训练kit](https://www.kaggle.com/datasets/scottweeden/gemma-4-developer-agent-tool-lora-kit)：本地归档为 `vendor/lora_research_20261112/scott_kit/4/scripts/train.py`（43326字节）及 `0/recipe/gemma4_31b_peft_recipe.md`（6607字节）；原始来源和许可记录见该目录外的 `SOURCE_NOTES.txt`。实际代码提供了按query分块的checkpoint attention、仅目标位置的chunked CE、 … [长行省略]
10: - 上述作者实际脚本还使用NF4，并在注意力注册失败时将长度降至6144、过滤长行；这些行为不进入本框架。我们保留完整32768上限，任何超长行或注册失败都显式失败。原PEFT adapter保持不变，按实际competition wheel另做K→V服务导出，不套作者宽后缀配置。
11: - 作者榜上最佳成绩没有绑定到这一份adapter/脚本，不作为本包效果证据。其他已归档教程存在Gemma2/随机fallback、模拟训练或全token标签问题，未作为运行实现采用。
12: 
13: ## 第一方版本依据
14: 
15: - [Transformers 5.13.1 Gemma4接口](https://huggingface.co/docs/transformers/v5.13.1/en/model_doc/gemma4)：核对实际 `Gemma4ForConditionalGeneration`、原生工具模板与文本配置；不把普通31B或新版接口替代现有QAT父模型。
16: - [Transformers 5.13.1 compressed-tensors量化器](https://github.com/huggingface/transformers/blob/v5.13.1/src/transformers/quantizers/quantizer_compressed_tensors.py)：`run_compressed=False`在加载后走decompress_model。归档同版本wheel源也已检查，沿用此API而不改成后续版本的参数。
17: - [PEFT 0.21.2 LoRAConfig源码](https://github.com/huggingface/peft/blob/v0.21.2/src/peft/tuners/lora/config.py)：核对target_modules、rank/alpha/dropout与bias参数。该版本官方文档URL本次不能访问，因此版本特定判断以实际wheel/源码及已有真实CPU序列化检查为准；不将重定向到0.21.0的文档冒充0.21.2。
````

## s085

来源：`reports/trajectory_cycle02_fsdp4_r01_return_review_20261116/overview.md:13`

````text
13: | 导出/服务 | 820→840 BF16逐值正确，195042419B六成员单adapter ZIP，真实vLLM短加载通过 | 可作为评测候选；短探针不等于解题提分 |
14: 
15: ## 需要纠正的loss口径
16: 
17: 全524步的全局损失都不是0。末步524只有2条真实样本，全局均值**0.00413464645**；rank0 rich3905的18目标token为0.00000260272，rank1 fastapi5077的48目标token为0.00826669019，rank2/3没有真实行、只有零loss通信槽。将单卡近零值、空槽或少量简单目标写成“训练loss已归零”会夸大结论。全局日志聚合代码正常，不需要为此重跑训练。
18: 
19: `mean_supervision_loss`是行内行动/说明联合损失的真实样本均值；反向传播另有task均权。上述每轮均值描述同一批数据在持续更新过程中的日志，不是固定checkpoint重放或独立验证。C01和C02数据构成不同，不能用两轮训练的绝对loss直接判模型谁更强。
20: 
21: ## 梯度不是持续发散，但也不能说毫无波动
````

## s086

来源：`reports/trajectory_cycle02_fsdp4_r01_return_review_20261116/overview.md:29`

````text
29: ## 参数变化与能力变化分开判断
30: 
31: 本轮122429440个LoRA参数中122429400个值改变；原生adapter整体ΔL2/L2父=0.1259964，最大单值变化0.00457803。这是参数空间的变化描述，不是“能力提升12.6%”。继承父权重、首步真实更新、完整样本消费与权重变化共同支持续训不是空跑。
32: 
33: 本轮47549010输入token，比C01的40296066多约18%，最长26979也高于19356；吞吐971.6对1087.2不是同工作量性能基准。当前batch/序列保持即可，不因为allocated52.7就推断可以直接扩大批次。
34: 
35: ## 下一步判断标准
36: 
37: 建议保持当前prompt、六工具与预算，进入无教师129题开发对照，分别比较C02 T1.0与C01的52、C02 T0.2与C01的48。除总resolved外，重点看已定位后及时修改、真实入口/兼容范围、有效触发检查、空patch及无效继续生成，并确认传参改善没有回退。129题已经训练暴露，不能把该分数称为独立泛化，Kaggle结果另行观察。
````

## s087

来源：`reports/trajectory_cycle02_fsdp4_r01_return_review_20261116/overview.md:15`

````text
15: ## 需要纠正的loss口径
16: 
17: 全524步的全局损失都不是0。末步524只有2条真实样本，全局均值**0.00413464645**；rank0 rich3905的18目标token为0.00000260272，rank1 fastapi5077的48目标token为0.00826669019，rank2/3没有真实行、只有零loss通信槽。将单卡近零值、空槽或少量简单目标写成“训练loss已归零”会夸大结论。全局日志聚合代码正常，不需要为此重跑训练。
18: 
19: `mean_supervision_loss`是行内行动/说明联合损失的真实样本均值；反向传播另有task均权。上述每轮均值描述同一批数据在持续更新过程中的日志，不是固定checkpoint重放或独立验证。C01和C02数据构成不同，不能用两轮训练的绝对loss直接判模型谁更强。
20: 
21: ## 梯度不是持续发散，但也不能说毫无波动
22: 
23: 实际515/524步的裁剪前范数超过1；step182为7984.323，随后第183步27.957、第184步3.926，后续日志和最终权重均有限。执行代码在AdamW前以max_norm=1.0裁剪，日志记录的是返回的裁剪前范数。该语义可对照[PyTorch2.10官方说明](https://docs.pytorch.org/docs/2.10/generated/torch.nn.utils.clip_grad_norm_.html)。不能把7984称为裁剪后的更新幅度，也不能由“没有NaN”推断它一定有益。
````

## s088

来源：`reports/trajectory_cycle02_fsdp4_r01_return_review_20261116/overview.md:21`

````text
21: ## 梯度不是持续发散，但也不能说毫无波动
22: 
23: 实际515/524步的裁剪前范数超过1；step182为7984.323，随后第183步27.957、第184步3.926，后续日志和最终权重均有限。执行代码在AdamW前以max_norm=1.0裁剪，日志记录的是返回的裁剪前范数。该语义可对照[PyTorch2.10官方说明](https://docs.pytorch.org/docs/2.10/generated/torch.nn.utils.clip_grad_norm_.html)。不能把7984称为裁剪后的更新幅度，也不能由“没有NaN”推断它一定有益。
24: 
25: 作为背景，C01已有470/498步超过1、最大7792.382；本轮裁剪更频繁，但这个量级的孤立峰并非首次出现。尚无逐样本梯度或step182前后参数/优化器快照，不将尖峰归因于某一道题、不宣称精确裁剪后实测范数。当前证据支持保留为观察项，先做行为评测，不据此直接改学习率或追加epoch。
26: 
27: ![C02训练损失与裁剪前梯度](training_curve.png)
28: 
29: ## 参数变化与能力变化分开判断
````

## s089

来源：`learning_pipeline/specs/FRESH_SFT_C03.md:17`

````text
17: 发布身份：`trajectory_cycle03_fresh_fsdp4_r01`。数据身份：`learning_runs/TC-r04-001/datasets/D05_fresh`，复用该轮无adapter的6842B Agent来源；新数据拥有独立来源/选择/派生/资格账本，原round catalog和原D03/D04不可变。最终N=990、160个任务ID，U=124/W=7，保存31/62/93/124，末批6条按2/2/1/1分配（按真实长度决定rank）。
18: 
19: ## 本次实际冻结结果
20: 
21: | 项目 | 实际值 |
22: |---|---:|
23: | 本地D04重选 |720回合/113任务ID；原2090中1370排除、16题不凑只读样本 |
24: | 公开Native |233回合/41任务ID |
25: | 公开A |37回合/6任务ID；source-converted，不冒称本SDK重执行 |
````

## s090

来源：`reports/fresh_lora_implementation_20261116/training/implementation.md:13`

````text
13: - final 仅保留一份 AdamW，另有各 rank Python/Torch CPU/本地 CUDA RNG（numpy 已加载时也保留）、冻结调度/输入绑定及 next epoch/batch cursor。明确 `resume_entry_implemented=false`，没有声称已有可执行恢复入口。
14: 
15: 最长完整输入、无更新容量预检、MLP 分块重算、LoRA SUM/world 归约、nonfloat ignore、尾批真实行和零通信槽沿用旧机制。没有截断、补样、重采样或额外 update。提示/六工具/预算/评测不在本次编辑范围。
16: 
17: ## 日志口径
18: 
19: 保留旧 `mean_supervision_loss`。新增 `weighted_objective=sum(loss_weight*rowCE)/global8`，这是实际梯度目标；`mean_weighted_supervision_loss` 以实际行数作分母，尾批两者不同。每步记录 applied_learning_rate、实际消费行数、动作/说明各自的行均 CE / 有标签行数 / 正 token 数，以及 preclip norm、clip 上限/是否发生/实际缩放因子。通信空槽不计行或组件诊断。
20: 
21: ## CPU 验证
````

## s091

来源：`reports/self_c01_d03_t10_kaggle_20261115/t02_completed_observation.json:1`

````text
1: {
2:   "observed_utc": "2026-11-15T09:01:01.994150+00:00",
3:   "source": "Kaggle CLI authenticated competitions submissions API",
4:   "submission": {
5:     "ref": <OPERATION_ID>,
6:     "fileName": "submission.zip",
7:     "date": "2026-11-14T14:10:54.567000",
8:     "description": "SELF-C01-D03-T02-r01 | 31B QAT v2 | LoRA r16 joint trajectory SFT, 2 epochs | native6 | 480s/28 tools/80 turns | 8192 total/4096 thinking | T0.2 | public-task data",
9:     "status": "SubmissionStatus.COMPLETE",
````

## s092

来源：`learning_pipeline/evaluation/panel_materialize.py:43`

````text
43: def export_snapshot(source, target, model_config):
44:     import torch
45:     from safetensors.torch import load_file, save_file
46:     from export_serving import expand
47:     config=read(source/'adapter_config.json')
48:     if config['r']!=16 or config['lora_alpha']!=16 or config.get('use_rslora',False) or len(config['target_modules'])!=410:raise ValueError('Fresh r16/a16 recipe differs')
49:     native=load_file(source/'adapter_model.safetensors',device='cpu')
50:     if len(native)!=820 or any(t.dtype!=torch.float32 or not torch.isfinite(t).all() for t in native.values()):raise ValueError('Expected finite native820 FP32')
51:     output,cfg,copied=expand(native,config,read(model_config)['text_config'])
````

## s093

来源：`learning_pipeline/specs/FRESH_SFT_C03.md:38`

````text
38: 实际交付目录`dist/harness_k_a800/trajectory_cycle03_fresh_fsdp4_r01`，7件67,227,657B；source_bundle.zip67,203,586B/63成员。独立发行审查确认完整字节、真实990行服务器CPU合同、配方/原Agent/GPU/回传策略全部通过。旧C02评测运输5件207,643,257B已直接字节核同归档到`archive/harness_k_a800/trajectory_cycle02_joint_t02_t10_pair_r01_transport_D095`，IDE与服务器未动。
39: 
40: 实际31B CUDA最长检查、首更新、124步训练、加载以及候选行为评测仍NOT_RUN，须由当前zcode交接执行。中间快照只作可选权重，final未被宣称最佳；完整resume入口仍未实现。所有完成状态/报告入口在[本轮总览](../../reports/fresh_lora_implementation_20261116/overview.md)。
41: 
42: ## 数据与监督合同
43: 
44: - 本地D04复用已通过的原生mask、动作语义及公开说明资格；重选阶段价值，避免再次审阅全部历史HTTP。每题保留必要定位、源码编辑、命中真实行为的验证、恢复/事实收尾；既有源身份不是质量分数，不能只选教师或只选成功链。
45: - 公开候选只Native/A。训练split47issue/85轨迹由两个干净Sol/max curator分24/23题阅读其全部动作/回执链；正标签reasoning必须全文已读并接受，历史未读reasoning继续masked。dev/final各9issue/8repo不用于筛选目标；B/B2不扩入首轮。
46: - 每题选主要有效路径，可保留互补真实状态。无信息重复、错误/未知源码、工具格式错、假成功不进正标签。不得把源resolved当本SDK重执行通过；保留SOURCE_NATIVE_CAPTURE_SUPPORTED与SOURCE_CONVERTED_SUPPORTED。
````

## s094

来源：`reports/checkpoint_panel_c03_20261116/overview.md:29`

````text
29: 五件压缩上传约12.5MB；source_bundle.zip 12,509,850B /65成员，字节与展开/当前源直接一致。README另按独审澄清CPU序列化fixture存在。只上传该新目录，避免再次同步训练目录。实际大小清单与最后直接绑定见delivery_receipt.json。
30: 
31: analysis回传8臂所有HTTP/ADK/patch/verification/官方trace/日志/配置/SDK路由/耗时/显存/清理/预留；中间权重、八臂精确输入ZIP保留服务器，外部引用明确NOT_RETURNED。本地可完整读行为证据，但不能声称已独立直比中间权重；选定后再取该候选准确温度的输入。--full可自含。原C03训练回传独立保留。
32: 
33: 无新hash、模型下载、依赖安装、服务器操作、教师、训练、129或Kaggle提交。旧训练发行和历史数据/adapter不变。
````

## s095

来源：`reports/checkpoint_panel_c03_20261116/overview.md:21`

````text
21: - 四卡各自worker、清理后切温；单卡启动失败不打断其他卡。CPU失败且未动GPU允许真实PRESERVED封包。未闭合子进程显式阻止切温和封包。
22: - 原env.sh的进程级PATH/LIBRARY_PATH/PYTHONNOUSERSITE/offline/proxy清理移入统一driver，避免继承训练shell造成包源漂移；官方模块不改。
23: - GPU0闲置外部≤2GiB/≤5%窄例外保留，0/1原状，2/3完成后恢复本人预留。无他人kill/reset；短IPC、现场UUID到index沿用。
24: 
25: root9项CPU检查通过；干净GPT-6.1 Sol/max对实际压缩包、八套生成配置、边界及失败路径独立16项通过，0开放阻断。真实目标GPU、模型服务和96题尚NOT_RUN，本地小张量/模拟失败不冒称目标通过。独审记录见[independent](independent/final_review.json)。最初本地tempfile ACL及独审Windows编码失败已保全；不属于评测结果。
26: 
27: ## 交付与回传
28: 
29: 五件压缩上传约12.5MB；source_bundle.zip 12,509,850B /65成员，字节与展开/当前源直接一致。README另按独审澄清CPU序列化fixture存在。只上传该新目录，避免再次同步训练目录。实际大小清单与最后直接绑定见delivery_receipt.json。
````

## s096

来源：`learning_pipeline/evaluation/lean_return.py:69`

````text
69: def pack_files(files,reference_submission,target,metadata=None,reference_requirement=None):
70:     """Original ZIP container bytes are replaced by complete member evidence."""
71:     target=Path(target)
72:     if target.exists():raise FileExistsError('Preserve the prior return')
73:     reference=Reference(reference_submission);rows=[];stored=[];saved=0;origins={}
74:     names=[safe(n) for _,n in files]
75:     if len(names)!=len(set(n.casefold() for n in names)) or any(n.startswith(PREFIX+'/') for n in names):
76:         raise ValueError('Duplicate or reserved logical path')
77:     try:
````

## s097

来源：`reports/checkpoint_panel_c03_20261116/overview.md:31`

````text
31: analysis回传8臂所有HTTP/ADK/patch/verification/官方trace/日志/配置/SDK路由/耗时/显存/清理/预留；中间权重、八臂精确输入ZIP保留服务器，外部引用明确NOT_RETURNED。本地可完整读行为证据，但不能声称已独立直比中间权重；选定后再取该候选准确温度的输入。--full可自含。原C03训练回传独立保留。
32: 
33: 无新hash、模型下载、依赖安装、服务器操作、教师、训练、129或Kaggle提交。旧训练发行和历史数据/adapter不变。
````

## s098

来源：`learning_pipeline/evaluation/panel_driver.py:246`

````text
246:         pack_files(files,ref,staging/(aid+'.tar.gz'),metadata={'arm':item['arm'],'reference_local_status':'SERVER_RETAINED_NOT_ASSUMED_LOCAL'},
247:                    reference_requirement='This panel input is retained on the server. All references were compared in full on producer; local full restoration requires fetching this exact per-arm input. All traces and non-reference bytes are present.')
248:         refs.append({'arm_id':aid,'server_path':str(ref),'bytes':ref.stat().st_size,'local_status':'NOT_RETURNED_UNLESS_FULL',
249:                      'input_state':'MATERIALIZED_INPUT' if input_ready else 'NOT_CREATED_USING_COMMON_BASE_FOR_EVIDENCE_DEDUP_ONLY'})
250:     metadata={'profile':'full' if full else 'analysis','required_external_refs':refs,'comparison':comparison,
251:               'middle_weights_policy':'Retained on server; fetch chosen input later. Never substitute final for middle weights.'}
252:     put(staging/'return_index.json',metadata)
253:     files=[(p,'arms/'+p.name) for p in sorted(staging.iterdir()) if p.is_file()]
254:     for prefix,base in [('control',CONTROL),('exports_evidence',ROOT/'work/evidence')]:
````

## s099

来源：`learning_pipeline/server/NOTICE.txt:1`

````text
1: This is a local action-SFT pilot, not a score or model-performance guarantee.
2: 
3: Task/action sources: the user's archived public development task runs for
4: FastAPI (https://github.com/fastapi/fastapi), Rich (https://github.com/Textualize/rich),
5: Requests (https://github.com/psf/requests), HTTPX (https://github.com/encode/httpx),
6: and actual offline Sol action corrections executed against the public task source.
7: The corresponding projects' licenses and notices continue to apply. No hidden
8: evaluation set, reference patch, full source snapshots or model weights are included.
9: 
````

## s100

来源：`reports/lora_method_reassessment_20261116/AYUSH_DATASET_AND_SCALE.md:7`

````text
7: 母集来自Dmitrii Gluzdov的[Gemma 4 Serving and LoRA Verification Kit](https://www.kaggle.com/datasets/dmitriigluzdov/gemma-4-serving-and-lora-verification-kit)，不是Ayush另行发布的私有52条集合。官方CLI核到datasetId12312635、94个文件，元数据许可证为CC BY4.0；仓库源码和教师材料仍按随附声明保留来源。
8: 
9: 数据卡及lora_summary.csv列出：
10: 
11: | 层 | 成功轨迹 | 独立题（各层内部） | 目标回合 |
12: |---|---:|---:|---:|
13: | Native |103|55|1766|
14: | External A |14|14|382|
15: | External B |729|729|25165|
````
