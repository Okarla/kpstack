# Spec template

Fill in every section. If one doesn't apply, write `N/A:` and the reason. Keep only the tier details for this idea's tier.

```markdown
# Spec: <name>

**Brief:** <path to the brief file; "this session" if it came from the conversation>
**Tier:** POC | MVP | Production

## Requirements
- **Functional:** <behavior a user or caller can observe>
- **Non-functional:** <latency, scale, security, cost: from the brief's Depth>

## User stories
1. **US-1:** As a <actor>, I want <capability>, so that <benefit>. (AC-1, AC-2)
2. **US-2:** <...>

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
<modules built or changed, who calls whom and how data moves between them; a diagram if it helps>

### Interfaces
<module interfaces, API contracts, events, flags: anything other code or teams depend on>

### Data
<schema changes, persistence, migrations, backward compatibility>

### Security, performance, observability
- **Security:** <who can do what, sensitive data, what's exposed and to whom>
- **Performance:** <expected load, latency targets, where it could get slow>
- **Observability:** <the logs, metrics and traces that show it works once it's running>

<as deep as the tier calls for: a sentence each can be enough for a POC; `N/A:` and the reason for any that doesn't apply>

### Walking skeleton
<new project only: the thinnest slice that runs end to end>

### Tier details
<POC: what's faked or hardcoded and where the throwaway line is>
<MVP: POC, plus persistence, auth, privacy>
<Production: MVP, plus rollout, monitoring, rollback, ownership, cost>

## Testing decisions
- **Seams:** <the points tests drive the feature through, reusing existing ones where possible>
- **Coverage:** <which seam proves each acceptance criterion>
- **Modules under test:** <the modules these tests exercise>
- **Existing examples:** <tests in the codebase that work the same way; new project: the walking skeleton's first test>

## Decisions

### D-1: <what was decided>
- **From:** brief | spec
- **Options:** <option> — <trade-off>; <option> — <trade-off>
- **Chosen:** <option>, because <why>
- **Cost:** <what this gives up>

## Open questions
- <question> — <who answers it>
```

Decisions from the brief (its direction and "My calls") are recorded with `From: brief`.
