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

## Three canonical artifact homes

Representation and physical placement are separate decisions. After deciding
what an artifact *is*, put every non-ephemeral artifact in exactly one of three
location classes:

1. **Repository — reusable, maintained, versioned source.** Libraries,
   reusable tooling, stable configuration, contracts, shared infrastructure,
   and source intended to evolve as maintained software belong in an existing
   repository or a deliberately created new repository.
2. **Project-local workspace — task-specific imperative scaffolding.** Driver
   scripts, orchestration glue, exploratory or literate notebooks, hardcoded
   experiment shells, local run manifests, and other one-off machinery belong
   under the project's agreed local directory. If the team intentionally
   versions this scaffolding, its repository becomes the canonical project
   home; otherwise do not leak it into unrelated repositories.
3. **Shared artifact storage — durable or distributed outputs.** Final reports,
   large datasets, model outputs, presentation-ready results, shared experiment
   artifacts, and data that must be concurrently accessible by workers and head
   nodes belong in the agreed object store, NFS, database, or artifact service.

> **There is no fourth durable home.**

`/tmp`, arbitrary home-directory folders, desktop paths, scratch mounts, and
node-local ad-hoc storage are valid only as ephemeral execution surfaces. They
must never become authoritative. If losing such a location would matter, the
artifact is misclassified and must be promoted into one of the three canonical
homes.

Before writing a durable file, classify it with one question: **is this
reusable source, project-specific working material, or a shared/durable
output?** The answer determines the location. Do not invent a new placement
convention because a new agent turn or experiment started.

Canonical placement does not mean permanent retention. `research-artifacts.md`
governs supersession: a correctly placed retry can still be garbage-collected
once a newer candidate is ratified.

## Make the reverse bridge executable

An API is the forward bridge: an agent turns natural-language intent
into calls on typed code. The reverse bridge should not depend on the
agent reading a traceback and guessing which document might matter. At
each **actionable semantic boundary** — a branch that changes the next
action, a recoverable failure, or a terminal outcome — the executable
should surface a **semantic outcome** with a stable identifier and
**guidance references** to the authoritative intent, rationale, or
remediation.

- **Route on meaning, not incidental control flow.** Do not annotate
  every `if` or `switch`. Emit an outcome when the caller's next action
  changes. Preserve the selected branch, exit code, signal, and
  traceback as evidence, but do not make agents infer the semantic
  outcome or relevant guidance from those low-level clues alone.
- **Keep the bridge structured.** Put the outcome identifier, raw
  evidence, and guidance references in a returned result or error,
  event, or canonical run record; render a concise message as a
  projection. Code owns the conditions and outcome identifiers. Docs
  own only the intent, rationale, and judgment-heavy remediation that
  have not hardened into code, and point back to the relevant outcome
  type or API.
- **Cover termination outside the process.** A process cannot emit a
  hook after `SIGKILL`, a host loss, or some crashes. The surrounding
  supervisor records timeouts, signals, and missing terminal events,
  then attaches only the guidance justified by that evidence; unknown
  causes stay unknown.
- **Verify the route.** Exercise important outcomes, assert their stable
  identifiers, and check that every guidance reference resolves within
  the versioned source bundle. A pointer is routing metadata, not proof
  that the linked guidance or underlying computation is correct.

When remediation becomes frequent and deterministic, harden it into the
executable and leave the document with rationale and a pointer. That is
the hardening gradient applied to the bridge itself.

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