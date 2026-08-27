# Governing Intent and Scope

Applies when creating or materially editing durable documents, modules, scripts,
notebooks, classes, interfaces, or other artifacts whose future maintainers need
to know why the artifact exists and what it is allowed to become.

## Core invariant

Code generation is cheap. Human judgment about purpose, scope,
responsibility, invariants, and non-goals is not. Convert that judgment into a
small **governance envelope** located with the artifact itself.

Agents may optimize implementation freely inside the envelope. They must not
silently broaden, repurpose, or move the envelope.

> **Optimize freely inside the envelope; never silently move the boundary.**

Governance is deliberate, not immutable. Scope changes are normal, but they
consume human judgment and therefore must be surfaced explicitly.

## What the governance envelope contains

The envelope should answer only questions that cannot be reliably reconstructed
from the implementation:

- **Purpose** — why this artifact exists, which problem it serves, and for whom.
- **Scope / responsibility** — what it owns, what it intentionally does not own,
  and which neighboring concerns belong elsewhere.
- **Constraints / invariants / status** — assumptions future work may rely on,
  boundaries that must remain true, or the artifact's lifecycle state when that
  materially affects how it should evolve.

Do not force all three categories when one sentence is enough. The goal is the
smallest durable statement that prevents a future agent from inventing the
boundary again.

## Put governance at the local boundary

Use the representation native to the artifact:

- **Markdown** — one to three front-matter properties or a short governing
  header. A useful default is `purpose`, `scope`, and optionally `status`.
- **Python/modules/scripts** — module-level docstring or governing comment.
- **Notebooks** — the first explanatory cell, including audience, execution
  model, section independence, and shared prerequisites when relevant.
- **Classes, functions, interfaces** — a docstring only when there is a real
  local responsibility, invariant, or non-obvious contract to preserve.

Example:

```yaml
---
purpose: User-facing interactive workflow for comparing candidate models.
scope: Owns experiment selection and presentation; training orchestration and storage layout are out of scope.
status: active
---
```

The field names are a convention, not an ontology. Use project-native names
when they are already established.

## Governance is not API reference

Do not spend durable prose restating information the source already expresses.
A capable agent can read the signature, types, implementation, and call sites.
Documentation such as "takes an integer and returns a string" is duplicated
surface area, not governance.

Prefer statements like:

- "This module owns the conceptual semantics of environment realization;
  deployment orchestration is deliberately out of scope."
- "Sections 2-6 of this notebook remain independently executable after the
  shared configuration section."
- "This document records architectural choices only; implementation notes and
  operational runbooks live elsewhere."

As behavior hardens into types and public APIs, those executable surfaces
remain the source of truth for mechanics. Governing material retains intent,
rationale, responsibility, non-goals, and invariants that code cannot express
on its own. See `source-allocation.md`.

## Scope change is an escalation event

An ordinary implementation change stays inside the existing governance
envelope. Surface the decision before proceeding when the requested work would
materially:

- broaden or narrow responsibility;
- change a public invariant or caller assumption;
- introduce a new public concept or ownership boundary;
- repurpose an artifact for a different audience or lifecycle;
- move a concern across modules, repositories, or canonical artifact homes;
- invalidate an explicit non-goal.

State the current envelope, the proposed change, and why the existing boundary
no longer fits. The operator may approve the new envelope, reject it, or redraw
the boundary. Once approved, update the governing material so future work sees
the new intent rather than the history of the discussion.

Do not escalate wording cleanup or implementation detail that leaves the
semantic envelope unchanged.

## Review order

For a durable artifact, review in this order:

1. **Envelope** — is the purpose, responsibility, audience, and invariant set
   still correct?
2. **Public surface** — do names, types, APIs, sections, and outputs faithfully
   express that envelope?
3. **Implementation** — does the artifact satisfy the surface efficiently and
   correctly?

This order follows the economics of agentic work: implementations are cheap to
regenerate; a wrong responsibility boundary can generate thousands of lines of
locally reasonable but globally incorrect work.

## Failure modes

- No stated boundary, so an agent expands the artifact until it becomes a grab
  bag.
- Defensive code for inputs that the surrounding system already guarantees
  cannot occur.
- Coupling neighboring responsibilities because no owner was declared.
- Public APIs growing to expose implementation conveniences rather than domain
  responsibilities.
- Docstrings that duplicate signatures while omitting the reason the function
  exists.
- Treating old comments as immutable policy after the human has deliberately
  changed the scope.
- Silently editing the governing statement to justify an implementation that
  escaped its original responsibility.

## Intended outcome

Human attention is paid once to establish a useful envelope, then reused by
many agent turns and future maintainers. The artifact remains free to evolve
inside its responsibility without repeatedly rediscovering what it is for or
accidentally drifting into adjacent concerns.
