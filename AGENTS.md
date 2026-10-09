# AGENTS.md — 给 AI Agent 的使用说明

本仓库是一个 **Agent Skill**（`credential-vault-design`，统一凭据保险库）。如果你是 AI Agent，读这一页就够用了。

## 什么时候用

当用户提到以下意图时，加载本技能：

- 凭据 / 密码 / 密钥 / 令牌**分散**、需要**收拢**或统一管理
- 需要 AI 使用凭据，但**不能让 AI 看见明文**
- 需要**分级授权**（按用途收窄可读范围）、**时效令牌**（按次/按时授权）
- 需要**可恢复、不锁死**的凭据存储（含备份与完整性校验）
- 需要**防篡改审计**（取用留痕、哈希可比对）
- 英文触发词：credential vault、secret management、password manager、zero-knowledge store、break-glass

## 怎么用

1. 读 `SKILL.md` —— 它是本技能的权威定义（YAML frontmatter 里的 `description` / `trigger_keywords` / `description_en` 是检索入口）。
2. CLI 位于 `tools/vault_cli.py`，命令：`create` · `add` · `list` · `get` · `remove` · `backup`。
3. 依赖与安装见 `README.md` 的「快速开始」段。

## 硬约束（不要违反）

- **不要**把明文凭据写入对话、日志、提交信息或任何远端。
- **不要**替用户跳过主密码或密钥文件（本设计**没有**后门，也不应添加）。
- **不要**把库文件（`.kdbx`）上传到任何云端，除非用户明确要求。
- **不要**声称本仓库已获认证、已商用或已服务某客户（见 `README.md` 免责声明）。
- 使用第三方依赖时注意许可：`pykeepass` 为 **GPL-3.0**（见 `NOTICE`）。

## 安全与贡献

- 安全策略与漏洞上报：`SECURITY.md`
- 贡献方式：`CONTRIBUTING.md`
- 权属与许可：`README.md` 的「许可说明 · License Notice」与 `ATTESTATION.md`

## 结构化入口

- `llms.txt` —— 精简索引
- `llms-full.txt` —— 完整说明（一次性摄入）
