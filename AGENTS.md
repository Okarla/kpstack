# Agent workflow

kpstack takes work from an idea to a reviewed pull request. Pick the parts of
this workflow that fit the task. It governs this repo and can be copied into
other repos; replace "Checks in this repo" with that repo's checks.

- Read-only work (questions, reviews, investigations) needs no branch, commit,
  or PR. If it turns into edits, isolate them first.
- Small documentation edits: isolate, run content and link checks, ship. No
  runtime evidence or review loop.
- Code changes: isolate, build, prove, review, ship.
- New features that aren't specified yet start with planning.
- A request for a plan, review, or local experiment does not imply a PR.

Each step names the skill that does it. If a skill isn't installed, follow the
step's description directly and say which skill was missing.

## Plan

1. **Grill.** `/grilling` when the idea is fuzzy or the user wants it
   stress-tested. Optional.
2. **Spec.** `/to-spec` turns the agreed idea into a spec.
3. **Tickets.** `/to-tickets` splits the spec into independently grabbable
   vertical slices. Each ticket then runs through Build below on its own.

Planning skills come from [mattpocock/skills](https://github.com/mattpocock/skills).

## Build

4. **Isolate.** `/isolate` before any repository edit: a new worktree and
   branch from `origin/main`, after checking open PRs for overlap. Keep a
   worktree already assigned to this task. Never build on `main`.
5. **Build.** `/tdd` for behavior changes. `/codebase-design` when choosing
   module boundaries or interfaces. The project's existing architecture wins.
6. **Prove.** `/prove` for changed runtime behavior: capture the failure
   before the fix and the success after it, as command output, test runs, or
   screenshots. Run the repo's checks.
7. **Review.** `/review-loop` for code and executable workflow changes. It
   runs `/code-review`, fixes actionable findings, and re-runs until the
   review is clean or it hits its iteration cap (default 5). Report the real
   outcome; never claim a clean review you didn't get, and never raise the
   cap on your own.
8. **Ship.** `/ship` when the task calls for a PR: commit, rebase onto
   `origin/main`, re-run checks, push, open the PR with a body written using
   `/pr`, and present the PR URL. Never merge unless asked.

## Writing for humans

Run `/unslop` over text a person will read before you commit, post, or send
it: commit messages, PR titles and bodies, docs, code comments. Apply it only
to text you wrote or changed.

## Multi-agent rules

- Never commit to `main`.
- One worktree and one branch per task and per agent. Never touch another
  agent's worktree, branch, or uncommitted work.
- Before starting, skim open PRs (`gh pr list`, `gh pr diff <n> --name-only`)
  for overlap. Ask only when changes conflict in behavior, depend on
  unfinished work, or ownership is unclear.
- Never plain `--force`. Use `--force-with-lease`, only on your own branch.
- Regenerate lockfiles on conflict; never hand-merge them.
- Worktrees share ports and databases. Confirm a dev server is *yours* before
  trusting it, and don't run schema experiments against a shared database.
- If a conflict can't be resolved confidently, stop and report.

## Checks in this repo

- `git diff --check` for whitespace.
- `npx skills add ./ --list` must list every folder under `skills/`.
- Each `skills/<name>/SKILL.md` has `name` (matching the folder) and a
  `description` that says when to use it.
- Check that local links and file references in changed docs resolve.
