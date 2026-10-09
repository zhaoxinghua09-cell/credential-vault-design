# Unified Credential Vault · credential-vault-design (local, zero-knowledge, runnable)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE.md)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-blue.svg)](SKILL.md)
[![Local-first](https://img.shields.io/badge/local--first-offline%20friendly-2ea44f.svg)](#)
[![No cloud](https://img.shields.io/badge/no-cloud%20required-informational.svg)](#)
[![Zero-knowledge](https://img.shields.io/badge/zero--knowledge-encrypted-8a2be2.svg)](#)
[![Version](https://img.shields.io/badge/version-2.0.0-informational.svg)](manifest.json)
[![Python](https://img.shields.io/badge/python-3.10%2B-3776ab.svg)](#)

> Consolidate scattered passwords, API keys and tokens into **one** local vault — zero-knowledge encrypted, recoverable (never lock-out), tiered authorization, tamper-evident audit trail. **No cloud. No plaintext handed to AI.**

A credential vault that actually runs locally. It answers three practical questions: *how do I gather keys that are scattered everywhere?*, *how can an AI use credentials without ever seeing the plaintext?*, and *what happens if I forget the master password?*

## 🚀 Quick Start

```bash
git clone https://github.com/zhaoxinghua09-cell/credential-vault-design.git
cd credential-vault-design

python -m venv .venv
.venv\Scripts\activate          # Windows
# source .venv/bin/activate     # macOS / Linux

pip install -r tools/requirements.txt
python tools/vault_cli.py --help
```

Core commands (`tools/vault_cli.py`):

| Command | Purpose |
|---|---|
| `create` | Create a new vault (set master password) |
| `add` | Add an entry (site / username / password / note) |
| `list` | List entries (**never prints passwords**) |
| `get` | Read one field of an entry |
| `remove` | Delete an entry |
| `backup` | Back up the vault file and print its **SHA-256** (for later tamper comparison) |

To use it as an AI agent skill, drop this directory into your agent's skill folder (e.g. `~/.workbuddy/skills/`) and trigger it through `SKILL.md`.

## What it solves

- **Scattered secrets** → one local encrypted store instead of browsers, `.env` files, notes and chats.
- **"I need AI to use it, but not see it"** → zero-knowledge boundary: ciphertext on disk, plaintext only in local memory.
- **"I don't want to get locked out"** → backups with SHA-256 verification; recoverable and checkable.
- **"I can't prove what happened"** → tamper-evident trail: change one byte of the vault file and the hash changes, detectable against historical backups.

## Key features

- **Zero-knowledge encryption** — 0 plaintext credentials found in the on-disk vault file (measured).
- **Two-factor unlock** — master password + key file; a single factor cannot open the vault.
- **Zero network surface** — the CLI contains no network calls (no requests / urllib / socket / subprocess); there is no request or token to replay.
- **Portable format** — standard `.kdbx`, readable by mainstream desktop and mobile clients.
- **Quantified security** — an 8-dimension security test (all 5.0) plus a radar chart at `references/panorama-radar.svg`.

For the measured results, see `tools/security_results.json` (tested 2026-08-26) and the third-party static audit `SECURITY_AUDIT_云鼎_2026-08-26.md` (95 / 100, zero P0 findings).

## License Notice

- **License status**: released under the **MIT License**; you may freely use, modify and redistribute it under those terms.
- **Attribution**: when citing, please name the repository and its URL
  `https://github.com/zhaoxinghua09-cell/credential-vault-design`
  with the rights holder "Zhao Xinghua / Steven Zhao·China".
- **Brand status**: MedXpert, SynomosAI and LGD are project marks; **none is a registered legal entity or registered trademark**. Their appearance is attribution only and asserts no corporate or trademark right.
- **Full terms**: see [LICENSE.md](LICENSE.md).
- **Contact**: zhaoxinghua06@126.com ｜ ORCID 0009-0001-0512-1237

> The Chinese section "许可说明 · License Notice" in [`README.md`](README.md) is the authoritative version; this English text is a faithful translation provided for wider reach.

## Disclaimer

This repository is **theoretical positioning and tooling exploration**. It does not claim any certification, commercial delivery, or service to any specific client. External standards, certifications and clause information are paraphrased from public material — **verify independently** before formal citation. APIs, authorization codes and mascots are roadmap items and are not live.
