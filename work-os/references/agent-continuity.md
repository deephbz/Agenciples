# Agent Continuity

Applies when writing skills, memory files, CLAUDE.md/AGENTS.md content,
or designing multi-session and multi-agent workflows. Lineage: context
engineering (Anthropic, Manus), agent memory (MemGPT/Letta,
filesystem-as-context), spec-driven development (spec-kit), Anthropic
Skills + SkillsBench.

Continuity is the point of the whole Work OS: without it agent work is
fast but forgetful — every task looks finished, but the organization
accumulates nothing. With it, each solved problem leaves concepts,
contracts, traces, and artifacts that make the next problem cheaper.

## Preserve three information classes

Do not collapse durable evidence, maintained context, and current judgment
into one note or status:

- **Historical evidence** preserves source records and observations:
  transcripts, commits, task history, experiment records, and runtime
  observations. It says what occurred *according to that source*, not that
  the source is complete or correct. Preserve it and append corrections; do
  not rewrite it to match the latest story.
- **Current working context** records what still matters: active goals,
  constraints, accepted decisions, unresolved questions, and takeaways.
  Curate it deliberately. Mark corrections, retirement, and supersession so
  stale context does not silently remain authoritative, while keeping links
  back to the evidence that justified each change.
- **Assessment or projection** records what is currently inferred: priority,
  readiness, risk, need for human attention, or a synthesized status. Treat
  it as recomputable output, not source truth. Record its evidence links,
  freshness, uncertainty, and derivation/model version.

This separation gives compression honest mutation semantics: evidence is
preserved, working context evolves, and assessments can be discarded and
recomputed. When presenting a judgment, expose enough provenance for a human
or later agent to recover the evidence instead of trusting the label.

## Start from artifacts, end by writing back

- Begin sessions from durable artifacts (problem docs, design docs,
  memory files, repo docs), never from "as we discussed". If required
  context lives only in a previous chat, first extract it into a file.
- End nontrivial work by writing back: what changed, what was learned,
  what remains. Keep a stable *task doc* (goal, constraints — rarely
  edited) plus two files with opposite mutation semantics: an
  append-only **journal** (attempts, back-and-forth, solved problems,
  superseded blockers — historical evidence, never rewritten) and a
  curated **evergreen doc** (the declared lifecycle stage, decisions
  still in force, latest status, pending problems, current blockers,
  next steps — working context, ruthlessly pruned). This is the evidence / working-context
  separation above, made concrete as files. When a problem is solved,
  its story stays in the journal and it leaves the evergreen doc; a
  solved problem or dead blocker still sitting there is a bug. The
  next session boots from the evergreen doc and consults the journal
  only when it needs to know *why*. Density runs diagram < evergreen
  doc < journal: the diagram holds only critical concepts
  (research-artifacts.md), the evergreen doc slightly more, the
  journal everything.
- "If agents can't see it, it doesn't exist." Tribal knowledge in
  heads, Slack, or chat history is invisible; encode it as versioned
  files. Entry-point files (CLAUDE.md/AGENTS.md) are a table of
  contents, not an encyclopedia — keep them ~100 lines pointing into
  structured docs, or they dilute every future context window.

## Recover the audience and starting point

Agent turns are episodic. After several feedback cycles or a context
compaction, the immediately preceding state can feel like the beginning of the
story even when it was only a rejected intermediate attempt. Durable artifacts
must not inherit that local perspective.

Before producing or revising current-facing prose, identify:

- **Audience** — who will read or operate from this artifact;
- **Starting point** — the accepted state or mental model that audience begins
  from;
- **Current accepted state** — what is intended to remain true now.

Then rewrite from those coordinates rather than patching the previous turn.
For example, if the requested outcome was A, an agent temporarily implemented
A+B, and the operator rejected B, the evergreen explanation normally describes
A directly. It does not add prose about "avoiding B" unless B is an accepted
historical alternative that the audience genuinely needs to understand.

Use the counterfactual test from `semantic-changes.md`: if the rejected
intermediate state had never existed, would this explanation, abstraction,
compatibility path, or warning still be useful? If not, remove it from current
working context. Preserve the attempt only in historical evidence when it is
worth keeping.

This is the continuity equivalent of a base-relative diff: **narratives are
baseline-relative and audience-relative, not turn-relative.**

## What makes persisted context good

Empirics from SkillsBench and production practice:

- **Human-curated beats model-generated** (+16% vs net-negative).
  Agents propose skill/memory updates; a human ratifies before they
  become durable. Unsupervised self-accumulation compounds noise.
- **Procedural beats declarative.** "Do X, then Y, check Z" outperforms
  paragraphs of facts and philosophy.
- **Concise beats comprehensive.** Skills land best around 800-1500
  tokens; 2-3 relevant skills beat a pile. Progressive disclosure
  (index → description → full content) instead of loading everything.
- **Keep errors in context, not every residue.** Failed attempts can steer the
  model away from repeats, so retain the useful failure evidence in the
  journal or run record when it matters. This does not require preserving every
  failed output file; superseded execution artifacts are garbage-collected per
  `research-artifacts.md`.

## Continuity decays — re-anchor

Iteration does not monotonically improve things: unsupervised
agent loops measurably degrade correctness and security after roughly
4-7 rounds, and each compaction/summarization loses information
(raw-first rule applies to transcripts too). So:

- Re-anchor periodically against the spec/problem artifact: re-read it,
  check the current state still serves it, prune drift.
- Impose iteration budgets on autonomous loops; escalate to a human
  instead of looping past them.
- Treat "more artifacts" as a cost as well as an asset: stale memory is
  worse than no memory. Prune on contradiction, date what decays.

## Bet on durable primitives

Today's elaborate scaffolds (skill graphs, bespoke memory protocols,
task-bead formats) partly compensate for current model limits and may
be dissolved by better models (Bitter Lesson). Invest continuity in
primitives that survive regardless: the filesystem, git, plain
markdown, SQLite. Prefer boring formats with obvious semantics over
clever bespoke ones — the cleverness is exactly what won't transfer.