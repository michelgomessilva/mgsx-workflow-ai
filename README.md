<div align="center">
  <img src="./assets/banner.svg" alt="MGSX Workflow-AI" width="100%"/>

  <h1>MGSX Workflow-AI</h1>
  <h3><em>Turn any repository, new or existing, into a Spec-Driven Development harness with one command.</em></h3>

  <p>
    <a href="https://github.com/michelgomessilva/mgsx-workflow-ai/tags"><img src="https://img.shields.io/github/v/tag/michelgomessilva/mgsx-workflow-ai?sort=semver&color=6366F1&label=version" alt="Latest version"/></a>
    <a href="https://github.com/michelgomessilva/mgsx-workflow-ai/blob/main/LICENSE"><img src="https://img.shields.io/github/license/michelgomessilva/mgsx-workflow-ai?color=22D3EE" alt="License"/></a>
    <a href="https://github.com/michelgomessilva/mgsx-workflow-ai/stargazers"><img src="https://img.shields.io/github/stars/michelgomessilva/mgsx-workflow-ai?style=social" alt="Stars"/></a>
    <img src="https://img.shields.io/badge/Claude_Code-plugin-A855F7" alt="Claude Code plugin"/>
  </p>

  <p>
    <strong>🇬🇧 English</strong> · <a href="./README-PT.md">🇧🇷 Português</a>
  </p>
</div>

---

A **Claude Code plugin** that installs and runs a complete, professional engineering workflow in any project: a **product layer** (PRD → PROJECT), **Spec-Driven Development** with vertical slices, the **RDPI** cycle, and a team of review/audit subagents, all from a single command.

> **Why?** Skipping documentation and prompting your agent straight into code feels magical for 20 minutes. Then the context window fills with shallow commands and quality collapses. This workflow front-loads the thinking (PRD → sprints → specs) so implementation becomes small, auditable, and almost boring. The boring part is the point.

## 📑 Table of Contents

- [⚡ Quick start](#-quick-start)
- [🤔 What you get](#-what-you-get)
- [🧭 How it works](#-how-it-works)
- [🧩 Components](#-components)
- [🔁 The day-to-day flow](#-the-day-to-day-flow)
- [📦 Repository layout](#-repository-layout)
- [🔄 Updating](#-updating)
- [📄 License](#-license)

## ⚡ Quick start

```text
# 1) once per machine: add the marketplace and install the plugin
/plugin marketplace add michelgomessilva/mgsx-workflow-ai
/plugin install mgsx-workflow-ai@mgsx-workflow-ai

# 2) in any project (an empty folder or an existing repo)
cd my-project && claude
/mgsx-workflow-ai:setup
```

That's it. `/mgsx-workflow-ai:setup` runs **in phases with checkpoints**: it detects your stack, **copies** the ready-made assets, and **generates** only the stack-specific files. You review and confirm between phases.

## 🤔 What you get

| | |
|---|---|
| 🧠 **Product layer** | `/new-prd` (a business document: problem, goals, scope, epics, user stories) and `/new-project` (breaks an epic into **sprints** with acceptance criteria, including the **sad paths** the AI usually forgets). |
| 🧱 **Spec-Driven Development** | Parent feature `F00XX` → vertical slices `F00XX.N` → **1 PR ≤ ~400 lines**. No implementation without an approved spec. |
| 🔬 **RDPI cycle** | **R**esearch → **D**esign → **P**lan → **I**mplement, with `/clear` between phases to keep context clean. |
| 🤖 **9 skills + 8 agents** | RDPI skills plus a senior implementer, a code reviewer, an architecture advisor, read-only security auditors, an integration-test engineer and a docs writer. |
| ⚙️ **Hybrid setup** | Reusable assets ship ready; only the stack-specific pieces (`CLAUDE.md`, settings, technical context) are generated per project. |

## 🧭 How it works

```
PRD  (business: what & why)
 └─ PROJECT  (technical: broken into sprints)
     └─ Sprint  (deliverable + acceptance criteria incl. sad paths)
         └─ Feature F00XX  (parent spec)
             └─ Slice F00XX.N  →  RDPI  →  one small PR
```

The macro layer only **plans**; execution stays in the small, auditable vertical-slice loop. Each sprint is just a set of features that flow through the existing RDPI cycle and review/audit agents.

## 🧩 Components

The plugin lives under [`claude-code/`](claude-code/):

- **Command:** `/mgsx-workflow-ai:setup` (the bootstrap orchestrator).
- **Skills:** `new-prd`, `new-project`, `new-feature-spec`, `new-feature-slice`, `research-slice`, `design-slice`, `plan-slice`, `new-hotfix-spec`, `deliver-slice`.
- **Agents:** `senior-implementer`, `code-reviewer`, `architecture-advisor`, `tenant-isolation-auditor`, `injection-reviewer`, `secret-scanner`, `integration-test-engineer`, `docs-writer`.
- **Resources:** templates (PRD, PROJECT, feature, slice, research), base docs, settings skeletons, and the full `00–15` prompt library (PT/EN).

See [`claude-code/README.md`](claude-code/README.md) for the full plugin reference.

## 🔁 The day-to-day flow

```text
/new-prd                     # business doc, created/updated once per product, evolves over time
/new-project EP01            # break an epic into sprints (acceptance criteria incl. sad paths)
/new-feature-spec            # F00XX parent spec → vertical slices F00XX.N
  → /research-slice → /clear → /design-slice → /clear → /plan-slice → implement (RDPI)
```

**Golden rules:** 1 slice = 1 PR ≤ ~400 lines · at least 2 slices per feature · `/clear` between Research / Design / Plan · never use `Co-Authored-By` in commits.

## 📦 Repository layout

```
mgsx-workflow-ai/                 # this repo: a per-AI monorepo
├── README.md  ·  README-PT.md  ·  LICENSE
├── assets/                       # logo + banner
├── .claude-plugin/marketplace.json
└── claude-code/                  # the Claude Code plugin (future AIs = new folders)
    ├── .claude-plugin/plugin.json
    ├── commands/  ·  skills/  ·  agents/  ·  resources/
    └── README.md
```

## 🔄 Updating

Updates are released by bumping the `version` in the marketplace. To upgrade:

```text
/plugin update mgsx-workflow-ai
```

## 📄 License

[MIT](LICENSE) © MGSX

<div align="center"><sub>Built for <a href="https://www.claude.com/product/claude-code">Claude Code</a> · inspired by the Spec-Driven Development tradition.</sub></div>
