# MGSX Workflow-AI

> Transforme **qualquer** projeto — novo ou existente — num harness de engenharia assistida por IA com **um comando**. Camada de produto (PRD → PROJECT), Spec-Driven Development com fatias verticais, ciclo RDPI, skills e agentes de revisão/auditoria — distribuído como **plugin do Claude Code**.
>
> 🇬🇧 English below ↓

Este repositório é um **monorepo por AI**: cada ferramenta de IA tem sua própria subpasta de plugin. Hoje há um plugin:

| AI | Pasta | Plugin |
|---|---|---|
| Claude Code | [`claude-code/`](claude-code/) | `mgsx-workflow-ai` |

*(Futuras AIs entram como novas subpastas — ex.: `cursor/`, `codex/` — sem alterar as existentes.)*

## Instalação (Claude Code)

```text
# uma vez por máquina
/plugin marketplace add mgsx/mgsx-workflow-ai      # ou o caminho local do repo
/plugin install mgsx-workflow-ai@mgsx-workflow-ai

# em qualquer projeto (pasta vazia ou repo existente)
cd meu-projeto && claude
/mgsx-workflow-ai:setup
```

O comando `/mgsx-workflow-ai:setup` roda **por fases com checkpoints**: detecta a stack, **copia** os assets prontos (skills, agentes, templates, docs, biblioteca de prompts) e **gera** só o específico da stack (`CLAUDE.md`, `settings`, contexto técnico). Veja [`claude-code/README.md`](claude-code/README.md) para detalhes.

## O que você ganha

- **Camada de produto** — `/new-prd` (documento de negócio) e `/new-project` (quebra um épico em sprints com critérios de aceite, incluindo *sad paths*).
- **Spec-Driven Development com fatias verticais** — feature mãe `F00XX` → fatias `F00XX.N` → ciclo **RDPI** (Research → Design → Plan → Implement) → 1 PR ≤ ~400 linhas.
- **9 skills** RDPI e **8 agentes** (implementer, code-reviewer, auditores de segurança read-only, testes e docs).
- **Híbrido**: o que é reutilizável vem pronto; o que é específico da stack é gerado.

## Filosofia

Nada de implementação sem spec aprovada. Hierarquia: **PRD → PROJECT → Sprint → Feature `F00XX` → Fatia `F00XX.N` → RDPI**. O planejamento macro alimenta a execução por fatias pequenas e auditáveis.

---

# MGSX Workflow-AI (English)

> Turn **any** project — new or existing — into an AI-assisted engineering harness with **one command**. Product layer (PRD → PROJECT), Spec-Driven Development with vertical slices, the RDPI cycle, plus review/audit skills and agents — shipped as a **Claude Code plugin**.

A **per-AI monorepo**: each AI tool gets its own plugin subfolder. Today there is one plugin: `mgsx-workflow-ai` under [`claude-code/`](claude-code/).

## Install (Claude Code)

```text
/plugin marketplace add mgsx/mgsx-workflow-ai
/plugin install mgsx-workflow-ai@mgsx-workflow-ai

cd my-project && claude
/mgsx-workflow-ai:setup
```

`/mgsx-workflow-ai:setup` runs **in phases with checkpoints**: detects the stack, **copies** the ready-made assets, and **generates** only the stack-specific files. See [`claude-code/README.md`](claude-code/README.md).

## License

MIT — see [LICENSE](LICENSE).
