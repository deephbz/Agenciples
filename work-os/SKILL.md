---
name: work-os
description: Personal Work OS methodology playbooks for research-heavy, agent-assisted engineering. Use this whenever starting or reviewing non-trivial work — designing a new module/API/system, naming domain concepts, drawing component boundaries, building or debugging data/ML pipelines, deciding what to version/cache/persist, reviewing an interface or a research conclusion, running experiments or analyses, producing research outputs (reports, notebooks, charts, dashboards), or setting up agent workflows/skills/memory — even if the user never says "work os". Especially load it before writing significant code from scratch or before answering "how should I structure this".
---

# Work OS

Three principles govern all work here. They exist because agents made
implementation cheap: the scarce resources now are concept clarity,
investigability, and continuity. Everything below serves those three.

1. **Full-stack traceability** — abstractions must be investigable.
   Clean default output, plus a trace mode exposing intermediate state
   and provenance. Keep raw records and anchor important claims to an
   external verification signal; a trace or document is not proof of
   correctness.
2. **Ontology-first design** — model the domain's irreducible nouns,
   verbs, and relations before designing APIs or generating code. Good
   ontology yields a minimal public surface. Interfaces preserve human
   intent; implementations are the agents' search space — so review
   interfaces harder than implementations.
3. **Artifact-first, backend-first** — work starts from a durable
   written problem artifact and ends with a durable result artifact
   whose canonical form is machine-operable; visuals are renderings.
   Version-control the full source bundle (code, configs, prompts,
   plans, evaluators), persist final artifacts, treat intermediates as
   disposable, and expose reasoning/data lineage as reviewable graphs.

## Scenario routing

Read the reference for the scenario at hand before doing the work, not
after. Load at most what the task needs.

| Scenario | Read |
|---|---|
| Designing/refactoring a module or pipeline; debugging "why is this number wrong"; adding observability; cache/materialization semantics | [references/traceable-computation.md](references/traceable-computation.md) |
| New system or API design; naming concepts; schema design; deciding component boundaries or whether to split; reviewing an interface | [references/domain-modeling.md](references/domain-modeling.md) |
| Research work: framing a question, running experiments, analyzing data, writing up results, producing charts/notebooks/reports; deciding what to version vs persist vs discard; reviewing a research conclusion | [references/research-artifacts.md](references/research-artifacts.md) |
| Agent workflow setup: writing skills, memory files, CLAUDE.md/AGENTS.md content, multi-session or multi-agent continuity | [references/agent-continuity.md](references/agent-continuity.md) |
| Explicitly designing or reviewing an agent harness | [the canonical human-facing harness rationale](../README.md#designing-the-harness-around-the-human), then the relevant references above |

Tasks often span two scenarios (e.g. a new experiment pipeline is both
domain-modeling and research-artifacts). Read both; they are short.

## Cross-cutting invariants

These hold in every scenario, and they are where the principles
most often fail in practice:

- **Anchor to a verification signal.** A trace, a doc, or a memory is
  not truth. Code with full architecture docs and passing tests can
  still be wrong by 20000x. Every important claim needs an external
  anchor: a benchmark, a spec check, a test against known-good output,
  or explicit human review. If no anchor exists, say so in the artifact.
- **Separate evidence, working context, and assessment.** Historical
  evidence preserves source records and observations, which may be
  incomplete or wrong. Current working context records what still matters
  and is corrected or superseded as understanding changes. Assessments and
  projections record what is currently inferred; keep them recomputable and
  label their provenance, freshness, uncertainty, and derivation version.
  Never destroy evidence to save a summary, and never present an assessment
  as an observed fact.
- **Separate projections by audience.** Agent-visible content spends context
  on the meaning needed for reasoning. Machine-facing details preserve full
  structured state, provenance, versions, and receipts. Human-facing views
  use hierarchy, layout, comparison, visualization, and, when useful, sound.
  Keep all three traceable to the same authoritative records rather than
  letting a convenient projection become a competing source of truth.
- **Human quality gate on anything that persists.** Model-generated
  skills, memories, and ontologies are net-negative without human
  curation. Propose; let the human ratify before it becomes durable
  context.

## Provenance

Distilled from personal "Work OS" essays (core principles plus the
artifact-layer continuation: source bundles, intent as optimization
target, diagrammatic lineage),
cross-checked against a methodology review of ~80 curated sources
mapping each principle to established lineages: Parnas, Evans (DDD),
Ousterhout, Brooks, Knuth, Luhmann, Hamming, Dapper/OpenTelemetry,
W3C PROV, and the emerging context-engineering canon.
