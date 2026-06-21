# mgsx-workflow-ai · Claude Code plugin

The Claude Code plugin for the [MGSX Workflow-AI](../README.md). Ships one orchestrator command plus reusable RDPI skills and review/audit agents, and bootstraps the whole workflow into any project.

## Install

```text
# the parent repo is the marketplace
/plugin marketplace add michelgomessilva/mgsx-workflow-ai
/plugin install mgsx-workflow-ai@mgsx-workflow-ai
```

> Full project overview: [main README](../README.md) · [README (Português)](../README-PT.md).

## Usage

In any project (empty directory or existing repo):

```text
/mgsx-workflow-ai:setup            # interactive, English, phased with checkpoints
/mgsx-workflow-ai:setup --lang pt  # follow the Portuguese prompt library
/mgsx-workflow-ai:setup --yes      # no pauses between phases
```

### What setup does (phases)
- **Phase 0 (Detect):** new vs existing project; detect language/framework/test/build/format commands. Checkpoint.
- **Phase A (Copy static):** skills → `.claude/skills/`, agents → `.claude/agents/`, templates → `docs/templates/`, base docs → `docs/`, prompt library → `docs/prompts/`, settings skeletons. Never overwrites without asking. Checkpoint.
- **Phase B (Generate stack-specific):** `CLAUDE.md`, tuned `settings*`/`.pre-commit-config.yaml`, the technical context of `docs/spec-driven-development.md`, `.mcp.json`. Checkpoint.
- **Phase C (Plugins + verify):** recommends `superpowers` and `context-mode`; runs the parity checklist; points you to the product cycle.

## Components

### Command
- `setup` → `/mgsx-workflow-ai:setup` (the bootstrap orchestrator).

### Skills (`skills/`)
Product layer: `new-prd`, `new-project`. RDPI: `new-feature-spec`, `new-feature-slice`, `research-slice`, `design-slice`, `plan-slice`. Hotfix: `new-hotfix-spec`. Delivery: `deliver-slice`. They are auto-registered (e.g. `/mgsx-workflow-ai:new-prd`) and also copied into the target project's `.claude/skills/` (usable as `/new-prd`, versioned in the repo).

### Agents (`agents/`)
`senior-implementer`, `code-reviewer`, `architecture-advisor`, `tenant-isolation-auditor`, `injection-reviewer`, `secret-scanner`, `integration-test-engineer`, `docs-writer`.

### Resources (`resources/`)
`templates/` (prd, project, feature-mae, feature-slice, research-notes), `docs/` (spec-driven-development, claude-code-guide), `settings/` (skeletons), `prompts/{pt,en}/` (the 00–15 prompt library: the methodology each generated artifact follows).

## After bootstrap: the workflow

```text
/new-prd                     # business document (problem, goals, scope, epics, user stories)
/new-project EP01            # break an epic into sprints (acceptance criteria incl. sad paths)
/new-feature-spec            # F00XX parent spec → vertical slices F00XX.N
  → research-slice → /clear → design-slice → /clear → plan-slice → implement (RDPI)
```

See `docs/claude-code-guide.md` (copied into your project) for the full recipes.

## License
MIT. See [../LICENSE](../LICENSE).
