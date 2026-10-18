---
purpose: Explain Agenciples to humans, and show how to install and maintain it.
scope: Why, the working model, the parts, install, maintenance, lineage.
out-of-scope: Rules for agents (the snippet, the skill, and its references).
---

# Agenciples

Principles for agents, and for the humans who design their harnesses.

## Why

Agents made implementation cheap. Four things stayed scarce. Without
**intent**, agents optimize the wrong thing quickly. Without **clear
concepts**, a vague ontology becomes thousands of lines of confident, wrong
code. Without **investigability**, agents run stacks that they cannot help
you debug. Without **continuity**, work is fast and forgetful, and the
organization accumulates nothing. The usual failures: silent scope changes,
tests that encode guesses, traces taken as proof, summaries that replace raw
evidence, and rejected designs that leak into current text.

## One human, many agents

Agenciples serves one human who directs many agents. The culture is a
start-up's: a few conventions, much flexibility, and fast decisions. The
human sets intent and ratifies durable guidance and changes of intent. Agents
search, decide reversible choices inside the envelope, and work in parallel.
Effort goes to the real field work: the research, the product, the data. The
harness gets the minimum that keeps it out of the way. A harness serves the
human's decision loop
([runtime-and-continuity.md](work-os/references/runtime-and-continuity.md)).

The corpus has three levels. The snippet is always on and states seven
short stances. The `work-os` skill loads when a task matches,
completes each stance, and routes to one reference. A reference loads on
demand and owns the procedure for one kind of task.

## Parts

| File | Purpose | Scope | Out of scope |
|---|---|---|---|
| [README.md](README.md) | Explain Agenciples to humans, and show how to install and maintain it. | Why, the working model, the parts, install, maintenance, lineage. | Rules for agents (the snippet, the skill, and its references). |
| [AGENTS-snippet.md](AGENTS-snippet.md) | Always-on core for an agent's global instructions file. | Seven short stances, two invariants, and a pointer to the register. | Complete stances and procedure (work-os). |
| [work-os/SKILL.md](work-os/SKILL.md) | Complete the seven stances, and route each task to one reference. | The stances at their shortest complete form; the router. | Short stances and invariants (the snippet); procedure (the references). |
| [references/rigor-and-evidence.md](work-os/references/rigor-and-evidence.md) | Match each practice to maturity, and decide what counts as evidence. | Stages, stage windows, anchors, checks, compatibility, evidence reuse. | Pipeline traces (traceable-computation.md); change review (change-composition.md). |
| [references/intent-and-interfaces.md](work-os/references/intent-and-interfaces.md) | Turn human intent into envelopes, ontologies, and interfaces that agents search inside. | Envelope fields, scope change, review, ontology, agent interfaces, views, boundaries, where facts live. | Envelope placement (conventions.md); sentence style (writing-for-readers.md). |
| [references/parallel-work.md](work-os/references/parallel-work.md) | Cut the elapsed time to accepted results with parallel work by default. | Batching, fan-out, competing attempts, parallel checks, workspaces, merging, early starts, serial reasons, cost. | Composing edits for review (change-composition.md); the evidence standard (rigor-and-evidence.md). |
| [references/writing-for-readers.md](work-os/references/writing-for-readers.md) | Make every text usable by its reader, and own the writing register. | Reader coordinates, order, words, numbers, register, communication shape, accepted-state writing, residue test. | Where a fact lives (intent-and-interfaces.md); envelope placement (conventions.md). |
| [references/traceable-computation.md](work-os/references/traceable-computation.md) | Make computations whose correctness depends on data meaning inspectable step by step. | What to trace, trace mode, node identity, provenance on results, the reverse bridge. | Operating a pipeline; proof (rigor-and-evidence.md); homes and retention (conventions.md). |
| [references/research-artifacts.md](work-os/references/research-artifacts.md) | Keep research reproducible, and keep its results usable by agents and humans. | Durable problem and objective, optional plans and diagrams, results, source bundle, backend first. | Homes and retention (conventions.md); provenance fields (traceable-computation.md); views (intent-and-interfaces.md). |
| [references/change-composition.md](work-os/references/change-composition.md) | Keep the intent of each change recoverable and reviewable while many agents edit one codebase. | Work headed for review; base, changes, amend or sibling or stack, isolated and integrated review, descriptions, handoff. | Exploration; VCS commands; shipping a bit (ship-a-bit). |
| [references/runtime-and-continuity.md](work-os/references/runtime-and-continuity.md) | Keep work continuous across sessions, agents, models, and tools. | Harness design, runtime and applications, work identity, information classes, write-back, ratification, re-anchoring. | The format of context.md and the journal (conventions.md). |
| [references/conventions.md](work-os/references/conventions.md) | State the few conventions that every artifact follows. | Envelope placement, context.md and the journal, the three homes, retention. | Meaning of envelope fields (intent-and-interfaces.md); the register (writing-for-readers.md). |
| [ship-a-bit/SKILL.md](ship-a-bit/SKILL.md) | Ship one small, mature bit of exploratory work as a clean, verified lineage on the product base. | Problem statement, choice of bit, cleanup, quality bar, graph shape, verification. | Principles (work-os); publication without the human's go. |

## Install

For Claude Code, or any harness that supports
[Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview):

```bash
# 1. Append the "Agenciples" section of AGENTS-snippet.md
#    to ~/.claude/CLAUDE.md or your AGENTS.md.

# 2. Install the skill.
cp -r work-os ~/.claude/skills/work-os

# 3. Optional: install skills that run only when you invoke them.
cp -r ship-a-bit ~/.claude/skills/ship-a-bit
```

The snippet and the skill do not know your other instruction files. Before
you adopt them, read your `CLAUDE.md`, `AGENTS.md`, hooks, and skills, and
remove or reconcile each rule that duplicates or conflicts with the
snippet. The register and the two invariants are the usual overlaps. Each
rule keeps one home after integration.

Edit freely; this corpus is a starting point. Keep one structural idea:
match the durability of an instruction to how often you need it.

## Maintain

Before each commit, check three things:

1. Each rule has one home, and other files point to it.
2. Each file's front matter states at least purpose and scope. Other fields
   earn their place by helping the reader. The parts table matches the front
   matter, and each reference has a router row.
3. The text follows the register in `work-os/references/writing-for-readers.md`.

## Lineage

Parnas, Evans, Ousterhout, Brooks, Jackson, Knuth, Hunt and Thomas, Shape
Up, Gall, Beck, Popper, Feynman, Dapper, OpenTelemetry, W3C PROV, Claerbout
and Donoho, Luhmann, Matuschak, Hamming, Minto, ASD-STE100, concurrent
engineering, speculative parallelism, and context engineering. The new part
is the packaging: one corpus for humans and agents who share a harness.

## License

MIT
