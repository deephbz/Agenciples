---
purpose: Keep research reproducible, and keep its results usable by agents and humans.
scope: Durable problem and objective, optional plans and diagrams, results, source bundle, backend first.
out-of-scope: Homes and retention (conventions.md); provenance fields (traceable-computation.md); views (intent-and-interfaces.md).
---

# Research artifacts

## The problem and the objective are durable

Before the work, write the problem: the question, context, motivation,
desired outcome, constraints, prior attempts, suspected failure modes,
expected outputs, and a prediction. Rough is fine. If you cannot write the
problem, it has not formed yet, and writing it is the first experiment. The
problem and the objective persist in `context.md` or in the project's
envelope. Agents boot from them and re-anchor against them. Chat is
transport.

## Plans and diagrams are optional working material

Write a plan when the decomposition is not obvious, or when the human wants
to see the direction before the cost is spent. Keep unresolved branches
provisional, start the parts that can proceed (parallel-work.md), and
refresh or discard the plan as evidence arrives.

Draw a diagram when prose would hide the structure:

- An **idea graph** links observations, questions, hypotheses, experiments,
  findings, and decisions. Its edges are tests, supports, weakens,
  follows-from, and part-of.
- A **data graph** shows how raw data became evidence: sources, filters,
  joins, aggregations, features, metrics. The hard questions sit upstream.
  What was filtered out? Was the join valid? Was the ground truth true?

A diagram holds only the critical concepts and relations. During execution,
generate the lineage from metadata that each step declares as it runs. Do not
redraw it by hand. A diagram is a review surface: missing steps, unsupported
claims, and suspicious jumps show at a glance. A refuted hypothesis is a
finding, so it stays in the idea graph with its weakens edge. An abandoned
approach goes to the journal.

## The result

The result holds the findings, their interpretation, and their provenance.
The next round starts from it. Say what surprised you, what the result
changes, and what it contradicts. A summary that adds no connection, tension,
or claim beyond its sources stores nothing.

## Version the source bundle

The source bundle is everything needed to reproduce a result: code, config,
pipeline definitions, dependency declarations, prompts, natural-language
instructions, evaluation criteria, and environment assumptions. An
instruction needed for reproduction is source, so version it. The test:
given only the bundle, can a human or an agent rerun the work, change a
parameter, test a counterfactual, and extend it? Persist final artifacts
under a clear name in their home. Intermediates are disposable
(conventions.md).

## Backend first

Humans read visuals. Agents operate on structured text. The canonical form
of a result is its machine-operable record, and a figure is a rendering of
it. A chart whose data is gone is a screenshot. Lose the figure and you
regenerate it. Lose the backend and the result is gone.

- Use open, plain formats: markdown, CSV, SQLite, JSON. Openness of format
  matters here. A flat markdown corpus searched with grep satisfies it. Add
  clever retrieval only when scale forces it.
- Prefer declarative chart specs (Vega-Lite style) when practical. The spec
  serves agents, and the render serves humans.
