# carb6661 · Cybersecurity portfolio

面向网络安全岗位建设的开源作品集，关注 **应用安全、DevSecOps 与 AI/RAG 安全**。以全栈开发作为实践基础，将安全分析落实到 API、数据流、自动化验证与部署边界。

## 安全能力与证据

| 项目 | 可展示能力 | 当前边界 |
| --- | --- | --- |
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
