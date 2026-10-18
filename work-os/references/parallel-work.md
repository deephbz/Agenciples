---
purpose: Cut the elapsed time to accepted results with parallel work by default.
scope: Batching, fan-out, competing attempts, parallel checks, workspaces, merging, early starts, serial reasons, cost.
out-of-scope: Composing edits for review (change-composition.md); the evidence standard (rigor-and-evidence.md).
---

# Parallel work

Parallel work is the default. Serial work needs a named reason. Progress is
the elapsed time to an accepted result, or useful learning per unit of time.
Activity and agent count do not measure progress.

The evidence for this default is provisional. Concurrent engineering and
speculative parallelism support the mechanism, but no comparison yet shows
the elapsed-time gain for a particular agent workflow. Measure the gain
before you claim it.

## Batch independent calls

Send independent reads, searches, commands, and queries in one batch. Wait
only for a result that a later call needs.

## Fan out exploration

Explore several areas at once: regions of a codebase, papers, data sources,
logs. Give each subagent one bounded question, the inputs it needs, the
scope it may touch, and the shape of the answer. Ask for a compact finding
with its evidence (paths, commands, numbers), so you can merge without
rereading. A subagent that writes gets its own workspace.

## Run competing attempts side by side

When the right approach is unclear and attempts are cheap, run competing
hypotheses or implementations in parallel. Fix the evaluator and its floor
before the attempts start, so no attempt defines its own success. When the
comparison is itself a result, the attempts are variants (conventions.md).
Otherwise keep the winner, and record in the journal what the others showed.

## Verify in parallel

Run independent checks together: tests, a spot check, a reviewer agent with
no shared context, an independent reimplementation. Independent verifiers
fail in different ways, so together they cover more.

## Give concurrent edits separate workspaces

Two agents never write to one working copy. Give each a branch, a worktree,
or a scratch directory. Compose their results when you review dependencies
and compatibility (change-composition.md).

## Merge fan-out results

- Check each result against the objective and its anchor before you merge.
- Remove duplicates. Resolve each contradiction with evidence, or report it.
- Keep raw subagent output while a decision depends on it.
- Write the merged result from the accepted state. The fan-out is no part of
  the story (writing-for-readers.md).

## Overlap dependent work on explicit assumptions

A task can need a final input for acceptance and only a preliminary input to
start. A shared protocol lets backend and frontend start together. A data
analysis can start while a literature review refines the definitions. Test
preparation can start before the implementation exists.

1. For each early start, state the assumption or preliminary input it uses,
   and the check that must pass before you accept its result. Match the
   detail to the risk.
2. Prefer work that stays useful when the input changes. Split off a small
   shared decision (a contract, a protocol, a schema) when it releases
   several parts.
3. Share partial results early. Revise the plan inside the agreed scope when
   new information changes what can proceed.
4. When an input changes, find the affected work. Keep valid results, repair
   invalid results, and stop work whose value has gone.
5. Check each assumption against the actual input before you accept a
   dependent result.

Feedback can run both ways: `question_v1 -> analysis_v1 -> question_v2`. A
plan need not predict later revisions to support useful work now.

## What forces serial work

- **An input not yet available** that prevents useful progress or safe
  correction.
- **Shared mutable state** that no separate workspace can isolate.
- **An irreversible action.** Resolve its assumptions and get the approvals
  it needs first.
- **The human's review bandwidth.** The human reads one thing at a time.
  Batch questions, and queue results for review in priority order.
- **A resource limit**: rate limits, compute, one lock.

## Count the elapsed-time cost

Compare the time saved with the cost of coordination, human review, rework,
and competition for resources. Some rework is worth a sooner result. Start
fewer parts early when late changes repeatedly invalidate large amounts of
work. Spend scarce capacity on work that controls the completion time or
resolves important uncertainty. Judge a parallel policy by the elapsed time
to the same accepted result, including waiting, checks, and repair. More
parallel work stays inside the agreed scope.
