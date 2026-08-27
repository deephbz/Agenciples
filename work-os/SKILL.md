---
name: work-os
description: Personal Work OS methodology playbooks for research-heavy, agent-assisted engineering. Use this whenever starting or reviewing non-trivial work — designing a new module/API/system, naming domain concepts, drawing component boundaries, establishing an artifact's purpose/scope/invariants, starting or resuming implementation, identifying a base revision, deciding whether work should amend, split, or stack, reviewing base-relative changes, building or debugging data/ML pipelines, deciding what to version/cache/persist/delete or where artifacts should live, deciding whether a spec or procedure should live in docs, types, or scripts, judging whether tests/linters/CI fit the project's current stage, reviewing an interface or a research conclusion, running experiments or analyses, producing research outputs (reports, notebooks, charts, dashboards), or setting up agent workflows/skills/memory — even if the user never says "work os". Especially load it before writing significant code from scratch or before answering "how should I structure this".
---

# Work OS

Eight principles govern all work here. They exist because agents made
implementation cheap: the scarce resources now are human-governed intent,
concept clarity, investigability, and continuity. Everything below serves
those four.

1. **Full-stack traceability** — abstractions must be investigable.
   Clean default output, plus a trace mode exposing intermediate state
   and provenance. Keep raw records and anchor important claims to an
   external verification signal; a trace or document is not proof of
   correctness.
2. **Ontology-first design** — model the domain's irreducible nouns,
   verbs, and relations before designing APIs or generating code. Good
   ontology yields the smallest safe, stable public contract: it preserves
   caller intent, compatibility, and externally observable guarantees while
   implementations evolve behind it. Review interfaces harder than
   implementations.
3. **Artifact-first, backend-first** — work starts from a durable
   written problem artifact and ends with a durable result artifact
   whose canonical form is machine-operable; visuals are renderings.
   Version-control the full source bundle (code, configs, prompts,
   plans, evaluators), persist final artifacts, and treat intermediate
   retries/materializations as disposable unless they carry distinct
   experimental or audit value. Keep one canonical generation; while a
   candidate awaits ratification, at most keep the latest useful predecessor,
   then garbage-collect superseded runs. Expose reasoning/data lineage as
   reviewable graphs. Idea/data diagrams and the evergreen doc are first-class
   authored artifacts: diagrams agreed before development starts and refreshed
   at milestones, not drawn after the software exists.
4. **Single source of truth, fluid representation** — every fact,
   spec, or procedure has one authoritative home, and that home
   migrates as work matures: shaping-stage truth lives in docs and
   diagrams; once interfaces stabilize it moves into types and public
   APIs, and docs trim to intent plus pointers. Natural and
   programming language read the same to agents but differ in
   verifiability and reliability — allocate deliberately and
   re-allocate as procedures harden (SOP doc → script → library).
   Keep the bridge bidirectional: typed APIs carry intent into code;
   actionable runtime outcomes carry stable identifiers and pointers
   back to authoritative guidance. Physical placement also has one of three
   canonical homes: repository for reusable/versioned source, project-local
   workspace for task-specific scaffolding, shared artifact storage for durable
   or distributed outputs; scratch locations are ephemeral only.
5. **One truth, baseline- and audience-fit projections** — keep one
   authoritative semantic record, then derive separate views for each consumer
   and starting point. Recover the audience's accepted baseline before
   narrating a change; do not write from the sequence of agent turns. Rejected
   intermediate states leave no semantic residue unless their history is
   itself relevant evidence. Model-facing content is validated,
   decision-complete, and economical with context. Machine and trace records
   preserve exact structured state, provenance, versions, and receipts. Human
   interfaces use hierarchy, progressive disclosure, comparison,
   visualization, and sound when useful. A projection can change form, but it
   must not invent domain state or become a competing authority.
6. **Stage-calibrated rigor ("where are we?")** — establish the
   project's lifecycle stage before choosing practices: infer it, ask
   when ambiguous, declare it in the evergreen doc. Exploration wants
   glue scripts and fast insight to discover the right shape;
   extensive unit tests before the shape exists are tautological;
   linters, formatters, and CI/CD earn their keep once work is
   functional, performant, and heading for a PR. Stage lowers the
   formality of verification anchors, never their existence.
7. **Intent-preserving change composition** — organize implementation
   as explicit semantic changes with an exact base and declared
   dependencies. Before editing, inspect pending work for overlap in
   files, interfaces, and concepts. Amend refinements to the same
   intent; create a separate change for distinct intent; stack it when
   it requires pending work; escalate conflicting sibling designs.
   Review the isolated contribution relative to its base and dependency
   frontier, then verify it in the integration state. Rewrite each
   change toward the final accepted design, not its editing history.
   If a rejected intermediate design had never existed, any comment,
   compatibility path, test, or explanation that only refers to it is residue
   and should be removed from the current change.
8. **Governing intent and scope** — every durable artifact carries a
   lightweight governance envelope expressing why it exists, what
   responsibility it owns, and the boundaries, invariants, or non-goals future
   work must respect. This material preserves human judgment that cannot be
   reconstructed from implementation; it does not restate signatures or API
   reference. Agents may freely optimize implementation inside the envelope,
   but materially broadening, repurposing, or moving the envelope is an
   escalation event for deliberate review. Governance evolves, but never
   silently: optimize freely inside the envelope; never silently move the
   boundary.

## Scenario routing

Read the reference for the scenario at hand before doing the work, not
after. Load at most what the task needs.

| Scenario | Read |
|---|---|
| Designing/refactoring a module or pipeline; debugging "why is this number wrong"; adding observability; cache/materialization semantics | [references/traceable-computation.md](references/traceable-computation.md) |
| New system or API design; naming concepts; schema design; deciding component boundaries or whether to split; reviewing an interface | [references/domain-modeling.md](references/domain-modeling.md) |
| Research work: framing a question, running experiments, analyzing data, writing up results, producing charts/notebooks/reports; deciding what to version vs persist vs discard; distinguishing variants from retries; reviewing a research conclusion | [references/research-artifacts.md](references/research-artifacts.md) |
| Agent workflow setup: writing skills, memory files, CLAUDE.md/AGENTS.md content, multi-session or multi-agent continuity; recovering audience/baseline after episodic turns | [references/agent-continuity.md](references/agent-continuity.md) |
| Deciding where a spec or procedure lives (doc vs diagram vs types vs script); deciding repository vs project-local vs shared artifact storage; trimming docs after APIs stabilize; hardening an SOP/skill into scripts or libraries; docs↔code cross-pointers | [references/source-allocation.md](references/source-allocation.md) |
| Starting or resuming implementation; identifying semantic overlap; deciding amend vs split vs stack; reviewing concurrent or base-relative work; rewriting a change without residue from rejected intermediate designs | [references/semantic-changes.md](references/semantic-changes.md) |
| Creating or materially editing a durable document/module/script/notebook/interface; defining purpose, scope, responsibility, invariants, non-goals, or deciding whether a requested change moves a responsibility boundary | [references/governing-intent.md](references/governing-intent.md) |
| Kicking off or joining work: determining the project's lifecycle stage; deciding whether tests, linters, CI/CD, refactors, or abstraction are appropriate yet | [references/stage-calibration.md](references/stage-calibration.md) |
| Explicitly designing or reviewing an agent harness | [the canonical human-facing harness rationale](../README.md#designing-the-harness-around-the-human), then the relevant references above |

Tasks often span two scenarios (e.g. a new experiment pipeline is both
domain-modeling and research-artifacts). Read both; they are short.

## Cross-cutting invariants

These hold in every scenario, and they are where the principles
most often fail in practice:

- **Use risk-proportional change and verification.** Every added mechanism and
  check must address a current, named risk. Compatibility protects an accepted
  contract, a verified active consumer, or an explicit migration; it is not a
  precaution for obsolete or rejected designs. Before adding or repeating a
  check, name the plausible failure it uniquely detects. Do not mirror a
  declaration in a unit test, and reuse evidence when the relevant code,
  inputs, tools, and environment have not changed. The detailed compatibility
  and verification rules live in domain-modeling.md and stage-calibration.md.
- **Anchor to a verification signal.** A trace, a doc, or a memory is
  not truth. Code with full architecture docs and passing tests can
  still be wrong by 20000x. Every important claim needs an external
  anchor: a benchmark, a spec check, a test against known-good output,
  or explicit human review. If no anchor exists, say so in the artifact.
  The anchor's formality scales with stage — a spot-check in
  exploration, a test suite at hardening — but its existence does not
  (stage-calibration.md).
- **Separate evidence, working context, and assessment.** Historical
  evidence preserves source records and observations, which may be
  incomplete or wrong. Current working context records what still matters
  and is corrected or superseded as understanding changes. Assessments and
  projections record what is currently inferred; keep them recomputable and
  label their provenance, freshness, uncertainty, and derivation version.
  Never destroy evidence to save a summary, and never present an assessment
  as an observed fact.
- **Write from the audience's baseline, not the previous turn.** Agent turns are
  episodic. Before durable prose, recover the audience, its accepted starting
  state, and the final accepted state. A rejected intermediate design belongs
  in historical evidence when useful, not in current comments, compatibility
  code, or narrative merely because the agent once produced it. If the wrong
  turn had never happened, current artifacts should normally read the same.
- **Keep projections audience-fit.** Model-facing content spends context on
  the meaning needed for reasoning. Machine and trace records preserve exact
  structured state, provenance, versions, and receipts. Human-facing views use
  hierarchy, progressive disclosure, comparison, visualization, and, when
  useful, sound. Keep all three traceable to the same authoritative records.
  Test them independently, and never let a projection become competing truth.
- **One home per fact and one canonical place per artifact.** When a spec lives
  in both a doc and code, one is already stale. After behavior stabilizes into
  types and public APIs, code is the source of truth; trim docs to intent,
  rationale, and pointers, and keep docs and code pointing at each other in
  both directions. Reusable source belongs in a repository, task-specific
  scaffolding in the agreed project-local workspace, and durable/distributed
  outputs in shared artifact storage. `/tmp` and ad-hoc node-local paths are
  scratch, never authority.
- **Garbage-collect superseded execution residue.** Retain multiple artifacts
  when they represent distinct experimental variants, controls, or audit
  evidence. Ordinary retries do not gain semantic value from existing. During
  candidate promotion, keep at most the latest useful predecessor; after
  ratification, remove superseded runs.
- **Govern scope explicitly.** Durable artifacts carry a local statement of
  purpose and responsibility where implementation alone cannot preserve it.
  Do not use governance text as a duplicate API reference. Implementation may
  churn inside the envelope; materially moving the responsibility, invariant,
  audience, or non-goal boundary must be surfaced for deliberate review
  (governing-intent.md).
- **Human quality gate on anything that persists.** Model-generated
  skills, memories, ontologies, and governance changes are net-negative without
  human curation. Propose; let the human ratify before they become durable
  context.

## Provenance

Distilled from personal "Work OS" essays (core principles plus the
artifact-layer continuation: source bundles, intent as optimization
target, diagrammatic lineage, semantic-change discipline, narrative and
artifact hygiene, and governing scope). The first six principles were
cross-checked against a methodology review of ~80 curated sources mapping them
to established lineages: Parnas, Evans (DDD), Ousterhout, Brooks, Knuth,
Luhmann, Hamming, Hunt & Thomas (DRY), Shape Up, Dapper/OpenTelemetry, W3C PROV,
and the emerging context-engineering canon. Intent-preserving change
composition and governing intent/scope are current syntheses from direct
agentic workflow reflection; their external literature review is pending.