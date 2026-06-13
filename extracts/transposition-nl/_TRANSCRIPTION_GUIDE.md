# Transcription guide — `extracts/transposition-nl/` (the Dutch implementing text)

This directory holds the **diffable operative text of the Dutch transposition** — the analogue of the
template's `extracts/council/` versioned compromise sets. Each **significant version** of the implementing
text gets one file per `extract_slices` entry, prefixed with the document id:

- `36924_<slice>.md` — the bill as it stands (voorstel van wet 36924-2).
- A later version (e.g. after a nota van wijziging, or the enacted Staatsblad text) gets its own prefixed
  set (`stb-<nnnn>_<slice>.md`), structured identically so `git diff` shows what moved.

Slices (from [`tracker.yaml`](../../tracker.yaml)): `scope-art2`, `creditworthiness`, `minors-ban`,
`advertising`, `precontractual`, `arrears-forbearance`, `final-provisions`, `recitals`.

## Status: stubs — not yet transcribed
Files contain a header + the Wft/BW anchors referenced from `docs/` (flagged `TODO`). Fill from the
Kamerstukken when verbatim wording is needed.

## Source & method
- **Source:** the Kamerstukken text (voorstel van wet 36924-2 + MvT 36924-3), hosted by the Tweede Kamer;
  the AMvB from the internetconsultatie. These are **not tracked-change PDFs** — transcribe the
  consolidated statutory text manually. (The template's `pdf_changes.py` PyMuPDF driver does not apply and
  is unavailable on this host.)
- **Anchors:** anchor to the **Dutch provision** numbering — `<a id="wft-4-34a"></a>`, `<a id="bw-7-75-4"></a>`,
  `<a id="bgfo-advertising"></a>`, etc. — because `docs/provisions/*` and `data/positions.csv` link to
  these. Keep them stable across versions so links survive a nota van wijziging.
- **Faithfulness:** verbatim statutory wording (the amended article text as it would read); mark
  not-yet-final passages `[draft — verify against Staatsblad]`.

## Link check
`python3 ../../.claude/skills/transcribe-council-extract/linkcheck.py ../..` — must end "0 broken".
