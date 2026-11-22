---
name: vibe
description: Vibe orchestrator planner (XL). Use when the user invokes /vibe or asks for a governed plan for multi-step work. It clarifies the task, freezes requirements, runs superpowers brainstorming/writing-plans, maps modules to installed skills and agents, saves one plan file in docs/plans/, and tracks step status (done/blocked) in that file while executing and verifying. Not for one-line fixes or quick questions.
---

# Vibe Orchestrator Planner (XL)

Govern a complex task from intent to verified delivery. **One plan file** is both the spec and the progress tracker. There is only one workflow tier: **XL** (waves of modules; at most 2 parallel units per wave, and only when their write scopes are disjoint).

User instructions (CLAUDE.md / AGENTS.md / direct requests) take priority over this skill. This skill's save location and tracking rules take priority over defaults in the skills it calls.

## Stages (do not skip; keep simple tasks light)

| # | Stage | Output | Gate |
|---|---|---|---|
| 1 | Skeleton check | Repo state, existing plans, dirty files | none |
| 2 | Deep interview | Goal, deliverables, constraints, acceptance, non-goals | user answers |
| 3 | Requirement freeze | Plan file, sections 1–3 | **Stop: user approves requirement** |
| 4 | XL plan | Modules, skill map, waves, tasks, verification | **Stop: user approves plan** |
| 5 | Execute | Work by wave, status kept in the plan file | blocked → ask |
| 6 | Cleanup and delivery | Verification evidence, result section, summary | none |

### 1. Skeleton check (2 probes + 1 verify, within the first 3 actions)
- `git status` and the repo shape. Search `docs/plans/` for an existing plan on this topic. **If one exists, resume it** (see Resume) instead of creating a duplicate.
- Search for the key terms of the task, then check one key assumption against real files.
- Record which dirty files are not ours. Never touch them.

### 2. Deep interview → `superpowers:brainstorming`
Invoke `superpowers:brainstorming` and follow it: ask questions one at a time, offer 2–3 approaches with tradeoffs, and recommend one. Capture:
goal · deliverables · constraints · acceptance criteria (each verifiable) · manual spot checks · non-goals · assumptions · open questions.
Also capture anti-drift fields: the **primary objective**, the **proxy signals** that must not be mistaken for success (for example "build passes" when the goal is "matches design"), and the **intended scope**.

### 3. Requirement freeze (hard stop)
Create the plan file from [plan-template.md](plan-template.md) at
`docs/plans/YYYY-MM-DD-<kebab-slug>-xl.md` (today's date; if the project already keeps plans elsewhere, use that folder). Fill sections 1–3 and set `Status: Draft – requirement`.
Do **not** write separate files to `docs/superpowers/specs` or `docs/requirements`: this file is the single source of truth.
Show the user a short summary and **end the turn**. A detailed original request does not count as approval.

### 4. XL plan → module and skill organization, then `superpowers:writing-plans` (hard stop)
1. **Split the work into modules** (each with one clear outcome).
2. **Find owners for each module.** List the installed skills (Skill tool listing) and agents (`~/.claude/agents/*.md` and the Agent tool types). For each module, read at most 3 candidates' `SKILL.md` or agent description. Choose by actual purpose, not keyword match. Roles: `owner` (does the work), `support` (runs first and feeds the owner), `verifier` (runs after the owner). A module with no fitting skill is `agent_direct`. Mark a module `gap` honestly if the work cannot be covered.
   Typical map: UI → `frontend-developer`/`ux-architect`; architecture → `software-architect`/`backend-architect`; project-specific skills (e.g. `clone-website`) for their domain; tests → `superpowers:test-driven-development`; bugs → `superpowers:systematic-debugging`; review → `code-reviewer` + `superpowers:requesting-code-review`; QA → `evidence-collector`/`reality-checker`.
3. **Invoke `superpowers:writing-plans`** to write tasks into section 5 of the same file: bite-sized steps, exact paths, runnable verification commands. Override its save path with the plan file.
4. **Waves:** order modules by dependency. A wave holds at most 2 units, and only when they are dependency-ready and their write scopes do not overlap. Overlapping or nested scopes run one after another.
5. Fill verification, rollback and cleanup. Set `Status: Draft – plan`. Show the module, skill and wave table and any gaps, then **end the turn**.
   If the user revises the plan, change only the affected modules, log the change under Revisions, and stop again.

### 5. Execute (after plan approval, no routine permission requests)
- Set `Status: Executing`. For each task, follow the skill assigned to it: read its `SKILL.md` and follow it, instead of improvising.
- Serial work: `superpowers:executing-plans`. Parallel units: `superpowers:dispatching-parallel-agents` or `superpowers:subagent-driven-development`. **Every parallel agent works in its own worktree** (`superpowers:using-git-worktrees`), and the lead merges at the end of the wave.
- Give each delegated unit a task contract: goal · scope in/out · inputs · outputs · definition of done · verification command · `status`. If it lacks information it returns `blocked` with questions and does not guess.
- Trust, but verify each agent's result: check the diff and rerun its verification before marking the unit done.
- Only the lead (this session) edits the plan file and makes final completion claims.
- If a check fails: make at most **one** targeted repair, then rerun the check. If it still fails, mark the unit `[!]` blocked with the reason, and continue with independent work.
- New skills, modules or scope changes need a user-approved revision. Do not add any silently.

### 6. Cleanup and delivery → `superpowers:verification-before-completion`
- Run every verification command and record the actual output summary in the plan file. Then remove temp files and merged worktrees, touching only paths this run created (no blanket recursive deletes).
- Fill the Result section: delivered items, checks run, gaps, and follow-ups. Set `Status: Done` only if every required task is `[x]` with evidence. Otherwise use `Done – partial` or `Blocked`.
- Tell the user briefly: results with paths, checks run, and real blockers. Do not commit unless asked.

## Progress tracking (mandatory)
The plan file is the live tracker. **Update it at every state change, before reporting to the user.**
- Task markers: `[ ]` pending · `[~]` in progress · `[x]` done · `[!]` blocked · `[-]` dropped (by approved revision).
- Each `[x]` gets a short evidence note (command and result, or file). Each `[!]` gets its reason, and the question goes to the Blockers table.
- Keep the **Progress** header current: status, current wave, counts done/total, last updated (date), next action.
- The Handoff note (3–5 lines) at the end must let a fresh session continue without this conversation.

## Resume
On `/vibe` with an existing plan: read the plan file, re-verify the last `[x]` items that later work depends on, set any stale `[~]` back to `[ ]`, then continue from **Next action**. Never re-ask questions the plan already answers.

## Quality rules
- No success claim without evidence. "Should work" or "I'm confident" is not evidence.
- Never present fallback or degraded results as success. State them explicitly.
- No mock passes, swallowed errors or template-only results.
- Do not mistake proxy signals for the primary objective.
- Effort on verification should be comparable to effort on execution.
