---
purpose: Match each practice to maturity, and decide what counts as evidence.
scope: Stages, stage windows, anchors, checks, compatibility, evidence reuse.
out-of-scope: Pipeline traces (traceable-computation.md); change review (change-composition.md).
---

# Rigor and evidence

## Find the stage

Infer the stage of each component from its files, its repository (tests, CI,
tags, glue scripts), and how the human talks about it. Ask when the signals
conflict and the stakes are high. Say the inferred stage when it changes
what you do, so a wrong guess is cheap to correct. Check the stage again at
each milestone. Stage belongs to a component. A new research start drops
one component back to exploration while the rest stays hardened.

- **Shaping.** The problem is still forming. Everything is prose: the
  question, a glossary, diagrams with imagined type names. The output is a
  problem worth exploring.
- **Exploration.** Glue scripts pull data early to find the right shape.
  Code is disposable. Speed of learning is the metric.
- **Consolidation.** The shape is found. The ontology settles into types,
  interfaces settle, contract tests start, and truth migrates from prose
  into code.
- **Hardening.** Make it right and fast: error handling, failure-path tests,
  performance.
- **Sharing.** Others depend on it: linters, formatters, CI, review gates,
  docs trimmed to pointers.

## Practices have stage windows

| Practice | Window | Cost outside the window |
|---|---|---|
| Unit and contract tests | consolidation onward | Earlier, tests encode guesses, defend them, and tax every pivot. |
| Abstraction and reuse | consolidation onward | Earlier, shared modules freeze guesses. Duplicate freely while exploring. |
| Linters, formatters, CI | sharing | Earlier, they polish code that will be deleted. |
| Glue code | exploration | Later, it is debt. Schedule its rewrite along the hardening gradient (intent-and-interfaces.md). |
| Trace mode | consolidation onward | Earlier, it traces a shape that will change (traceable-computation.md). |

Rigor fails in two directions. Premature rigor is the agent default: a quick
data pull becomes a package with tests, type stubs, and CI, and every pivot
fights the scaffolding. Overdue rigor is the human default: exploration code
becomes production because nobody named the transition. Both apply a
practice without asking for the stage.

## Every important claim has an anchor

A trace, a document, a memory, or a passing test that restates a declaration
proves nothing. A trace tells you what happened. Only an external anchor
tells you the result is right. Anchors, cheapest first:

- a prediction written before the run;
- a spot check against a known-good output;
- a golden output or a benchmark;
- a property check;
- an independent reimplementation;
- human review of the specific claim.

When no anchor is feasible yet, mark the claim provisional. The result
record names the anchor that was actually checked. Stage lowers the
formality of the anchor: a spot check in exploration, a test suite at
hardening. Stage never removes the anchor. A throwaway script still gets a
spot check.

Write the expected result before you run. A prediction is the cheapest
anchor, it turns each run into calibration, and it separates research from
rationalization.

## A check names the failure it detects

- A check earns its cost when it detects a plausible failure that no cheaper
  check detects. Name that failure before you add or rerun the check.
- Do not test defaults, constants, static mappings, field lists, type
  declarations, or direct delegation. Such a test copies the source, so each
  edit needs two matching changes and adds no evidence. Test a default only
  when it is an accepted external guarantee, and then test its effect on the
  caller.
- Prefer existing tests and direct runtime checks: run the code, open the
  page, query the output. A request to fix or verify does not authorize new
  test files, test-only helpers, or fixtures. Propose them to the human with
  the stage and the benefit.
- Test observable behavior. Do not assert source strings, implementation
  shapes, or the existence of tests. Coverage does not measure consequence.
- A thin module with no meaningful branch, transformation, state transition,
  invariant, or failure policy needs no unit test. Check it at the boundary
  the caller sees. Unit-test deep behavior, where inputs produce
  non-obvious outputs or failures.

## Compatibility follows contracts

Preserve old behavior only for an accepted contract, a verified active
consumer, or an explicit migration. Otherwise replace the old design and
delete the obsolete path. Add no aliases, shims, fallbacks, dual reads or
writes, or legacy configuration. The age of code does not prove that a
consumer exists. Compatibility code names the contract or the consumer that
it protects.

## Evidence stays valid until an input changes

A passed check stays evidence while the code, inputs, tool versions, and
environment stay unchanged. Rerun it only for a named change, a freshness
need, a prior failure, known nondeterminism, or a mandatory integration
gate. Hashes, receipts, and inventories are bookkeeping. They say nothing
about correctness, so missing or stale bookkeeping never justifies a rerun.
