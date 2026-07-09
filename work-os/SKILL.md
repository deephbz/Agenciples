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
   and provenance. Pipelines are semantic DAGs: cached intermediates
   are materialized views whose identity is inputs+params+code, not
   file existence.
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

Tasks often span two scenarios (e.g. a new experiment pipeline is both
domain-modeling and research-artifacts). Read both; they are short.

## Cross-cutting invariants

These three hold in every scenario, and they are where the principles
most often fail in practice:

- **Anchor to a verification signal.** A trace, a doc, or a memory is
  not truth. Code with full architecture docs and passing tests can
  still be wrong by 20000x. Every important claim needs an external
  anchor: a benchmark, a spec check, a test against known-good output,
  or explicit human review. If no anchor exists, say so in the artifact.
- **Keep raw, derive compressed.** Summaries, aggregations, and
  "lessons learned" are derived views; the raw record (data, trace,
  transcript) is the source of truth and must survive. Self-summarized
  memory measurably corrupts (100%→54% on ARC-AGI). Never destroy raw
  to save the summary.
- **Human quality gate on anything that persists.** Model-generated
  skills, memories, and ontologies are net-negative without human
  curation. Propose; let the human ratify before it becomes durable
  context.
- **Identity is semantic, not material.** A cached file existing is not
  the result being valid. A computation's identity is its inputs,
  parameters, code, and assumptions; when those change, the old
  materialization is a different (stale) thing, whatever its filename
  says.

## Provenance

Distilled from personal "Work OS" essays (core principles plus the
artifact-layer continuation: source bundles, materialization
transparency, intent as optimization target, diagrammatic lineage),
cross-checked against a methodology review of ~80 curated sources
mapping each principle to established lineages: Parnas, Evans (DDD),
Ousterhout, Brooks, Knuth, Luhmann, Hamming, Dapper/OpenTelemetry,
W3C PROV, and the emerging context-engineering canon.
