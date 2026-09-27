# Early Start

Purpose: Reduce elapsed time to accepted results and useful learning.
Scope: Dividing, scheduling, and revising work in development, testing,
exploration, research, experiments, and other goal-directed work.

## Find two kinds of opportunity

A part of the work can need a final input for acceptance while needing only
preliminary information to start. Identify the inputs needed to start and
those needed to accept the result.

- **Independent work.** Start parts together when their inputs are available.
  A shared contract can remove a dependency on another implementation. An
  agreed protocol can let backend and frontend development start together.
- **Dependent work.** Start useful, reversible parts with preliminary inputs
  or explicit assumptions. A data analysis can start while a literature review
  refines the definitions. Test preparation can start before implementation;
  checks of the implementation still need the implementation.

Prefer work that remains useful across likely input changes. Split off a
small shared decision or input when it can release several parts of the work.
Wait where missing information prevents useful progress or safe correction.

## Plan and execute together

1. Define the goal, shared contracts, and enough of the plan to start useful
   work. Keep unresolved branches provisional. Before an irreversible action,
   resolve the assumptions it depends on and obtain required approvals.
   [Governing intent](governing-intent.md) owns scope and approval boundaries.
2. For each early start, state the preliminary input or assumption and the
   check needed before accepting its dependent result. Keep these with the
   work. Match the detail to the risk and stage.
3. Share useful partial results before all parts finish. Revise the
   plan within the agreed scope when new information changes what can proceed.
   Apply this to a project, an experiment, or a step within either.
4. When an input changes, find the affected work. Keep valid results, repair
   invalid results, and stop work whose expected value has disappeared.
5. Check the assumptions against the actual inputs before accepting dependent
   results. [Verified claims](verified-claims.md) owns the evidence standard.

Feedback can go both ways between parts of the work. A dependency DAG can
represent successive versions: `question_v1 -> analysis_v1 -> question_v2`.
A plan does not need to predict every later revision to support useful work now.

## Choose early starts for their elapsed-time benefit

Compare the expected time saved with coordination, human review, rework, and
competition for limited resources. Spend scarce capacity on work that controls
completion time or resolves important uncertainty. Some rework is worthwhile when the accepted
result arrives sooner. Start fewer parts early when late changes repeatedly
invalidate large amounts of work.

When evaluating an early-start policy, compare elapsed time to the same accepted
result, including preparation, waiting, human review, checks, and repair. For
exploration, compare useful learning within the same time and resource budget. Worker count
and activity alone do not measure progress. More work must stay within the
agreed scope.

## Lineage and limits

- Krishnan, Eppinger, and Whitney, [Accelerating Product Development by the
  Exchange of Preliminary Product Design Information](https://doi.org/10.1115/1.2826709)
  (1995): iterative overlapping starts downstream work with unfinished inputs
  and accommodates later changes.
- The same authors, [A Model-Based Framework to Overlap Product Development
  Activities](https://doi.org/10.1287/mnsc.43.4.437) (1997): overlap depends on
  how upstream information evolves and how downstream work responds to change.
- Prabhu, Ramalingam, and Vaswani, [Safe Programmable Speculative
  Parallelism](https://www.microsoft.com/en-us/research/publication/safe-programmable-speculative-parallelism/)
  (2010): predicted dependency values can expose parallel work.

These sources support the mechanism and its trade-offs. They do not establish
an elapsed-time gain for a particular agent workflow. That claim needs its own
comparison.
