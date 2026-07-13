# Agenciples

**Principles for agents and for the humans who design their harnesses.**
Agenciples has two audiences: agent-facing instructions, progressively
disclosed as an always-on snippet, a routing skill, and scenario playbooks;
and human-facing guidance for shaping agentic work systems without mistaking
tool activity for useful work.

## Why

Agents made implementation cheap. What they did not make cheap:

- **Concept clarity** — vague ontologies turn into thousands of lines of
  confidently wrong code, fast.
- **Investigability** — opaque stacks let agents run things they cannot
  help you debug.
- **Continuity** — transient chat answers make agentic work fast but
  forgetful; the organization accumulates nothing.

The agent-facing package encodes three principles targeting exactly those scarcities,
each with the failure modes that show up in practice (a trace is not a
verification; summaries corrupt, keep raw; model-generated memory needs
a human gate).

None of this is new — deliberately. The principles map onto
long-established lineages: Parnas information hiding, Domain-Driven
Design, Ousterhout's deep modules, Knuth's literate programming,
Luhmann's Zettelkasten, Hamming, distributed tracing, W3C PROV, and the
emerging context-engineering canon. The contribution is the packaging:
methodology shaped for consumption by humans *and* agents sharing one
harness, while keeping their different needs explicit.

## Designing the harness around the human

Agent visibility, throughput, tool calls, and cost are useful signals, but
they are not the product objective. Start from the human control and decision
loop: in what situation does the operator return, what decision must they
make, what evidence and still-current context make that decision sound, and
what action lets work continue? Only then choose capabilities, component
responsibilities, tools, storage, or dashboards. This keeps the system aimed
at attention quality, context recovery, and continuity of deep work rather
than maximizing visible agent motion.

Keep semantic continuity separate from runtime continuity. Semantic work
identity and context lineage may outlive any given model invocation, process,
terminal, host, or other replaceable carrier. A crash, restart, relocation,
or model change must not by itself create a new semantic boundary, while one
durable project may intentionally span many conversations and executions.
Model work identity, context lineage, reasoning execution, runtime process,
location, and external aliases as distinct concepts whenever their lifecycles
differ. Otherwise operational accidents become false boundaries in the work.

This README is the canonical human-facing explanation. Agents do not need it
for ordinary work, but when explicitly designing or reviewing an agent
harness they should read this rationale before selecting its abstractions.

## Agent-facing structure (progressive disclosure)

| Level | File | When it's in context |
|---|---|---|
| 0 | [AGENTS-snippet.md](AGENTS-snippet.md) | Always — one paragraph for your global instructions file |
| 1 | [work-os/SKILL.md](work-os/SKILL.md) | When the skill triggers — scenario routing + cross-cutting invariants |
| 2 | [work-os/references/](work-os/references/) | On demand — one playbook per scenario |

The four playbooks:

- [traceable-computation.md](work-os/references/traceable-computation.md)
  — dual-mode interfaces, canonical run records, raw-first provenance,
  semantic DAGs with materialization transparency (a cache is a view,
  not a result), and why traceability still needs a verification
  anchor.
- [domain-modeling.md](work-os/references/domain-modeling.md)
  — glossary-first ontology design, distinctions into types, minimal
  cognitive surface, intent as the optimization target (review
  interfaces harder than implementations), and a three-force ordering
  for component boundaries.
- [research-artifacts.md](work-os/references/research-artifacts.md)
  — the problem → plan → result artifact chain, source-to-artifact
  discipline (version the bundle, persist the artifact, discard the
  intermediates), diagrammatic research lineage (idea DAG + data DAG
  as the review surface), the transformation test, backend-first
  dual-display results, predict-before-run.
- [agent-continuity.md](work-os/references/agent-continuity.md)
  — start from artifacts and write back, separate historical evidence
  from current working context and recomputable assessment, human-gated
  persistence, re-anchoring against decay, betting on durable primitives.

## Install

For Claude Code (or any harness supporting
[Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)):

```bash
# 1. Add the always-on paragraph to your global instructions
#    (append the "Work OS Principles" section from AGENTS-snippet.md
#     to ~/.claude/CLAUDE.md or your AGENTS.md)

# 2. Install the skill
cp -r work-os ~/.claude/skills/work-os
```

Adapt freely — this is a starting point meant to be edited into *your*
working principles, not adopted verbatim. The one structural idea worth
keeping: match the durability of an instruction to the frequency you
need it (always-on paragraph ≪ triggered skill ≪ on-demand reference).

## License

MIT
