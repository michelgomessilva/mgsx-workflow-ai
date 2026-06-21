---
description: Bootstrap the full MGSX Workflow-AI into the current project (new or existing) — Spec-Driven Development with vertical slices, the PRD→PROJECT product layer, RDPI skills and review/audit agents. Runs in phases with checkpoints.
argument-hint: "[--lang pt|en] [--yes]  (optional; defaults to interactive English)"
---

# MGSX Workflow-AI — Bootstrap

You are installing the **MGSX Workflow-AI** into the project at the current working directory. The plugin ships ready-made, stack-agnostic assets under `${CLAUDE_PLUGIN_ROOT}`; your job is to (1) **copy** those static assets into this project and (2) **generate** only the stack-specific pieces. Work in **phases with checkpoints** — after each phase, summarize what changed and **wait for the user to say "continue"** before the next phase (skip the pauses only if the user passed `--yes`).

Arguments: `$ARGUMENTS` — `--lang pt|en` chooses the prompt-library language to follow (default `en`); `--yes` runs all phases without pausing.

## Plugin asset map (source = `${CLAUDE_PLUGIN_ROOT}`)
- `skills/` — 9 RDPI skills (new-prd, new-project, new-feature-spec, new-feature-slice, research-slice, design-slice, plan-slice, new-hotfix-spec, deliver-slice).
- `agents/` — 8 agents (senior-implementer, code-reviewer, architecture-advisor, tenant-isolation-auditor, injection-reviewer, secret-scanner, integration-test-engineer, docs-writer).
- `resources/templates/` — prd, project, feature-mae, feature-slice, research-notes.
- `resources/docs/` — spec-driven-development.md (base), claude-code-guide.md.
- `resources/settings/` — settings.json, settings.local.json, .pre-commit-config.yaml (skeletons with `<PLACEHOLDERS>`).
- `resources/prompts/{pt,en}/00..15.md` — the full prompt library (the detailed methodology for each generated artifact; copy into the project for reference and use as your generation guide).

## Phase 0 — Detect context (no writes yet)
1. Determine **new vs existing**: is the working directory essentially empty, or does it contain code/manifests?
2. **Detect the stack** by inspecting real manifests (`package.json`, `*.csproj`/`*.sln`, `pyproject.toml`/`requirements.txt`, `go.mod`, `pom.xml`/`build.gradle`, etc.). Identify language, framework, package manager, **test command**, **build command**, **format/lint command**, and any data-access/security model (e.g. multi-tenancy).
3. Print a short report: detected stack + the exact commands you'll wire into settings/hooks. **Checkpoint** — confirm before proceeding.

## Phase A — Structure + static copy
Copy from `${CLAUDE_PLUGIN_ROOT}` into this project (create folders as needed). For an **existing** project, NEVER overwrite a file that already exists without asking; for indexes/configs, **merge** rather than truncate.
1. `.claude/skills/` ← copy every `skills/<name>/SKILL.md` (so they are usable unprefixed as `/new-prd`, etc., and versioned in the repo).
2. `.claude/agents/` ← copy every `agents/*.md`.
3. `docs/templates/` ← copy `resources/templates/*`.
4. `docs/spec-driven-development.md` and `docs/claude-code-guide.md` ← copy from `resources/docs/` (if present already, ask).
5. `docs/prompts/{pt,en}/` ← copy `resources/prompts/*` (reference library + manual reruns).
6. Create empty `docs/features/`, `docs/hotfixes/`, `docs/research/`, `docs/product/` (add `.gitkeep` if empty).
7. `.claude/settings.json`, `.claude/settings.local.json`, `.pre-commit-config.yaml` ← copy the skeletons from `resources/settings/` (leave `<PLACEHOLDERS>` for Phase B).
**Checkpoint** — list created/skipped files; wait.

## Phase B — Stack-specific generation (dispatch heavy steps to subagents with clean context)
Follow the methodology in `resources/prompts/$LANG/` for each item (use `01`, `04`, `05`, `07`). Generate, do not copy:
1. **`CLAUDE.md`** at the repo root — commands (real test/build/format), architecture, patterns, quality bar, an SDD section, and a "Workflow-AI" section pointing to `/mgsx-workflow-ai:setup`, the skills and the product layer (follow prompt `01`). Also add an **"Implementation tool map"** subsection that makes the guide's Implementation Playbook concrete for THIS stack — fill the placeholders with the detected reality:
   - real library/API docs → the docs MCP you wire (default `context7`; add `microsoft-docs` on Microsoft stacks);
   - semantic navigation → the language LSP (e.g. `typescript-lsp` / `pyright-lsp` / `csharp-lsp`), noting if not installed;
   - failing-test debugging → `superpowers:systematic-debugging`;
   - "done" gate → `superpowers:verification-before-completion` + the real run command (test suite + how to launch the app/feature);
   - UI work → `frontend-design` (only if the project has a front-end).
   Keep the fallbacks. State that explicit user/CLAUDE.md instructions take precedence.
2. **Tune `.claude/settings.json` / `settings.local.json` / `.pre-commit-config.yaml`** — replace every `<PLACEHOLDER>` with the detected stack's real commands; add `ask` rules for the sensitive files that actually exist; ensure `.gitignore` keeps `settings.local.json` machine-local (follow prompt `04`).
3. **Fill the technical-context placeholder** in `docs/spec-driven-development.md` with the detected architecture/persistence/auth (follow prompt `07`). Adapt the engineering/security standards to the language.
4. **`.mcp.json`** — create with `{ "mcpServers": {} }`, and by default propose adding **`context7`** (language-agnostic live docs) so the implementation phase can fetch real APIs instead of guessing; wire the obvious read-only DB server if clearly applicable (follow prompt `05`). Do not invent credentials.
**Checkpoint** — show diffs/summaries; wait.

## Phase C — External plugins + verification
1. Recommend the companion market plugins and the exact install commands (installed once, globally; optional but recommended):
   - `superpowers` and `context-mode` (always);
   - the **language LSP** for the detected stack (e.g. `typescript-lsp`, `pyright-lsp`, `csharp-lsp`) for semantic navigation;
   - `frontend-design` if the project has a UI;
   - `code-review` to complement the `code-reviewer` agent.
   These power the Implementation Playbook — call out which ones matter most for this stack.
2. Run the **parity checklist**: `.claude/skills` present, `.claude/agents` present, templates + SDD doc + guide present, `docs/product/`, settings tuned, `CLAUDE.md` written. Report PASS/MISSING per item.
3. Tell the user the next step: the product cycle — `/new-prd` → `/new-project` → then `/new-feature-spec` and the RDPI flow (research → design → plan → implement). Reference `docs/claude-code-guide.md`.

## Rules
- **Idempotent**: safe to re-run; ask before overwriting; merge indexes.
- **Stack-agnostic**: never hardcode a language's commands — use what you detected.
- **Honor context economy**: dispatch the large generation steps (CLAUDE.md, SDD technical context) to subagents so the main thread stays light.
- **Never** add `Co-Authored-By` to commits. Do not commit anything unless the user asks.
