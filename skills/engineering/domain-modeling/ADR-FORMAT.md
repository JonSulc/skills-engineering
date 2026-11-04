# ADR Format

ADRs live in `docs/adr/` and use sequential numbering: `0001-slug.md`, `0002-slug.md`, etc.
Create the `docs/adr/` directory lazily: only when the first ADR is needed.
Number a new one by scanning `docs/adr/` for the highest number and adding one.

## Template

```md
# {The decision, stated as a short sentence}

## Status

{proposed | accepted | deprecated | superseded by [ADR NNNN](NNNN-slug.md)}

## Context

{The facts and forces that made a decision necessary. Neutral: what is true, not what to do.}

## Decision

{What was decided and why, in full sentences.}

## Considered options

- **{Alternative}**: {why it was rejected}.

## Consequences

{What follows from the decision, costs included: what it gives up, what it constrains, what it leaves open.}
```

Sections (except status) can be dropped if they would be empty or add nothing. If one or more of them would be empty, consider whether the decision requires an ADR at all.

## Writing rules

- **One decision per record.** Two decisions that happen to land together get two records.
- **Decision says what and why, not how.** Step-by-step mechanism belongs in the code and its docstrings.
- **Consequences include the costs** of the trade-off.
- **Cite evidence by a tracked path** when measurements back the decision, and give the numbers that carry the argument.
- **Length:** about 250–400 words.

## Status and changes

- **proposed** while the decision still waits on evidence (an acceptance check, a benchmark). A proposed record may still be edited.
- **accepted** once agreed, and once any checks it waited on have passed. Measured outcomes go into Consequences at that point.
- **Accepted records are append-only**, except the Status line.
- **Superseding:** a decision that replaces another gets a new record, and both ADRs get links pointing to the other.

## When to offer an ADR

All three of these must be true:

1. **Hard to reverse**: changing your mind later costs something real.
2. **Surprising without context**: a future reader would wonder why, or "fix" it back to the obvious path.
3. **The result of a real trade-off**: there were genuine alternatives.

If a decision is easy to reverse, skip it: you'll just reverse it. If it's not surprising, nobody will wonder why. If there was no real alternative, there's nothing to record beyond "we did the obvious thing."

Skip the ADR when:

- **Its only job would be to prevent a regression.** Tests do that: name the test for the property it protects, and put the reason in the docstring.
- **It is temporary**, and a named plan or issue removes it.
- **It refines an existing ADR.** Amend that record instead.

### What qualifies

- **Architectural shape.** "We're using a monorepo." "The write model is event-sourced, the read model is projected into Postgres."
- **Integration patterns between contexts.** "Ordering and Billing communicate via domain events, not synchronous HTTP."
- **Technology choices that carry lock-in.** Database, message bus, auth provider, deployment target. Not every library: just the ones that would take a quarter to swap out.
- **Boundary and scope decisions.** "Customer data is owned by the Customer context; other contexts reference it by ID only." The explicit no-s are as valuable as the yes-s.
- **Deliberate deviations from the obvious path.** "We're using manual SQL instead of an ORM because X." Anything where a reasonable reader would assume the opposite. These stop the next engineer from "fixing" something that was deliberate.
- **Constraints not visible in the code.** "We can't use AWS because of compliance requirements." "Response times must be under 200ms because of the partner API contract."
- **Rejected alternatives when the rejection is non-obvious.** If you considered GraphQL and picked REST for subtle reasons, record it; otherwise someone will suggest GraphQL again in six months.
