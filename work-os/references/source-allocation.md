# Source Allocation

Applies when deciding where a fact, spec, or procedure should live —
doc, diagram, type, public API, skill, script, library — and when to
move it. Lineage: DRY / single point of truth (Hunt & Thomas),
executable specification, Unix prototyping culture (prototype in
shell, rewrite in C), gradual typing, literate programming (Knuth),
spec-driven development (spec-kit).

## One authoritative home per fact

Every spec, behavior, event sequence, hierarchy, and procedure has
exactly one source of truth; every other mention is a pointer to it.
When the same fact lives in a doc and in code, one of them is already
stale — usually the doc, silently. Trimming a doc down to pointers is
not information loss; duplication is what loses information, by making
it ambiguous which copy is authoritative.

Pointers run both directions: docs name the modules, types, and APIs
that now carry their specs; code (module docs, README headers) names
the design docs and diagrams that motivate it. A reader — human or
agent — landing on either side reaches the other in one hop.

## Truth migrates as work matures

Where the source of truth lives is a lifecycle property, not a fixed
choice:

- **Shaping stage.** Everything is natural language: problem
  artifacts, design docs, diagrams with imagined type and API names.
  Docs-heavy is correct here — there is no stable implementation to
  point at.
- **After initial R&D.** Core implementations and public surfaces
  stabilize. The spec for API shape, behavior, event sequences, and
  hierarchy migrates from docs and diagrams into concrete types,
  method signatures, and public surfaces — code that is executed,
  tested, and versioned wins over prose that is merely read.
- **After migration.** Trim the doc: keep intent, rationale,
  constraints, and rejected alternatives — the things code cannot
  express — and replace duplicated spec with pointers to module, type,
  and API names. A doc that restates a type definition is stale the
  day after it is written.

Milestones and new R&D kickoffs are the natural checkpoints for this
re-allocation — the same moments the diagrams get refreshed
(research-artifacts.md). This migration is the source-of-truth view of
the lifecycle ladder in stage-calibration.md.

## One medium, different properties

For a capable agent, natural language and programming language are not
different media — reading prose and reading code are the same act.
They differ in properties:

- **Verification.** Code can be executed, tested, and type-checked;
  prose can only be reviewed.
- **Reliability.** A script performs identically on every run; a
  natural-language instruction is re-interpreted each time it is read.
- **Interpretation floor.** Frontier models handle nuanced prose;
  smaller and cheaper models follow explicit code more safely.

Allocate by these properties, not by habit. Some brittle error
handling is painful to encode in code but reliable as one prose
sentence an agent applies with judgment; a procedure that must run
identically every day should not remain prose. The source bundle is a
fluid composition of designs and implementations, of natural and
programming language — and the agent is expected to notice when the
current allocation is wrong and propose the move (human ratifies, per
the persistence gate).

## The hardening gradient

Procedures typically enter as natural language and harden with use: an
SOP or skill doc → a bash script → a python module → a compiled
library. Promote when frequency, determinism, or performance justify
the rigidity; demote back toward prose when a hardened script turns
out to encode assumptions that no longer hold. Each promotion is also
a verification upgrade — a script can be tested, a prose SOP cannot —
so the gradient doubles as the cheapest path to anchored procedures
(traceable-computation.md).
