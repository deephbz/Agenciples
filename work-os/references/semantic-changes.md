# Base-Relative Semantic Change Discipline

Applies when starting or resuming implementation, identifying the base
revision, deciding whether work should amend, split, or stack, reviewing
concurrent work, or recovering intent after a handoff or context loss.

## Core invariant

Treat the semantic change, not the workspace or editing history, as the unit
of development. A semantic change carries one independently meaningful intent
through implementation, review, rewriting, and integration.

Development can proceed on an optimistic integration state that contains many
pending changes. That convenience must not erase their identities, ownership,
dependencies, or review surfaces. Edits occur in temporary workspaces; intent
persists in the change graph.

## Ontology

Use these concepts consistently:

- **Product base** — the accepted state against which pending work ultimately
  contributes. Record it as an exact immutable revision. A name such as
  `origin/main` is only a locator until resolved to a revision.
- **Semantic change** — one coherent intent and the net modifications that
  realize it. Its boundary follows meaning, not files, commits, agents, or
  elapsed time.
- **Change identity** — the durable handle used to refine and discuss one
  semantic change. A VCS identifier can represent this handle, but the
  identifier does not replace the intent.
- **Declared dependency** — another pending semantic change required for this
  change to make sense or operate correctly.
- **Dependency frontier** — the composed state of the product base plus all
  declared dependencies. This is the direct comparison point for a stacked
  change.
- **Integration state** — an optimistic composition of accepted and pending
  changes used for day-to-day development and compatibility checks.
- **Working copy** — a temporary surface on which edits occur. It is not a
  durable ownership or review boundary.
- **Review projection** — the isolated net contribution of one change relative
  to its product base and dependency frontier.

A stacked change can be independently reviewable without being independently
applicable to the product base. Remove unrelated pending work for review, but
retain declared dependencies. The governing question is: what does this
change contribute when unrelated pending work is removed?

## Recover the graph before editing

Before implementation:

1. Resolve the designated product base to an exact revision.
2. Identify the current integration state and the active semantic change, if
   one exists.
3. Inspect pending changes that affect the same files, interfaces, concepts,
   behaviors, or invariants.
4. Read their current descriptions and dependencies. Do not infer ownership
   from file names alone.
5. Classify the requested work before assigning edits to a change.

File overlap is evidence, not the decision. Two changes can edit the same file
for independent reasons. They can also conflict semantically while editing
separate files.

If no designated base exists, stop and establish one. If a moving name is the
only available reference, record the exact revision it resolves to before
review or implementation begins.

## Classify the requested work

Choose one of four outcomes:

- **Refinement of existing intent** — amend or squash the work into the
  existing semantic change. User feedback, bug fixes needed to satisfy its
  stated contract, and implementation replacements usually belong here.
- **Distinct and independent intent** — create a sibling change that remains
  meaningful relative to the product base.
- **Distinct but dependent intent** — create a stacked change and declare the
  pending change or changes that form its dependency frontier.
- **Competing or unclear intent** — stop and escalate. State the exact overlap,
  the candidate ownership boundaries, and whether amendment, stacking, or a
  sibling design would preserve the intended review surface.

Do not create a new change because a new agent turn started. Do not amend an
existing change merely because the same files are open. The question is always
whether the requested outcome expresses the same intent.

Textual dependence and semantic dependence are different. Two principles can
be semantically independent but both renumber one list. Stack them to manage
the textual dependency without pretending they are one idea.

## Preserve boundaries while editing

Inspect the working-copy diff during implementation and assign every
modification to one semantic change. Split, move, drop, or reassign an edit
that does not serve the active intent. Narrative does not make an unrelated
edit relevant.

Branches, worktrees, temporary commits, and agent sessions are control and
editing mechanisms. Use them when they prevent collisions or make a tool safe,
but do not treat them as semantic identities. One change can survive many of
these surfaces, and one workspace can temporarily contain several changes.

A shared integration state does not require concurrent agents to edit the same
physical working copy. Use separate editing surfaces when simultaneous writes
would be unsafe. Compose their semantic changes into the integration state
when reviewing dependencies and compatibility.

Keep the change description evergreen. It should state the final valid
intent, constraints, dependencies, and externally visible result. Rewrite it
as the design improves.

## Review isolation and integration separately

Every semantic change has two verification surfaces:

1. **Isolated contribution** — reconstruct the review projection relative to
   the exact product base and dependency frontier. Review the files,
   interfaces, lines, behavior, and description that belong to this intent.
   Check for undeclared dependencies and unrelated edits.
2. **Integrated result** — compose the change into the intended integration
   state. Run the focused checks that detect conflicts with accepted and
   expected future work.

Neither surface replaces the other. An integration diff can hide ownership
leaks because it includes unrelated work. An isolated diff can pass while the
composed system fails.

When the product base or dependency frontier changes, reconstruct the review
projection. Confirm that the change still has the same meaning and that a
rebase did not absorb or lose unrelated modifications.

Review the actual net change, not the sequence of agent turns, intermediate
attempts, or conversational explanations. If a modification does not belong
to the stated intent, change the graph or the diff.

## Keep descriptions evergreen and evidence historical

Rejected designs and failed attempts are historical evidence. Preserve them
in VCS operation history, task history, or an append-only journal when they
remain useful. Do not keep them in the current change description as though
they are part of the accepted system.

The change description is working context. Write it from the audience's
accepted baseline and the final accepted state, and apply the residue test
to the change itself: a comment, abstraction, compatibility path, test, or
explanation that exists only because of a rejected intermediate design is
removed. The coordinates, the worked example, and the test itself live in
source-allocation.md under "Write from the audience's baseline".

## Rewrite and publication

Before publication, semantic changes should remain easy to amend, squash,
split, rebase, reorder, or drop. Preserve a change identity when the intent
continues. Create new identities when one intent splits into independently
meaningful outcomes.

Revision identity and semantic identity are not always the same. Rewriting can
change a revision identifier while preserving intent. Some VCS tools expose a
stable change identifier; others require a review record, branch convention,
or other durable handle.

Rewriteability is not permission to disrupt consumers. Once others depend on
a published revision, coordinate rewrites and account for downstream work.
The semantic boundary remains primary, but publication changes the cost and
risk of changing its representation.

## Minimal handoff record

A later agent must be able to recover:

- the change identity and current intent;
- the exact product base;
- declared dependencies and the dependency frontier;
- the isolated review projection;
- the integration revision used for compatibility checks;
- unresolved overlaps or ownership decisions.

Use the VCS graph and change description as the source when they already carry
these coordinates. Do not create a second manifest that duplicates them. Add
a task or evergreen record only for context the VCS cannot express.

## Failure modes

- One change per agent turn, feedback cycle, branch, or worktree.
- One large integration change with no recoverable intent boundaries.
- Treating every file conflict as semantic overlap.
- Missing conceptual overlap because changes touch different files.
- Requiring a valid stacked change to apply directly to the product base.
- Reviewing only the integration state or only the isolated change.
- Leaving unrelated edits in a change and explaining them through narrative.
- Recording a moving bookmark name without the exact base revision.
- Preserving rejected designs as compatibility requirements or current prose.
- Writing from the previous agent turn instead of the audience's actual baseline.
- Rewriting published revisions without checking downstream dependencies.

## `jj` mapping

In `jj`, a change ID can preserve semantic identity while commit IDs change as
implementation is rewritten. The working-copy commit is an editing surface,
and a merge revision can represent an integration state. These are useful
representations of the ontology above, not the ontology itself.

Use installed `jj` help as the command authority. Keep the principle and review
criteria tool-neutral so they remain valid if the VCS or integration mechanism
changes.

## Intended outcomes

This discipline provides semantic isolation, high-velocity composition,
context resilience, and rewriteable history. Agents can build against the
expected future state without making that state the unit of ownership or
review.