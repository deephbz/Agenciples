# Research Artifacts

Applies when framing a research question, running experiments, analyzing
data, writing up results, deciding what to keep or delete, deciding where an
artifact lives, or designing how one record serves agents, machines, and
humans. Lineage: literate programming (Knuth),
reproducible research (Claerbout/Donoho, nbdev/Quarto), Zettelkasten
(Luhmann/Ahrens), evergreen notes (Matuschak), memex (Bush), design
doc/RFC culture, Hamming "You and Your Research".

## The three-artifact chain

Research work produces three durable artifacts, in order. Chat messages
are transport, not storage; anything that matters graduates to a file.

1. **Problem artifact** (before any work): question, context,
   motivation, desired behavior, known constraints, prior attempts,
   suspected failure modes, expected outputs. Rough is fine; it exists
   to force the thinking (writing *is* the thinking) and to give agents
   a bootstrappable context. If the problem can't be written down, it
   hasn't formed yet — writing it is the first experiment.
2. **Plan/design artifact** (before implementation): the decomposition,
   the approach, the tradeoffs considered. This is the human review
   checkpoint — reviewing a plan is cheap, reviewing 3000 lines of
   generated code is not. Practitioners converge on this independently:
   have the agent produce a reviewable design doc, not an ephemeral PR.
   The authored idea/data diagram belongs here (see below): agreed
   before development starts, refreshed at each milestone.
3. **Result artifact** (after): findings, interpretation, and the full
   provenance bundle (see below). The next round of work starts from
   this artifact, not from memory or chat scrollback.

**The transformation test.** An artifact counts as thinking only if it
contains connections, tensions, or claims that were not already in the
sources. Verbatim transcription and dutiful summarization are "research
cosplay" — they feel like work and store nothing. When producing a
result artifact, state at least: what was surprising, what this changes,
what it contradicts.

Decide once which of the three canonical homes (below) each artifact type
uses (e.g. a notes vault in shared storage for
distillation, the repo's `docs/` for artifacts coupled to code) and keep
it consistent, so humans and agents both know where to look.

## Source-to-artifact discipline

In agentic work, "source" is no longer just code. The **source bundle**
is everything required to reproduce the result: code, configs, pipeline
definitions, dependency declarations, prompts, natural-language
instructions and playbooks, experiment plans, evaluation criteria,
environment assumptions. If a natural-language instruction is required
for reproduction, it is source — version it; don't leave it in chat.
The test of a good bundle: given only this, can someone (human or
agent) rerun the pipeline, change parameters, test counterfactuals, or
extend the work?

Three lifecycles, three treatments:

- **Source bundle: version-controlled.** It is the generative harness.
- **Final artifact: persisted.** The compressed durable result (report,
  table, metrics snapshot, plots + their backend). Needs clear naming
  and a stable home; does not need code-style versioning.
- **Intermediates: disposable materializations.** Extracted tables,
  temp parquet, embeddings, debug snapshots can be huge and accidental.
  Keep them for speed, inspection, or audit; otherwise delete freely —
  source bundle + DAG can regenerate them (see
  traceable-computation.md for the semantic-identity rules that make
  this safe).

## Supersession-based retention

Artifacts survive because they represent distinct information, not because an
execution happened. Retries, failed preflights, smoke runs, and partially
corrected candidates are normally successive attempts at producing one valid
result; they are not automatically first-class history.

Use **promotion plus garbage collection** as the default lifecycle:

1. During iteration, replace obsolete retry outputs rather than accumulating
   every attempt.
2. While a serious candidate is still awaiting verification, it is reasonable
   to keep the latest useful smoke/preflight predecessor beside the candidate.
   This is a temporary two-generation transition state, not a permanent archive.
3. Once the candidate is verified or ratified, promote it to canonical and
   delete the superseded predecessor and earlier attempts.

The compact rule is: **one canonical generation; temporarily one predecessor
while promotion is unresolved.** Cleanup is part of successful execution, not
an optional destructive afterthought.

Do not apply supersession to intentionally distinct experimental conditions.
Ablations, controls, parameter sweeps, alternative implementations, benchmark
variants, and declared baselines are first-class artifacts because the
comparison itself is the experiment. Name and retain them with their condition
and provenance.

Before retaining multiple outputs, classify them explicitly:

- **Variant** — represents a distinct hypothesis, control, treatment,
  parameterization, or implementation worth comparing. Retain it.
- **Retry** — exists only because the previous attempt failed, was incomplete,
  or was superseded while pursuing the same intended result. Replace it.

Preserve failed attempts as historical evidence only when their failure is
itself useful evidence, needed for audit, or expensive/impossible to
reconstruct. Otherwise their durable residue makes later review harder.

## Diagrams first: idea DAG + data DAG

The idea/data diagram is a first-class artifact, not documentation
rendered after the software exists. It is authored, reviewed, and
agreed before development starts — it aligns the human (does the plan
cohere?), the team (are we building the same thing?), and the agents
(a graph is bootstrappable context that prose smooths over) — and it
is refreshed at every milestone and every new R&D kickoff. A
conclusion like "A beats B because it is faster" hides the path that
produced it; prose is fragile (agents omit, smooth over, hallucinate
connections) and code hides structure. The review target is the
graph, not only the narrative. Two coupled layers:

- **Idea DAG** — the epistemic structure: observations, questions,
  hypotheses, subproblems, experiments, findings, decisions, rejected
  paths, accepted claims. Edges are epistemic relations: *tests*,
  *supports*, *weakens*, *follows-from*, *part-of*. Rejected paths
  belong in the graph; they are evidence too.
- **Data DAG** — how raw data became evidence: sources, filters, joins,
  aggregations, feature calculations, tests, metrics. Edges are
  transformations. Claims are only as good as this path, and the
  killer questions are always upstream (what was filtered out? was the
  join valid? was ground truth actually ground truth?).

The diagram is an **interface, not decoration**: nodes are public
concepts, edges are morphisms, and layout/grouping/hierarchy are part
of the review surface — missing steps, unsupported claims, and
suspicious jumps become visible at a glance. The design-stage graph is
authored; during execution, keep it true by generating the lineage
view from structured metadata declared as steps run (each step
declares what data enters/exits and what claim it supports) rather
than redrawing it by hand afterward. Per backend-first: structured
enough for agents to parse, rendered for humans to review.

**Diagram language is design language.** Label nodes with concrete —
or, before implementation exists, imagined — type and API names when
apt. Programming language is still language: a design that reads well
as diagram phrases tends to yield good types, and distilling existing
code into diagram phrases exposes its weaknesses immediately — a node
that can't be named cleanly is a boundary drawn wrong, and an ontology
flaw that does not read cleanly here will be inherited and hidden by
generated code. This is the cheapest interface review available; it is
the design-time test that domain-modeling.md's ontology procedure relies
on.

**Density discipline.** The diagram carries the least detail per concept
in the chain: critical concepts and relations only. The evergreen doc
carries more, the journal everything (agent-continuity.md). If a detail
doesn't change how a reviewer judges the work, it doesn't belong in the
diagram.

## Backend-first, dual display

Humans ingest visuals; agents operate on structured text. An artifact
that exists only as a rendered view is dead to half its audience, so
the canonical form is always the machine-operable backend; the visual is
a rendering.

- A chart is not a result. The result is: dataframe (or a
  content-addressed pointer), config, metrics, environment, code
  version, assumptions, and interpretation — with the figure rendered
  from it. If the figure is lost, regenerate it; if the backend is
  lost, the result is gone.
- Prefer open, plain, tool-independent formats: markdown, CSV, SQLite,
  JSON, plain-text config. Backend-first is about *format openness*,
  not retrieval sophistication — a flat markdown corpus grepped by CLI
  satisfies it as well as an embedding stack, at a fraction of the
  complexity. Add clever retrieval only when scale forces it.
- Declarative viz specs (Vega-Lite style: data + encoding as data) give
  dual display for free — the spec is agent-operable, the render is
  human-legible. Prefer them over imperative plotting when practical.

## One record, audience-fit views

One authoritative record can serve three consumers without acquiring three
meanings:

- **Model-facing content** uses a validated schema. It is decision-complete
  for the next reasoning step and deliberately economical with context.
- **Machine and trace records** preserve exact structured state,
  identifiers, versions, provenance, receipts, and whatever deterministic
  composition, reconstruction, and QA need.
- **Human-facing views** exploit human perception: hierarchy, progressive
  disclosure, spatial layout, comparison, visualization, animation, and,
  where it adds real signal, sound.

These are views, not authorities. They can change shape on separate
schedules without changing domain semantics. Test each view independently
so an omission, a dense encoding, or a renderer defect cannot hide a
decisive fact. A terse model result or a dashboard links back to the
machine record and ultimately to its evidence; a renderer never invents
domain state.

The same split shapes systems: a backend owns the model-facing and
machine-facing faces of a record, a frontend owns the human-facing one.

## Three canonical artifact homes

Representation and physical placement are separate decisions. After
deciding what an artifact *is*, put every non-ephemeral artifact in exactly
one of three location classes:

1. **Repository — reusable, maintained, versioned source.** Libraries,
   reusable tooling, stable configuration, contracts, shared infrastructure,
   and source intended to evolve as maintained software.
2. **Project-local workspace — task-specific imperative scaffolding.**
   Driver scripts, orchestration glue, exploratory or literate notebooks,
   hardcoded experiment shells, local run manifests. If the team versions
   this scaffolding, its repository becomes the canonical project home;
   otherwise do not leak it into unrelated repositories.
3. **Shared artifact storage — durable or distributed outputs.** Final
   reports, large datasets, model outputs, presentation-ready results, and
   data that workers and head nodes must reach concurrently, in the agreed
   object store, NFS, database, or artifact service.

There is no fourth durable home. `/tmp`, arbitrary home-directory folders,
desktop paths, scratch mounts, and node-local ad hoc storage are ephemeral
execution surfaces. If losing such a location would matter, the artifact is
misclassified and must be promoted into one of the three homes.

Before writing a durable file, ask one question: is this reusable source,
project-specific working material, or a shared durable output? Do not invent
a new placement because a new agent turn or experiment started. Canonical
placement does not mean permanent retention; supersession above still
applies.
