# Stage Calibration

Applies before choosing practices, tooling, or quality bars for any
piece of work — and again at every milestone, when the stage may have
changed. Lineage: Shape Up (shaping vs building), tracer bullets and
disposable prototypes (Hunt & Thomas), "make it work, make it right,
make it fast" (Beck/Cunningham), Gall's law (working complex systems
evolve from working simple ones), lean build-measure-learn.

## First, ask "where are we?"

Every practice in this Work OS has a stage where it pays and a stage
where it actively hurts. So before working, establish the project's
lifecycle stage: infer it from the artifact chain (does a problem
artifact exist? a diagram? stable types?), from the repo (tests? CI?
version tags? glue scripts?), and from how the human talks about the
work — and when the signals conflict or the stakes are high, ask.
State the inferred stage out loud ("this reads as exploration, so I'm
optimizing for insight speed and skipping test scaffolding") so a
wrong inference is cheap to correct. Record the declared stage in the
evergreen doc (agent-continuity.md) and re-evaluate it at each
milestone and each new R&D kickoff.

## A working ladder

- **Shaping.** The problem artifact is still forming: the question,
  the glossary, diagrams with imagined type and API names. Everything
  is natural language. The deliverable is a problem worth exploring.
- **Exploration.** Write glue scripts to extract data as soon as
  possible and learn from it. The goal is insight that decides the
  correct module or product shape — not the module itself. Code is
  disposable by design; speed of learning is the only metric.
- **Consolidation.** The shape is found. The ontology settles into
  types, interfaces are drawn and reviewed, and the source of truth
  migrates from docs into code (source-allocation.md).
- **Hardening.** It works; now make it right and fast: tests that
  encode the now-stable contract, error handling, performance.
- **Sharing.** Others will depend on it: linters, formatters, CI/CD,
  review gates, docs trimmed to pointers — the ceremony that makes
  collaboration cheap, applied once there is something worth sharing.

Projects are not monotone: a new R&D kickoff drops one component back
to exploration while the rest stays hardened. Stage is a property of a
component, not of the repo.

## Practices have stage windows

- **Tests.** Extensive unit tests during shaping or exploration are
  tautological: with no settled shape, tests encode guesses and then
  defend them — they verify that the code does what the code does,
  and they tax every pivot. Write them at consolidation, when the
  interface means something. This never suspends the verification
  invariant: even a throwaway script needs its anchor — a spot-check
  against known-good output, a predicted-before-run number. Stage
  lowers the *formality* of the anchor, never its existence.
- **Linters, formatters, CI/CD.** They earn their keep once the work
  has a good shape, is functional and performant, and is heading
  toward a PR. Before that they are friction spent polishing code
  that should be deleted.
- **Abstraction and reuse.** Same window as tests: extracting shared
  modules from exploration code freezes guesses. Duplicate freely
  while exploring; consolidate when the shape stops moving.
- **Glue code.** Correct at exploration, a liability after
  consolidation — the stage transition converts yesterday's virtue
  into today's debt. That transition is exactly the hardening
  gradient (source-allocation.md): schedule the rewrite deliberately
  rather than letting it happen by accretion.

## Failure modes, both directions

- **Premature rigor** is the agent default: asked for a quick data
  pull, an agent delivers a package with a test suite, type stubs,
  and CI config. It feels professional, and it strangles exploration
  — every pivot now fights the scaffolding.
- **Overdue rigor** is the human default: exploration code silently
  becomes production because nobody declared the transition, and the
  glue scripts everyone depends on have no tests, no types, no owner.

Both are the same root error — applying a practice without asking
which stage it belongs to. Declaring the stage, and declaring its
transitions, is what makes the question answerable.
