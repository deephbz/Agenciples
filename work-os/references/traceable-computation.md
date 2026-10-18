---
purpose: Make computations whose correctness depends on data meaning inspectable step by step.
scope: What to trace, trace mode, node identity, provenance on results, the reverse bridge.
out-of-scope: Operating a pipeline; proof (rigor-and-evidence.md); homes and retention (conventions.md).
---

# Traceable computation

## Trace the computation

Trace a computation when its correctness depends on the meaning of the data
and of each step:

- **Pipelines that depend on data meaning.** Joins, filters, concatenations,
  and aggregations rely on column meaning, units, grain, and input schemas.
  One wrong assumption about a column corrupts everything downstream.
- **Deep derivations behind a simple output.** A scalar, an index, or a
  ranking comes from many steps, and the output gives no hint which step
  went wrong.

Operating the pipeline gets no trace apparatus: running a script, editing
code, an ad hoc run, a batch job that calls an analysis.

Sort the work before you build anything:

1. **Core work**: producers, consumers, schemas, final outputs. Build it.
2. **Focused validation**: behavior, schema, samples, measured performance.
   Build it (rigor-and-evidence.md).
3. **Bookkeeping**: hashes, locks, receipts, inventories, catalogues,
   dashboards, markers. Skip it unless the human asks or the product needs
   it.

Redo a stage only when its input meaning changed, its output is malformed,
or a dependency changed.

## Add a trace mode once the shape settles

In exploration, inspect state ad hoc. Once the pipeline's shape settles, give
it two modes:

- **Default mode** is clean and stable, and returns only what the caller
  needs, such as one scalar per day.
- **Trace mode** is opt-in. It returns a trace object that lets a human or an
  agent descend into any step: input snapshot or hash, rows filtered and
  why, the assumptions about each input, treatments, intermediate
  aggregates, weights, missing-value handling, config, code version. Think
  of a computation graph inspected node by node, as in Dask or PyTorch.

The second time anyone pokes at the same internal state in a notebook or a
debugger, that state earns a place in the trace. Recurring ad hoc
inspection is a missing API.

- Keep each step callable and testable on its own. If step 3 runs only
  through steps 1 to 5, debugging step 3 is archaeology.
- Emit one canonical record per meaningful run: inputs, config,
  environment, code version, timing, outcome. The record is a side effect of
  the run. It never gates the next run.
- Content-address large trace payloads and store the reference in the
  record.
- Traces are evidence: raw and append-only. Summaries derive from them
  (runtime-and-continuity.md).
- Recording after the fact costs more than constraining the surface so that
  untraced state changes cannot happen: containerized runs, no mutation over
  SSH, config-only entry points. Prefer constraint for experiment
  infrastructure.

## Nodes have identity; files are materializations

Model a pipeline as a DAG. Nodes are meaningful data states (raw, filtered,
joined, features, metrics, report). Edges are transformations. A node's
identity is its input versions, parameters, code, and environment
assumptions. A file on disk does not define it.

- A persisted intermediate is a materialized view of a node. A caller cannot
  tell a cache load from a live computation.
- The cache key encodes the node's identity. A change in input, parameter,
  code, or assumption makes a different node: invalidate it. Store the
  identity beside the file, so staleness is checkable.
- An existing file is no proof of a valid result. Stale files from old code
  under old assumptions are the classic quiet corruption of research
  pipelines.
- Materialize an intermediate for speed, inspection, or audit. The source
  bundle and the DAG regenerate any node (research-artifacts.md).

## Provenance rides on persisted results

A result that outlives the session (a figure, a metric, a table) carries the
data or a content-addressed pointer to it, the exact config, the code
version, the environment, and the anchor checked (rigor-and-evidence.md).
Hashes and revisions are required here and nowhere else. The operation that
made the result needs none.

## The reverse bridge

An API is the forward bridge from intent to calls. The reverse bridge
carries runtime outcomes back to guidance, so an agent does not guess from a
traceback which document matters. At each actionable boundary (a branch that
changes the next action, a recoverable failure, a terminal outcome), the
executable reports an outcome with a stable identifier and references to the
authoritative intent or remedy.

- Emit an outcome only when the caller's next action changes. Keep the
  branch, exit code, signal, and traceback beside it as evidence.
- Put the identifier, evidence, and guidance references in a returned
  result, an error, an event, or the run record, and render a short message
  from them. Code owns conditions and identifiers. Documents own the intent
  and the judgment-heavy remedies that have not hardened into code.
- A process cannot report after `SIGKILL` or a host loss. The supervisor
  records timeouts, signals, and missing terminal events, and attaches only
  the guidance that this evidence justifies. An unknown cause stays unknown.
- Exercise important outcomes, assert their identifiers, and check that each
  guidance reference resolves inside the source bundle. A resolved pointer
  proves nothing about the guidance itself.

When a remedy becomes frequent and deterministic, harden it into the
executable, and leave the rationale and a pointer in the document.
