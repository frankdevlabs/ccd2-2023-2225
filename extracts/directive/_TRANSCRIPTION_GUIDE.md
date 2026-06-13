# Transcription guide — `extracts/directive/` (the adopted CCD2 base text)

This directory holds the **diffable operative text of Directive (EU) 2023/2225 (CCD2)** — the baseline for
the tracker. One file per `extract_slices` entry in [`tracker.yaml`](../../tracker.yaml):
`scope-art2`, `creditworthiness`, `minors-ban`, `advertising`, `precontractual`, `arrears-forbearance`,
`final-provisions`, `recitals`.

## Status: stubs — not yet transcribed
The files currently contain a header + anchors only, flagged `TODO`. The analysis layer (`docs/`,
`STATUS.md`) is already populated from the research dossier; transcribe the operative text when you need
verbatim article wording or a `git diff` against the Dutch implementing text.

## Source & method
- **Source:** EUR-Lex, CELEX **32023L2225** — text at https://eur-lex.europa.eu/eli/dir/2023/2225/oj/eng
  (HTML). There is **no tracked-change PDF** for an adopted directive, so the template's
  `transcribe-council-extract` / `pdf_changes.py` (PyMuPDF) workflow does **not** apply — and PyMuPDF is
  unavailable on this host. Transcribe from the EUR-Lex HTML.
- **Faithfulness:** verbatim operative wording; preserve Article/Chapter/Section numbering; mark genuinely
  illegible passages `[illegible in source]`. This is primary legal text — not a summary.
- **Anchors:** `<a id="article-N"></a>` for articles, `<a id="recital-N"></a>` for recitals, so links from
  `docs/` stay stable. Anchors must line up with the matching slice in
  [`../transposition-nl/`](../transposition-nl/) where a topic maps across both (note: the directive and
  the Dutch bill renumber differently — anchor each layer to its **own** numbering).

## Link check
`python3 ../../.claude/skills/transcribe-council-extract/linkcheck.py ../..` — must end "0 broken".
