---
purpose: Turn human intent into envelopes, ontologies, and interfaces that agents search inside.
scope: Envelope fields, scope change, review, ontology, agent interfaces, views, boundaries, where facts live.
out-of-scope: Envelope placement (conventions.md); sentence style (writing-for-readers.md).
---

# Intent and interfaces

Agents search implementations cheaply. They cannot supply the objective.
This reference turns the human's intent into artifacts that outlive the
conversation.

## The envelope

An envelope holds the durable properties of one artifact. It states only
what the implementation cannot express. Agents optimize freely inside the
envelope and never move it silently.

| Field | States |
|---|---|
| purpose | why the artifact exists, what it aims to achieve, and for whom |
| scope | what the artifact owns |
| out-of-scope | neighboring concerns it does not own, and where they live |
| objective | a quantity to improve, with its direction |
| invariant | a statement that must stay true |
| floor / ceiling | the worst acceptable value of a metric, or the most a cost may reach |
| preference order | which objective wins when two conflict |
| non-goal | an outcome the work does not pursue, even when it is reachable |
| anchor | how a property is checked |
| status | the stage, when it changes how the artifact evolves |

Purpose and scope are the minimum. Add other fields only when they clearly
help a future reader understand or check the artifact's intent and boundaries.
One sentence is often enough. Write "This module makes environments
reproducible by owning their realization." Add an explicit out-of-scope field
when a neighboring responsibility needs clarification. Never write "Takes an
integer and returns a string."
conventions.md gives the format and the placement.

Each property moves toward a check as it stabilizes. An invariant becomes a
type or an assertion. An objective becomes a benchmark with a floor. Code
may rely on the envelope: add no defensive code for inputs that its
invariants exclude.

## Scope change is an escalation

Surface the change before you proceed when the work would:

- add or change a goal, objective, floor or ceiling, preference order, or
  governing constraint (a limit set by the human or an accepted envelope);
- broaden or narrow a responsibility;
- change a public invariant or a caller assumption;
- add a public concept or an ownership boundary;
- repurpose an artifact for another audience or lifecycle;
- move a concern across modules, repositories, or homes;
- invalidate a non-goal.

These triggers apply wherever the change is written, including `context.md`
and the journal. State the current envelope, the proposal, and why the intent
or boundary needs to change. After human ratification, rewrite the envelope
to state the new intent. Do not escalate wording or implementation inside the
envelope. Do not edit an envelope to justify an implementation that left it.
When the human changes the scope, old comments stop being policy.

## Review and gates

Review is optional by default. The human asks for review when work is
complex, risky, or hard to reverse. Do not force plan submission or approval
states on every task. A harness blocks execution only to enforce a
demonstrated invariant that convention cannot protect.

Review in this order: envelope, public surface (names, types, APIs,
outputs), implementation. A wrong boundary produces thousands of lines that
are locally reasonable and globally wrong, while an implementation
regenerates cheaply. At the surface, ask: are the nouns and verbs right, are
the contracts explicit, are the boundaries well placed, and do the tests
measure the real goal? The evaluator is part of the source.

## Ontology before API

1. **Write the glossary first.** List the irreducible nouns, verbs,
   relations, state transitions, and constraints of the domain. Give each
   term one sentence on what it means and what it excludes. If `build` means
   "construct on one machine" and `deploy` means "place a realized
   environment on many machines", the API keeps that distinction.
2. **Encode behavior-affecting distinctions in types**, so illegal states
   cannot be represented. A distinction that lives only in a docstring will
   be violated.
3. **Publish the smallest safe contract** once the ontology supports it.
   Review it for intent, invariants, and observable guarantees before callers
   depend on it.
4. **Ship through the contract and improve behind it.** A deep module has a
   small interface and a substantial implementation.

Test a design by drawing its concept graph with type and API names as node
labels. Before code exists, use imagined names. A node that resists a clean
name marks a boundary drawn wrong.

Distilling definitions stays human work. Agents produce content from a good
ontology cheaply, and cheap content removes the pressure to distill one.
Budget time for it.

- In well-known domains (entities, schemas), start from the ontology. In
  decision-heavy domains, nobody has recorded the reasoning yet. Make the
  daily workflow record its decisions, then distill the ontology from them.
- Minimize the number of concepts a reader must hold. Stable published APIs
  matter most at platform boundaries. Inside one project, agents read the
  whole codebase.

## Interfaces for capable agents

Over-modeling raises the load on every reader, freezes guesses into the
contract, and adds tool calls to ordinary changes.

- Expose few verbs. Each verb is an irreducible domain action.
- Put in the schema only what safe composition needs: identity, target,
  durable relations, the desired state transition, and version coordinates.
- Carry goals, context, constraints, rationale, and case-specific
  instructions in prose.
- Hide locking, retries, idempotency, validation, history, and receipts
  behind the contract, unless a caller must coordinate them.

Encode the distinctions that safe, deterministic behavior needs. Leave in
prose what an agent can understand from context. A field that neither
prevents an invalid action nor enables a needed query or transition belongs
in prose or in a derived view.

## One record, several views

One authoritative record serves three readers:

- **Model-facing views** use a validated schema, hold what the next
  reasoning step needs, and spend little context.
- **The machine record** keeps exact state, identifiers, versions,
  provenance, and what composition, reconstruction, and QA need.
- **Human-facing views** use perception: hierarchy, progressive disclosure,
  layout, comparison, visualization, and sound where it adds signal.

Views change shape on their own schedules. A view never becomes an
authority, links back to the record, and never invents state. Test each view
on its own, so an omission or a renderer defect cannot hide a decisive fact.
A backend owns the model and machine faces; a frontend owns the human face.

## Component boundaries

Split components by three forces, in order: **cohesion** (one domain
concept stays in one component), **change velocity** (a stable core apart
from a fast periphery), and **non-functional constraints** (latency,
security, deployment, reproducibility). An irreversible constraint
(protocol, data residency, on-disk format) ranks with cohesion, because a
late decision locks the system into the wrong shape. The order prevents
both micro-fragments and blobs.

## One home per fact

Every spec, behavior, and procedure has one authoritative home. Every other
mention points to it. When a document and code hold the same fact, one copy
is already stale. Pointers run both ways: a document names the module that
carries its spec, and the module names the document that motivates it.
Trimming a document to pointers loses nothing. Duplication loses the answer
to which copy is authoritative.

## Truth migrates from prose into code

- In shaping, the truth lives in prose and diagrams.
- As interfaces stabilize, the spec for shape, behavior, event order, and
  hierarchy moves into types and signatures. Trim the document to intent,
  rationale, constraints, real alternatives, and pointers.
- Procedures harden with use: prose instruction, then shell script, then
  module, then library. Promote a procedure when frequency, determinism, or
  performance justify it. Demote it when the script encodes assumptions that
  no longer hold. Each promotion also upgrades its verification.

Re-allocate at milestones and at each new research start.

## One medium, chosen by verifiability

To an agent, prose and code are one medium with different properties. Code
runs and can be tested; prose can only be reviewed. A script runs the same
way each time; prose is reinterpreted at each read. Prose is often shorter
where judgment is needed. Small models follow code more safely. Choose per
statement: a brittle error policy can be one prose sentence, and a daily
procedure becomes a script. Propose a move when the choice is wrong.
Separate prose by concept and change velocity, as you separate code.

## Where explanation lives

- **User-facing documents** state the problem the artifact solves, why to
  use it, how, examples, and the guarantees a caller may rely on. They live
  where users look.
- **Maintainer material** states architecture, invisible constraints, and the
  reasons for a design. Short material lives in the source: the envelope and
  local comments. A separate maintainer document exists only when the
  material is long or cross-cutting.
- **Notebooks and literate reports** interleave prose and executable cells,
  so each claim stays next to its evidence.
