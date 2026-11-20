# kpstack

kpstack is a software factory for coding agents: an [`AGENTS.md`](AGENTS.md)
workflow plus a set of agent skills that take work from an idea to a reviewed
pull request. It's written for Claude Code first and uses the portable
`AGENTS.md` + `SKILL.md` format, so Codex and Cursor can load it too.

Inspired by [michaelshimeles/rstack](https://github.com/michaelshimeles/rstack).

## The pipeline

| # | Step | Skill | Comes from |
|---|---|---|---|
| 1 | Grill the idea | `ideate` | kpstack |
| 2 | Spec | `to-spec` | kpstack |
| – | Writing for humans | `unslop` | vendored (planned) |

`AGENTS.md` decides which steps a task needs. A docs fix skips planning and
review; a new feature runs all of them.

## Install

```bash
npx skills add Okarla/kpstack
```

Then copy [`AGENTS.md`](AGENTS.md) into the root of the repo you want agents
to work in and replace its "Checks in this repo" section. Claude Code reads
`CLAUDE.md`, so add one containing `@AGENTS.md`.

## Repo layout

```
AGENTS.md        workflow and multi-agent rules
CLAUDE.md        @AGENTS.md, for Claude Code
<name>/          one folder per skill at the root, e.g. ideation/: SKILL.md,
                 plus references/ or scripts/
```

See [CONTRIBUTING.md](CONTRIBUTING.md) to add a skill.
