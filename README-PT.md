<div align="center">
  <img src="./assets/banner.svg" alt="MGSX Workflow-AI" width="100%"/>

  <h1>MGSX Workflow-AI</h1>
  <h3><em>Transforme qualquer repositório — novo ou existente — num harness de Spec-Driven Development com um comando.</em></h3>

  <p>
    <a href="https://github.com/michelgomessilva/mgsx-workflow-ai/tags"><img src="https://img.shields.io/github/v/tag/michelgomessilva/mgsx-workflow-ai?sort=semver&color=6366F1&label=version" alt="Última versão"/></a>
    <a href="https://github.com/michelgomessilva/mgsx-workflow-ai/blob/main/LICENSE"><img src="https://img.shields.io/github/license/michelgomessilva/mgsx-workflow-ai?color=22D3EE" alt="Licença"/></a>
    <a href="https://github.com/michelgomessilva/mgsx-workflow-ai/stargazers"><img src="https://img.shields.io/github/stars/michelgomessilva/mgsx-workflow-ai?style=social" alt="Stars"/></a>
    <img src="https://img.shields.io/badge/Claude_Code-plugin-A855F7" alt="Plugin do Claude Code"/>
  </p>

  <p>
    <a href="./README.md">🇬🇧 English</a> · <strong>🇧🇷 Português</strong>
  </p>
</div>

---

Um **plugin do Claude Code** que instala e roda um workflow de engenharia completo e profissional em qualquer projeto: uma **camada de produto** (PRD → PROJECT), **Spec-Driven Development** com fatias verticais, o ciclo **RDPI** e um time de subagentes de revisão/auditoria — tudo a partir de um único comando.

> **Por quê?** Pular a documentação e mandar o agente direto pro código parece mágico por 20 minutos — até a janela de contexto encher de comandos rasos e a qualidade desmoronar. Este workflow coloca o pensamento na frente (PRD → sprints → specs) para que a implementação vire algo pequeno, auditável e quase entediante. O "entediante" é justamente o ponto.

## 📑 Índice

- [⚡ Início rápido](#-início-rápido)
- [🤔 O que você ganha](#-o-que-você-ganha)
- [🧭 Como funciona](#-como-funciona)
- [🧩 Componentes](#-componentes)
- [🔁 O fluxo do dia a dia](#-o-fluxo-do-dia-a-dia)
- [📦 Estrutura do repositório](#-estrutura-do-repositório)
- [🔄 Atualizando](#-atualizando)
- [📄 Licença](#-licença)

## ⚡ Início rápido

```text
# 1) uma vez por máquina — adicione o marketplace e instale o plugin
/plugin marketplace add michelgomessilva/mgsx-workflow-ai
/plugin install mgsx-workflow-ai@mgsx-workflow-ai

# 2) em qualquer projeto (pasta vazia ou repositório existente)
cd meu-projeto && claude
/mgsx-workflow-ai:setup
```

É isso. O `/mgsx-workflow-ai:setup` roda **por fases com checkpoints** — detecta a sua stack, **copia** os assets prontos e **gera** só o que é específico da stack. Você revisa e confirma entre as fases.

## 🤔 O que você ganha

| | |
|---|---|
| 🧠 **Camada de produto** | `/new-prd` (documento de negócio: problema, objetivos, escopo, épicos, user stories) e `/new-project` (quebra um épico em **sprints** com critérios de aceite — incluindo os **caminhos alternativos / sad paths** que a IA costuma esquecer). |
| 🧱 **Spec-Driven Development** | Feature mãe `F00XX` → fatias verticais `F00XX.N` → **1 PR ≤ ~400 linhas**. Nenhuma implementação sem spec aprovada. |
| 🔬 **Ciclo RDPI** | **R**esearch → **D**esign → **P**lan → **I**mplement, com `/clear` entre as fases para manter o contexto limpo. |
| 🤖 **9 skills + 8 agentes** | Skills do RDPI mais um implementador sênior, um revisor de código, um conselheiro de arquitetura, auditores de segurança read-only, um engenheiro de testes de integração e um escritor de docs. |
| ⚙️ **Setup híbrido** | Os assets reutilizáveis vêm prontos; só o específico da stack (`CLAUDE.md`, settings, contexto técnico) é gerado por projeto. |

## 🧭 Como funciona

```
PRD  (negócio — o quê & por quê)
 └─ PROJECT  (técnico — quebrado em sprints)
     └─ Sprint  (entregável + critérios de aceite incl. sad paths)
         └─ Feature F00XX  (spec mãe)
             └─ Fatia F00XX.N  →  RDPI  →  um PR pequeno
```

A camada macro só **planeja**; a execução fica no laço pequeno e auditável das fatias verticais. Cada sprint é apenas um conjunto de features que passam pelo ciclo RDPI já existente e pelos agentes de revisão/auditoria.

## 🧩 Componentes

O plugin fica em [`claude-code/`](claude-code/):

- **Comando** — `/mgsx-workflow-ai:setup` (o orquestrador de bootstrap).
- **Skills** — `new-prd`, `new-project`, `new-feature-spec`, `new-feature-slice`, `research-slice`, `design-slice`, `plan-slice`, `new-hotfix-spec`, `deliver-slice`.
- **Agentes** — `senior-implementer`, `code-reviewer`, `architecture-advisor`, `tenant-isolation-auditor`, `injection-reviewer`, `secret-scanner`, `integration-test-engineer`, `docs-writer`.
- **Recursos** — templates (PRD, PROJECT, feature, fatia, pesquisa), docs base, esqueletos de settings, e a biblioteca completa de prompts `00–15` (PT/EN).

Veja [`claude-code/README.md`](claude-code/README.md) para a referência completa do plugin.

## 🔁 O fluxo do dia a dia

```text
/new-prd                     # documento de negócio — criado/atualizado uma vez por produto, evolui no tempo
/new-project EP01            # quebra um épico em sprints (critérios de aceite incl. sad paths)
/new-feature-spec            # spec mãe F00XX → fatias verticais F00XX.N
  → /research-slice → /clear → /design-slice → /clear → /plan-slice → implementar (RDPI)
```

**Regras de ouro:** 1 fatia = 1 PR ≤ ~400 linhas · no mínimo 2 fatias por feature · `/clear` entre Research / Design / Plan · nunca usar `Co-Authored-By` nos commits.

## 📦 Estrutura do repositório

```
mgsx-workflow-ai/                 # este repo — um monorepo por AI
├── README.md  ·  README-PT.md  ·  LICENSE
├── assets/                       # logo + banner
├── .claude-plugin/marketplace.json
└── claude-code/                  # o plugin do Claude Code (futuras AIs = novas pastas)
    ├── .claude-plugin/plugin.json
    ├── commands/  ·  skills/  ·  agents/  ·  resources/
    └── README.md
```

## 🔄 Atualizando

As atualizações são publicadas ao incrementar o `version` no marketplace. Para atualizar:

```text
/plugin update mgsx-workflow-ai
```

## 📄 Licença

[MIT](LICENSE) © MGSX

<div align="center"><sub>Feito para o <a href="https://www.claude.com/product/claude-code">Claude Code</a> · inspirado na tradição do Spec-Driven Development.</sub></div>
