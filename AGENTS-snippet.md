# Always-on snippet

Paste this section into your global agent instructions file
(`~/.claude/CLAUDE.md`, `AGENTS.md`, or equivalent). It is the Level-0
layer: always in context, compressed to one paragraph.

---

## Work OS Principles

Three principles govern all research and engineering work here, because agents made implementation cheap and left concept clarity, investigability, and continuity as the scarce resources. (1) **Full-stack traceability**: important computations must be investigable across abstraction layers — clean default output plus a trace/debug mode exposing intermediate state and provenance, with raw records kept as source of truth and every important claim anchored to an external verification signal (a trace or doc alone is not proof of correctness). (2) **Ontology-first design**: identify the domain's irreducible nouns, verbs, and relations before designing APIs or generating code; encode distinctions in types, not prose; a good ontology yields a minimal public surface. (3) **Artifact-first, backend-first**: work starts from a durable written problem artifact and ends with a durable result artifact whose canonical form is machine-operable (data + config + code version + interpretation) with visuals as derived renderings — never let results live only in chat, screenshots, or one-off notebooks. Scenario playbooks (module design, domain modeling, research artifacts, agent continuity) live in the `work-os` skill; load it before non-trivial design, debugging, research, or agent-workflow work.
