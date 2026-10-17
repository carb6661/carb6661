# 安全项目演示与核验

以下项目是公开工程实践，不作为商业项目经验或真实事件处置经历。命令从相应项目目录执行，安装依赖以各仓库 README 为准。

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

## Compliance RAG Assistant：可溯源的合规知识入库

[仓库](https://github.com/carb6661/compliance-rag-assistant) · [测试](https://github.com/carb6661/compliance-rag-assistant/blob/main/tests/test_ingestion.py) · [预期Document](https://github.com/carb6661/compliance-rag-assistant/blob/main/examples/expected/documents.jsonl) · [验证记录](https://github.com/carb6661/compliance-rag-assistant/blob/main/docs/VALIDATION.md)

按仓库说明安装 requirements.lock 后，从仓库根目录运行：

```bash
python -X utf8 -m compliance_rag parse examples/standards --out output/demo.jsonl
python examples/filter_chunks.py output/demo.jsonl --level 第三级 --security-class 安全物理环境
python -X utf8 -m pytest -q
```

合成样例导出6个块；过滤第三级/安全物理环境得到1个块。缺少安全类的原始样本严格拒绝，补结构后导出2块。Windows/Python 3.12.8 本地59项测试通过；实际回环HTTP有效请求200/6块，不完整请求422/零导出。[首次发布CI](https://github.com/carb6661/compliance-rag-assistant/actions/runs/38591066146)的 Windows/Linux × Python 3.11/3.12/3.13 六组检查全部通过，对应提交 d4be3d3。

可讨论：标题栈、级别与类别作用域、源码区间和摘要、标准版本配置、原子块保留、不完整入库门禁、API不接收路径/URL的边界。源码摘要不证明来源真实；标题正确不代表内容正确；文档中的指令不执行，但本模块不是提示词注入防火墙。

这是第一阶段数据准备，没有嵌入模型、向量问答、真实标准全文覆盖或合规判定，也没有企业测评经历与生产性能承诺。

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
