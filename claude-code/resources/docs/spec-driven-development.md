# Spec-Driven Development with Vertical Slices (RDPI)

> The heart of the workflow. **Every feature starts with a spec; no implementation
> without an approved spec.** Large features are broken into **vertical slices** — small,
> end-to-end increments — each passing through **Research → Design → Plan → Implement
> (RDPI)** and becoming **one PR ≤ ~400 lines of diff**.
>
> This is the central index and rulebook for the methodology. Templates live in
> `docs/templates/`; the operational recipes live in `docs/claude-code-guide.md`.

---

## Philosophy

- **No implementation without an approved spec.** Code follows a spec that has been
  written down and approved — never the reverse. The spec is the contract; the diff
  is the fulfillment.
- **Specs are small and layered.** A parent feature spec stays short (~150–250 lines)
  and is split into slice sub-specs that carry the detail.
- **Facts before opinions.** Research records what *is*; design decides what *changes*.
  The two are never mixed in the same step.
- **Clean context windows.** Each RDPI phase runs in its own clean window (`/clear`
  between Research, Design, and Plan) so earlier reasoning does not contaminate later
  decisions.

---

## Product Layer

The methodology has a product layer above the engineering layer. The full hierarchy:

```
PRD  →  PROJECT  →  Sprint  →  Feature F00XX  →  Slice F00XX.N  →  RDPI
```

- **PRD** (`docs/product/PRD.md`) — the **business** source of truth: problem, goals,
  scope, epics, user stories, high-level technical context. One living PRD per product.
  Created/updated via `/new-prd`. **No implementation detail.**
- **PROJECT** (`docs/product/PROJECT-{slug}.md`) — takes one epic from the PRD and breaks
  it into **Sprints**. Each sprint says what it delivers, which `F00XX` features compose
  it, and its acceptance criteria **including mandatory sad paths**. Technical planning,
  but **not** the file-by-file plan. Created via `/new-project`.
- **Sprint (`Sxx`)** — a coherent shippable increment inside a PROJECT, mapped to a set
  of `F00XX` features.
- **Feature (`F00XX`)** — a parent feature spec (`docs/features/F00XX-{slug}.md`),
  created via `/new-feature-spec`. Lists **≥ 2** vertical slices.
- **Slice (`F00XX.N`)** — a vertical-slice sub-spec (`docs/features/F00XX.N-{slug}.md`),
  created via `/new-feature-slice`. One slice = one RDPI cycle = one PR.
- **RDPI** — Research → Design → Plan → Implement, the per-slice execution cycle.

---

## Non-Negotiable Engineering Standards

Every slice must satisfy these. They are the baseline, not aspirations.

- **Clean code** — small, focused functions; no dead code; no commented-out code.
- **SOLID** — single responsibility, open/closed, Liskov, interface segregation,
  dependency inversion — applied to the language's idioms.
- **DRY** — no duplicated logic; reuse the patterns surfaced in research.
- **Separation of concerns** — layers do one job (transport ≠ business ≠ persistence).
- **Guard clauses** — validate and fail fast at the top; avoid deep nesting.
- **Naming** — names reveal intent; no abbreviations that need a comment to decode.
- **Explicit error handling** — errors are handled deliberately; no swallowed
  exceptions; no silent failures.
- **Readability & maintainability** — code is written to be read; the next person
  understands it without the author present.
- **Testability** — units are isolatable and tested; dependencies are injectable.
- **Scalability & production-readiness** — the code is fit to run in production:
  observable, resource-aware, and degrades gracefully.
- **Rich domain** — behavior lives in the domain, not in anemic data bags pushed
  around by procedural services.

---

## Implementation Standards & Tooling

Implementation (the **I** in RDPI) is tool-driven, not just typing code. Two rules
are non-negotiable during coding:

- **Never hallucinate an external API** — fetch the real signature from a live-docs
  source (e.g. the `context7` MCP) or the dependency's own source.
- **Never guess-patch a failing test** — debug systematically (form a hypothesis,
  instrument, isolate) before changing code, and run real verification before
  declaring anything done.

Which skill/plugin/MCP to reach for in each situation is in the
**Implementation Playbook** in [`claude-code-guide.md`](./claude-code-guide.md). The
`/mgsx-workflow-ai:setup` command writes a concrete version of that map (for this
project's stack) into `CLAUDE.md`.

---

## Mandatory Security Standards

Non-negotiable on every slice (also enforced by the slice security checklist):

- **Auth on every endpoint** — no entry point ships without authentication and the
  correct authorization check.
- **Data isolation** — every query is scoped to its tenant/owner; no cross-tenant or
  cross-user access is possible.
- **Parameterized queries** — never build SQL/commands by string concatenation or
  interpolation of input; whitelist any dynamic identifiers.
- **No sensitive data in logs** — never log secrets, credentials, tokens, or PII.
- **Rate limiting** — protect expensive or abusable endpoints with limits/quotas.
- **No stack traces in responses** — internal errors are logged server-side; responses
  carry safe, generic messages.

---

## The Vertical-Slice Golden Rule

- **1 slice = 1 PR ≤ ~400 lines of diff** (excluding tests). If it exceeds that, split
  it into two slices.
- **Minimum 2 slices per parent feature.** A feature that is "just one slice" is either
  too small to be a feature or not yet broken down.
- **`/clear` between R, D, and P.** Clean windows prevent context contamination across
  phases.
- **Never use `Co-Authored-By`** in commits.
- **Per-slice flow:** `new-feature-spec` → `new-feature-slice` → `/clear` →
  `research-slice` → `/clear` → `design-slice` → review Critical Files in the editor →
  `/clear` → cut branch → `plan-slice` → TDD (Red/Green/Refactor) → audit → PR.

---

## Branch & PR Conventions

- **Feature branches:** `feature/F00XX.N-{slug}` cut from `develop`. PRs target `develop`.
- **Hotfix branches:** `hotfix/HF00XX-{slug}` cut from `main`. PRs target `main`, then
  **merge back** into `develop`.
- **Slugs** are in **English**, kebab-case.
- **PR size:** ≤ ~400 lines of diff (excluding tests). Each PR must include ≥ 1 happy
  and ≥ 1 sad E2E test, pass CI, and carry a `code-reviewer` APPROVED verdict.
- **Commits:** never include `Co-Authored-By`.

---

## Domain Glossary

> _Placeholder — fill with this project's domain terms so specs use one shared vocabulary._

| Term            | Meaning                                          |
| --------------- | ------------------------------------------------ |
| {term}          | {definition}                                     |
| {term}          | {definition}                                     |

---

## Project Technical Context

> **PLACEHOLDER — generated per project.** Replace this section with the detected
> technical context: architecture style, persistence, auth/authorization model
> (e.g. multi-tenancy, RBAC), messaging, test runner, build/CI commands, and layer
> conventions. This is the **detailed** engineering context; the PRD's §6 stays
> high-level and must not duplicate it.

_To be filled when the workflow is set up in the target project._

---

## Index — PRDs & PROJECTs

| Document             | Type    | Source epic | Status | Path                                |
| -------------------- | ------- | ----------- | ------ | ----------------------------------- |
| _(none yet)_         |         |             |        |                                     |

---

## Index — Features (F00XX)

| ID     | Slug         | Domain | Status | Slices | Path                              |
| ------ | ------------ | ------ | ------ | ------ | --------------------------------- |
| _(none yet)_ |        |        |        |        |                                   |

---

## Index — Hotfixes (HF00XX)

| ID      | Slug         | Status | Branch                     | Path                             |
| ------- | ------------ | ------ | -------------------------- | -------------------------------- |
| _(none yet)_ |         |        |                            |                                  |

---

## Folder Layout

```
docs/
  spec-driven-development.md   ← this file (central index + rules)
  claude-code-guide.md         ← operational recipes
  product/
    PRD.md                     ← living PRD (business)
    PROJECT-{slug}.md          ← one per initiative
  templates/
    prd.md  project.md  feature-mae.md  feature-slice.md  research-notes.md
  features/                    ← F00XX-{slug}.md (parent) + F00XX.N-{slug}.md (slices)
  hotfixes/                    ← HF00XX-{slug}.md
  research/                    ← F00XX.N-{slug}.md (research notes, facts only)
```
