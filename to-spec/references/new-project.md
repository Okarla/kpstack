# When there's no code yet

A new project has no current state to follow, so the spec has to make the decisions that existing code would otherwise make for you. Treat it as new when the repo is empty or the idea is a new service or app.

## Find the constraints before you choose

No code doesn't mean no constraints. Look for them in this order:

1. The brief's Depth: hosting, compliance, scale, who owns it.
2. Systems it must talk to: existing services, APIs, databases, identity providers.
3. What the team already runs: sibling repos, a monorepo, shared libraries, deploy configs. Look these up yourself where you can.
4. What you can't find: ask the user, one question at a time. "What does your team already deploy to?" is a fact only they may know.

Write what you found under "Starting point" in place of "Current state".

## Work through the foundations

These are the decisions most expensive to change later. They come first in the decision map and go through the same step-by-step conversation as the rest of the architecture. Cover only what the tier needs:

- **POC:** language and framework, where it runs.
- **MVP:** POC, plus data store, auth, how it's deployed.
- **Production:** MVP, plus repo layout, CI and testing, observability, secrets and config.

Default to what the team already knows. A new language, framework or data store needs a reason in Decisions that ties back to the brief.

## Name the walking skeleton

Describe the thinnest slice that runs end to end on the chosen foundations, such as one request through one handler into one table and back. `/to-tickets` makes it the first ticket so every later ticket builds on something that already runs. With no existing tests to follow, the walking skeleton's first test also sets the first seam.
