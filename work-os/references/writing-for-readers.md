---
purpose: Make every text usable by its reader, and own the writing register.
scope: Reader coordinates, order, words, numbers, register, communication shape, accepted-state writing, residue test.
out-of-scope: Where a fact lives (intent-and-interfaces.md); envelope placement (conventions.md).
---

# Writing for readers

The register governs sentence form. It does not decide what the reader needs
first, which words they know, or why they care. A text can obey every
sentence rule and still fail:

```text
This narrative report preserves the frozen one-day study. It consumes the
immutable replay outputs in ignored data/. config.json owns the design.
```

Each sentence is short and active. Together they describe the container of
the text and the process that made it. The reader wanted neither.

## Know the reader and where they start

Before you write, recover four coordinates:

1. **Reader**: a trader, a researcher, a maintainer, a reviewer, an agent
   with no context.
2. **Starting point**: the words, models, and accepted state they start from.
3. **Final accepted state**: what is true after this work.
4. **Outcome**: what they should know, believe, or do after reading.

The same facts need a different text for each reader.

## Give a reason to care, then the answer

Open with what matters to the reader: the problem, its stakes, or the
decision it informs. Then give the answer. Then give the support.

```text
context and stakes -> question -> answer -> why it holds -> limits -> how to check
```

The answer includes every finding that would change the reader's decision,
even one that weakens the headline. Put such a caveat beside the
answer. Short forms keep the order. A reply leads with the answer. A commit
message states what changed for its users and why. A README opens with the
problem it solves. A report opens like the abstract of a paper.

## Use the reader's words

- Write about the subject: the market, the model, the user, the bug. Do not
  write about the text ("this section describes").
- Keep the producer's vocabulary out: internal form names, process states
  (frozen, candidate, clean run), file paths, tool names, agent workflow.
  Name them only when the reader acts on them, as in reproduction steps.
- Define a term at first use in one plain clause, or use a plainer word.
- Give each number a unit, a direction, a comparison that sizes it, and its
  uncertainty. "Net uplift is 0.50" is a value. "A coupon order keeps $0.50
  after it gives away $3.60 of the $4.10 it adds" is a finding.

## The register

These rules apply to every reply and every durable text. ASD-STE100
Simplified Technical English is the default register. Durable language,
prose or code, carries its information in the fewest, plainest words.

- Write short sentences in active voice, subject before verb before object.
- Use one term for one concept. Names follow the same rule. A name that needs
  a comment is a name drawn wrong.
- Do not use cleft sentences ("it is X that"), contrastive appositives ("X,
  not Y"), appended glosses ("X, meaning Y"), or trailing clauses. A heading
  may use "X, not Y" when the contrast is its point.
- State a claim before its limits, and what a thing is before what it is not.
- Cut every sentence that a competent reader can derive from the rest, every
  hedge, and every sentence about the text itself.
- Do not restate what a signature, a type, or an implementation already says.
- Keep the reasoning. Brevity removes words. Keep the connectives that carry
  the argument: because, so, but, therefore. A paragraph reads like an
  argument that a person would say aloud.

## Communication shape

Choose the smallest form that makes the point clear.

- Use compact ASCII diagrams, trees, and code sketches when they show
  relationships more clearly than prose: arrows for flow or dependency, call
  trees for runtime calls, shallow trees for structure, pseudocode for logic
  or interfaces, a focused diff when the reader knows the starting shape.
  Keep only the relevant labels. Mark a partial sketch as partial. Put each
  visual beside its explanation and cut the prose that repeats it.
- Use bullets for distinct items and numbers for ordered steps. Nest only for
  a real relationship, and keep nesting shallow.
- Use short paragraphs for connected reasoning and one sentence for a simple
  answer. Add headings only when distinct topics need navigation.

## Write from the accepted state

Agent turns are episodic. Each turn, and each turn after a compaction,
overweights the state just before it, so a rejected attempt can feel like
the start of the story. A common pattern: A requested, A+B implemented, B
rejected, B removed, then a note that the system "intentionally avoids B".
From the accepted baseline, B never existed. The durable story is
`base -> A`, unless the reader needs B as an accepted alternative.

Never refer to a conversation, a thread, or a turn that the reader has not
seen ("as discussed"). One accepted state can need one text for a maintainer
of an older release and another for a new reader. Neither text inherits
intermediate states of the agent loop.

**The residue test.** If the rejected intermediate state had never existed,
would this comment, abstraction, compatibility path, test, or explanation
still be necessary? If not, remove it. Keep the rejected path only in the
journal or the VCS history, and only when it is useful evidence. Write as if
the wrong turn never happened. This hygiene deletes no history.

## Keep bookkeeping where its readers look

Provenance, comparisons with earlier runs, machine records, file ownership,
and open process questions serve maintainers and agents. Put them in an
appendix, a README, the journal, or module documentation. When the reader
needs what a record shows, say it in a sentence or a small table, and name
the file that holds the record.

## Check before you finish

Read the title and the first paragraph as the reader, with no other context.
The reader should know why it matters, what was found or changed, and what
it means for them. Would you say each sentence aloud to this person? Rewrite
each sentence that fails.
