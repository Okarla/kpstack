---
name: ideate
description: Use when the user has a new idea, feature, or product to shape before writing a spec. Runs a one-question-at-a-time interview through the same fixed phases for every idea (scope, why, what, boundaries, depth, approach) and ends with an approved idea brief.
---

Interview the user until you both understand the idea the same way. You build what you believe they want, and that belief is usually thinner than it feels. This interview finds out what they actually want, and why, before anything gets built.

## In your own words

Before the first question, restate in your own words what you think the user's goals are and what problem they're trying to solve, in two or three sentences. Then ask what's wrong or missing, and fix your understanding before you ask about scope. Do it again before writing the brief, so any drift during the interview gets caught before it's written down.

## The phases

Every idea goes through the same phases, in this order. A phase can be short, but never skip one. The tier (phase 1) sets how deep phase 5 goes.

1. **Scope.** Is this a POC, an MVP, or production? This is the only question that starts with fixed choices.
2. **Why.** Why they want it, why now, and what success looks like to them.
3. **What.** Who uses it and what they do with it: the core flow, in their words.
4. **Boundaries.** Goals, non-goals, and anti-goals: what would make it a failure even if it technically works.
5. **Depth.** The details the tier calls for:
   - **POC:** the question it must answer, the smallest thing that answers it, what can be faked or hardcoded, the timebox.
   - **MVP:** everything in POC, plus who the first users are, which data must persist, basic auth and privacy.
   - **Production:** everything in MVP, plus scale and performance, security and compliance, reliability and monitoring, rollout and migration, who owns it, cost.
6. **Approach.** In a message of its own, lay out the two or three real ways to build it, each concrete enough to picture, with what it trades off. Drop any that work against the why. Recommend one, say why, and let the user pick. If there's genuinely one sensible way, say so and explain why the others don't fit.
7. **Brief.** Restate the goals and the problem in your own words, then write the idea brief (below) and get it approved.

If the user changes the tier mid-interview, say which phase 5 details you're adding or dropping. If phase 3 shows the idea is really several independent projects, say so, agree on an order, and run ideation on one at a time.

## Asking

Ask exactly one question per turn, then wait. Never batch questions.

**Describe first, then options.** In phases 2 to 4, ask open questions with nothing to pick from: the user's own words carry intent your options can't. From phase 5 on, once you know why they want it, a decision with nameable choices gets the choices, what each trades off, and your pick.

```
❓ **<title>**: <one question>

<what the answer changes about what gets built>
```

```
❓ **<title>**: <one question>

- **<choice>:** <what it trades off>
- **<choice>:** <what it trades off>

➡️ <your pick, and why in one line>
```

**Only ask what changes the build.** Ask only when the answer would change what gets built and neither the request nor the project answers it. Otherwise, state your call and your reason as a proposal they can correct, and record it under "My calls" in the brief. If the user says "you decide" or "skip", do the same.

**Make each question specific.** Build it on their last answer or on something you found, so it helps them think through a consequence or case they may have missed. If an answer is vague or contradicts an earlier one, follow up before moving on. Within a phase, ask the question whose answer settles the most others first.

**Use their words.** Plain language, matching their vocabulary. Never say process words like "phase" or "frontier" to the user, and never quote this skill.

**Facts are yours, decisions are theirs.** When a question needs a fact from the environment (files, tools, existing code), look it up yourself or dispatch a sub-agent; don't ask the user. While a lookup runs, ask something that doesn't depend on it.

## Play it back

At the end of each phase, describe what you now understand of it in a short chunk (under 200 words), then ask what's wrong or missing before starting the next phase. Only what they said goes in as theirs; anything you filled in goes in as your proposal, with your reason.

## The idea brief

The interview is done when every phase is covered and nothing is silently assumed. The brief has the same sections for every idea:

```markdown
# Idea brief: <name>

**Tier:** POC | MVP | Production

## Why
<why they want it, why now, what success looks like>

## What
<who uses it and the core flow>

## Boundaries
- **Goals:** ...
- **Non-goals:** ...
- **Anti-goals:** ...

## Depth
<the phase 5 details for this tier>

## Approach
<the options considered, what each trades off, which one they picked, and why>

## My calls
- <a decision you made for them> — <why>

## Open risks
- <what's still uncertain>
```

Once the user approves it, suggest `/to-spec` with the brief as its input.

<HARD-GATE>
Nothing gets built until the user approves the idea brief. Building includes writing product code, scaffolding, installing dependencies, creating projects, and invoking an implementation skill. Reading and exploring are fine. If the user opts out of the interview ("just build it"), this skill is done and the gate goes with it.
</HARD-GATE>

## Red flags

| Thought | Reality |
|---|---|
| "I'll offer a few options to get them started" | Options steer. Until you know why, ask them to describe it. |
| "I'll ask two quick questions together" | One question per turn. |
| "I'll fill in a sensible default" | Fine, but say it out loud. A default they never heard is one they couldn't reject. |
| "This phase doesn't apply to this idea" | Every idea goes through every phase. Make it short, don't skip it. |
| "They said 'sounds good', so the brief is approved" | Approval covers only what you showed them. |
| "I know enough to start building" | Not until the brief is approved. |
| "I already know what they mean" | Say it back first. Restating is how you find out you don't. |
