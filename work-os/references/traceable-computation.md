# Traceable Computation

Applies when designing or refactoring a data or computation pipeline,
debugging a wrong number, or deciding what to log or persist. Lineage:
distributed tracing (Dapper, OpenTelemetry), data provenance (W3C PROV,
OpenLineage), reproducible research (Claerbout/Donoho), Stripe canonical log
lines, lazy computation graphs (Dask, PyTorch).

This reference pairs with verified-claims.md. This one says where to invest
in investigability. That one says what counts as proof and what spending is
waste.

## Scope: the computation, not the operation

Traceability is for computations whose correctness depends on the meaning of
the data and of each step. Two typical cases:

- **Semantic data pipelines.** ETL and analysis pipelines where joins,
  filters, selections, concatenations, and aggregations rely on column
  semantics, units, grain, and input schemas. A wrong assumption about one
  column silently corrupts everything downstream.
- **Deep derivations behind a simple output.** A scalar, an index, a single
  ranking, produced by many steps. The output gives no hint of which step
  went wrong.

Operating the pipeline is not in scope: running a script, editing code,
an ad hoc analysis run, a batch job that calls an analysis script. These are
research ops, and they get no traceability apparatus of their own.

Classify work before building anything (after forward-implementation-first):

1. **Semantic implementation** — producers, consumers, schemas, final
   outputs. Build it.
2. **Focused validation** — behavior, schema, samples, measured performance.
   Build it (verified-claims.md).
3. **Administrative bookkeeping** — hashes, locks, receipts, inventories,
   catalogues, dashboards, markers. Skip it unless the user asks or the
   artifact is part of the product.

Missing or stale bookkeeping is never a reason to redo semantic work. Redo a
stage only when input meaning changed, output is malformed, or a dependency
changed.

## The dual-mode interface

In-scope computations get challenged at every layer: the data, the filter,
the aggregation, the metric, the framing. So they need two modes, designed
together from the start:

- **Default mode**: clean, stable, returns only what the caller needs
  (one scalar per day).
- **Trace mode**: opt-in. Returns a trace object alongside the result that
  lets a human or agent descend into any step: input snapshot or hash, rows
  filtered and why, the semantic assumptions made about each input,
  treatments applied, intermediate aggregates, weights, missing-value
  handling, config, code version. The model is a computation graph
  inspected node by node, lazy like Dask or eager like PyTorch. A reader
  must be able to descend into every step without asking the author.

The promotion rule: the second time anyone pokes at the same internal state
in a notebook or debugger, that state has earned a place in the trace
interface. Recurring ad hoc inspection is a missing API, not a debugging
habit.

Implementation notes:

- Keep low-level primitives directly callable and testable on their own,
  even when normally invoked through higher-level workflows. If the only
  way to run step 3 is to run steps 1 to 5, debugging step 3 is archaeology.
- Emit one canonical record per meaningful run: a single structured entry
  with inputs, config, environment, code version, timing, and outcome. This
  is a side-effect of running, not a gate on the next run.
- Trace payloads get big. Content-address them (hash-named files, git
  objects) and store the reference in the canonical record.
- Traces are raw and append-only. Any compressed view (summary stats, "what
  happened" notes) is derived and rebuildable from raw. Compressing then
  discarding raw is how systems quietly corrupt: measured on agents,
  self-summarization took task accuracy from 100% to 54%.
- Two complementary strategies: record everything after the fact, or
  constrain the interaction surface so untraceable state changes cannot
  happen (containerized runs, no SSH mutation, config-only entry points).
  Constraining is usually cheaper than reconstructing. Prefer it for
  experiment infrastructure.

## Semantic DAGs and materialization transparency

Model a pipeline as a DAG whose nodes are meaningful data states (raw,
filtered, joined, features, metrics, report) and whose edges are
transformations. A node's identity is **semantic**: input data versions +
parameters + transformation code + environment assumptions. It is not
whether a file currently exists on disk.

- A persisted intermediate is a *materialized view* of a DAG node. The node
  means the same thing whether computed live or loaded from cache; callers
  cannot tell the difference.
- Cache keys encode semantic identity. If inputs, parameters, code, or
  assumptions changed, it is a different node: invalidate, don't reuse.
  Store the identity alongside the materialization so staleness is
  checkable.
- "The file exists" is not "the result is valid." Stale materializations
  produced by old code under old assumptions are the classic quiet
  corruption of research pipelines.
- This bounds what to keep: materialize intermediates for speed,
  inspection, or audit; delete them freely otherwise, since the source
  bundle plus the DAG can regenerate any node (research-artifacts.md).

## Provenance rides on persisted results

Whenever a result outlives the session (a figure, a metric, a table),
persist next to it: the underlying data or a content-addressed pointer, the
exact config, the code version, the environment identity, and the
verification anchor used (verified-claims.md). A figure whose dataframe is
gone is a screenshot, not a result.

This is the only place hashes and revisions are required. They belong to the
result artifact, not to the operation that produced it.

## The reverse bridge

An API is the forward bridge: an agent turns intent into calls on typed code.
The reverse bridge should not depend on the agent reading a traceback and
guessing which document matters. At each **actionable semantic boundary** (a
branch that changes the next action, a recoverable failure, a terminal
outcome) the executable surfaces a **semantic outcome** with a stable
identifier and **guidance references** to the authoritative intent or
remediation.

- Route on meaning, not incidental control flow. Do not annotate every
  `if`. Emit an outcome when the caller's next action changes. Preserve the
  selected branch, exit code, signal, and traceback as evidence, but do not
  make agents infer the outcome from those clues alone.
- Keep the bridge structured. Put the outcome identifier, raw evidence, and
  guidance references in a returned result, error, event, or canonical run
  record; render a concise message as a projection. Code owns the conditions
  and identifiers; docs own only the intent, rationale, and judgment-heavy
  remediation that have not hardened into code.
- Cover termination outside the process. A process cannot emit a hook after
  `SIGKILL` or a host loss. The supervisor records timeouts, signals, and
  missing terminal events, then attaches only the guidance justified by
  that evidence; unknown causes stay unknown.
- Verify the route. Exercise important outcomes, assert their identifiers,
  and check that every guidance reference resolves within the versioned
  source bundle. A pointer is routing metadata, not proof that the guidance
  or the computation is correct.

When remediation becomes frequent and deterministic, harden it into the
executable and leave the document with rationale and a pointer
(source-allocation.md, the hardening gradient).

## Failure modes

- Tracing the operation: SHA-256 of every input file, git revision of every
  script run, receipts and catalogues for an ad hoc analysis.
- Treating a stale receipt as a correctness problem and replaying stages.
- A pipeline whose step 3 can only be run through steps 1 to 5.
- Cache hits keyed on file name rather than semantic identity.
- A persisted metric with no data pointer, config, or anchor next to it.
