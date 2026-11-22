---
name: execplan
description: Use when a task needs a repository-persisted execution plan for cross-session resumption, handoff, or multi-agent/stateless delegation orchestration (planning, dispatching leaf tasks, enforcing hard constraints, verifying gates, and tracking rework). Do not trigger for ordinary chat plans, narrow fixes, or routine single-session multi-file edits.
---

# ExecPlan

Maintain one lightweight Markdown plan in the repository as the **single source of truth** (shared blackboard) across sessions or stateless subagents.

## Boundary

- Follow precedence: system/developer instructions > `AGENTS.md` / project `PLANS.md` or local plan convention > this skill.
- Do not create a plan file for ordinary single-session work unless the user asked for persistence, an existing ExecPlan applies, or you are orchestrating multi-round / stateless delegation.
- Built-in ephemeral progress UIs (todo/task lists) mirror in-turn status; they do not replace a durable ExecPlan.
- Use the repository's existing plan path when one exists; otherwise pick one obvious local path (e.g. `docs/plans/<task>.md`) or ask briefly—never invent repo-wide conventions unasked.

## Minimal Plan Shape

Keep only sections the task needs; add extra sections (migration, rollback) only when warranted.

```markdown
# <Task>

Status: Draft | In Progress | Blocked | Complete

## Goal
## Scope / Out of scope
## Hard constraints
<!-- Locked user decisions, invariants, and do-not-repeat pitfalls -->
## Current facts
## Tasks & Gates
<!-- Non-overlapping tasks with Tech gate (objective check) and Human gate (if user judgment is needed) -->
## Progress
<!-- [ ] todo | [~] in progress or rework | [x] verified | [!] blocked — attach evidence/path/commit -->
## Decisions
<!-- Key discoveries, tradeoffs, and superseded approaches with brief rationale -->
## Outcome
```

## Core Discipline

1. **Verify reality before acting**: On creation or resumption, inspect current files and repo state. An approved plan is never permission to ignore current code or safety rules.
2. **Single source of truth**: Define each constraint in one authoritative place (keep invariants in `## Hard constraints` rather than restating them across `## Goal`, `## Scope`, and `## Decisions`). When a design or constraint is overturned, edit the authoritative section in place and move the retired approach to `## Decisions` as superseded—never leave contradictory old and new specs side by side.
3. **Close with fresh evidence**: Do not report completion from memory, stale checks, or apparent code changes. Run or verify the lightest fresh check proving the requested behavior, then write back final evidence and `## Outcome`.

## Multi-Agent / Stateless Delegation (when delegating)

1. **Exclusive writeback**: Only the main agent edits the ExecPlan, checks `[x]`, and clears gates. Subagents return code, artifact paths, and concise evidence; they do not edit the Plan or declare overall completion.
2. **Thin, self-contained dispatch**: Stateless delegates have no parent chat history. Pass only their non-overlapping leaf scope, excluded areas, the relevant **Hard constraints**, and exact acceptance evidence—do not paste full conversation history or unrelated prior tasks.
3. **Two-track gates**:
   - **Tech gates**: objective build/test/lint/diff checks verified independently by the main agent.
   - **Human gates**: visuals, UX feel, irreversible actions, or product tradeoffs—stop and ask the user before proceeding.
4. **Independent verification & bounded rework**:
   - Require delegates to report status (`DONE`, `DONE_WITH_CONCERNS`, `NEEDS_CONTEXT`, or `BLOCKED`) plus concrete evidence.
   - Never treat a subagent's self-report or a single passing command as final completion without main-agent review against `## Goal` and `## Hard constraints`.
   - If the same task does not converge after 2–3 rework rounds, stop retrying blindly: reassess assumptions, update the Plan in place, or escalate to the user.
