# Writing for Readers

Applies whenever a person or an agent other than the author reads the text:
replies, reports, documents, READMEs, docstrings, comments, design notes,
change descriptions, commit messages, and messages. Where the text lives is
source-allocation.md's concern; this reference covers how to write it.
Lineage: Strunk & White, ASD-STE100 Simplified Technical English, minimum
description length, the pyramid principle (Minto), the abstract of a good
research paper, and public speaking.

Simplified Technical English governs sentence form. It does not decide what a
reader needs first, which words they know, or why they should care. A text can
obey every sentence rule and still fail its reader:

```text
This narrative report preserves the frozen one-day study. It consumes the
immutable replay outputs in ignored data/. config.json owns the design.
```

Each sentence is short and active. Together they tell the reader nothing they
want: the text describes its own container and the process that made it.

## Know the reader and where they start

Before writing or revising any text, recover four coordinates:

1. **Reader** — who reads this: a trader, a researcher, a maintainer, a
   reviewer, an agent with no context?
2. **Starting point** — which words, mental models, and accepted state or
   revision do they begin from?
3. **Final accepted state** — what is true after this work?
4. **Outcome** — what should they know, believe, or do after reading?

The same facts need a different projection for each reader. A good speaker
changes the talk for the room, but always says why the topic matters.

## Give a reason to care, then the answer

Readers decide in the first lines whether to keep reading. Open with what
matters to them: the problem, its stakes, or the decision it informs. Then
give the answer. Then give the support.

```text
context and stakes -> question -> answer -> why it holds -> limits -> how to check
```

The answer includes every finding that would change the reader's decision,
even one that weakens the headline. A caveat of that weight sits beside the
answer, not only in the limits.

Short forms keep the order. A reply leads with the answer. A commit message
states what changed for its users and why. A README opens with the problem the
project solves. A report opens like a paper's abstract.

## Use the reader's words

- **Talk about the subject.** Write about the market, the model, the user, or
  the bug. Do not write about the text ("this report", "this section
  describes").
- **Keep the producer's vocabulary out.** Internal form names, process states
  (frozen, accepted, candidate, rewrite, clean run), file paths, file ownership,
  tool names, and agent workflow mean nothing to the reader. Name them only when
  the reader acts on them, as in reproduction steps.
- **Define a term at first use** in one plain clause, or use a plainer word.
  An undefined term stops the reader.
- **Give each number its meaning.** State the unit and the direction. Compare
  the number with something that sizes it: a cost, a baseline, or a threshold.
  Add its uncertainty. "Net uplift is 0.50" is a value; "a coupon order
  keeps $0.50, after giving away $3.60 of the $4.10 it adds" is a finding.

## Write for minimal description length

Durable language, prose or code, is judged by how little it takes to carry
its information. The same information in fewer, plainer words or constructs
is better.

- The [Level-0 snippet](../../AGENTS-snippet.md#writing-register) owns the
  always-on writing register and communication shape for replies and durable text.
- Names and signatures are held to the same standard as sentences. A name
  that needs a comment to be understood is a name drawn wrong.
- Cut every sentence a competent reader could derive from the rest. Padding
  is a cost paid on every read. Cut sentences that hedge or describe the text
  itself. State a claim before its limits, and what something is before what
  it is not.
- Do not restate what a signature, type, or implementation already says.
  Governance prose (governing-intent.md) says why and what is out of scope;
  it never paraphrases the code beside it.

Brevity removes words, not reasoning. A list of disconnected facts is short
but costs the reader more. Keep the connectives that carry the argument:
because, so, but, therefore. A paragraph should read like an argument a
person would say aloud.

## Write from the accepted baseline

Agent turns are episodic. Each turn, and especially each turn after a
context compaction, overweights the immediately preceding state, so a
rejected intermediate attempt can feel like the beginning of the story. That
makes a common failure mode look locally reasonable:
`A requested → A+B implemented → B rejected → B removed`, followed by a
comment, design note, or evergreen entry explaining that the system
"intentionally avoids B". From the accepted baseline, B was never part of
the design. The correct durable story is `base → A`, unless B is an accepted
historical alternative the audience genuinely needs to understand.

Write from the coordinates above, not from conversational chronology. A
durable document never refers to a conversation, thread, or turn the reader
has not seen; "as discussed" and "per the earlier message" are residue. The
same final state may need different projections for a maintainer familiar with
an older release and for a new reader, but neither inherits accidental
intermediate states from the agent loop.

Apply the **residue test** to anything a rejected path introduced:

> If the rejected intermediate state had never existed, would this comment,
> abstraction, compatibility path, test, or explanation still be necessary?

If not, remove it from current text and code. Keep the rejected path only
where history itself is useful evidence, in the journal, VCS history, or
the idea DAG (agent-continuity.md, historical evidence). The operational
shorthand: **write as if the wrong turn never happened.** This is narrative
hygiene, not history deletion. Narratives are baseline-relative and
audience-relative, not turn-relative.

## Keep bookkeeping where its readers look

Governance envelopes belong in metadata, as governing-intent.md places them.
Provenance, verification against earlier runs, machine records, file
ownership, and open process questions serve maintainers and agents. They
belong in an appendix, a README, a journal, or module documentation. When the reader needs what a machine record shows, say
it in a sentence or a small table and name the file that holds the record.

## Failure modes

- A result stated as a bare value, with no unit, direction, or comparison.
- A list of short, true sentences with no "because", "so", or "but".
- A decision-changing caveat that appears only in the limits.
- A docstring that restates the signature and omits the reason.
- A README that explains what a rejected design would have done.
- Long sentences, passive voice, two names for one concept.

## Check before finishing

Read the title and the first paragraph as the intended reader with no other
context. They should be able to say why it matters, what was found or changed,
and what it means for them. Then ask: would I say these sentences aloud to this
person? Rewrite each sentence that fails.
