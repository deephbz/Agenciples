---
purpose: State the few conventions that every artifact follows.
scope: Envelope placement, context.md and the journal, the three storage locations, retention.
out-of-scope: Meaning of envelope fields (intent-and-interfaces.md); the register (writing-for-readers.md).
---

# Conventions

## Every artifact describes itself

Every durable artifact carries an envelope with at least `purpose` and
`scope`. Add other fields when they clearly help: `out-of-scope` to clarify
neighboring responsibilities, or `status` when the stage changes how the
artifact evolves. intent-and-interfaces.md defines these and the other
fields. Put the envelope in the native metadata of the artifact, where
maintainers and agents look and the audience does not:

| Artifact | Placement |
|---|---|
| Markdown file | YAML front matter |
| `SKILL.md` | a `metadata:` map in the front matter, after `name` and `description` |
| Python module or script | module docstring or opening comment |
| Notebook | notebook metadata; add audience, execution model, independent sections, and shared prerequisites when relevant |
| Package or component | its README front matter or package docstring |
| Repository or system | README front matter; it links each child and copies none |
| Class, function, interface | docstring, only for a real responsibility, invariant, or non-obvious contract |

```yaml
---
purpose: Help researchers choose a model by comparing candidates.
scope: Experiment selection and presentation.
---
```

Use project-native field names when they exist. When the audience needs part
of the envelope, such as the scope of a report, rewrite that part in the
voice of the document. This file follows the convention it states.

## context.md and the journal

A project keeps two continuity files with opposite rules:

- **`context.md`** holds working context: goals, constraints, the stage of
  each component, decisions in force, status, open problems, blockers, next
  steps. Prune it hard. A solved problem or a dead blocker in `context.md`
  is a bug.
- **`journal.md`** is append-only: dated evidence, assessments, attempts,
  solved problems, superseded blockers, rejected paths. Never rewrite it.

The next session boots from `context.md` and reads the journal only to learn
why. Detail grows from diagram to `context.md` to journal. Add no other
continuity file.

## Three storage locations

Each artifact that must survive lives in exactly one storage location:

1. **Repository**: reusable, maintained, versioned source.
2. **Project workspace**: task-specific scaffolding, such as driver scripts,
   notebooks, experiment shells, and run manifests. When the team versions
   this scaffolding, its repository is the location. Keep it out of unrelated
   repositories.
3. **Shared storage**: final reports, large datasets, model outputs, and data
   that several workers read.

A storage location sets where an artifact lives; retention below sets its
lifetime. Temporary directories, home folders, desktops, and scratch mounts
are ephemeral. If losing a file there would matter, move it to a storage
location. Decide once which location each artifact type uses. A new turn or
a new experiment is no reason for a new location.

## Retention

- **Keep variants**: controls, ablations, sweeps, alternative
  implementations, declared baselines. Name each with its condition and
  provenance.
- **Replace retries**: an output that exists only because an earlier attempt
  failed or fell short of the same result.
- Delete a superseded output once its successor is verified. Cleanup is part
  of finishing.
- Keep a failed attempt only when the failure is informative, needed for
  audit, or expensive to reconstruct. Record it in the journal.
- Keep an intermediate for speed, inspection, or audit. Delete it otherwise.
  The source bundle regenerates it.
