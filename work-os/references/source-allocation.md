# Source Allocation and Literate Expression

Applies when deciding where a fact, spec, or procedure should live (doc,
diagram, type, public API, skill, script, library), when to move it, and how
to write it so that humans and agents read it cheaply. Lineage: DRY / single
point of truth (Hunt & Thomas), literate programming (Knuth), executable
specification, Unix prototyping culture (prototype in shell, rewrite in C),
gradual typing, minimum description length, ASD-STE100 Simplified Technical
English.

## One authoritative home per fact

Every spec, behavior, event sequence, hierarchy, and procedure has exactly
one source of truth; every other mention is a pointer to it. When the same
fact lives in a doc and in code, one of them is already stale, usually the
doc, silently. Trimming a doc down to pointers is not information loss;
duplication is what loses information, by making it ambiguous which copy is
authoritative.

Pointers run both directions: docs name the modules, types, and APIs that
now carry their specs; code (module docs, README headers) names the design
docs and diagrams that motivate it. A reader landing on either side reaches
the other in one hop.

## Truth migrates as work matures

Where the source of truth lives is a lifecycle property, not a fixed choice:

- **Shaping.** Everything is natural language: problem artifacts, design
  docs, diagrams with imagined type and API names. Docs-heavy is correct;
  there is no stable implementation to point at.
- **After initial R&D.** Core implementations and public surfaces
  stabilize. The spec for API shape, behavior, event sequences, and
  hierarchy migrates from docs and diagrams into concrete types, method
  signatures, and public surfaces. Code that is executed, tested, and
  versioned wins over prose that is merely read.
- **After migration.** Trim the doc: keep intent, rationale, constraints,
  and the alternatives that were genuinely considered, and replace
  duplicated spec with pointers to module, type, and API names. A doc that
  restates a type definition is stale the day after it is written.

Milestones and new R&D kickoffs are the checkpoints for re-allocation, the
same moments the diagrams get refreshed (research-artifacts.md). This is the
source-of-truth view of the ladder in stage-calibration.md.

## One medium, different properties

For a capable agent, natural language and programming language are not
different media: reading prose and reading code are the same act. They
differ in properties:

- **Verification.** Code can be executed, tested, and type-checked; prose
  can only be reviewed.
- **Reliability.** A script performs identically on every run; a
  natural-language instruction is re-interpreted each time it is read.
- **Concision.** Prose often says the same thing shorter, especially where
  judgment is required.
- **Interpretation floor.** Frontier models handle nuanced prose; smaller
  models follow explicit code more safely.

Allocate per statement by these properties, not by habit. Some brittle error
handling is painful to encode in code but reliable as one prose sentence an
agent applies with judgment; a procedure that must run identically every day
should not remain prose. The source bundle is a fluid composition of natural
and programming language, and the agent is expected to notice when the
current allocation is wrong and propose the move (human ratifies, per
agent-continuity.md).

Separation rules that apply to code apply to prose: separate by ontology
(one concept per unit) and by change velocity (stable core, fast periphery).

## Write for minimal description length

Durable language, prose or code, is judged by how little it takes to carry
its information. The same information in fewer, plainer words or constructs
is better. Concretely:

- The writing register (Simplified Technical English, subject-verb-object
  constructions) is an always-on rule in the Level-0 snippet. It applies to
  durable text as it applies to replies.
- Names and signatures are held to the same standard as sentences. A name
  that needs a comment to be understood is a name drawn wrong.
- Cut every sentence a competent reader could derive from the rest. Padding
  is a cost paid on every read.
- Do not restate what a signature, type, or implementation already says.
  Governance prose (governing-intent.md) says why and what is out of scope;
  it never paraphrases the code beside it.

## Interleave prose and code

Literate programming had the right instinct: put the explanation next to
the thing it explains. Two audiences, two placements:

The unit is one feature or one relatively standalone module. Treat the pair
below as a target shape, not a hard invariant.

- **User-facing documents** state which problem the artifact solves, why it
  is worth using, how to use it, worked examples, and the assumptions and
  invariants a caller may rely on. That is what, why, and how. One such
  document serves at once as requirements spec, design-choice record, and
  shaping artifact. It lives where users look (README, package docs).
- **Maintainer-facing material** states what users do not see but
  extenders must know: architecture, invisible constraints, non-functional
  requirements, the reason a design was chosen. When it is short and
  code-adjacent, it lives in the source: module header, class or function
  docstring where a real contract exists, the first cell of a notebook. A
  separate maintainer document exists only when the material is long or
  cross-cutting. It is read whenever someone extends the module, changes its
  implementation, or recomposes it.

The governance envelope of governing-intent.md is the minimal maintainer
note. Notebooks and literate reports are the same pattern applied to
research: prose and executable cells interleaved so that the narrative and
its evidence stay together.

## Write from the audience's baseline

Agent turns are episodic. Each turn, and especially each turn after a
context compaction, overweights the immediately preceding state, so a
rejected intermediate attempt can feel like the beginning of the story. That
makes a common failure mode look locally reasonable:
`A requested → A+B implemented → B rejected → B removed`, followed by a
comment, design note, or evergreen entry explaining that the system
"intentionally avoids B". From the accepted baseline, B was never part of
the design. The correct durable story is `base → A`, unless B is an accepted
historical alternative the audience genuinely needs to understand.

Before writing or revising any durable text (change description, comment,
docstring, design note, evergreen entry, migration narrative), recover three
coordinates:

1. **Audience** — who will consume this and what can be assumed?
2. **Starting point** — which accepted state, revision, or mental model does
   the audience begin from?
3. **Final accepted state** — what is true after this work?

Write from those coordinates, not from conversational chronology. A durable
document never refers to a conversation, thread, or turn the reader has not
seen; "as discussed" and "per the earlier message" are residue. The same
final state may need different projections for a maintainer familiar with an
older release and for a new reader, but neither inherits accidental
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

## The hardening gradient

Procedures typically enter as natural language and harden with use: an SOP
or skill doc → a bash script → a python module → a compiled library. Promote
when frequency, determinism, or performance justify the rigidity; demote back
toward prose when a hardened script turns out to encode assumptions that no
longer hold. Each promotion is also a verification upgrade, so the gradient
doubles as the cheapest path to anchored procedures (verified-claims.md).

## Failure modes

- A spec maintained in both a doc and a type.
- A docstring that restates the signature and omits the reason.
- A README that explains what a rejected design would have done.
- A daily procedure that is still a prose SOP after its tenth run.
- A maintainer note split into a separate document when three lines in
  the module header would do.
- Long sentences, passive voice, two names for one concept.
