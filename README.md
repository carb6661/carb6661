# carb6661 · Cybersecurity portfolio

面向网络安全岗位建设的开源作品集，关注 **应用安全、DevSecOps、SOC 日志分析与 AI/RAG 安全**。以全栈开发作为实践基础，将安全分析落实到 API、数据流、自动化验证与部署边界。

## 安全能力与证据

| 项目 | 可展示能力 | 当前边界 |
| --- | --- | --- |
| [Compliance RAG Assistant](https://github.com/carb6661/compliance-rag-assistant) | 持久资料库、真实BGE/BM25/RRF、引用及ETag维护；0.4补令牌角色、正文前额度与取消后worker占用，227项测试；[项目复盘](https://github.com/carb6661/compliance-rag-assistant/blob/main/docs/PROJECT_STORY.md) | 单进程共享库，无文档ACL/租户隔离/硬截止；20问仍有拒答缺口，真实生成服务尚未联调 |
| [Archive Ingest Guard](https://github.com/carb6661/archive-ingest-guard) | ZIP接收边界、跨平台路径命名空间、观测字节预算、隔离发布与失败清理；[验证记录](https://github.com/carb6661/archive-ingest-guard/blob/main/docs/VALIDATION.md) | 纯本地小型文档包；ASCII文件名，不审核内容，不是强资源沙箱或崩溃事务 |
| [Webhook Replay Lab](https://github.com/carb6661/webhook-replay-lab) | 原始字节HMAC认证、时间检查、持久事务幂等、并发/故障验证、78项测试；[验证记录](https://github.com/carb6661/webhook-replay-lab/blob/main/docs/VALIDATION.md) | 原创本地协议与合成计数器；非供应商兼容实现，不保证外部exactly-once |
| [Incident Timeline Lab](https://github.com/carb6661/incident-timeline-lab) | 多源UTC规范化、校时误差区间、行级证据、45项测试；[验证记录](https://github.com/carb6661/incident-timeline-lab/blob/main/docs/VALIDATION.md) | 校时假设未自动核验；时间先后不等于因果，无原生采集或证据链认证 |
| [Workflow Trust Auditor](https://github.com/carb6661/workflow-trust-auditor) | CI权限覆盖、PR信任边界、直接依赖固定、脱敏定位、43项测试；[验证记录](https://github.com/carb6661/workflow-trust-auditor/blob/main/docs/VALIDATION.md) | 离线静态子集；不执行工作流、不验证远程源码，保留明确误报/漏报 |
| [RAG Document Authorization Lab](https://github.com/carb6661/rag-document-authz-lab) | 多租户授权、授权后评分、发布前会话/权限/版本复核、提交审计、200项测试；[竞态修正](https://github.com/carb6661/rag-document-authz-lab/blob/main/docs/REVOCATION_CHECKPOINTS.md) | 单进程合成API；最终检查后仍有发送窗口，无生产IdP/数据库/向量RAG |
| [Auth Log Triage](https://github.com/carb6661/auth-log-triage) | 认证日志规范化、时间窗口规则、事件时间线、精确证据、调查手册与 CI；[验证记录](https://github.com/carb6661/auth-log-triage/blob/main/docs/VALIDATION.md) | 合成离线实验；明确保留 NAT 误报与分布式漏报，不证明账号失陷 |
| [Prompt Injection Firewall](https://github.com/carb6661/prompt-injection-firewall) | FastAPI 策略网关、RAG 筛选、来源隔离、临时封禁与审计仪表盘 | 可解释规则原型；未接入模型，仍有语义绕过与误报 |
| [RAGShield](https://github.com/carb6661/ragshield) | 威胁建模、租户隔离评测、受限 HTTP 目标、SARIF 与 CI | 确定性场景评测，不能证明真实攻击覆盖完整 |
| [Local Secret Scanner](https://github.com/carb6661/local-secret-scanner) | 代码检查、规则匹配、输出脱敏与 CI 退出码 | 教学原型；规则有限，有误报和漏报 |
| [HTTP Security Headers Auditor](https://github.com/carb6661/http-security-headers-auditor) | HTTP 配置检查、风险解释与修复建议 | 离线基础检查，尚未完整验证策略语义 |
| [File Integrity Monitor](https://github.com/carb6661/file-integrity-monitor) | SHA-256 基线、文件变更识别 | 基线需要额外保护 |

审阅入口：[安全项目演示与核验](docs/SECURITY_PORTFOLIO.md) · [建设路线与验收标准](docs/ROADMAP.md)

## 全栈工程基础

| 项目 | 技术与亮点 |
| --- | --- |
| [RAGShield](https://github.com/carb6661/ragshield) | Vue 3 + FastAPI 的 RAG 安全评测平台；含 API、CLI、Docker、SARIF 报告与 CI。 |
| [RAGOps Assistant](https://github.com/carb6661/ragops-assistant) | 企业级 RAG 工程实践：混合检索、引用、可观测性与离线评估。 |
| [启程留学](https://github.com/carb6661/qicheng-study-abroad) | 面向欧洲硕士申请的本地 Web MVP，覆盖选校、时间规划、材料与文书工作流。 |
| [C盘清理家](https://github.com/carb6661/C-Drive-Cleaner) | PowerShell 系统工具：白名单清理、预演、审计报告与 GitHub Actions 验证。 |

## What I build

- **Full-stack applications**：将真实工作流转化为界面、服务端逻辑和可复现的交付物。
- **AI & RAG systems**：检索、评估、可观测性和安全控制。
- **Developer tools**：本地优先、自动化、可审计的开发与系统工具。
- **Learning products**：将算法、英语学习与复杂流程做成易用工具。

## More projects

- [Local Secret Scanner](https://github.com/carb6661/local-secret-scanner) — 本地敏感信息扫描与脱敏输出。
- [Markdown to Docx](https://github.com/carb6661/md-word) — Markdown、公式、代码块与图片的 Word 文档转换。
- [CPU Scheduling Simulator](https://github.com/carb6661/cpu-scheduling-simulator) — FCFS、SJF、RR 调度算法模拟。
- [Scene English Photo Pack](https://github.com/carb6661/scene-english-photo-pack) — 从实景照片生成六张英语学习卡的 Codex Skill。

## Engineering principles

- 以可运行的代码、清晰的 README、可复现实验和自动化验证交付项目。
- 对涉及数据或系统操作的功能，优先采用本地优先、最小权限、显式确认与可审计设计。
- 欢迎通过 Issue 提交使用反馈、Bug、文档改进或功能建议。

浏览 [全部公开仓库](https://github.com/carb6661?tab=repositories) · 如果某个项目对你有帮助，欢迎 Star 或分享给需要它的人。
