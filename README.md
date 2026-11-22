# myskills

My Claude Code skills and subagents.

## Layout

- `skills/` — skills. Copy a folder into `~/.claude/skills/`.
- `agents/` — subagents (one `.md` per agent). Copy into `~/.claude/agents/`.
- `plugins/` — full copies of the installed plugins `agentic-council` (Apache-2.0, by Daniel Song) and `superpowers` (MIT, by Jesse Vincent), each with its own skills, agents and `LICENSE`. Originals: github.com/dtsong/agentic-council and github.com/obra/superpowers.

## Licenses

- `agents/` is the AgentLand "agency-agents" collection, MIT licensed. See `agents/AGENCY-AGENTS-LICENSE.txt`.
- `skills/vibe` is my own.
- Most other skills are Anthropic's (`algorithmic-art`, `docx`, `internal-comms`, `pdf`, `pptx`, `skill-creator`, `web-artifacts-builder`, `xlsx` under Apache-2.0; `artifact-emulator`, `built-in-browser`, `chrome-browser`, `computer-use` under Anthropic's own license in their `LICENSE.txt`). Each keeps its original license file. `deep-research`, `docs`, `google-workspace`, `import-memory` and `morning` have no license file.
- `reverse-engineer-anything` comes from the REA tool and has no license file.

I don't own the third-party skills. They are here as a personal backup. Check each skill's license before reusing it.
