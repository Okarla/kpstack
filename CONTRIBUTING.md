# Contributing

## Adding a skill

1. Work on a branch in your own worktree, never on `main` (see [AGENTS.md](AGENTS.md)).
2. Create `skills/<name>/SKILL.md` with a kebab-case `<name>`.
3. Frontmatter needs `name` (same as the folder) and `description`. Agents
   pick skills by description, so lead with the trigger: "Use when...".
4. Keep the body short. Move long material into `references/` and scripts
   into `scripts/` in the same folder.
5. Vendored skills keep their upstream `LICENSE` and get a row in the README
   sources table.
6. Run the checks in [AGENTS.md](AGENTS.md#checks-in-this-repo).
