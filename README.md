# Agenciples

**Principles for agents and for the humans who design their harnesses.**
Agenciples has two audiences: agent-facing instructions, progressively
disclosed as an always-on trigger map, a routing skill, and scenario
playbooks; and human-facing guidance for shaping agentic work systems
without mistaking tool activity for useful work.

## Why

Agents made implementation cheap. What they did not make cheap:

- **Human-governed intent** — purpose, scope, responsibility, invariants, and
  non-goals still require judgment; without them agents optimize the wrong
  thing quickly.
- **Concept clarity** — vague ontologies turn into thousands of lines of
  confidently wrong code, fast.
- **Investigability** — opaque stacks let agents run things they cannot
  help you debug.
- **Continuity** — transient chat answers make agentic work fast but
  forgetful; the organization accumulates nothing.

The agent-facing package encodes eight principles grouped under those four
scarcities, plus a meta-principle (stage calibration) that sets how strictly
each applies. Each carries the failure modes that show up in practice: a
trace is not a verification; tracing the operation instead of the
computation; summaries corrupt, keep raw; model-generated memory needs a
human gate; a doc that duplicates stabilized code is already stale; tests
written before the shape exists are tautological; rejected intermediate
designs leak into current narratives; superseded retries become a permanent
artifact graveyard; scope changes happen silently.

None of this is new, deliberately. Ontology-first design, artifact-first
work, single source of truth, traceable computation, stage calibration, and
agent continuity map onto long-established lineages: Parnas information
hiding, Domain-Driven Design, Ousterhout's deep modules, Hunt & Thomas's DRY,
Knuth's literate programming, Luhmann's Zettelkasten, Hamming, Basecamp's
Shape Up, distributed tracing, W3C PROV, and the emerging context-engineering
canon. Governing intent, intent-preserving change composition, literate
expression, and the traceability scope rule distill current workflow
reflection; their external literature review is pending. The contribution
is the packaging: methodology shaped for consumption by humans *and* agents
sharing one harness, while keeping their different needs explicit.

## Designing the harness around the human

Agent visibility, throughput, tool calls, and cost are useful signals, but
they are not the product objective. Start from the human control and decision
loop: in what situation does the operator return, what decision must they
make, what evidence and still-current context make that decision sound, and
what action lets work continue? Only then choose capabilities, component
responsibilities, tools, storage, or dashboards. This keeps the system aimed
at attention quality, context recovery, and continuity of deep work rather
than maximizing visible agent motion.

The rules that follow from that stance live in the playbooks, where agents
can load them: governance envelopes close to the artifact
([governing-intent.md](work-os/references/governing-intent.md)); agent-facing
interfaces with few verbs, schema for coordinates, prose for meaning, and
review as a risk-based policy rather than a universal gate
([domain-modeling.md](work-os/references/domain-modeling.md)); one
authoritative record with model-, machine-, and human-facing views, and one
canonical home per artifact
([research-artifacts.md](work-os/references/research-artifacts.md));
baseline-relative narrative and the residue test
([source-allocation.md](work-os/references/source-allocation.md)); and the
separation of semantic continuity from runtime continuity
([agent-continuity.md](work-os/references/agent-continuity.md)). A human
designing a harness should read those five before selecting abstractions.

## Agent-facing structure (progressive disclosure)

| Level | File | When it's in context |
|---|---|---|
| 0 | [AGENTS-snippet.md](AGENTS-snippet.md) | Always — a trigger map for your global instructions file |
| 1 | [work-os/SKILL.md](work-os/SKILL.md) | When the skill triggers — the principles at their shortest complete form, plus scenario routing |
| 2 | [work-os/references/](work-os/references/) | On demand — one playbook per scenario, the authority for procedure |

The nine playbooks, one per router row:

- [stage-calibration.md](work-os/references/stage-calibration.md) — "where
  are we?": the shaping → exploration → consolidation → hardening → sharing
  ladder, stage windows for tests, linters, CI, and abstraction, premature
  vs overdue rigor.
- [domain-modeling.md](work-os/references/domain-modeling.md) —
  glossary-first ontology design, distinctions into types, the smallest safe
  contract, interfaces for capable agents, gates that encode invariants
  rather than ceremony, intent as the optimization target, the three-force
  ordering for component boundaries.
- [governing-intent.md](work-os/references/governing-intent.md) —
  governance envelopes: purpose, scope, invariants, non-goals; local
  placement; not API reference; scope change as an escalation event; review
  order envelope, surface, implementation.
- [source-allocation.md](work-os/references/source-allocation.md) — one
  home per fact, truth migrating from docs into types, natural vs
  programming language as one medium chosen by verifiability, minimal
  description length and Simplified Technical English, interleaved
  maintainer notes, user-facing vs maintainer-facing documents, writing
  from the audience's baseline, the residue test, the hardening gradient.
- [traceable-computation.md](work-os/references/traceable-computation.md)
  — scope (the computation, not the operation), the semantic /
  validation / bookkeeping split, dual-mode interfaces, semantic DAGs and
  materialization transparency, provenance on persisted results, the
  reverse bridge from runtime outcomes to guidance.
- [verified-claims.md](work-os/references/verified-claims.md) — a trace is
  not a verification, the anchor menu, predict before run, checks that
  detect independent risk, compatibility follows contracts, evidence reuse.
- [research-artifacts.md](work-os/references/research-artifacts.md) — the
  problem → plan → result chain, the transformation test, source bundle vs
  final artifact vs intermediates, supersession-based retention, diagrams
  first, backend-first dual display, audience-fit views, the three canonical
  artifact homes.
- [semantic-changes.md](work-os/references/semantic-changes.md) — exact
  product bases, dependency frontiers, amend vs sibling vs stack, isolated
  and integrated review surfaces, evergreen descriptions, rewriteable
  history, `jj` mapping.
- [agent-continuity.md](work-os/references/agent-continuity.md) — three
  information classes, start from artifacts and write back, evergreen doc
  and journal, semantic vs runtime continuity, human-gated persistence,
  re-anchoring against decay, durable primitives.

## Composition contract (maintainers)

- Level 0 is a trigger map and two invariants. It never restates a principle.
- `work-os/SKILL.md` is the only router. Each principle appears there once,
  at its shortest complete form. The references are the authority for
  procedure; when a summary and a reference disagree, fix the summary.
- Each reference owns one scenario and may point to others, but does not
  restate their rules.
- Add a router row and a playbook bullet here before adding a reference.
- This README explains rationale to humans and points into the playbooks.
  It does not carry rules the playbooks lack.

### Maintenance method

Use two tests before adding, merging, or rewriting a principle.

- **Compression ladder.** Write the principle at five rungs: one line, two
  sentences, three to five bullets, the current summary, the reference.
  Label each rung's delta as stance (a commitment a reader could not
  derive), procedure (how to carry a stance out), or padding. The natural
  altitude is the last rung whose delta is stance. State the principle in
  SKILL.md at that rung and nothing longer. A principle whose bullet rung
  still adds stance is carrying more than one principle; split it. A stance
  found only in a reference has no principle home; give it one.
- **Mutation test.** Copy the corpus, delete one section or flip one rule,
  and have a reader with no other context answer a probe question from the
  copy alone. A deletion that leaves the probe answerable from the summaries
  means the summaries carry procedure that belongs in the reference. A flip
  that no other passage contradicts means the rule has one home, which is
  correct. A flip that several passages contradict means the rule is
  duplicated. Run an unmutated control to find existing conflicts.

## Install

For Claude Code (or any harness supporting
[Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)):

```bash
# 1. Add the trigger map to your global instructions
#    (append the "Work OS" section from AGENTS-snippet.md
#     to ~/.claude/CLAUDE.md or your AGENTS.md)

# 2. Install the skill
cp -r work-os ~/.claude/skills/work-os
```

The snippet and the skill are written for every user, so they do not know
what your other instruction files already say. Before adopting them, read
your `CLAUDE.md`, `AGENTS.md`, hooks, and other skills, and remove or
reconcile any rule that duplicates or conflicts with the snippet (the
writing register and the two invariants are the usual overlaps). Each rule
should keep one home after integration.

Adapt freely — this is a starting point meant to be edited into *your*
working principles, not adopted verbatim. The one structural idea worth
keeping: match the durability of an instruction to the frequency you
need it (always-on trigger map ≪ triggered skill ≪ on-demand reference).

## License

MIT
