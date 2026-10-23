---
name: work-os
description: Work OS principles for one human working with many agents. Load before non-trivial design, implementation, debugging, research, parallel work, change review, writing for a reader, or agent-workflow work. Its router selects the one reference needed.
metadata:
  purpose: Complete the seven stances, and route each task to one reference.
  scope: The stances at their shortest complete form; the router.
  out-of-scope: Short stances and invariants (the snippet); procedure (the references).
---

# work-os

The always-on snippet states seven short stances and two invariants. This
file completes each stance. Each reference owns the procedure. When this
file and a reference disagree, fix this file.

## 1. The human owns intent; agents own the search

- Intent is the problem, the contract, the invariants, the acceptable
  trade-offs, and the definition of success. Envelopes and interfaces carry
  it. Implementations are the search space of agents, so review an
  interface harder than its implementation.
- An agent loop needs an evaluator. Without one, it optimizes an unstated
  objective.
- Name the domain before the API: a glossary of irreducible nouns, verbs,
  and relations. Encode behavior-affecting distinctions in types. Publish
  the smallest safe contract, then improve behind it. Distilling
  definitions stays human work; budget time for it.
- Escalate a scope change (intent-and-interfaces.md lists the triggers),
  competing intents, an exhausted iteration budget, or an irreversible
  action. Review is optional by default. A harness blocks only to enforce an
  invariant that convention cannot protect (intent-and-interfaces.md).

## 2. Durable properties, ephemeral plans

- An envelope holds the durable properties of an artifact: at least purpose
  and scope, plus out-of-scope, objective, invariant, floor or ceiling,
  preference order, non-goal, and status when they clearly help. Each
  property names its anchor, or is marked provisional, and becomes a check
  as it stabilizes.
- The problem and the objective are durable. Refresh or discard plans and
  diagrams as evidence arrives.

## 3. Rigor follows maturity

- Stages: shaping, exploration, consolidation, hardening, sharing, per
  component. Tests and abstraction start at consolidation, linters and CI
  at sharing. Stage lowers the formality of an anchor. No stage removes it.
- Each check names the failure that it uniquely detects. Compatibility
  protects an accepted contract, a verified consumer, or an explicit
  migration. Evidence from unchanged inputs stays valid.
- A computation whose correctness depends on data meaning gets a trace mode
  once its shape settles.

## 4. Parallel by default

- Elapsed time is the cost. Compute, subagents, and discarded attempts are
  cheap.
- Give each subagent one bounded question, and ask for a compact answer
  with evidence. Fix the evaluator before competing attempts start.
- Concurrent edits get separate workspaces. An early start states its
  assumption and the check that accepts its result. It may assume that an
  earlier step succeeds. Irreversible effects wait for real inputs.
- Progress is the elapsed time to an accepted result, including
  coordination, review, and rework.

## 5. One home per fact; every artifact describes itself

- Other mentions point to the home. When a document and code hold one fact,
  one copy is stale. Truth migrates from prose into types and scripts as
  work matures.
- The envelope sits in the native metadata of the artifact, sized to its
  intent. Each artifact lives in one of three storage locations. Variants
  stay; retries are replaced.
- A result is a machine-operable record with its provenance. Figures and
  dashboards are views of it.

## 6. Write from the accepted state, for the reader

- Name the reader, their starting state, and the final accepted state. Open
  with why it matters, then the answer, then the support.
- An agent reader gets full context, instructions, and pointers. A human
  reader gets a short message that stands alone. Every question or decision
  for the human carries its own context. A status report frames progress
  against the goal and gives each request one state: delivered, blocked with
  actionable alternatives, or in progress. A request is blocked when it is
  unreasonable or rests on a wrong understanding, when a better alternative
  exists, or when a real blocker stops it.
- Residue test: if a rejected intermediate state had never existed, would
  this text, code, test, or compatibility path still be needed? If not,
  remove it.
- A change carries one intent. Amend for the same intent, add a sibling for
  independent intent, stack for dependent intent, and escalate for
  competing intent. Review the isolated contribution and the integrated
  result separately.

## 7. Thin durable runtime, replaceable applications

- Design the harness from the human's decision loop. Work identity outlives
  any process, model, host, or conversation.
- Label information as evidence, working context, or assessment. Never
  destroy evidence to keep a summary.
- Boot from `context.md` and the journal, and write back at the end.
  Re-anchor against the objective. Give autonomous loops an iteration
  budget.

## Router

Read the reference for the task before the work. Load only what the task
needs. A task that spans two rows reads both.

| Task | Read |
|---|---|
| Finding a component's stage; choosing tests, linters, CI, or abstraction; deciding what counts as proof or whether to rerun; reviewing a research conclusion | [rigor-and-evidence.md](references/rigor-and-evidence.md) |
| Designing a system, API, schema, or agent interface; naming concepts; drawing boundaries; upgrading a dependency; writing an envelope; changing scope; review policy; where a contract or procedure lives | [intent-and-interfaces.md](references/intent-and-interfaces.md) |
| Dividing work; fanning out to subagents; running attempts or checks in parallel; deciding what can start early or speculatively | [parallel-work.md](references/parallel-work.md) |
| Writing or reviewing any text a person or an agent reads; briefing an agent; status updates, questions, and replies to the human | [writing-for-readers.md](references/writing-for-readers.md) |
| Designing or debugging a data or computation pipeline; "why is this number wrong"; caching; deciding what to log or persist with a result | [traceable-computation.md](references/traceable-computation.md) |
| Framing a research question; running experiments; writing up results; charts, notebooks, reports | [research-artifacts.md](references/research-artifacts.md) |
| Starting, resuming, or reviewing implementation headed for review; amend, sibling, or stack | [change-composition.md](references/change-composition.md) |
| Setting up skills, memory, instruction files, or tools; multi-session or multi-agent continuity; work identity across restarts | [runtime-and-continuity.md](references/runtime-and-continuity.md) |
| Creating a durable file; choosing where it lives; deciding what to keep or delete; `context.md` and the journal | [conventions.md](references/conventions.md) |
