---
name: to-spec
description: Use when an idea has been agreed, usually as an approved idea brief from /ideate. It needs a spec before it's split into tickets. Decides the architecture and system design, writes requirements and numbered acceptance criteria scaled to the idea's tier then ends with an approved spec. Works for changes to existing code and for new projects with no code yet.
---

Turn an agreed idea into a spec someone else could build from. The idea brief says why the user wants it, what it does and which direction they picked. The spec is where the architecture gets decided. It says what must be true when it's done and how the system will make it true.

The brief picks the direction: the shape of the solution the user can picture, such as extending an existing service or building a separate worker. The spec decides the architecture within that direction: data stores, sync or async, which component owns what, the contracts between them and how failures are handled.

## Input

Look for the idea brief in this session first. It's usually there because `/ideate` wrote it earlier in the conversation. If the user passed a path along with `/to-spec`, read that file instead.

If there's no brief in the session, ask the user for a file reference to one. Read the whole file before you start.

The brief is already approved. Don't ask again about why, what or boundaries and don't reopen the direction. If the code contradicts the brief or two parts of the brief conflict, raise it as a question. Never resolve it silently.

If the user has no brief, offer `/ideate`. If they decline, ask only what the spec can't be written without, one question at a time: the tier (POC, MVP or production), why they want it, the core flow and the direction. Put their answers at the top of the spec in place of the brief link.

## Steps

1. **Find the starting point.** If there's existing code, find what the idea touches: services, modules, schemas, APIs and how similar features are already built. This becomes "Current state", with file references. The existing architecture wins unless the brief says otherwise. If there's no code yet, follow [When there's no code yet](#when-theres-no-code-yet) instead.
2. **Requirements and acceptance criteria.** Derive the requirements from the brief's What, Boundaries and Depth. Number the acceptance criteria (AC-1, AC-2, ...) and make each one testable, either as a command with its expected output or as Given/When/Then. Every anti-goal becomes a "must not" criterion.
3. **First gate (MVP and production only).** Show the requirements and acceptance criteria and get them approved before you design anything. For a POC, go straight to the decisions.
4. **Decide the architecture.** List the architecture decisions this idea needs, based on the brief and the starting point. For each one with more than one reasonable answer, ask the user with options, trade-offs and your pick. Settle the rest yourself and tell the user. Record every decision under Decisions, including the options you rejected.
5. **Design.** Write the design on top of those decisions, as deep as the tier calls for. Write decisions and contracts, not code: "orders-service publishes `order.cancelled` with `id` and `reason`", not the function that emits it. If the design depends on one of the brief's "My calls", point it out so the user can confirm it.
6. **Play back and approve.** In under 200 words, say what will change, the riskiest decision and what's still open. Then ask for approval of the whole spec.

Save the spec to `docs/specs/<slug>.md`, where `<slug>` is the idea's name in kebab-case, with status `Draft` while you work. Set it to `Approved` when the user approves it, then suggest `/to-tickets` with the spec's path as its input.

If the design shows the brief's direction can't work, stop and tell the user why. Don't switch to another direction on your own.

## When there's no code yet

A new project has no current state to follow, so the spec has to make the decisions that existing code would otherwise make for you. Treat it as new when the repo is empty or the idea is a new service or app.

**Find the constraints before you choose.** No code doesn't mean no constraints. Look for them in this order:

1. The brief's Depth: hosting, compliance, scale, who owns it.
2. Systems it must talk to: existing services, APIs, databases, identity providers.
3. What the team already runs: sibling repos, a monorepo, shared libraries, deploy configs. Look these up yourself where you can.
4. What you can't find: ask the user, one question at a time. "What does your team already deploy to?" is a fact only they may know.

Write what you found under "Starting point" in place of "Current state".

**Decide the foundations.** These are the decisions most expensive to change later, so each one goes under Decisions with its options. Decide only what the tier needs:

- **POC:** language and framework, where it runs.
- **MVP:** POC, plus data store, auth, how it's deployed.
- **Production:** MVP, plus repo layout, CI and testing, observability, secrets and config.

Default to what the team already knows. A new language, framework or data store needs a reason in Decisions that ties back to the brief.

**Name the walking skeleton.** Describe the thinnest slice that runs end to end on the chosen foundations, such as one request through one handler into one table and back. `/to-tickets` makes it the first ticket so every later ticket builds on something that already runs.

## What belongs in the spec

The spec decides anything that crosses a boundary: between services, in public APIs, events or schemas. It also decides anything another team depends on. How code is arranged inside a single ticket is left to the build step.

Fill in every section of the template. If one doesn't apply, write `N/A:` and the reason. Deleting a section hides whether you considered it.

## Asking

Ask one question per turn, then wait. Ask only when the answer changes the design and neither the brief nor the code answers it. Otherwise, make the call and record it under Decisions. Tell the user so they can correct it.

Look up facts yourself (files, existing code, tools); don't ask the user for them. When a decision has choices you can name, give the choices, what each trades off and your pick:

```
❓ **<title>**: <one question>

- **<choice>:** <what it trades off>
- **<choice>:** <what it trades off>

➡️ <your pick and why, in one line>
```

## The spec

```markdown
# Spec: <name>

**Status:** Draft | Approved
**Brief:** <path to the brief file; "this session" if it came from the conversation>
**Tier:** POC | MVP | Production

## Requirements
- **Functional:** <behavior a user or caller can observe>
- **Non-functional:** <latency, scale, security, cost: from the brief's Depth>

## Acceptance criteria
- **AC-1:** <testable statement>
- **AC-2:** <...>
- **AC-n:** Must not <anti-goal>.

## Design

### Current state | Starting point
<existing code: what this touches, with file references>
<new project: constraints found, systems it talks to, what the team already runs>

### Foundations
<new project only: the stack the decisions below settled>
<existing code: N/A: builds on the current stack>

### Components and flow
<the parts involved and how data moves between them; a diagram if it helps>

### Interfaces
<APIs, events, schemas, flags: anything other code or teams depend on>

### Data
<new or changed models, persistence, migrations, backward compatibility>

### Walking skeleton
<new project only: the thinnest slice that runs end to end>

### Tier details
<POC: what's faked or hardcoded and where the throwaway line is>
<MVP: POC, plus persistence, auth, privacy>
<Production: MVP, plus rollout, monitoring, rollback, ownership, cost>

## Decisions

### D-1: <what was decided>
- **From:** brief | spec
- **Options:** <option> — <trade-off>; <option> — <trade-off>
- **Chosen:** <option>, because <why>
- **Cost:** <what this gives up>

## Open questions
- <question> — <who answers it>
```

Keep only the tier details for this idea's tier. Decisions from the brief (its direction and "My calls") are recorded with `From: brief`.

<HARD-GATE>
No tickets and no code until the user approves the spec. That covers product code, scaffolding, dependencies or invoking `/to-tickets` or an implementation skill. Reading code and writing the spec file are fine.
</HARD-GATE>

## Red flags

| Thought | Reality |
|---|---|
| "A different direction would be better" | The brief picked it. Raise the concern as a question; don't switch. |
| "I'll ask why they want this" | The brief answers that. Read it again. |
| "I'll write the design, then list the decisions" | Decide first. The design follows from the decisions. |
| "The choice is obvious, no need to record it" | Record it with the options you rejected. The next reader won't find it obvious. |
| "There's no code, so anything goes" | Find the team's stack and constraints first. New tech needs a reason. |
| "I'll sketch the implementation" | Decisions and contracts, not code. |
| "This section doesn't apply" | Write `N/A:` and the reason. |
| "This criterion is clear enough" | If you can't say how to test it, it isn't. |
| "I know how this codebase works" | Read it. Current state comes from the code, not memory. |
| "They approved the requirements, so the spec is approved" | Approval covers only what you showed them. |
