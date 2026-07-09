# Traceable Computation

Applies when designing or refactoring modules and pipelines, debugging a
wrong number, or deciding what to log/persist. Lineage: distributed
tracing (Dapper, OpenTelemetry), observability engineering
(logs/metrics/traces), data provenance (W3C PROV, OpenLineage),
reproducible research (Claerbout/Donoho), Stripe canonical log lines.

## The dual-mode interface

Research code gets challenged at every layer: the data, the filter, the
aggregation, the metric, the framing. So important computations need two
modes, designed together from the start:

- **Default mode**: clean, stable, returns only what the caller needs
  (e.g. one scalar per day).
- **Trace mode**: returns a trace object alongside the result — input
  snapshot or hash, rows filtered and why, intermediate aggregates,
  weights, missing-value handling, config, code version.

The promotion rule: the second time anyone (human or agent) pokes at the
same internal state in a notebook or debugger, that state has earned a
formal place in the trace interface. Ad hoc inspection that recurs is a
missing API, not a debugging habit.

Implementation notes:

- Keep low-level primitives directly callable and testable on their
  own, even when normally invoked through higher-level workflows. If
  the only way to run step 3 is to run steps 1-5, debugging step 3 is
  archaeology.
- Emit one canonical record per meaningful run (the Stripe canonical
  log line pattern): a single structured entry with inputs, config,
  environment, code version, timing, and outcome. One greppable place
  beats fragments scattered across log levels.
- Trace payloads can get big. Content-address them (hash-named files,
  git objects) instead of duplicating; store the reference in the
  canonical record.

## Rules that prevent self-deception

- **Raw and append-only.** Traces are the source of truth; any
  compressed view (summary stats, "what happened" notes) is derived and
  must be rebuildable from raw. Compressing then discarding raw is how
  systems quietly corrupt: measured on agents, self-summarization took
  task accuracy from 100% to 54%.
- **A trace is not a verification.** A fully documented, fully traced,
  test-passing computation can still be wrong (a 576k-line LLM SQLite
  rewrite passed its tests and ran 20171x slow). Traceability tells you
  *what happened*; only an external anchor — a benchmark, a golden
  output, a property check, an independent reimplementation — tells you
  it was *right*. Every pipeline needs at least one such anchor, and
  the trace should record which anchor was checked.
- **Two complementary strategies.** Recording everything after the fact
  is one option; the other is constraining the interaction surface so
  untraceable state changes cannot happen at all (containerized runs,
  no SSH mutation, config-only entry points). Constraining is usually
  cheaper than reconstructing. Prefer it for experiment infra.

## Minimum provenance for any persisted result

Whenever a result outlives the session (a figure, a metric, a table),
persist next to it: the underlying data (or a content-addressed pointer
to it), the exact config, the code version (commit hash), the
environment identity, and the verification anchor used. A figure whose
dataframe is gone is a screenshot, not a result.
