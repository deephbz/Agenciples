# Domain Modeling & Boundaries

Applies when designing a new system or API, naming concepts, defining
schemas, or deciding whether/where to split components. Lineage: Parnas
information hiding (1972), Evans *Domain-Driven Design* (ubiquitous
language, bounded contexts), Ousterhout *A Philosophy of Software
Design* (deep modules), Brooks conceptual integrity, Jackson *The
Essence of Software* (concept design).

## Ontology before API

Do not start from "what functions does this module need". Start from
"what are the irreducible concepts in this domain" — the nouns, verbs,
relations, state transitions, and constraints of the world being
modeled. Then let the public API emerge from that ontology.

Working procedure:

1. **Write the glossary first.** List candidate nouns and verbs. For
   each, one sentence on what it means and what it is *not*. If `build`
   means "construct on one machine" and `deploy` means "place a
   realized environment onto many machines, runnable", that distinction
   is an ontological commitment, not a naming preference — it must
   survive into the API.
2. **Distinctions that matter go into types, not docs.** If a sealed
   environment behaves differently from a mutable one, encode it so
   illegal states are unrepresentable. A behavioral difference living
   only in a docstring will be violated, by humans and agents alike.
   Once a distinction stabilizes into types and public surfaces, the
   code becomes the single source of truth for it — trim the doc to
   intent and pointers (source-allocation.md).
3. **Trim to a minimal public surface.** Only stable, irreducible
   concepts deserve to be public. Everything else stays internal where
   it can churn freely. Deep modules: small interface, substantial
   implementation behind it.
4. **Implementation may be messy behind a clear contract.** Mess behind
   a clean surface is debuggable and replaceable; a confused surface
   makes the mess structural.

A cheap design-time test: sketch the concept graph with candidate type
and API names as node labels — imagined names are fine before any code
exists. Programming language is still language; if the design does not
read cleanly as diagram phrases, the ontology has a flaw that generated
code will inherit and hide (see research-artifacts.md on diagrams
first).

Why this got more important, not less: agents generate thousands of
lines frictionlessly, so implementation is no longer scarce — coherent
structure is. And note the asymmetry (Tao's observation from Mathlib):
AI automates *theorems*, not *definitions*. Producing content from a
good ontology is cheap; distilling the ontology stays human work, and
cheap content actively reduces the social pressure to do it. Budget for
it deliberately.

Two refinements from practice:

- **Prescribed vs emergent ontology.** For stable, well-trodden domains
  (entities, schemas), ontology-first works directly — most entity
  vocabularies are solved problems. For decision-heavy domains, the
  bottleneck is that the *reasoning* was never captured; there is
  nothing to model yet. There, pair ontology-first with workflow-first:
  make the daily workflow emit decision traces, then distill the
  ontology from the exhaust.
- **Minimize cognitive surface, not published surface.** In
  single-project, agent-heavy code, agents read the whole codebase;
  the economics of stable published APIs matter mainly at true platform
  boundaries. The invariant that survives is: how many concepts must a
  reader hold to use this correctly. Keep *that* minimal.

## Design interfaces for capable agents

A capable agent does not need every collaboration convention encoded as a
workflow state or every task-specific detail decomposed into schema fields.
Over-modeling raises cognitive load, freezes guesses about future work into
the public contract, and makes ordinary changes require more tool calls.

- **Expose few, semantically stable verbs.** Each public operation should be
  an irreducible domain action, not a transient UI step or implementation
  convenience.
- **Structure only what safe composition needs.** Identity, target, durable
  relations, desired state transition, and concurrency/version coordinates
  belong in schema when machines must query, validate, or act on them.
- **Carry rich meaning in prose.** Goals, context, constraints, rationale,
  criteria, and case-specific instructions are often better expressed as
  agent-readable text than as a proliferating set of optional fields.
- **Hide complex correctness behind the contract.** Locking, retries,
  idempotency, graph validation, history, and receipts remain implementation
  responsibilities unless a caller must coordinate them explicitly.

This does not weaken the rule that distinctions which affect behavior belong
in types. It sharpens it: encode the distinctions required for safe,
deterministic behavior, not every distinction an agent can understand from
context. If a proposed field neither prevents an invalid action nor enables a
necessary machine query or transition, it is probably prose or a derived
projection rather than core schema.

## Separate projections by audience

One authoritative record can support different interfaces without acquiring
different meanings:

- **Agent-visible content** is concise, semantically complete for the next
  reasoning step, and deliberately economical with context.
- **Machine-facing details** preserve complete structured state, identifiers,
  versions, provenance, receipts, and other data needed for deterministic
  composition and reconstruction.
- **Human-facing views** exploit human perceptual strengths: hierarchy,
  spatial layout, comparison, visualization, animation, and, where it adds
  real signal, sound.

These are projections, not independent authorities. A terse agent result or
visual dashboard must link back to the machine-facing record and ultimately
to its evidence; a renderer must not silently invent domain state.

## Gates encode invariants, not ceremony

Review should be optional by default. The assigner asks for review when a task
is complex, high-risk, difficult to reverse, or otherwise benefits from a
second judgment; simple work proceeds directly. Do not force every task
through plan submission, approval, or equivalent states merely to standardize
the workflow.

A mechanical gate is justified only when the system must enforce a
demonstrated invariant that prose instructions and team convention cannot
safely protect. Otherwise, review remains a collaboration policy expressed in
the task context and communication flow, so teams can add scrutiny without
paying its latency and cognitive cost on every action.

## Intent is the optimization target

Agents search the implementation space cheaply: given clear intent,
constraints, tests, and an evaluator, they can iterate designs and
implementations faster than any human. What they cannot supply is the
objective function. The scarce human contribution is specifying intent:
what problem, what public contract, what invariants must hold, which
failure modes matter, which tradeoffs are acceptable, what defines
success.

- **Interfaces preserve human intent; implementations are where agents
  search.** A bad interface is not a technical inconvenience — it is
  crystallized misunderstanding, and agents will optimize diligently
  inside the wrong frame.
- **Review interfaces harder than implementations.** In review, ask:
  are the nouns and verbs right? are the contracts explicit? are the
  boundaries placed correctly? do the tests measure the real goal?
  The implementation can be regenerated, refactored, or replaced; the
  interface is where the worldview lives.
- **The evaluator is part of the source.** Tests and evaluation
  criteria are not an afterthought — they are the encoded intent that
  makes agent search safe. An agent loop without an evaluator is
  optimizing an unstated objective.

## Component boundaries: three forces, in order

Evaluate splits in this order:

1. **Semantic cohesion.** Things that are one concept in the real
   domain start together. Splitting one concept across components
   creates artificial complexity that no tooling recovers.
2. **Change velocity.** Within one domain, if part is stable and part
   is under active experimentation, split there: stable core, fast
   periphery. (Common Closure Principle: things that change together
   live together.)
3. **Non-functional constraints.** Latency, security, deployment,
   reproducibility can justify boundaries the ontology doesn't demand.

Exception that overrides the ordering: when a non-functional constraint
is effectively **irreversible** once chosen (protocol selection, data
residency, on-disk format), promote it to co-equal with semantic
cohesion. Deciding it third and late is how systems get locked into the
wrong shape.

This ordering avoids both failure modes: over-fragmenting into
meaningless micro-abstractions, and under-structuring into a blob.
