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

Each file's front matter states its purpose and scope; that front matter is
authoritative. This table only says when each file loads.

| File | Loads |
|---|---|
| [README.md](README.md) | read by humans |
| [AGENTS-snippet.md](AGENTS-snippet.md) | always, pasted into the global instructions file |
| [work-os/SKILL.md](work-os/SKILL.md) | before non-trivial work, by skill description |
| [references/rigor-and-evidence.md](work-os/references/rigor-and-evidence.md) | on a router match |
| [references/intent-and-interfaces.md](work-os/references/intent-and-interfaces.md) | on a router match |
| [references/parallel-work.md](work-os/references/parallel-work.md) | on a router match |
| [references/writing-for-readers.md](work-os/references/writing-for-readers.md) | on a router match; its register applies to every reply |
| [references/traceable-computation.md](work-os/references/traceable-computation.md) | on a router match |
| [references/research-artifacts.md](work-os/references/research-artifacts.md) | on a router match |
| [references/change-composition.md](work-os/references/change-composition.md) | on a router match |
| [references/runtime-and-continuity.md](work-os/references/runtime-and-continuity.md) | on a router match |
| [references/conventions.md](work-os/references/conventions.md) | on a router match |
| [ship-a-bit/SKILL.md](ship-a-bit/SKILL.md) | only when the human invokes it |

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

If you adopt it, edit freely; this corpus is a starting point. Keep one
structural idea: match the durability of an instruction to how often you
need it.

## Maintain

Before each commit, check three things:

1. Each rule has one home in a reference, and other files point to it. The
   snippet and the skill may summarize a rule; the reference wins on
   conflict.
2. Each file's front matter states at least purpose and scope. Other fields
   earn their place by helping the reader. The front matter is
   authoritative; the parts table lists each file and when it loads, and
   each reference has a router row.
3. The text follows the register in `work-os/references/writing-for-readers.md`.

Inside this repository, an agent proposes changes and the human ratifies
them. Changing a file's front matter is a scope change
(`work-os/references/intent-and-interfaces.md`). Check a change to
agent-facing text by probing fresh agents with and without it, more than
once.

## Lineage

Parnas, Evans, Ousterhout, Brooks, Jackson, Knuth, Hunt and Thomas, Shape
Up, Gall, Beck, Popper, Feynman, Dapper, OpenTelemetry, W3C PROV, Claerbout
and Donoho, Luhmann, Matuschak, Hamming, Minto, ASD-STE100, concurrent
engineering, speculative parallelism, and context engineering. The new part
is the packaging: one corpus for humans and agents who share a harness.

## License

MIT
