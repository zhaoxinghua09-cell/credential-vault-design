# 统一凭据保险库 · credential-vault-design（本地零知识 · 真实可跑）

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE.md)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-blue.svg)](SKILL.md)
[![Local-first](https://img.shields.io/badge/local--first-offline%20friendly-2ea44f.svg)](#)
[![No cloud](https://img.shields.io/badge/no-cloud%20required-informational.svg)](#)
[![Zero-knowledge](https://img.shields.io/badge/zero--knowledge-encrypted-8a2be2.svg)](#)
[![Version](https://img.shields.io/badge/version-2.0.0-informational.svg)](manifest.json)
[![Python](https://img.shields.io/badge/python-3.10%2B-3776ab.svg)](#)

> 把散落各处的口令 / 密钥 / 令牌收成**一个**本地保险库——零知识加密、可恢复不锁死、分级授权、带防篡改审计链；**不连云、不把明文交给 AI**。

一个真正能跑的本地凭据保险库，用于回答「密码密钥散得到处都是怎么收拢」「AI 要用凭据但不能让它看见明文」「密码忘了会不会锁死」这类问题。它是 SynomosAI 技能包，同时是一份可复算的工程实现。

---

## 🚀 快速开始 / Quick Start

```bash
# 1) 取得仓库
git clone https://github.com/zhaoxinghua09-cell/credential-vault-design.git
cd credential-vault-design

# 2) 建虚拟环境（不要装进系统 Python）
python -m venv .venv
.venv\Scripts\activate          # Windows
# source .venv/bin/activate     # macOS / Linux

# 3) 装依赖
pip install -r tools/requirements.txt

# 4) 看一眼命令
python tools/vault_cli.py --help
```

核心命令（`tools/vault_cli.py`）：

| 命令 | 作用 |
|---|---|
| `create` | 新建保险库（设主密码） |
| `add` | 添加条目（站点 / 用户名 / 密码 / 备注） |
| `list` | 列出条目（**不显示密码**） |
| `get` | 取出条目的某个字段 |
| `remove` | 删除条目 |
| `backup` | 备份库文件并打印 **SHA-256**（用于事后比对防篡改） |

> 作为 AI Agent 技能使用时：把本仓库目录放入你的 Agent 技能目录（例如 `~/.workbuddy/skills/`），按 `SKILL.md` 的触发词调用。

---

## 它能解决什么问题

- **散**：口令、API Key、授权码分散在浏览器、`.env`、笔记、聊天记录里 → 收敛为**一个**本地加密库。
- **怕给 AI 看**：要让 AI 帮你用凭据，又不想让它看见明文 → **零知识边界**：密文落盘，明文只在本机内存。
- **怕锁死**：主密码忘了数据就没了 → 提供**备份 + SHA-256 校验**链路，可恢复、可验证、不锁死。
- **怕说不清**：谁在什么时候取了什么 → **防篡改审计链**：库文件任一字节改动，哈希即变，可与历史备份比对发现。

---

## 主要特性

- **零知识加密**：磁盘上的库文件全文中不含任何明文凭据（**实测泄露 0 处**）。
- **双因子强制**：主密码 + 密钥文件组合才可解锁，单一凭据打不开。
- **可恢复不锁死**：`backup` 产出带 SHA-256 的备份件，历史哈希可比对。
- **分级授权 + 时效令牌**（技能方法论层）：按用途收窄可读范围、按次/按时授权。
- **全生态同读**：库文件为标准 `.kdbx` 格式，桌面/移动主流客户端可读。
- **零网络面**：CLI **不含任何网络调用**（无 requests / urllib / socket / subprocess），不存在可被重放的请求或令牌。
- **可量化安全**：附 **8 维安全实测**（全部 5.0）与雷达图（`references/panorama-radar.svg`）。

---

## 仓库内容

本仓库为 `credential-vault-design` 技能的发布包：核心文件 `SKILL.md` 遵循 Agent Skills 规范（YAML frontmatter），可直接放入主流 AI Agent 的技能目录使用。

- **分类**：安全工具
- **版本**：2.0.0
- **署名**：诺卫(Phylax)@SynomosAI
- **许可**：MIT（详见仓库 LICENSE）

### 安全实测（`tools/security_results.json`，实测时间 2026-08-26）

| 维度 | 得分 | 关键度量 |
|---|---|---|
| 抗暴力破解 | 5.0 | 错误密码拒绝 200/200，明文泄露 0 处 |
| 防篡改审计 | 5.0 | 篡改识别：哈希变化，可检测 |
| 授权时效强制 | 5.0 | 双因子缺一不可 |
| 抗重放 | 5.0 | 纯本地文件协议，无网络面，重放入口 0 |
| 零知识边界 | 5.0 | 库文件明文泄露 0 处 |
| 解绑完整性 | 5.0 | 删除后哈希变化，状态可审计 |
| 并发稳定性 | 5.0 | 20 并发读取异常 0 处 |
| 边界容错 | 5.0 | 损坏文件友好拒绝 |

**合计 5.00 / 5.00** · 另有第三方（腾讯云鼎方法论）静态审计报告 `SECURITY_AUDIT_云鼎_2026-08-26.md`：**95 / 100**、P0 风险 0 个。

### 目录结构

```
├── SKILL.md                    # Agent Skills 规范技能定义（含中英描述）
├── manifest.json               # 技能清单
├── README.md / README.en.md    # 中文 / 英文说明
├── NOTICE                      # 第三方依赖许可声明（pykeepass = GPL-3.0）
├── ATTESTATION.md              # 权属证明与许可声明（含包指纹）
├── SECURITY.md                 # 安全策略与漏洞上报
├── CONTRIBUTING.md             # 贡献指引
├── CITATION.cff                # 引用信息
├── llms.txt / llms-full.txt    # 给 AI 爬虫/检索的结构化入口
├── AGENTS.md                   # 给 AI Agent 的使用说明
├── references/                 # 全景图与安全雷达图（SVG/PNG）
└── tools/                      # CLI、加密库、自测脚本
```

---

## 常见问题 FAQ

**Q：这算「把密码交给 AI」吗？**
不算。库文件是加密的，AI 在默认链路里看不到明文；需要用到某项凭据时，由本机进程解密并在内存中短时使用。

**Q：数据存在云上吗？**
不。全部为本地文件（`.kdbx`），工具零网络调用。是否自行同步由你决定。

**Q：主密码忘了会怎样？**
库无法解锁（这是加密的本质），但你可以用 `backup` 产出的历史备份与 SHA-256 记录核对完整性。零知识设计下，没有后门可绕过主密码。

**Q：能用在哪些 AI 平台？**
技能以纯文本 + Python CLI 交付，平台无关；`SKILL.md` 中列出的平台仅为已验证的使用场景。

---

## 许可说明 · License Notice

- **权利状态**：本仓库以 **MIT 许可** 许可发布，可依该许可证条款自由使用、修改与再分发。
- **引用建议**：引用时请标注仓库名与原文链接 `https://github.com/zhaoxinghua09-cell/credential-vault-design`
  与权利人「赵兴华 / Steven Zhao·China」。
- **品牌状态限定**：MedXpert、SynomosAI、LGD 等为相关项目标识，
  **均未申请实体注册、未申请商标注册**；出现仅作来源标识，
  不构成对法人实体或商标权的任何主张。
- **完整条款**：见仓库根目录 [LICENSE.md](LICENSE.md)。
- **联系**：zhaoxinghua06@126.com ｜ ORCID 0009-0001-0512-1237

---

## 免责声明

本仓库内容为**理论站位与工具化探索**，不代表任何已获认证、已商业化交付或已服务特定客户的声明；文中涉及的外部标准、认证与条款信息为公开资料转述，正式引用前请**独立核实**。API、授权码与形象大使等为路线图（roadmap）事项，尚未上线。
