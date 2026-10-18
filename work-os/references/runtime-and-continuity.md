---
purpose: Keep work continuous across sessions, agents, models, and tools.
scope: Harness design, runtime and applications, work identity, information classes, write-back, ratification, re-anchoring.
out-of-scope: The format of context.md and the journal (conventions.md).
---

# Runtime and continuity

Without continuity, agent work is fast and forgetful: every task looks
finished, and the organization accumulates nothing. With continuity, each
solved problem leaves concepts, contracts, traces, and artifacts that make
the next problem cheaper.

## Design the harness around the human

Start from the human's decision loop. When does the human return? What must
they decide? What evidence and current context make that decision sound?
What action lets the work continue? Choose tools, storage, and dashboards
after these answers. Agent visibility, throughput, tool calls, and cost are
signals. Attention quality, context recovery, and continuity of deep work
are the objective.

## Thin runtime, replaceable applications

The runtime is the small part that survives every change of model, tool, or
host: the agent loop, plain files (markdown, SQLite), git, `context.md`,
and the journal. Every other tool is an application: a tracker, a dashboard,
a search index, a memory service, a CI system. The agent uses an application through a CLI, a
skill, or an API, and keeps the state of the work in the runtime. So you can
replace an application without losing work.

- Add to the runtime only what every task needs and every tool can read.
- Give each application a CLI or an API that agents can call, and a skill
  that says when to call it.
- Some scaffolds compensate for current model limits: skill graphs, bespoke
  memory protocols. Better models may dissolve them. Keep continuity in
  the runtime primitives, which survive. Boring formats with obvious
  semantics transfer. Clever formats do not.

## Work identity outlives the process

Work identity and context lineage outlive any model call, process, terminal,
host, or conversation. A crash, a restart, a move, or a model change does
not start new work. Model work identity, context lineage, execution,
process, location, and aliases as separate concepts when their lifecycles
differ. Otherwise operational accidents become false boundaries in the work.

## Three information classes

Label each item by class inside its content. Separate files are not
required.

- **Evidence** records what a source said or what was observed: transcripts,
  commits, task history, experiment records, runtime observations. It says
  what happened according to that source, and the source may be wrong.
  Preserve it, and append corrections.
- **Working context** records what still matters: goals, constraints,
  decisions in force, open questions. Correct it in place. Mark what is
  superseded, and link each change to the evidence behind it.
- **Assessment** records a recomputable inference: priority, readiness,
  risk, need for the human's attention, status. Record its evidence,
  freshness, uncertainty, and derivation.

A summary is an assessment. Never destroy evidence to keep a summary:
compressing and then discarding raw records is how systems corrupt quietly.
Never present an assessment as an observation.

## Boot from files, write back

Begin from durable files: `context.md`, the journal, and the sources. When
needed context lives only in an old chat, recover it into a file. End
nontrivial work by writing back what changed, what you learned, and what
remains (conventions.md gives the files). If agents cannot see a fact, it
does not exist for them. Encode knowledge from heads, chat, and messages as
versioned files. Before you revise working context, recover its reader and
the accepted state, and apply the residue test (writing-for-readers.md).

## Ratify durable guidance and changes of intent

Agents propose durable guidance: skills, memories, instruction files, and
ontologies. The human ratifies it before it becomes governing context.
Unsupervised self-accumulation compounds noise.

Routine write-back needs no ratification: observations, status, and decisions
inside the accepted envelope. New or changed goals, governing constraints,
and standing instructions need human ratification wherever they are written,
including `context.md` and the journal. Reversible implementation choices
inside the envelope remain agent work. intent-and-interfaces.md defines
governing constraints and gives the escalation procedure.

- A skill or instruction states intent, properties, and checks, written as
  actions and checks an agent can carry out. Paragraphs of facts and
  philosophy do not steer an agent. Add step-by-step procedure only where a
  step must run the same way each time, and then prefer a script
  (intent-and-interfaces.md).
- Concise beats comprehensive. Load a few relevant skills through
  progressive disclosure: index, description, full content.
- An entry-point file (`CLAUDE.md`, `AGENTS.md`) is a short table of
  contents into structured files. A long entry point dilutes every context
  window.
- Keep useful failure evidence in the journal, so agents do not repeat the
  failure. Superseded output files still go (conventions.md).

## Re-anchor against decay

Iteration does not improve work monotonically. Unsupervised loops degrade,
and each compaction loses information.

- Re-read the problem and the objective at intervals. Check that the current
  state still serves them, and prune the drift.
- Give each autonomous loop an iteration budget. Escalate to the human at
  the limit.
- Stale memory is worse than no memory. Prune on contradiction, and date what
  decays.
