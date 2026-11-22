# {Title} — XL Plan

> Managed by `/vibe`. This file is the single spec and progress tracker. Execute with `superpowers:executing-plans`. Update the markers and Progress block on every state change.

## Progress
| Status | Wave | Done | Last updated | Next action |
|---|---|---|---|---|
| Draft – requirement | – | 0/0 | YYYY-MM-DD | Await requirement approval |

Status values: `Draft – requirement` → `Draft – plan` → `Executing` → `Done` | `Done – partial` | `Blocked`
Markers: `[ ]` pending · `[~]` in progress · `[x]` done (+evidence) · `[!]` blocked (+reason) · `[-]` dropped

## 1. Requirement
- **Goal:**
- **Deliverables:**
- **Constraints:**
- **Non-goals:**
- **Assumptions:**
- **Open questions:**

## 2. Acceptance
| ID | Criterion (verifiable) | Mode (auto/manual) | Check |
|---|---|---|---|
| A1 | | auto | `command` |

Manual spot checks:

## 3. Anti-drift
- **Primary objective:**
- **Proxy signals (not success by themselves):**
- **Intended scope:**
- **Completion target:** (full delivery, or a bounded part)

## 4. Modules and skill map
| Module | Outcome | Skill / agent (role) | Write scope | Depends on |
|---|---|---|---|---|
| M1 | | `skill` (owner) | `path/` | – |

Gaps (no fitting skill): none

Waves (at most 2 parallel units, disjoint write scopes; parallel agents use their own worktrees):
- Wave 1: M1
- Wave 2: M2 ‖ M3

## 5. Tasks
### M1 — {name} (wave 1, owner: `skill`)
- [ ] Step … — verify: `command`

## 6. Verification
- `<project check command>` (e.g. lint, typecheck, tests, build)

## 7. Rollback
Prefer additive changes. Name the files to restore if a wave fails. Leave unrelated dirty files untouched.

## Blockers
| Task | Reason | Question / needed | Since |
|---|---|---|---|

## Revisions
- YYYY-MM-DD: plan approved.

## Result
Delivered: · Checks run: · Gaps: · Follow-ups:

## Handoff
Where things stand, next action, and caveats in 3–5 lines, so a fresh session can resume.
