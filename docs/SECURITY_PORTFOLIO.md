# 安全项目演示与核验

以下项目是公开工程实践，不作为商业项目经验或真实事件处置经历。命令从相应项目目录执行，安装依赖以各仓库 README 为准。

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
