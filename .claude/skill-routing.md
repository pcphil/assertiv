# Skill Routing

Authority for choosing between overlapping skills from `mattpocock-skills`, `agent-skills` (Addy Osmani), `caveman`, and Claude Code built-ins. This file outranks any plugin's own routing advice (e.g. `agent-skills:using-agent-skills`). `ecc` / `everything-claude-code` are disabled and out of scope; re-enabling either means revising this file.

## Precedence

1. A skill the user names (`/plugin:skill`) wins and owns its stage.
2. Otherwise route by the tables below.
3. Caveman is output style only. It shapes how text reads, never which workflow runs.

## Kickoff

Every feature request opens with one proposal line, then waits for the user's confirm or edit:

```
Tier: M · Pipeline: grill → plan → build(TDD) → verify → review(addy+matt) → PR · OK?
```

## Feature tiers

| Tier | Shape | Pipeline |
|---|---|---|
| **S** | 1–2 files, obvious | build(TDD) → review(addy) → commit |
| **M** | multi-file, clear goal | grill → plan → build(TDD) → verify → review(addy+matt) → PR |
| **L** | new subsystem, fuzzy, cross-cutting | grill → [idea-refine / research / prototype] → spec → design → plan → build(TDD) → verify → review(addy+matt) → `/security-review` → PR → [ship] |

**Gates:** stop for user approval after Spec and after Plan. Code starts only after the gated artifact is approved.

## Stage → skill

| Stage | Skill | When |
|---|---|---|
| Intake | `mattpocock-skills:grilling` | default; also any "grill" / "grill me" |
| Intake (diverge) | `agent-skills:idea-refine` | idea still vague, needs options before narrowing |
| Research | `mattpocock-skills:research` | L tier, upfront investigation |
| Prototype | `mattpocock-skills:prototype` | UI or state model needs a feel-check before spec; throwaway code lives in scratchpad |
| Spec | `agent-skills:spec-driven-development` | L tier; save to `docs/specs/<slug>.md` |
| Design: domain | `mattpocock-skills:domain-modeling` | new terms or hard-to-reverse decisions; writes `GLOSSARY.md`, `docs/adr/` |
| Design: modules | `mattpocock-skills:codebase-design` | choosing module boundaries, seams, interfaces |
| Design: public API | `agent-skills:api-and-interface-design` | HTTP / public contract involved |
| Plan | `agent-skills:planning-and-task-breakdown` | M/L; writes `tasks/plan.md`, `tasks/todo.md` |
| Build | `agent-skills:incremental-implementation` + `agent-skills:test-driven-development` | every tier; refactor inside the red-green-refactor loop |
| Build: libraries | `agent-skills:source-driven-development` | code touches framework/library APIs |
| Build: security | `agent-skills:security-and-hardening` | auth, user input, secrets, external services |
| Build: errors | `agent-skills:debugging-and-error-recovery` | test/build/runtime error surfaces mid-build |
| Verify | `run` + `claude-in-chrome` (UI) | M/L |
| Review: quality | `agent-skills:code-review-and-quality` | every tier |
| Review: spec fidelity | `mattpocock-skills:code-review` | M/L; spec source `docs/specs/` |
| Review: security | `/security-review` | L tier or security-sensitive diff, before PR |
| Git | `agent-skills:git-workflow-and-versioning` | branching, atomic commits |
| Commit message | `caveman:caveman-commit` | every commit |
| PR body | `mattpocock-skills:pr` | every PR |
| Ship | `agent-skills:shipping-and-launch` | production launches only |

## Side flows

- **Bug:** `mattpocock-skills:diagnosing-bugs` → failing regression test → fix → `agent-skills:code-review-and-quality` → commit.
- **Refactor:** `agent-skills:code-simplification` (or `/simplify` for a quick pass on the current diff) → `agent-skills:code-review-and-quality` → commit.

## Meta

- Editing `CLAUDE.md`, skills, or this file → `mattpocock-skills:writing-for-agents`.
- Session handoff or context reset → `agent-skills:context-engineering`.
- Delegation: `caveman:cavecrew-investigator` locates code; `caveman:cavecrew-builder` makes bounded 1–2 file edits. Spawn subagents only on user request, except read-only fact lookups a skill dispatches itself.

## Manual-only

Reached only when the user names them:

- `mattpocock-skills:tdd`, `agent-skills:interview-me`, `code-simplifier` agent, `agent-skills:browser-testing-with-devtools` (no chrome-devtools MCP configured).
- Agents: `agent-skills:code-reviewer`, `test-engineer`, `security-auditor`, `web-performance-auditor`.
- User-invoked mattpocock skills; each replaces the stages listed:

| Skill | Takes over |
|---|---|
| `/grill-with-docs` | Intake + Design: domain |
| `/to-spec` | Spec (published to issue tracker) |
| `/to-tickets` | Plan (tracer-bullet tickets) |
| `/implement` | Build + Review |
| `/implement-spec` | Build + Review across a whole spec, parallel subagents |
| `/wayfinder` | Plan for work larger than one session |
| `/improve-codebase-architecture` | Refactor flow, architecture scale |
| `/triage` | Issue triage |
| `/retro` | Post-session environment review |
| `/ask-matt` | Picks among the above |
