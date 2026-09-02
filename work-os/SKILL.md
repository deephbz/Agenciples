---
name: work-os
description: Decision guide for research-heavy, agent-assisted engineering. Use before designing a system, API, or pipeline; naming concepts; starting or resuming implementation; deciding amend vs sibling vs stack; reviewing an interface, a change, or a research conclusion; running experiments or analyses; producing reports, notebooks, charts, or dashboards; deciding what to version, persist, or delete and where it lives; deciding whether a spec lives in docs, types, or scripts; judging whether tests, linters, or CI fit the stage; or setting up agent workflows, skills, or memory. Routes each task to its smallest applicable playbook.
---

# Work OS

Agents made implementation cheap. The scarce resources are now
human-governed intent, concept clarity, investigability, and continuity.
Eight principles serve those four, and one meta-principle sets how strictly
each applies. The references are the authority; the statements below are
their shortest complete form.

## Principles

**Stage calibration (meta).** Before choosing practices, infer or ask the
project's lifecycle stage (shaping, exploration, consolidation, hardening,
sharing), declare it in the evergreen doc, and re-evaluate at milestones,
per component rather than per repository. Every practice has a stage
window: tests and abstraction at consolidation, linters and CI at sharing,
glue code correct at exploration and debt afterwards. Stage lowers the
formality of every other principle's demands, never their existence.

### Human-governed intent

1. **Ontology-first design.**
   - Write the glossary of the domain's irreducible nouns, verbs, and
     relations before designing APIs or generating code. Encode
     behavior-affecting distinctions in types, not prose. Let the smallest
     safe, stable public contract emerge from that ontology, then ship
     through it and improve behind it.
   - Interfaces carry human intent and implementations are the agents'
     search space. Review interfaces harder than implementations. Tests and
     evaluation criteria are part of the source: an agent loop without an
     evaluator optimizes an unstated objective.
   - Design agent-facing interfaces for capable reasoning, not as workflow
     forms: few semantically stable verbs, schema only for the coordinates
     safe composition needs, goals and constraints in prose, locking and
     retries hidden behind the contract. Review is a risk-based choice the
     assigner makes; a harness blocks execution mechanically only where a
     demonstrated invariant requires it.
   - Draw component boundaries by semantic cohesion first, change velocity
     second, non-functional constraints third, except that an irreversible
     constraint is co-equal with cohesion.
2. **Governing intent and scope.** Attach a small governance envelope
   (purpose, scope and responsibility, invariants, non-goals, status) at the
   artifact's local boundary in its native representation, stating only what
   implementation cannot express. Broadening, repurposing, or moving the
   envelope is an escalation for human review; implementation inside it is
   free.

### Concept clarity

3. **Single source of truth, literate expression.**
   - Every fact, spec, or procedure has exactly one authoritative home with
     pointers in both directions; if a doc and code both hold it, one is
     already stale. That home migrates as work matures: from docs and
     diagrams into types and public APIs as interfaces stabilize, and from
     SOP to script to library as procedures harden, because code is
     executable and testable where prose is only reviewable.
   - Natural and programming language are one medium to an agent. Choose
     per statement: code where a compiler, type checker, or test verifies it
     cheaply; prose where it says the same thing shorter or judgment is
     required.
   - Durable language is judged by description length: the same information
     in the fewest, plainest words or constructs. Short sentences, one
     meaning per term, Simplified Technical English as the prose register,
     names held to the same standard.
   - Interleave prose with code. Maintainer material that is short and
     code-adjacent lives in the source; a separate maintainer document
     exists only when it is long or cross-cutting. User-facing documents
     state what, why, and how.
   - Write from the audience's accepted baseline and the final accepted
     state, never from the sequence of agent turns. Residue test: if a
     rejected intermediate state had never existed, would this sentence,
     comment, test, or compatibility path still be needed? If not, it goes
     to historical evidence at most.

### Investigability

4. **Traceable computation.**
   - Scope: computations whose correctness depends on the meaning of the
     data and of each step. Semantic ETL and analysis pipelines where joins,
     filters, and aggregations rely on column semantics, and deep multi-step
     derivations behind a simple output. Operating the pipeline (running a
     script, editing code, an ad hoc analysis run) is out of scope.
   - In-scope computations expose a clean default output and an opt-in trace
     that lets a reader descend into any step: inputs, semantic assumptions,
     treatments, intermediate state, config, code version, like inspecting a
     Dask or PyTorch graph node by node. Recurring ad hoc inspection of the
     same state is a missing trace API.
   - A pipeline node's identity is semantic (input versions, parameters,
     code, environment). A persisted intermediate is a materialized view;
     "the file exists" is not "the result is valid". Raw records are
     authoritative, summaries are derived.
   - Provenance rides on persisted results, not on operations. A result
     that outlives the session carries its data or pointer, config, code
     version, environment, and the anchor checked. Hashes, revisions,
     receipts, and catalogues are administrative bookkeeping: produce them
     only when part of the product or when asked, and never redo semantic
     work because bookkeeping is stale.
5. **Verified claims.**
   - A trace, a document, a memory, or a test that restates a declaration is
     not proof. Every important claim has an external anchor: a prediction
     written before the run, a spot-check, a golden output, a benchmark, a
     property check, an independent reimplementation, or explicit human
     review. Where none is feasible yet, the artifact says so and the claim
     is provisional. Formality scales with stage; existence does not.
   - Every check and every compatibility path must name the plausible
     failure it uniquely detects. Do not test defaults, constants, mappings,
     types, or delegation. Compatibility protects an accepted contract, a
     verified consumer, or an explicit migration, nothing else.
   - Evidence from unchanged code, inputs, tools, and environment stays
     valid. Rerun only for a named delta, a freshness need, a prior failure,
     known nondeterminism, or a mandatory integration gate.

### Continuity

6. **Artifact-first, backend-first.**
   - Work starts from a written problem artifact, passes through a reviewed
     plan whose idea and data diagrams are authored before development and
     refreshed at milestones, and ends with a result artifact. Chat is
     transport. An artifact counts as thinking only if it adds connections,
     tensions, or claims not already in its sources.
   - The canonical form of a result is machine-operable: data or a pointer
     to it, config, code version, interpretation. Visuals are renderings; a
     chart whose dataframe is gone is a screenshot.
   - One authoritative record serves three consumers through derived views.
     Model-facing content is validated, decision-complete, and economical
     with context. Machine-facing records keep exact state, provenance,
     versions, and receipts. Human-facing views exploit perception:
     hierarchy, disclosure, comparison, visualization, sound. Views are
     tested independently and never become a competing authority. Backends
     own the first two faces, frontends the third.
   - Version the full source bundle, including the natural-language
     instructions needed to reproduce. Persist final artifacts in one
     canonical home. Treat intermediates and retries as disposable.
7. **Intent-preserving change composition.** Before editing, resolve the
   exact base revision and inspect pending changes for conceptual overlap;
   amend for the same intent, create a sibling for independent intent, stack
   for dependent intent, escalate for competing intent. Review the isolated
   contribution against its base and dependency frontier and the integrated
   result separately, and rewrite each change toward its final accepted
   design, applying the residue test to code, tests, and compatibility
   paths.
8. **Agent continuity.**
   - Begin from durable artifacts and end by writing back. Keep a stable
     task doc, an append-only journal (historical evidence), and a curated
     evergreen doc (working context: declared stage, decisions in force,
     status, blockers, next steps). A solved problem still in the evergreen
     doc is a bug.
   - Keep three information classes apart. Historical evidence records what
     a source said and may be wrong. Working context records what still
     matters and is corrected in place. Assessments are recomputable
     inferences carrying provenance, freshness, and uncertainty. Never
     destroy evidence to save a summary; never present an assessment as an
     observation.
   - Anything that persists as context (skills, memories, ontologies,
     governance) is proposed by agents and ratified by a human. Continuity
     decays: impose iteration budgets, re-anchor against the problem
     artifact, prune on contradiction, prefer boring primitives (filesystem,
     git, markdown, SQLite) to bespoke scaffolds.
   - Semantic continuity is not runtime continuity. Work identity and
     context lineage outlive any model invocation, process, host, or
     conversation; a restart or model change does not create a work
     boundary.

## Scenario routing

Read the reference for the scenario at hand before doing the work. Load at
most what the task needs. Tasks often span two scenarios; read both.

| Scenario | Read |
|---|---|
| Kicking off or joining work; deciding whether tests, linters, CI, refactors, or abstraction are appropriate yet | [references/stage-calibration.md](references/stage-calibration.md) |
| New system or API design; naming concepts; schema design; component boundaries; agent-facing interfaces; review and gate policy; reviewing an interface | [references/domain-modeling.md](references/domain-modeling.md) |
| Creating or materially editing a durable document, module, script, notebook, or interface; defining purpose, scope, invariants, non-goals; deciding whether a change moves a responsibility boundary | [references/governing-intent.md](references/governing-intent.md) |
| Deciding where a spec or procedure lives (doc, diagram, type, script); trimming docs after APIs stabilize; hardening an SOP into scripts; writing docstrings, comments, design notes, or change descriptions; user-facing vs maintainer-facing docs | [references/source-allocation.md](references/source-allocation.md) |
| Designing or refactoring a data or computation pipeline; debugging "why is this number wrong"; cache and materialization semantics; deciding what to log or persist; deciding whether bookkeeping is warranted | [references/traceable-computation.md](references/traceable-computation.md) |
| Deciding what counts as proof; whether a test, check, or compatibility path is worth its cost; whether to rerun evidence; reviewing a research conclusion | [references/verified-claims.md](references/verified-claims.md) |
| Research work: framing a question, running experiments, analyzing data, writing up results, producing charts, notebooks, reports, dashboards; deciding what to version, persist, or discard and where; variants vs retries | [references/research-artifacts.md](references/research-artifacts.md) |
| Starting or resuming implementation; identifying semantic overlap; amend vs sibling vs stack; reviewing concurrent or base-relative work | [references/semantic-changes.md](references/semantic-changes.md) |
| Agent workflow setup: skills, memory files, CLAUDE.md or AGENTS.md content; multi-session or multi-agent continuity; work identity across restarts | [references/agent-continuity.md](references/agent-continuity.md) |

## Provenance

Distilled from personal "Work OS" essays. The ontology, artifact,
source-of-truth, traceability, stage, and continuity principles were
cross-checked against a methodology review of about 80 sources mapping them
to established lineages: Parnas, Evans (DDD), Ousterhout, Brooks, Knuth,
Luhmann, Hamming, Hunt & Thomas (DRY), Shape Up, Dapper/OpenTelemetry, W3C
PROV, and the emerging context-engineering canon. Change composition,
governing intent, literate expression, and the traceability scope rule are
syntheses from direct agentic workflow reflection, the last one informed by
forward-implementation-first; their external literature review is pending.
