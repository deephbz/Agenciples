# Research Artifacts

Applies when framing a research question, running experiments, analyzing
data, or writing up results. Lineage: literate programming (Knuth),
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

Decide once where each artifact type lives (e.g. a notes vault for
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
that can't be named cleanly is a boundary drawn wrong. This is the
cheapest interface review available (domain-modeling.md).

**Density discipline.** The diagram is the highest-density artifact in
the chain: critical concepts and relations only. The evergreen doc may
carry slightly more (agent-continuity.md); journals carry everything.
If a detail doesn't change how a reviewer judges the work, it doesn't
belong in the diagram.

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

## Verification in research specifically

Predict before you run: write the expected result into the problem
artifact before executing the experiment. It is the cheapest possible
verification anchor, it converts every run into a calibration exercise,
and it is the difference between research and rationalization.
