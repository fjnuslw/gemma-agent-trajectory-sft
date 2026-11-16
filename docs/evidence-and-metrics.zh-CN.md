# 证据与指标表

> 核对日期：2026-11-16。用途：在简历、作品集或面试中，每个数字和技术主张都能回到明确证据。本次仅定向阅读源码与已有报告；没有重新执行历史训练、全量 token 审计、模型加载、任务判分或权重逐值验算。表内“已验证”指相应既有报告记录的验证。

## 1. 如何读这份表

- **已实现**：当前源码存在相应路径，不自动等于目标机通过。
- **CPU/发行已验**：本地数学、小模型、数据合同或打包检查有记录，不替代 31B/CUDA 或官方任务执行。
- **历史目标机已验**：具体 C01/C02 运行已有回传和接纳，只覆盖对应配置。
- **数据已冻**：来源、选择、模板/mask 和快照通过冻结合同，不代表数据对行为必有收益。
- **用户报告**：用户转述或截图观察，没有在本次重新获得独立原件核验。
- **待验证**：链路/计划已准备，实际目标行为或完整恢复尚无证据。

每条主张有 E 编号；[小型机器索引](../evidence/claims.json)保存主要证据的项目相对路径、定位文本和核对时行号，不包含权重或成批轨迹。源码链接指本次整理的脱敏摘录，原行号属于本地证据快照；历史行为结论同时链接到对应报告，不能把当前源码当作所有旧运行的执行身份证明。

## 2. 系统执行与可观测性

| 编号 | 可对外主张 | 状态、版本与指标 | 源码/报告锚 | 限制与推荐说法 |
|---|---|---|---|---|
| E01 | 围绕官方 SDK 建立本地编码 Agent 实验与取证链路 | 已实现、历史目标机已验；当前学习线以 r04 六工具为输入身份 | [Evaluator 接线](source-excerpts.md#s007)、[观测层边界](source-excerpts.md#s013) | 官方 ADK 循环、工具与独立判分来自上游。可说“接入与适配官方执行/评分”，不要说“自研官方裁判”。 |
| E02 | 固定任务预算、采样和提交身份，支撑可复现对照 | r04：28计费调用/480s/80轮；命令240s、验证300s；8192总输出/4096思考；p=.95/k=64 | [冻结配置](source-excerpts.md#s006)、[C03合同](source-excerpts.md#s062) | 六工具是项目选择，非官方工具上限；免费 status/submit 与计费分开。当前活动配置不替代冻结输入。 |
| E03 | 通过 HTTP→ADK→官方 trace 关联模型生成与实际执行 | 历史 C01 双臂：7031请求、6464工具动作、258题臂有审查 | [被动 HTTP relay](source-excerpts.md#s012)、[覆盖报告](source-excerpts.md#s014) | 覆盖是对应运行的动作/路由绑定，不是所有长 reasoning 逐字深读，也不是服务吞吐指标。 |
| E04 | 保存官方压缩前后证据并核实摘要送达 | 已实现；interval15/overlap2/threshold14336/retention5；C01可解析摘要167/141均送达 | [ADK配置](source-excerpts.md#s009)、[真实摘要观察](source-excerpts.md#s010) | compaction 来自 ADK。送达不等于准确，C02有摘要状态失真；不要宣称“无损压缩”。 |
| E05 | 用空补丁/参考补丁控制限定本地评测适用性 | 129任务×2控制=258题次；117参考可过=115判别+2空补丁可过；12限制 | [校准原件](source-excerpts.md#s063)、[口径解释](source-excerpts.md#s015) | 复用既有校准，本次未重跑。原始129分母保留；不声称隐藏镜像等价。 |
| E06 | 分离 Agent 会话时长、任务总时长与官方结果 | 已实现，历史逐题原件可核；独立保存patch/verification/summary | [时间与结果字段](source-excerpts.md#s064) | HTTP墙钟包含等待/prefill/生成/传输，不能直接称纯decode或私人思考时间；包装exit0不等于全题成功。 |

## 3. 轨迹、教师与训练数据

| 编号 | 可对外主张 | 状态、版本与指标 | 源码/报告锚 | 限制与推荐说法 |
|---|---|---|---|---|
| E07 | 用可重叠错误分类指导下一状态与训练目标 | 已实现并用于既有轨迹复盘；调用表示、行动时机、语义覆盖分开 | [分类口径](source-excerpts.md#s017)、[具体失败形态](source-excerpts.md#s018) | 分类数不等于互斥主因，也不是“可挽回题数”；未知语义原因保留未知。 |
| E08 | 从真实学生请求边界进行受控教师接续 | 已实现、既有真实采集；请求/状态比较、实际唯一ADK调用和回执绑定 | [执行绑定](source-excerpts.md#s019)、[实际输入与提案](source-excerpts.md#s020) | 独立离线重建，非暂停原比赛时钟；RECONSTRUCTED不说成原现场逐字节续跑；本次无新教师。 |
| E09 | 把局部有效动作与整题修复资格分开 | 合同与导出已实现；局部lane、自然闭合TaskResult、review来源分别记录 | [短片段合同](source-excerpts.md#s065)、[自然链合同](source-excerpts.md#s023) | 工具ok不等于语义正确；无独立TaskResult的fragment不能记FULL_REPAIR或学生新得分。 |
| E10 | 使用动作前公开短决策说明，拒绝后验改写进入监督 | 已实现；TEACHER_PUBLIC_DECISION_SUMMARY；私人thinking未采集 | [CLI协议](source-excerpts.md#s024)、[来源字段](source-excerpts.md#s066)、[主SPEC](source-excerpts.md#s067) | 可以讲“公开决策说明监督”；不能讲“获取了教师内部思维链”或“思维链蒸馏已证提分”。 |
| E11 | 用真实模板/tokenizer严格定位当前目标的监督边界 | 已实现；21个既有真实serving渲染fixture保持；D05全990行/12199343位置已有独审核验 | [文本/token前缀](source-excerpts.md#s068)、[D05独审范围](source-excerpts.md#s069) | prefix、历史、工具回执标签均-100；完整前缀仍参与前向。该规模来自既有独审，本次未重跑全量审计。 |
| E12 | 为C03增加完整合格completion CE，保留旧action/joint | 已实现、CPU已验；多call全资格、action/reason不重叠且覆盖当前全部正token | [Native完整目标](source-excerpts.md#s025)、[loss分支](source-excerpts.md#s042) | 未合格当前说明先显式移除再渲染，不能把它masked后留作答案提示；不是旧数据回改。 |
| E13 | 解决A转换来源工具回执后的模板边界差异 | 数据已验；18字符thought入口；14轨迹/382源目标严格重建、选中37 | [修复原因与全量机械核对](source-excerpts.md#s028)、[源码](source-excerpts.md#s070) | 382是源目标结构检查数，非新增训练行；未改变当前训练模板，不是SDK重执行。 |
| E14 | C03冻结多来源、多个行动阶段的990目标回合 | 数据已冻：本地720/113任务，Native233/41，A37/6，合计990/160；外部47任务/45repo | [最终统计](source-excerpts.md#s071)、[来源拆分与重叠说明](source-excerpts.md#s072) | 990不是独立题、不是完整轨迹；本地Gemma245/Sol475的任务覆盖存在交集。 |
| E15 | 对训练token、长度和来源暴露做明确核算 | 12199343输入位置；199458正token=156980行动/边界+42478说明；最长26910，最大completion2208；0截断 | [统计](source-excerpts.md#s073)、[真实summary](source-excerpts.md#s074) | 训练上下文上限32768与推理输出上限8192分开；summary的初始状态字段不单独决定冻结结果。 |
| E16 | 按题/阶段均衡优化目标，避免长链天然占更多权重 | 数据与CPU已验：m=N/(T*K_i*n_i_s)，平均1；阶段313/271/195/211；倍率.1546875–3.09375 | [权重计算](source-excerpts.md#s030)、[统计及系数](source-excerpts.md#s075) | 均匀无放回遍历，不叠加加权重采样；说明27.3905%是目标系数，非梯度贡献；尚无行为最优证明。 |
| E17 | 外部数据按仓库留出，明确原生与转换来源边界 | 已审选/未实跑留出：train47任务/85attempt/1532回合；dev/final各8repo9issue未训练 | [审选范围](source-excerpts.md#s076)、[留出限制](source-excerpts.md#s077) | Native=SOURCE_NATIVE_CAPTURE_SUPPORTED，A=SOURCE_CONVERTED_SUPPORTED；均不冒称本SDK新执行。留出环境尚未重建，本地129已暴露。 |

## 4. 训练、长序列与导出

| 编号 | 可对外主张 | 状态、版本与指标 | 源码/报告锚 | 限制与推荐说法 |
|---|---|---|---|---|
| E18 | 恢复官方QAT权重为冻结BF16底座，只更新PEFT LoRA | 已实现、历史GPU已验；run_compressed=False，冻结恢复后底座，关闭QDQ wrapper | [加载/冻结实现](source-excerpts.md#s031)、[固定版本来源](source-excerpts.md#s078) | 不是重新预训练、全参微调或新NF4量化；恢复的是原QAT包权重。 |
| E19 | 精确限定文本LoRA范围，防止宽后缀扩到其他塔 | 已实现、历史GPU已验；60层410投影/820A-B张量/122429440参数；FP32 adapter | [模块枚举与PEFT](source-excerpts.md#s032)、[序列化scope](source-excerpts.md#s079) | C03 r16/a16/.05；C01/C02 r16/a32/.05为不同配方。没有执行rsLoRA或更大rank消融。 |
| E20 | 对冻结decoder做FSDP2分片，LoRA复制并显式归约 | 已实现、历史四A800已验；60decoder分片，rank0初始广播，LoRA SUM/world，每global批一次 | [分片与身份检查](source-excerpts.md#s033)、[有界bucket归约](source-excerpts.md#s034) | FSDP2为PyTorch能力；项目贡献是选择性分组、同步及执行边界，不说发明分布式训练框架。 |
| E21 | 处理混合dtype与QAT残余非浮点Parameter | 已实现、生命周期检查及历史训练已验；非浮点冻结状态复制保留，浮点scale仍分片 | [非浮点兼容](source-excerpts.md#s035)、[历史问题与边界](source-excerpts.md#s080) | 不删/不转dtype求通过；最初缺现场参数名，weight_shape只能称强候选，不能倒写确定根因。 |
| E22 | 让共享embedding/head留在FSDP外，避免动态head调用次数引起collective错序 | 已实现、历史GPU已验；无自有参数组objective root，保共享关系 | [设计说明](source-excerpts.md#s081)、[root/head检查](source-excerpts.md#s082) | 是针对本selected CE路径的工程设计，非通用最优分片方案；冻结decoder仍需传递输入梯度。 |
| E23 | 通过MLP分块重算和query分块attention保留完整长输入 | 已实现、CPU数学/历史GPU已验；MLP512，non-reentrant/preserve_rng；query512，完整因果/局部窗口 | [MLP实现](source-excerpts.md#s036)、[attention实现](source-excerpts.md#s037)、[参考来源与限制](source-excerpts.md#s083) | 不截断因果序列；不承诺分块与未分块dropout逐位同；attention只支持既定无padding/batch1文本场景，有显式GQA展开。 |
| E24 | 仅在正标签位置计算分块vocabulary CE，降低logits开销 | 已实现、CPU值/梯度及历史GPU已验；head chunk64、因果位置-1、softcap保留 | [selected CE](source-excerpts.md#s038)、[来源归属](source-excerpts.md#s084) | 完整底座前向仍在；设计参考公开kit，不称原创算法或无工作量控制的加速收益。 |
| E25 | 全局batch与任意尾批保持真实样本消费和通信顺序 | 已实现、CPU及历史GPU已验；4×micro1×accum2=8；尾6为2/2/1/1，尾2为1/1/0/0；保留分母8 | [微步/归约](source-excerpts.md#s040)、[零loss通信槽](source-excerpts.md#s041)、[C02实际消费](source-excerpts.md#s044) | 不补样、不drop-last；通信槽不计训练行且恢复RNG。尾批更新比满批小，是显式约定。 |
| E26 | 按Gemma共享K/V架构显式转换训练与服务adapter布局 | 已实现、C01/C02真实导出与短加载已验；native820FP32→serving840BF16，增加20个K→V复制项 | [导出函数](source-excerpts.md#s043)、[C02导出/加载](source-excerpts.md#s085) | 840不代表额外训练；需保存回读、SDK/路由和真实vLLM检查；短token数值变化不等于任务提分。 |
| E27 | 用目标最长样本、真实首更新和全行日志验证训练不是空跑 | 历史C01：1990×2/498更新、最长19356两累积、loop10.30h | [C01接纳表](source-excerpts.md#s045) | 首次load失败与后续成功分别保留；不能称全过程首次成功。C01测量窗口allocated43.84GiB不是推理峰值。 |
| E28 | C02续训确实继承父权重、更新并完成固定样本消费 | 历史C02：2090×2=4180消费/524更新；最长26979两累积；loop13.594h；真实短加载 | [C02执行/参数表](source-excerpts.md#s046)、[参数变化](source-excerpts.md#s086) | 新AdamW阶段，非optimizer精确续跑；820张量均变≠每个元素均变。ΔL2/L2=.1259964不是能力提升12.6%。 |
| E29 | 明确记录loss、梯度与显存的测量口径 | C02样本均值.226930→.192685；末步全局.00413465；515/524裁剪前norm>1；指定窗口allocated52.7096GiB、longest reserved59.45–62.85GiB | [loss解释](source-excerpts.md#s087)、[梯度解释](source-excerpts.md#s088) | 不是验证loss、不是全局归零；裁剪前峰值7984.323不是裁剪后更新幅度；无同工作量性能对照，不报加速倍数。 |
| E30 | C03实现按真实optimizer步数冻结的一pass调度与四快照 | CPU/发行已验、目标训练未接纳：990/global8→124更新；warmup7；peak1e-4→1e-5；31/62/93/124保存 | [调度合同](source-excerpts.md#s047)、[冻结规格](source-excerpts.md#s089) | 不能拿旧两pass的中点当独立一pass cosine；不是经本项目消融证明的最优LR/调度。 |
| E31 | 留存final optimizer/RNG/cursor，为后续恢复提供状态基础 | 已实现、CPU已验；final一份AdamW，各rank RNG，输入/调度绑定 | [留存实现](source-excerpts.md#s048)、[实现报告](source-excerpts.md#s090) | `resume_entry_implemented=false`；不能写“支持精确断点续训/故障自动恢复”。中间只有adapter快照。 |

## 5. 实验结果与未完成事项

| 编号 | 可对外主张 | 状态、版本与指标 | 源码/报告锚 | 限制与推荐说法 |
|---|---|---|---|---|
| E32 | C01改善部分协议指标，完整任务收益尚未建立 | 完整129开发：baseT.2=51，C01T.2=48，C01T1=52；同温度保持46/新增2/丢5；缺必填107→1、连续重复81→12、空patch11→23 | [结果与分母](source-excerpts.md#s049) | 全部为暴露开发题；缺当前baseT1控制；不写“LoRA提升解题率/稳定超过底座”。协议改善不代替任务成功。 |
| E33 | C02完整训练有效，但有限行为测试出现回退信号 | 主动中断：T.2=2/10，对C01同窗4/10；T1=1/6，对3/6；另2活动题中断，118/122未跑 | [部分结果](source-excerpts.md#s051) | 非随机FastAPI-only窗口，不补成129成绩，不确诊灾难遗忘、rank不足或GPU问题。 |
| E34 | 正确区分本地开发计数与Kaggle线上分数 | C01T.2=.17有既有API观测；T1=.15为2026-11-16用户截图，截图未显示ID，本次未API重核 | [T.2完成观测](source-excerpts.md#s091)、[最新截图记录](source-excerpts.md#s050) | 52/129不是.52或线上分数；不写奖牌、.22、排名或相对线上提升；无C03线上结果。 |
| E35 | C03 fresh重启由数据/方法复盘推动，明确联合变化与状态 | 数据已冻/发行完成；仅用户报训练23/124、loss1.25→.35、LR9.6e-5 | [动态状态](source-excerpts.md#s001)、[状态JSON](source-excerpts.md#s002)、[研究边界](source-excerpts.md#s052) | 不写已完成124步。数据/目标/alpha/LR/pass/调度同时变，未来改善不能单因素归因；未执行GRPO/DPO/在线RL。 |
| E36 | 预先定义checkpoint×温度面板并实现真实base与adapter分支 | CPU/发行已验、目标NOT_RUN：4候选×2温度×12=96题次；base/step62/93/124；25%只保留 | [面板设计](source-excerpts.md#s003)、[真实导出与检查](source-excerpts.md#s092) | 12题已暴露；checkpoint与GPU绑定、单次采样，不是独立泛化/多seed；final未被选为最佳；面板不自动执行129/提交。 |

## 6. 交付、恢复与来源归属

| 编号 | 可对外主张 | 状态、版本与指标 | 源码/报告锚 | 限制与推荐说法 |
|---|---|---|---|---|
| E37 | 用统一CLI和manifest管理数据、训练及评测交接 | 已实现；C03训练7件67227657B/63ZIP成员；面板5件、source ZIP12509850B/65成员 | [CLI](source-excerpts.md#s054)、[训练发行](source-excerpts.md#s093)、[面板发行](source-excerpts.md#s094) | 交接完成≠目标执行；入口不是自动无限采集/训练/发布闭环。成员数量必须说明所属ZIP。 |
| E38 | 安全展开后逐流直比，拒绝不同文件覆盖和路径逃逸 | 已实现、既有CPU/发行已验；成员集/类型/大小/CRC/链接/目标包含关系与最终字节检查 | [stage](source-excerpts.md#s055)、[部署合同](source-excerpts.md#s056) | 不是密码学签名或远程可信证明；CRC/大小不能单独证明内容身份；授权环境小修须保before/after/diff。 |
| E39 | 隔离失败、保护同卡切温顺序并保留真实partial状态 | 已实现/CPU已验；原IPC120B>107B零题失败已定位；短RPC目录；面板单卡失败隔离 | [IPC分配](source-excerpts.md#s057)、[worker清理检查](source-excerpts.md#s059)、[面板失败边界](source-excerpts.md#s095) | 只清可核归属的自有进程，不重置他人GPU；历史恢复缺after原表的地方不能补成“全证据齐”。 |
| E40 | 通过精确重复引用减少回传，同时保留行为证据 | 历史全内容验证：1706923496B→154622101B，减少90.94%；21093HTTP文件/7031组、1030ADK/observations原字节保留 | [瘦身实测](source-excerpts.md#s060)、[实现](source-excerpts.md#s096) | 仅对应包实测；依赖冻结输入才能恢复引用；ZIP成员字节≠原容器编码；未知/不同内容不省。 |
| E41 | 明确服务器留存权重与本地可恢复范围 | 面板实现并已交付；analysis全轨迹回传，中间输入/权重SERVER_RETAINED/NOT_RETURNED，full另选 | [回传限制](source-excerpts.md#s097)、[pack声明](source-excerpts.md#s098) | 不能用final替代step62/93，也不能把producer收据称本地已直比服务器全树。 |
| E42 | 保留官方能力、社区参考、数据及仓库许可证归属 | 来源记录已存在；公开母集元数据CC BY4.0；仓库源码/模型/依赖各自声明适用 | [实现来源](source-excerpts.md#s039)、[包NOTICE](source-excerpts.md#s099)、[母集许可记录](source-excerpts.md#s100) | 来源许可不自动覆盖模型、全部仓库或教师材料；本次未作发布授权认定，公开规则另标未核实。 |

## 7. 几个最容易写错的数字

| 容易混淆的表述 | 正确解释 |
|---|---|
| 990条数据 | 990个合格目标回合、160个任务ID；同题可以有多阶段、多前缀行。 |
| 199458个token | 当前正监督token；完整输入位置是12199343，不能二者互换。 |
| 820→840 | 训练与服务的张量布局转换；20个K/V复制项不是额外训练能力。 |
| 410投影 vs 420投影 | 训练真实共享K/V结构为410；服务独立槽展开为420。 |
| 27.3905%说明 | 依行内CE与任务/阶段倍率算得的目标系数；不是梯度、训练时长或数据行占比。 |
| .226930→.192685 | C02同一训练过程两epoch真实样本均值；不是固定checkpoint验证loss。 |
| 52/129 | C01 T1本地暴露开发结果；不是Kaggle public score，也不是同温度base增益。 |
| 23/124 | 用户报告的C03中途进度；不能写124更新已完成。 |
| 96题次 | 已交付检查点面板的计划执行量；不能计入已完成实验总数。 |
| 90.94% | 某份回传因重复权重/输入消除得到的压缩传输收益；不是训练显存下降或推理加速。 |

## 8. 证据冲突的处理方式

新动态规格和追加状态解释当前进度，旧报告保留原时点。D05的导出summary仍可能写 `EXPORTED_PENDING_NATIVE_MASK_AUDIT`，要结合独立审查、acceptance/frozen快照及后续状态，不能把早期字段单独当当前终态。同理，C01训练接纳报告中的线上PENDING已被后续完成观测补充，但原件不回写。

本次只读比较发现，当前 `agents/self_harness_v17` 的agent.yaml和prompt与r04冻结ZIP相同，eval_config.yaml和sampling.yaml不相同；因此历史预算/温度引用冻结ZIP、轮次配置与实际运行报告。这个比较没有改动任何文件，也没有据此认定活动配置错误。

证据证明的是项目能力和某次实验，不自动证明个人独立完成范围。简历动词须按本人真实分工选择；缺少职责、团队规模、任职时间、用户规模或上线范围时，一律不补造。
