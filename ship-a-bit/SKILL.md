---
name: ship-a-bit
description: Cut one small, mature bit out of exploratory work, solve one stated problem with it, and deliver it as a clean, verified lineage on the product base.
disable-model-invocation: true
---

# Ship a bit

Exploration leaves a sprawl: open lineages, WIP revisions, uncommitted
edits, artifacts. This skill ships one **bit** of it: a small set of mature
semantic changes that solves one stated problem, consolidated, verified, and
placed directly on the product base. The rest stays exploratory and keeps
its place in the graph.

Load the `work-os` skill and read `references/semantic-changes.md` in its
directory before step 1. Every `references/` path in this skill is in the
`work-os` directory. Its terms (product base, semantic change, dependency frontier,
isolated contribution, integrated result) are the vocabulary of this skill.

## Operating rules

- **Stay VCS-neutral.** The repository can use Git, jj, or both. Detect what
  is present and use installed help as the command authority. In a repository
  with both, use one tool for every rewrite in a run.
- **Snapshot before you rewrite.** Record a recovery point before the first
  rewrite: an operation-log ID, a backup ref, or the equivalent. A pattern
  that only adds refs records the source revisions. Report it.
- **Follow repo conventions.** Branch names, message format, check commands,
  verification procedures, and review shape come from the repository and its
  records. Where none exists, ask the user once and record the answer in
  `context.md`.
- **One quality bar.** The audience of the bit does not lower or raise the
  checks.
- **The problem statement governs.** Every later decision serves the problem
  statement from step 1: scope, fixes, names, messages, audience framing,
  and follow-ups.
- **Publication needs a go.** Push, open a review, or merge only when the user
  says so.
- **Verification has no outward effects.** Checks and probes write only to
  scratch locations. Tell probe agents not to write to external tools.

## 1. State the problem and the value

Shipping costs review, rewrite, and verification time that exploration could
use. Ship a bit only when it solves a concrete problem.

1. Read the pending work, `context.md`, and the journal. Propose a
   **problem statement**:
   - **Problem**: a concrete gap between what someone needs and what exists.
     Examples: a user lacks a convenience feature, the product lacks a
     capability, data is missing, a metric is wrong.
   - **Value**: what the bit changes for whom, and how you will observe the
     change after shipping.
2. Make the statement hard to vary: one problem, every part of the bit
   serves it, and a change that serves no part of it belongs outside the
   bit. When the user explicitly requests several change groups in one bit,
   write one statement per group and name the owner they share. When the
   user chose the revisions in advance, check that they share one problem.
   If they share only a theme, list each revision's problem and let the
   user choose.
3. Wait for the user to confirm or rewrite the statement.

Done when the user confirms a statement that names the problem, its owner,
the value, and an observable result. If no concrete problem exists, stop:
the work stays exploratory.

## 2. Choose the bit

This step is read-only.

1. Resolve the product base (for example `main` or a release branch) to an
   exact revision.
2. Read the graph from the product base to every pending tip, the
   working-copy status, and the recent messages. Find the conventions that
   mark a revision as ready or as exploratory.
3. Propose the bit: the semantic changes that serve the problem statement,
   plus their declared dependencies. Default to one semantic change. A bit
   with several change groups, for example across modules, needs an explicit
   request from the user.
4. Plan the lineage. For each revision in the bit, list what it relies on
   from revisions that will stay outside the bit:
   - **Code**: APIs, features, fixes, or data introduced outside the bit.
   - **Narrative**: messages, docstrings, comments, links, and names that
     refer to a change outside the bit, or that explain the bit relative to
     it.
   - **Textual**: conflicts caused only by context lines from revisions
     outside the bit.

   Resolve each code dependency: pull its revision into the bit, re-implement
   the minimum inside the bit, or drop the dependent revision. Resolve each
   narrative dependency: cut it, reword it against the new ancestors, or
   defer it to the bit that introduces its target. Resolve each textual
   dependency by applying the revision's intent to the base text. After the
   cuts, check that each revision still delivers its value; if not, drop it.
   A dependency that none of these resolves is a blocker; report it with the
   revisions involved.
5. Wait for the user to confirm the scope and the lineage plan. Choose the
   scope yourself only when the user explicitly delegated that choice.

Done when the user confirms a scope whose every dependency is resolved or
reported as a blocker.

## 3. Clean up what the bit needs

Other agents can edit the same repository in parallel. Unrelated WIP,
uncommitted edits, and messy revisions outside the bit are not yours to
clean.

1. List the **needed cleanups**: the cleanups without which the bit cannot
   be isolated, checked, or reviewed. Examples: unsaved edits in the bit's
   files, a revision that mixes the bit with other work, a message in the bit
   that does not state its intent. A cleanup the bit does not need stays out
   of the plan.
2. Use content-preserving operations where they suffice:
   - rewrite a message without changing the revision's content;
   - save working-copy edits as a described revision;
   - park unclear work on a temporary branch or bookmark.

   Splitting a mixed revision changes the graph shape. Propose it as a
   separate item.
3. Wait for approval, then apply the plan.

Done when every needed cleanup is applied, and each touched tip's file
snapshot equals its snapshot before cleanup. Compare snapshots with the tool
(tree identity in Git, an empty diff between the two revisions in jj). An
empty list is a valid result.

## 4. Bring the bit to the bar

1. Run the repository's checks on the bit: linter, formatter, tests, and the
   documentation convention (for example module-level docstrings). Remove
   exploratory residue inside the scope with the residue test from
   `references/writing-for-readers.md`.
2. Place every fix by the repository's review convention:
   - **fold** it into the revision that introduced the defect, so each
     semantic change stays in one revision; or
   - **separate** it as its own revision, so review sees the fix alone.
3. Place every edit outside the scope the same way: fold it into the revision
   that owns that module, or keep it as a separate revision outside the bit.
   An out-of-scope edit never enters a revision inside the bit. A scope
   change goes back to the user.

Done when each revision in the bit states one intent and passes every check,
so review and bisection work per revision, and every edit made in this step
belongs to exactly one revision.

## 5. Shape the final graph

Pick the pattern by repository or user preference. Ask when neither exists.

- **Reorder.** Rewrite the graph in place so the bit's revisions are the
  direct descendants of the product base and the exploratory work sits on
  top of them.
- **Duplicate.** Copy the bit's revisions and rebase the copies onto the
  product base. The original graph stays unchanged.
- **Split and mega-merge.** Move the bit into its own lineage on the product
  base. Then merge that lineage back where the bit came from, so the merge
  revision's snapshot equals the snapshot at the end of step 4.

```
Reorder          Duplicate              Split and mega-merge
base─bit─wip     base─bit'              base─bit────┐
                   └─wip─bit─wip          └─wip────merge─wip
```

After the move, re-read every revision in the bit against its new ancestors.
Rewrite each message, name, docstring, and comment that refers to a change
that is no longer an ancestor. Write each message from the product base and
the problem statement, never from the exploration path.

Then check both surfaces from `semantic-changes.md`:

- **Isolated contribution.** The bit, on the product base alone, passes every
  check. A bit stacked on another unpublished bit is checked on that bit,
  its dependency frontier.
- **Integrated result.** The exploratory tip's snapshot equals its snapshot
  at the end of step 4, unless the user approved a difference.

Done when both checks pass on the final revisions and no text in the bit
refers to a change outside its ancestry.

## 6. Verify the behavior

A clean, mergeable graph does not show that the bit works. Find the
verification procedures that apply: user instructions, project records
(`context.md`, the journal), and repository guardrails (CI configuration,
integration and end-to-end suites, verification skills or scripts).

1. From the net delta of the bit, write the behavior changes you expect
   before you run anything.
2. Run every applicable procedure on the bit's tip. Rerun tasks or pipelines
   that the delta affects. When no procedure covers the delta, write a
   minimal one. Keep its script and results with the verification record,
   outside the bit, unless the user asks to add it to the repository. When
   the bit changes agent guidance (skills, instructions), probe fresh agents
   on the bit and on a control without it, more than once.
3. Compare the observed changes with the expected ones. Each observed change
   must serve the problem statement. An unexpected change is a defect until
   the user accepts it. When the evidence shows the prediction misread code
   outside the delta, record a prediction error with that evidence and
   correct the prediction. A defect in your own verification script is not
   a defect in the bit; fix it and rerun.

Done when every applicable procedure has a recorded result, every observed
change maps to an expected one, and the value from step 1 is observable or
its observation is scheduled after publication. If the procedures show no
difference from the control, report the value as not observed. The user
decides to ship it with a provisional value or to drop it.

## Reply

- The problem statement.
- The graph before and after, as a small diagram.
- The bit: each revision with its one-line intent.
- Lineage-plan decisions and any blockers.
- Each check and verification with its command and result.
- The snapshot comparisons and the recovery point.
- What stays exploratory, and anything parked on a temporary branch.

Write back to `context.md`: the problem statement, the shipped bit, the
product-base revision, and the conventions the user supplied. If the
repository has no `context.md`, propose the entry to the user. Create a
new tracked file only with the user's approval.
