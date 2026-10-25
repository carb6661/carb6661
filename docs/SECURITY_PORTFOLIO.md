# 安全项目演示与核验

以下项目是公开工程实践，不作为商业项目经验或真实事件处置经历。命令从相应项目目录执行，安装依赖以各仓库 README 为准。

当前深入建设的主项目是 [Compliance RAG Assistant](https://github.com/carb6661/compliance-rag-assistant)，优先展示真实业务闭环、实验反例与[设计复盘/简历模板](https://github.com/carb6661/compliance-rag-assistant/blob/main/docs/PROJECT_STORY.md)，而不是用仓库数量替代能力证据。

## Archive Ingest Guard：不可信归档与文件系统边界

[仓库](https://github.com/carb6661/archive-ingest-guard) · [12情境证据](https://github.com/carb6661/archive-ingest-guard/blob/main/examples/expected.json) · [测试](https://github.com/carb6661/archive-ingest-guard/blob/main/tests/test_archiveguard.py) · [问题与最终方案](https://github.com/carb6661/archive-ingest-guard/blob/main/docs/TECHNICAL_DEEP_DIVE.md) · [威胁模型](https://github.com/carb6661/archive-ingest-guard/blob/main/docs/THREAT_MODEL.md)

Python3.11–3.13、纯标准库，从仓库根目录运行：

```bash
python -S -X utf8 -m unittest discover -s tests -v
python -S -X utf8 -m archiveguard demo
python examples/make_bundle.py
python -X utf8 -m archiveguard check output/synthetic.zip
python -X utf8 -m archiveguard extract output/synthetic.zip output/new-private-container
```

Windows/Python3.12.8本地106项注册测试，105通过、1项真实符号链接创建因权限跳过；没有把跳过记为通过。[首次发布CI](https://github.com/carb6661/archive-ingest-guard/actions/runs/39974907963)对应eea141b，Windows/Linux × Python3.11/3.12/3.13六组均106项通过、无跳过，12情境全部重放；CI允许创建的真实目录symlink用例也通过。良性ZIP368字节，输出2文件/132字节。

可展示AppSec/安全开发的不可信路径、NUL原名、设备/流/大小写前缀冲突、声明与实际解压预算、先校验再写、全新容器独占、私有暂存与payload发布、故障回滚和证据最小化。后续成员CRC失败时不发布首个文件；已有目录/文件与竞争创建者的marker保持原状；清理失败明确返回error而非宣称回滚。微型坏路径只检查元数据，不运行有漏洞基线、不写出隔离区外。

明确只支持保守ASCII路径，中文正文可保留；高压缩率正常文本可能被拒绝。后缀不是内容验证：txt包裹ZIP的案例仍获准隔离，不阻止恶意Markdown/CSV/提示词。无HTTP上传、杀毒、强CPU/内存沙箱、抗恶意本地竞态或崩溃持久事务；尚未与Compliance RAG跨仓库集成，不冒充完整RAG系统。公开文件为原创代码与合成数据。

## Webhook Replay Lab：可信回调与事务幂等

[仓库](https://github.com/carb6661/webhook-replay-lab) · [10情境报告](https://github.com/carb6661/webhook-replay-lab/blob/main/examples/report.json) · [测试](https://github.com/carb6661/webhook-replay-lab/blob/main/tests/test_webhooklab.py) · [威胁模型](https://github.com/carb6661/webhook-replay-lab/blob/main/docs/THREAT_MODEL.md) · [故障与重试手册](https://github.com/carb6661/webhook-replay-lab/blob/main/docs/OPERATIONS.md)

安装requirements.lock后，从仓库根目录运行：

```bash
python -X utf8 -m webhooklab demo
python -X utf8 -m webhooklab live-smoke
python -X utf8 -m pytest -q
```

Windows/Python3.12.8实际78项测试通过。真实回环HTTP状态[201,200,401,409,401,201]，最终2个回执/计数器8；固定10情境另验证轮换、发送方命名空间及重新打开DB。20个相同并发请求只产生1次新增，20个独立事件不丢增量；测试覆盖两表间故障回滚、提交后异常与同ID重试恢复历史ACK。

[首次发布CI](https://github.com/carb6661/webhook-replay-lab/actions/runs/39627078032)在Windows/Linux × Python3.11/3.12/3.13六组环境全部通过，包含78项测试、依赖检查、固定报告与真实回环HTTP；对应提交80492a7。

可展示AppSec/API消息认证、raw body与元数据覆盖、时间窗口/提交检查、稳定发送方命名空间、SQLite原子幂等、最小审计与故障解释。HMAC覆盖key id/时间/事件ID，防止仅改未认证元数据绕过去重；回执与本地合成效果在同一事务，不把内存集合当持久幂等。

WHLAB v1是原创本地教学协议，不兼容Stripe/GitHub，不处理真实付款或外部效果。HMAC不是加密或不可抵赖签名；HTTP500不证明未提交，审计null表示不能确认；重复ACK是历史结果，不是当前计数查询。无在线撤销、生产TLS/限流/持久审计或跨主机exactly-once，不包装成生产支付经验。

## Incident Timeline Lab：多源事件时间与证据解释

[仓库](https://github.com/carb6661/incident-timeline-lab) · [未修正假设报告](https://github.com/carb6661/incident-timeline-lab/blob/main/examples/report-uncorrected.json) · [校准假设报告](https://github.com/carb6661/incident-timeline-lab/blob/main/examples/report-calibrated.json) · [测试](https://github.com/carb6661/incident-timeline-lab/blob/main/tests/test_incidenttime.py) · [调查手册](https://github.com/carb6661/incident-timeline-lab/blob/main/docs/ANALYST_RUNBOOK.md)

Python3.11+，纯标准库，从仓库根目录运行：

```bash
python -X utf8 -m incidenttime examples/uncorrected --format json
python -X utf8 -m incidenttime examples/calibrated --format json
python -X utf8 -m unittest discover -s tests -v
```

Windows/Python3.12.8实际45项测试通过。两组原创合成案例各3来源、6事件、5项指定比较，原始JSONL摘要相同；终端修正−120秒后，一项关系从after变为不能判定，一项从after变为before，校准组仍有2项indeterminate。不是执行真实设备校时或处理真实入侵。

[首次发布CI](https://github.com/carb6661/incident-timeline-lab/actions/runs/39268453568)在Windows/Linux × Python3.11/3.12/3.13六组环境全部通过，包含45项测试、依赖一致性检查和两组CLI重放；对应提交9fbaa94。

可展示SOC/IR的时钟误差分析、UTC/显式偏移、闭区间比较、行级摘要与定位、严格输入、只读快照和整体失败语义。与Auth Log Triage的认证阈值告警不同，这里不检测攻击；与文件完整性监测不同，这里摘要绑定分析输入，不扫描文件变更。

必须解释校时/误差是未自动核验的分析假设；显示排序不等于因果；相交连通组不等于所有成员两两重叠。摘要不证明真实性或法律证据链，规范化JSONL不能冒充原生EVTX/pcap保全。工具不联网、不执行日志，不上传真实个人/客户记录。

## Workflow Trust Auditor：DevSecOps与CI信任边界

[仓库](https://github.com/carb6661/workflow-trust-auditor) · [修正前报告](https://github.com/carb6661/workflow-trust-auditor/blob/main/examples/report-before.json) · [修正后报告](https://github.com/carb6661/workflow-trust-auditor/blob/main/examples/report-after.json) · [测试](https://github.com/carb6661/workflow-trust-auditor/blob/main/tests/test_workflowguard.py) · [威胁模型](https://github.com/carb6661/workflow-trust-auditor/blob/main/docs/THREAT_MODEL.md)

安装requirements.lock后，从仓库根目录运行：

```bash
python -X utf8 -m workflowguard examples/before.workflow --format json --fail-on none
python -X utf8 -m workflowguard examples/after.workflow --format json
python -X utf8 -m workflowguard . --format json
python -X utf8 -m unittest discover -s tests -v
```

Windows/Python3.12.8实际43项测试通过。原创合成配置6项发现→修正后0匹配，报告精确重放；共7条规则，缺失权限声明由另一个测试验证。位置包含文件、job、行号、结构路径与输入摘要，不回显脚本原文；坏批次不输出部分报告。审查本身不联网、不执行工作流、不修改输入，依赖安装需要网络。

[首次发布CI](https://github.com/carb6661/workflow-trust-auditor/actions/runs/39035514199)在Windows/Linux × Python3.11/3.12/3.13六组环境全部通过，包含43项测试、依赖检查及自身工作流/修正后样本审查；对应提交7a64230。

可讨论顶层/job权限覆盖、令牌最小权限、PR到特权checkout/runner边界、直接依赖SHA固定、有界YAML事件/节点、CI退出码和敏感输出最小化。明确展示if:false仍命中的保守误报，以及下标表达式未识别的漏报；不是完整表达式污点引擎、GitHub schema、lockfile/传递依赖审计或执行利用证明。新版checkout保护可能实际阻断风险配置；0匹配不是安全证书。

## RAG Document Authorization Lab：对象权限与租户隔离

[仓库](https://github.com/carb6661/rag-document-authz-lab) · [修复前后报告](https://github.com/carb6661/rag-document-authz-lab/blob/main/examples/report.json) · [矩阵与API测试](https://github.com/carb6661/rag-document-authz-lab/blob/main/tests/test_lab.py) · [威胁模型](https://github.com/carb6661/rag-document-authz-lab/blob/main/docs/THREAT_MODEL.md)

安装仓库 requirements.lock 后运行：

```bash
python -X utf8 -m doclab experiment
python -X utf8 -m pytest -q
python -X utf8 -m doclab.live_smoke
python -X utf8 -m doclab.race_diagnostics
```

0.3版保留16组对照共32次固定请求；Windows/Python3.12.8 本地200项测试通过。真实回环HTTP除读取/列表/检索隔离，还验证评分中版本变化409、重新检索版本2成功、评分中撤销401且无原hits。

[0.3竞态修正报告](https://github.com/carb6661/rag-document-authz-lab/blob/main/docs/REVOCATION_CHECKPOINTS.md)记录四项修复前后证据：评分中撤销、版本变化、仅归属变化，以及认证后写入前撤销。方案为锁内写入会话检查及成功响应发布前会话/当前权限/版本复核，固定会话→文档锁顺序。新增36项回归，也明确验证提交后撤销可返回401但修改保留，审计mutation_committed区分副作用；最终检查后仍有发送窗口，不能撤回已发送数据。

[0.2历史技术深挖](https://github.com/carb6661/rag-document-authz-lab/blob/main/docs/TECHNICAL_DEEP_DIVE.md)保留五项旧版问题：重复JSON/关键头、异常审计遗漏、空ASGI帧积累及过期会话清理。未授权对象不进入评分的回归保持；0.2评分中撤销仍200的旧限制已由0.3发布检查缩小，不再当作当前版本预期。

[0.3版CI](https://github.com/carb6661/rag-document-authz-lab/actions/runs/39034667745)在 Windows/Linux × Python3.11/3.12/3.13 六组环境全部通过，包含200项测试、16组对照、依赖检查及真实回环HTTP竞态验证；对应提交5138e18。

[0.2版CI](https://github.com/carb6661/rag-document-authz-lab/actions/runs/38830703117)在 Windows/Linux × Python 3.11/3.12/3.13 六组环境全部通过，包含164项测试、16组对照与真实回环HTTP；对应提交9632b64。

[首次发布CI](https://github.com/carb6661/rag-document-authz-lab/actions/runs/38735835633)在 Windows/Linux × Python 3.11/3.12/3.13 六组环境全部通过，包含测试、对照实验和真实回环HTTP；对应提交5408e89。发布前扫描的3项疑似凭据经逐项审查为运行时变量赋值，具体记录见该仓库验证说明，不把扫描描述为零匹配。

可讨论认证与对象授权的区别、可信身份来源、角色×对象×动作矩阵、列表一致性、受保护字段拒绝、锁内版本检查、认证/提交/发布时点、审计副作用语义，以及统一404不等于消除时间侧信道。审计不保存令牌、摘要或正文。

只使用合成内存数据；关键词检索不等于语义RAG，短期会话不是生产IdP，无持久化/共享授权/生产性能承诺。缺陷版仅进程内对照，不开放外部目标或删除功能。接入真实向量库后需重新验证授权，不把本实验通过当作下游无泄漏证明。

## Compliance RAG Assistant：可持久更新的合规证据问答

0.3继续同一主项目的知识维护：[条件写设计/升级说明](https://github.com/carb6661/compliance-rag-assistant/blob/main/docs/CONDITIONAL_WRITES.md) · [实际合成证据](https://github.com/carb6661/compliance-rag-assistant/blob/main/examples/evaluation/conditional-writes.json) · [专项测试](https://github.com/carb6661/compliance-rag-assistant/blob/main/tests/test_source_conditions.py)。

补来源读取和ETag、create-only、来源generation与库身份；向量计算前廉价检查后，在SQLite IMMEDIATE事务内再次比较才写/删，防旧草稿覆盖、旧标签误删及内容/删除重建两类ABA。167项本地测试；8连接1提交/7冲突，真实4个CLI进程1成功/3冲突；回环HTTP428/200/412/412。独立schema1夹具升级保留12个原块，故障回滚列与settings。引用新增查询读到的source_etag，不混淆全局revision与来源状态。

[0.3 CI](https://github.com/carb6661/compliance-rag-assistant/actions/runs/40202875801)对应b0a5784，Windows/Linux × Python3.11/3.12/3.13六组各167项通过，含真实回环HTTP及条件写重放；模型权重只在本机独立验证，不把CI写成六平台真实LLM实测。

ETag不是授权，仍仅私有本地单用户；不能防直接SQL、旧写者混跑或恶意本机改库。升级前需停止旧进程并备份，不能在412后自动刷新标签覆盖。不是历史版本仓库/审批/分布式一致性，也没有改变最终发送窗口或3/4拒答缺口；没有把Mock写成真实LLM联调。

0.2在原仓库深入，而非另建切分演示。[业务运行/模型配置](https://github.com/carb6661/compliance-rag-assistant/blob/main/docs/RAG_GUIDE.md) · [背景、设计、难题和简历模板](https://github.com/carb6661/compliance-rag-assistant/blob/main/docs/PROJECT_STORY.md) · [真实向量前后报告](https://github.com/carb6661/compliance-rag-assistant/blob/main/examples/evaluation/semantic-after.json) · [新回归测试](https://github.com/carb6661/compliance-rag-assistant/blob/main/tests/test_rag.py)

补SQLite事务来源/块/向量替换与级联删除、mandatory标准/版本/级别SQL过滤、BM25/本地512维BGE/精确余弦/RRF、模型原文选摘验证、生成后revision复核及最多1000条不含正文的trace。兼容模型的URL/名称/Key只从服务端配置，出站需显式许可；有真实HTTP业务演示，无用户/租户鉴权，只能私有回环运行。

Windows/Python3.12.8本地131项测试通过。真实BGE在禁止出站socket/DNS下运行20问与token超限拒绝；12块合成开发集16可回答题，Dense Hit@3=16/16、MRR@3=1；BM25/Hybrid MRR@3均0.96875，没有宣称混合检索更好。4不可回答题仅拒3个，相关但缺事实案例保留；原文精确不等于语义正确。

[0.2 CI](https://github.com/carb6661/compliance-rag-assistant/actions/runs/39980575362)对应757c806，Windows/Linux × Python3.11/3.12/3.13六组全部成功，包含131项基础测试、离线评测和真实回环HTTP。CI不下载BGE权重、不调用真实生成服务，不能与本地真实向量验证混淆。

真实生成服务尚未配置联调，Mock只证明契约/错误分支，不代表某模型的生成效果；没有正式标准覆盖、合规判定、生产性能或企业实战承诺。源码/分词器token门禁和模型摘要绑定索引，精确扫描上限2000块不是ANN。最后revision复核到实际发送仍有窗口。

以下保留0.1数据准备的历史证据；不再代表0.2尚无向量能力。

[仓库](https://github.com/carb6661/compliance-rag-assistant) · [测试](https://github.com/carb6661/compliance-rag-assistant/blob/main/tests/test_ingestion.py) · [预期Document](https://github.com/carb6661/compliance-rag-assistant/blob/main/examples/expected/documents.jsonl) · [验证记录](https://github.com/carb6661/compliance-rag-assistant/blob/main/docs/VALIDATION.md)

按仓库说明安装 requirements.lock 后，从仓库根目录运行：

```bash
python -X utf8 -m compliance_rag parse examples/standards --out output/demo.jsonl
python examples/filter_chunks.py output/demo.jsonl --level 第三级 --security-class 安全物理环境
python -X utf8 -m pytest -q
```

合成样例导出6个块；过滤第三级/安全物理环境得到1个块。缺少安全类的原始样本严格拒绝，补结构后导出2块。Windows/Python 3.12.8 本地59项测试通过；实际回环HTTP有效请求200/6块，不完整请求422/零导出。[首次发布CI](https://github.com/carb6661/compliance-rag-assistant/actions/runs/38591066146)的 Windows/Linux × Python 3.11/3.12/3.13 六组检查全部通过，对应提交 d4be3d3。

可讨论：标题栈、级别与类别作用域、源码区间和摘要、标准版本配置、原子块保留、不完整入库门禁、API不接收路径/URL的边界。源码摘要不证明来源真实；标题正确不代表内容正确；文档中的指令不执行，但本模块不是提示词注入防火墙。

0.1仅数据准备；0.2新增的能力和未完成事项见上方。两版均无真实标准全文覆盖或合规判定，也没有企业测评经历与生产性能承诺。

## Auth Log Triage：SOC 认证日志与证据分诊

[仓库](https://github.com/carb6661/auth-log-triage) · [实际示例报告](https://github.com/carb6661/auth-log-triage/blob/main/examples/report-attack.json) · [规则测试](https://github.com/carb6661/auth-log-triage/blob/main/tests/test_authwatch.py) · [验证记录](https://github.com/carb6661/auth-log-triage/blob/main/docs/VALIDATION.md)

Python 3.11+，标准库实现，从仓库根目录运行：

```bash
python -X utf8 -m authwatch examples/attack.jsonl
python -X utf8 -m authwatch examples/shared-nat.jsonl
python -X utf8 -m authwatch examples/distributed-miss.jsonl
python -X utf8 -m unittest discover -s tests -v
```

首个样本 11 个合成事件产生 3 项发现；共享 NAT 样本产生 1 项待排除误报，分布式样本零发现是已知漏报。本地 Windows/Python 3.12.8 的 36 项测试通过；[首次发布 CI](https://github.com/carb6661/auth-log-triage/actions/runs/38398097434)的 Windows/Linux × Python 3.11/3.12/3.13 六组检查全部通过，对应提交 b80ab6e。

可讨论：严格输入校验、UTC 与同时间戳因果边界、滑动窗口及不同账号计数、首次窗口去重、证据 ID、错误输入不输出部分报告、调查与修复验证。报告含账号/IP，不自动脱敏；工具不联网、不尝试认证、不自动封禁。

这是离线合成实验，不是企业事件处置经历；没有真实 MFA/限速修复结果、生产性能或准确率承诺。正常样本不冒充修复后对照。

## RAGShield：从风险分析到开发流程

```bash
cd backend
python -m pip install -e ".[dev]"
python -m app.cli --list-controls
python -m app.cli --profile demo-vulnerable --format json --fail-on never
python -m app.cli --profile demo-hardened --format json --fail-on never
python -m pytest
```

观察同一组控制在脆弱版与加固版靶场中的差异；查看证据、修复建议与退出码。演示采用 `--fail-on never`；实际 CI 可使用默认严重性门禁。

证据入口：`docs/THREAT_MODEL.md`、`backend/tests/test_http_target.py`、`.github/workflows/ci.yml`。

可讨论：用户输入和检索内容的信任边界、租户过滤位置、HTTP 目标白名单、DNS/IP 校验、响应大小限制、正反例测试。确定性标记存在盲区，不能将演示结果解释为完整的 SSRF 或提示词注入防护证明。

## Local Secret Scanner：提交前检查

```bash
python -m unittest discover -s tests -v
python secret_scanner.py tests --fail-on-findings
```

测试目录包含合成占位值；扫描预期发现匹配并返回 1。观察规则匹配与脱敏输出。讨论误报、漏报、忽略目录和二进制处理；当前不扫描 Git 历史，不验证凭据是否有效。

## HTTP Security Headers Auditor：配置审查

```bash
python -m unittest discover -s tests -v
python headers_auditor.py examples/headers.txt --fail-on-findings
```

示例应通过基础检查。当前主要判断字段或指令是否存在，仍需严格解析策略语义：例如 `max-age=0`、宽泛的 `frame-ancestors` 和无效 Referrer-Policy 都可能误判。演示时说明这些局限。

## File Integrity Monitor：文件变更检测

运行单元测试，再在独立临时实验目录建立清单，修改、新增或删除实验文件，观察分类。解释 SHA-256 只能检测相对基线的变化，以及为什么必须保护基线文件。

## 面试演示结构

1. 说明问题、资产和实验范围。
2. 展示输入、发现项与修复后变化。
3. 解释核心代码、测试与安全边界。
4. 讨论误报、漏报、部署限制与下一步。

仅陈述自己已理解、复现并能解释的部分。测试和性能数字应保留环境与来源；不要展示真实凭据或个人数据。
