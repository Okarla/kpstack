---
name: to-spec
description: Use when an idea has been agreed, usually as an approved idea brief from /ideate. It needs a spec before it's split into tickets. Builds the architecture and system design step by step with the user then settles the test seams. Writes requirements, user stories and numbered acceptance criteria scaled to the idea's tier then ends with an approved spec that /to-tickets turns into tickets. Works for changes to existing code and for new projects with no code yet.
---

Turn an agreed idea into a spec someone else could build from. The idea brief says why the user wants it, what it does and which direction they picked. The spec is where the architecture gets decided. It says what must be true when it's done and how the system will make it true.

The brief picks the direction: the shape of the solution the user can picture, such as extending an existing service or building a separate worker. The spec builds the architecture within that direction, together with the user: data stores, sync or async, which component owns what, the contracts between them and how failures are handled.

## Input

Look for the idea brief in this session first. It's usually there because `/ideate` wrote it earlier in the conversation. If the user passed a path along with `/to-spec`, read that file instead.

If there's no brief in the session, ask the user for a file reference to one. Read the whole file before you start.

The brief is already approved. Don't ask again about why, what or boundaries and don't reopen the direction. If the code contradicts the brief or two parts of the brief conflict, raise it as a question. Never resolve it silently.

If the user has no brief, offer `/ideate`. If they decline, ask only what the spec can't be written without, one question at a time: the tier (POC, MVP or production), why they want it, the core flow and the direction. Put their answers at the top of the spec in place of the brief link.

## Steps

1. **Find the starting point.** If there's existing code, find what the idea touches: services, modules, schemas, APIs and how similar features are already built. This becomes "Current state", with file references. The existing architecture wins unless the brief says otherwise. If there's no code yet, read [references/new-project.md](references/new-project.md) and follow it instead.
2. **Requirements and user stories.** Derive the requirements from the brief's What, Boundaries and Depth. Number the user stories (US-1, US-2, ...) in the form "As a <actor>, I want <capability>, so that <benefit>". Cover every actor and flow in the brief, including edge and error cases. `/to-tickets` slices the work by story, so each one describes behavior someone can observe, not a layer of the code.
3. **First gate (MVP and production only).** Show the requirements and user stories. Get them approved before you design anything. For a POC, go straight to the decisions.
4. **Build the architecture with the user.** The user owns the architecture. Never decide it on your own. Build it together, one decision at a time, following [Building the architecture](#building-the-architecture).
5. **Design.** Write the design on top of those decisions, as deep as the tier calls for. Write decisions and contracts, not code: "orders-service publishes `order.cancelled` with `id` and `reason`", not the function that emits it. If the design depends on one of the brief's "My calls", point it out so the user can confirm it.
6. **Acceptance criteria.** Write them now that the architecture is decided, so each one can name the real contracts, data and targets the design settled. Derive them from the user stories and the design.
   - **Numbered and testable:** AC-1, AC-2, ..., each one either a command with its expected output or Given/When/Then. Every anti-goal becomes a "must not" criterion.
   - **Traceability:** each story lists the criteria that prove it. Every criterion belongs to at least one story.
7. **Testing decisions.** Sketch the seams where the feature will be tested. Prefer existing seams to new ones. Use the highest seam possible: the fewer seams across the codebase the better, with one as the ideal. If a new seam is needed, propose it at the highest point you can. Name the seam that proves each acceptance criterion. Good tests check external behavior through the seam, not implementation details. Point to similar tests already in the codebase as prior art. Check with the user that the seams match what they expect.
8. **Play back and approve.** In under 200 words, say what will change, the riskiest decision and what's still open. Then ask for approval of the whole spec.

Work on the spec in the session. Once the user approves it, suggest `/to-tickets` to pick it up from the session. `/to-tickets` turns the user stories into tickets, each carrying its acceptance criteria and the seams that test them.

**Export (optional).** After approval, offer to export the spec to a markdown file so the user can continue in another session or share it. Export only the approved spec, never a work in progress. Use the path the user chooses, suggesting `docs/specs/<slug>.md` with the idea's name in kebab-case. A later session can pass that path to `/to-tickets`.

If the design shows the brief's direction can't work, stop and tell the user why. Don't switch to another direction on your own.

## Building the architecture

Walk the user through the architecture step by step so both of you understand how it fits together and why. By the end, they should be able to explain it without you.

1. **Map the decisions.** From the brief and the starting point, list the architecture decisions this idea needs. Check each area so nothing is missed:
   - **Modules:** which ones are built and which are changed.
   - **Module interfaces:** what other code calls that will change.
   - **Schemas:** new or changed tables, fields, events.
   - **API contracts:** endpoints, payloads, versioning.
   - **Interactions:** who calls whom, sync or async, in what order.
   - **Failure handling:** what happens when each interaction fails.
   - **Security:** who can do what, sensitive data, what's exposed and to whom.
   - **Performance:** expected load, latency targets, where it could get slow.
   - **Observability:** the logs, metrics and traces that show it works once it's running.
   - **Constraints only the user knows:** team rules, deadlines, systems the code doesn't show.

   Skip an area only after saying why it doesn't apply. Security, performance and observability go as deep as the tier calls for: a POC may only need a sentence each. Order the decisions so each one builds on the ones before it. For a new project, the foundations come first. Show the list and the order. Ask what's missing or out of place before you start.
2. **One decision per turn.** For each decision, explain:
   - **Why it matters:** what it affects and what depends on it.
   - **What constrains it:** facts from the code, the brief or decisions already made.
   - **The options:** two or three real ones, each with what it trades off.
   - **Your pick:** which one you recommend and why.

   Then let the user decide. If they ask a question or push back, talk it through before moving on. When a decision has one obvious answer, still present it: say your pick and why, then ask them to confirm.
3. **Play back the architecture so far.** After each decision, show in a few lines how the pieces fit together now, as text or a small diagram. If the new decision changes an earlier one, say which and why. Agree on the change before you continue.
4. **Walk through the whole.** When every decision is made, walk the user through the full architecture: the components, how a request or event flows through them and where it can fail. Ask what's wrong or missing. Only then write the design.

Record every decision under Decisions as it's made, with the options the user rejected.

## What belongs in the spec

The spec decides anything that crosses a boundary: between services, in public APIs, events or schemas. It also decides anything another team depends on. How code is arranged inside a single ticket is left to the build step.

Fill in every section of the template. If one doesn't apply, write `N/A:` and the reason. Deleting a section hides whether you considered it.

## Asking

Ask one question per turn, then wait. Every architecture decision goes to the user, as described in [Building the architecture](#building-the-architecture). For anything else, ask only when the answer changes the spec and neither the brief nor the code answers it.

Look up facts yourself (files, existing code, tools); don't ask the user for them. When a decision has choices you can name, give the choices, what each trades off and your pick:

```
❓ **<title>**: <one question>

- **<choice>:** <what it trades off>
- **<choice>:** <what it trades off>

➡️ <your pick and why, in one line>
```

## The spec

Write the spec using the template in [references/spec-template.md](references/spec-template.md). Read it when you start writing.

<HARD-GATE>
No tickets and no code until the user approves the spec. That covers product code, scaffolding, dependencies or invoking `/to-tickets` or an implementation skill. Reading code and writing the spec are fine.
</HARD-GATE>

## Red flags

| Thought | Reality |
|---|---|
| "A different direction would be better" | The brief picked it. Raise the concern as a question; don't switch. |
| "I'll ask why they want this" | The brief answers that. Read it again. |
| "I'll write the design, then list the decisions" | Decide first. The design follows from the decisions. |
| "This decision is small, I'll just pick" | Present your pick and why. The user confirms it. |
| "I'll lay out the whole architecture at once" | One decision per turn, each building on the last. |
| "They picked an option, on to the next one" | Play back how the architecture looks now first. |
| "The choice is obvious, no need to record it" | Record it with the options you rejected. The next reader won't find it obvious. |
| "There's no code, so anything goes" | Find the team's stack and constraints first. New tech needs a reason. |
| "I'll sketch the implementation" | Decisions and contracts, not code. |
| "This section doesn't apply" | Write `N/A:` and the reason. |
| "This criterion is clear enough" | If you can't say how to test it, it isn't. |
| "A few broad user stories are enough" | `/to-tickets` slices by story. Cover every actor and flow, edge and error cases included. |
| "I'll add a test seam for each layer" | Highest seam, fewest seams. One is ideal. |
| "I know how this codebase works" | Read it. Current state comes from the code, not memory. |
| "They approved the requirements, so the spec is approved" | Approval covers only what you showed them. |
