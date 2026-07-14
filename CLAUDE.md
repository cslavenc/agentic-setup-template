# CLAUDE.md

Speak efficiently in user-facing messages to save tokens, but use clear, standard English for planning, code, structured
data, tool calls, and any other internal or technical outputs

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Tech & patterns (quick reference)

<!-- TODO : add technologies and patterns used in the project -->

## Subagent workflow

For any non-trivial task, drive the work through subagents in this order. Steps in _(parentheses)_ are conditional; the
last three are on-demand.**Feedback always loops back to the specific agent that owns the thing being changed and never
patched inline by whichever agent happens to be running.** If sparring-partner (or anyone else) finds a gap in the plan
or design, re-invoke **planner** (and **senior-architect** too, if the gap is architectural) with that feedback — don't
silently work around it. If code-reviewer finds an implementation problem, re-invoke **senior-software-engineer**; if it
finds a test-coverage gap, re-invoke **senior-test-engineer**. Keep looping a step until its reviewer/critic is
satisfied before moving on.

```
planner
  → (senior-architect)        ── fold its design back into the plan
  → sparring-partner          ── stress-test the plan
  → planner / senior-architect ── revise the plan/design if critique found gaps
  → senior-software-engineer  ── implement
  → code-reviewer             ── review the implementation
  → senior-software-engineer  ── address findings (loop until clean)
  → senior-test-engineer      ── add / strengthen tests
  → code-reviewer             ── 2nd pass: are any test types missing?
  → senior-test-engineer      ── address findings (loop until clean)
  → (on demand) teacher · tech-decision-recorder · architecture-docs-keeper
```

1. **planner** — turn the task into a concrete, sequenced plan.
2. **senior-architect** _(if it crosses module/service boundaries, adds a
   dependency or pattern, or has competing designs)_ and deals with a scalable and good architecture — feed its output
   back into the plan.
3. **sparring-partner** — challenge assumptions, edge cases, failure modes,
   second-order effects, simpler alternatives.
4. **planner and/or senior-architect** _(whenever sparring surfaced gaps)_ —
   revise the plan/design directly with planner or senior-architect; re-run sparring-partner again if the plan is
   substantially different.
5. **senior-software-engineer** — implement the agreed plan.
6. **code-reviewer** — correctness, security, reliability, design.
7. **senior-software-engineer** _(whenever code-reviewer found issues)_ —
   address the findings; re-run code-reviewer if the fix is substantial.
8. **senior-test-engineer** — cover the new/changed code.
9. **code-reviewer** — second pass focused on test coverage: are unit /
   integration / edge-case / regression tests missing?
10. **senior-test-engineer** _(whenever this pass found gaps)_ — address the
    findings.
11. **on demand, once a substantial feature has landed:**
    - **tech-decision-recorder** — record non-trivial / hard-to-reverse decisions.
    - **architecture-docs-keeper** — sync ERD / module / system diagrams.
    - **teacher** — turn the finished work into onboarding material.

## Implementing many user stories at once

When you are provided with many user stories, always do the following for the subagent workflow when creating plans and
implementations:

1. First, look at all user stories and ask any relevant questions you might have to improve the final plan
2. Then, create an overview of all user stories to implement so that progress can be tracked
3. Then, create a plan for the first user story doing the usual subagent workflow
4. Once, the plan is fully ready, start the implementation of the first user story and at the same time, begin planning
   the next user story. You can plan two new user stories at most.
5. Do not implement different user stories at the same time! This can lead to editing the same files and code conflicts!
   Instead, once the plans of the next user stories are fully ready, wait until the first user story is fully
   implemented before starting the implementation of the next user story.
6. When the next user story is being implemented, you can start planning the user story after that
7. After the current user story has finished the engineering and review part and continues to the test part, you can
   start implementing the next user story already, since no changes to the business logic are expected at that point.
8. Once a user story is fully implemented, create a short and precise commit (see git log for examples) and push it

## Repository shape

<!-- TODO : add repository shape -->

## Commit message convention

Write commit messages in **imperative present tense**, short and to the point.
Start with a capital letter, no period at the end.

Examples:

- ✅ `Add employee domain module`
- ✅ `Fix null pointer in salary calculation`
- ✅ `Refactor auth middleware for clarity`
- ✅ `Update pom.xml with new springdoc dependency`
- ❌ `added employee module` (lowercase)
- ❌ `Adds employee domain module.` (future tense, period)
- ❌ `chore: Add employee module` (prefix tag not used)

This keeps the git log readable and uniform when many agents touch the repo.

> Note: both halves are still close to their generated skeletons. The backend
> currently contains only `TemporasPlatformApplication`; the SPA is the Nx
> starter shell. Expect to be building out modules/domains, not just editing
> existing ones.

## Backend (root)

<!-- TODO : add information about the backend -->

### Architecture & conventions

<!-- TODO : add information about the backend architecture and conventions -->

## Frontend (`temporas-ui/`)

<!-- TODO : add information about the frontend -->

## Cross-cutting contract (backend ↔ frontend)

<!-- TODO : add information about interaction between backend and frontend -->

