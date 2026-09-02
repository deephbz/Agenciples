# Verified Claims

Applies whenever a result, a change, or a check is about to be trusted:
deciding what counts as proof, deciding whether a test or compatibility path
is worth its cost, and deciding whether to rerun evidence. Lineage: Popper
(falsifiability), Feynman ("the easiest person to fool"), property-based
testing, golden-master testing, Hunt & Thomas on tautological tests.

This reference pairs with traceable-computation.md. That one says where to
invest in investigability. This one says what counts as proof and what
spending is waste.

## A trace is not a verification

A fully documented, fully traced, test-passing computation can still be
wrong. A 576k-line LLM SQLite rewrite passed its tests and ran 20171x slow.
Traceability tells you what happened; only an external anchor tells you it
was right.

Every important claim has an anchor. The anchor menu, from cheapest up:

- a prediction written into the problem artifact before the run;
- a spot-check against a known-good output;
- a golden output or a benchmark;
- a property check;
- an independent reimplementation;
- explicit human review of the specific claim.

Where no anchor is feasible yet, the artifact says so and the claim is
marked provisional. The run record names which anchor was checked.

The anchor's formality scales with stage (stage-calibration.md): a spot-check
in exploration, a test suite at hardening. Its existence does not scale.

## Predict before you run

Write the expected result into the problem artifact before executing the
experiment. It is the cheapest anchor there is, it turns every run into a
calibration exercise, and it is the difference between research and
rationalization.

## A check must detect an independent risk

A check earns its cost only when it can detect a plausible failure that a
cheaper existing check cannot. Name that failure before adding or repeating
the check.

Do not write a unit test that copies a declaration from the implementation.
Tests that restate a default value, a constant, a static mapping, a field
list, a type declaration, or a direct delegation are tautological: a source
edit then requires two matching edits and adds no independent evidence. A
default value merits a contract test only when the value is itself an
accepted external guarantee, and then test its caller-visible effect.

Prefer existing tests and direct runtime checks before adding test
artifacts: run the code, open the browser, query the output. A request to
implement, fix, test, or verify something does not by itself authorize new
test files, test-only helpers, or fixtures. Creating them is a scope
expansion. Surface it with the stage and the concrete benefit named.

Where test changes are in scope, exercise observable behavior. Do not assert
source-code strings, implementation shapes, or that tests exist. Coverage is
not consequence.

Thin modules with no meaningful branch, transformation, state transition,
invariant, or failure policy need no unit tests. Verify their composition at
the caller-visible boundary. Unit-test deep behavior where inputs can
produce non-obvious outputs or failures.

## Compatibility follows contracts, not history

Existing code does not create a compatibility requirement by itself.
Preserve behavior only when it belongs to an accepted contract, has a
verified active consumer, or needs an explicit migration.

If none of these applies, replace the old design directly. Remove obsolete
paths instead of adding aliases, shims, fallbacks, dual reads or writes,
legacy configuration, or migration machinery. Do not infer a consumer from
the age or existence of code.

Compatibility code names the contract or consumer it protects. Without that
evidence it is speculative complexity, and the residue test in
source-allocation.md applies.

## Evidence stays valid until something changes

A passed check remains evidence while the relevant code, inputs, tool
version, and environment are unchanged. Rerun it only for a named delta, a
freshness need, a prior failure, known nondeterminism, or a mandatory
integration gate.

Administrative bookkeeping (hashes, receipts, inventories) is not evidence
of correctness and its absence or staleness is not a reason to rerun
semantic work (traceable-computation.md).

## Failure modes

- Treating a green suite of declaration-mirror tests as verification.
- Preserving a code path "in case" someone depends on it.
- Rerunning an unchanged pipeline because a receipt went stale.
- Skipping the anchor in exploration because "it's just a script".
- Recording an anchor in the run record that was never actually checked.
