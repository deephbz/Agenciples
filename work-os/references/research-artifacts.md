# Research Artifacts

Applies when framing a research question, running experiments, analyzing
data, or writing up results. Lineage: literate programming (Knuth),
reproducible research (Claerbout/Donoho, nbdev/Quarto), Zettelkasten
(Luhmann/Ahrens), evergreen notes (Matuschak), memex (Bush), design
doc/RFC culture, Hamming "You and Your Research".

## The three-artifact chain

Research work produces three durable artifacts, in order. Chat messages
are transport, not storage; anything that matters graduates to a file.

1. **Problem artifact** (before any work): question, context,
   motivation, desired behavior, known constraints, prior attempts,
   suspected failure modes, expected outputs. Rough is fine; it exists
   to force the thinking (writing *is* the thinking) and to give agents
   a bootstrappable context. If the problem can't be written down, it
   hasn't formed yet — writing it is the first experiment.
2. **Plan/design artifact** (before implementation): the decomposition,
   the approach, the tradeoffs considered. This is the human review
   checkpoint — reviewing a plan is cheap, reviewing 3000 lines of
   generated code is not. Practitioners converge on this independently:
   have the agent produce a reviewable design doc, not an ephemeral PR.
3. **Result artifact** (after): findings, interpretation, and the full
   provenance bundle (see below). The next round of work starts from
   this artifact, not from memory or chat scrollback.

**The transformation test.** An artifact counts as thinking only if it
contains connections, tensions, or claims that were not already in the
sources. Verbatim transcription and dutiful summarization are "research
cosplay" — they feel like work and store nothing. When producing a
result artifact, state at least: what was surprising, what this changes,
what it contradicts.

Decide once where each artifact type lives (e.g. a notes vault for
distillation, the repo's `docs/` for artifacts coupled to code) and keep
it consistent, so humans and agents both know where to look.

## Backend-first, dual display

Humans ingest visuals; agents operate on structured text. An artifact
that exists only as a rendered view is dead to half its audience, so
the canonical form is always the machine-operable backend; the visual is
a rendering.

- A chart is not a result. The result is: dataframe (or a
  content-addressed pointer), config, metrics, environment, code
  version, assumptions, and interpretation — with the figure rendered
  from it. If the figure is lost, regenerate it; if the backend is
  lost, the result is gone.
- Prefer open, plain, tool-independent formats: markdown, CSV, SQLite,
  JSON, plain-text config. Backend-first is about *format openness*,
  not retrieval sophistication — a flat markdown corpus grepped by CLI
  satisfies it as well as an embedding stack, at a fraction of the
  complexity. Add clever retrieval only when scale forces it.
- Declarative viz specs (Vega-Lite style: data + encoding as data) give
  dual display for free — the spec is agent-operable, the render is
  human-legible. Prefer them over imperative plotting when practical.

## Verification in research specifically

Predict before you run: write the expected result into the problem
artifact before executing the experiment. It is the cheapest possible
verification anchor, it converts every run into a calibration exercise,
and it is the difference between research and rationalization.
