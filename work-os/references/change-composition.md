---
purpose: Keep the intent of each change recoverable and reviewable while many agents edit one codebase.
scope: Work headed for review; base, changes, amend or sibling or stack, isolated and integrated review, descriptions, handoff.
out-of-scope: Exploration; VCS commands; shipping a bit (ship-a-bit).
---

# Change composition

A change is the unit of development. Workspaces and edit history are
surfaces that carry it. Agents can build on an optimistic state with many
pending changes, as long as each change keeps its identity, dependencies,
and review surface.

## Terms

- **Product base**: the accepted state that pending work contributes to.
  Record it as an exact, immutable revision. A name such as `origin/main` is
  a locator until you resolve it.
- **Change**: one independently meaningful intent and the net modifications
  that realize it. Meaning sets its boundary. Files, commits, agents, and
  turns do not.
- **Change identity**: the durable handle for one change. Use the VCS's
  stable change identifier when it has one; otherwise use a review record
  or a branch convention.
- **Declared dependency**: another pending change that this change needs to
  make sense or to work.
- **Dependency frontier**: the product base plus all declared dependencies.
- **Integration state**: an optimistic composition of accepted and pending
  changes for daily work and compatibility checks.
- **Isolated contribution**: the net modifications of one change relative to
  its dependency frontier.

A stacked change can be reviewable on its own without applying on its own to
the product base.

## Before you edit

Resolve the product base to an exact revision. If no designated base
exists, stop and establish one. Find pending changes that
touch the same concepts, interfaces, behaviors, or invariants, and read
their descriptions and dependencies. File overlap is evidence. It does not
decide ownership. Two changes can edit one file for independent reasons, and
two changes can conflict in separate files.

## Amend, sibling, stack, or escalate

- **Same intent** (feedback, a fix its own contract needs, a replacement
  implementation): amend the existing change.
- **Independent intent**: create a sibling on the product base.
- **Dependent intent**: stack it, and declare the dependency.
- **Competing or unclear intent**: escalate. State the overlap, the
  candidate boundaries, and which shape keeps the intended review surface.

A new agent turn does not create a change. Open files do not justify an
amend. Textual dependence differs from dependence of intent: two independent
changes that renumber one list can stack without becoming one idea.

## Keep boundaries while you edit

Assign every edit in the working copy to one change. Move or drop an edit
that serves no active intent. A story does not make an unrelated edit
relevant. Branches, worktrees, temporary commits, and sessions are editing
surfaces. One change can cross many of them, and one workspace can hold
several changes for a while. Concurrent agents use separate workspaces
(parallel-work.md).

## Review the isolated and the integrated result

1. **Isolated contribution.** Rebuild it against the exact product base and
   dependency frontier. Review the files, interfaces, behavior, and
   description of this intent. Look for undeclared dependencies and
   unrelated edits.
2. **Integrated result.** Compose the change into the integration state, and
   run the focused checks that detect conflicts.

Neither surface replaces the other. An integration diff hides ownership
leaks. An isolated diff can pass while the composed system fails. When the
base or the frontier moves, rebuild the isolated contribution, and confirm
that the rebase kept the meaning and absorbed no unrelated edit. Review the
net change. The sequence of turns is irrelevant to review.

## Descriptions and rewriting

A change description is working context. It states the final intent,
constraints, dependencies, and visible result, from the accepted state, with
the residue test applied (writing-for-readers.md). Rewrite it as the design
improves. Rejected designs belong in the VCS history or the journal.

Before publication, keep changes easy to amend, squash, split, reorder, and
drop. Keep a change identity while its intent continues. Create new
identities when one intent splits. A rewrite can change a revision
identifier and keep the intent. Once others depend on a published revision,
coordinate each rewrite with them.

## Handoff

A later agent recovers each change from the VCS graph and its description:
the intent, the exact product base, the dependencies, and any unresolved
overlap. Add to `context.md` only what the VCS cannot express.
