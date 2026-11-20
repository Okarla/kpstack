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
| 2 | Spec | `to-spec` | vendored from mattpocock/skills |
| 3 | Tickets | `to-tickets` | vendored from mattpocock/skills |
| 4 | Isolate | `isolate` | kpstack (planned) |
| 5 | Build | `tdd`, `codebase-design` | vendored from mattpocock/skills |
| 6 | Prove | `prove` | kpstack (planned) |
| 7 | Review | `review-loop` (wraps `code-review`) | kpstack (planned); `code-review` vendored from mattpocock/skills |
| 8 | Ship | `ship` (PR body via `pr`) | kpstack (planned); `pr` vendored from mattpocock/skills |
| – | Writing for humans | `unslop` | vendored (planned) |

`AGENTS.md` decides which steps a task needs. A docs fix skips planning and
review; a new feature runs all of them.

## Install

```bash
npx skills add Okarla/kpstack
```

The skills kpstack uses from
[mattpocock/skills](https://github.com/mattpocock/skills) are vendored here,
each with its upstream MIT `LICENSE`, so you don't need to install that repo.
The vendored `to-spec`, `to-tickets`, and `code-review` don't need its
`/setup-matt-pocock-skills` step: they use the tracker named in your
`AGENTS.md`, or GitHub issues, or local files under `.scratch/`.

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
