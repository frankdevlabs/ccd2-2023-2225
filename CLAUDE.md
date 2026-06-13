# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

> **Built from the EU legislative-tracker template.** The file this repo follows is defined once in
> [`tracker.yaml`](tracker.yaml); identity values in the static docs are filled from it by
> `python3 bootstrap.py`. Per the maintainer's workflow, **do not re-run `bootstrap.py` once `STATUS.md`
> is filled** (it regenerates `STATUS.md` from the template and would wipe hand-written content).

## What this repo is

A personal, open **legislative tracker** — not a software project. It follows one EU instrument:
**Directive (EU) 2023/2225** — the *Consumer Credit Directive II (CCD2)* — and, principally, its
**transposition in the Netherlands**. The strand: consumer credit — BNPL / credit cards / authorised
overdrafts brought into scope, the creditworthiness test recast, a ban on credit to minors, tighter
advertising. (These facts come from [`tracker.yaml`](tracker.yaml).)

**Scope guard:** this repo tracks **CCD2 / Directive (EU) 2023/2225** and its NL transposition **only**.
It is *not* `Directive 2008/48/EC (CCD1)` (the predecessor CCD2 repeals from 20 Nov 2026), and *not* the
adjacent NL files (NPL Implementatiewet kredietservicers en kredietkopers; Wetsvoorstel kredietregistratie
WGK025409). The authoritative scope is [`tracker.yaml`](tracker.yaml) (`file_id` vs `keep_apart`).

There is no build, no app, no test suite. The "code" is Markdown + YAML + Python skill drivers.

## Directive adaptation (first directive in the family — read this)

The template ships shaped for a **regulation** in the Ordinary Legislative Procedure (OLP): a Commission
proposal that moves Commission → Council → Parliament → trilogue → adoption, with `extracts/commission/`
the base text and `extracts/council/` the versioned compromises. **CCD2 does not fit that shape** — its
EU legislative process is *finished* (adopted 18 Oct 2023, in force 19 Nov 2023). What is live is
(a) **national transposition** in the Netherlands and (b) **EU infringement**. So this repo inverts the
template:

| Template (regulation) | This repo (directive) |
|---|---|
| `extracts/commission/` — base text (the diff baseline) | **`extracts/directive/`** — the adopted Directive (EU) 2023/2225 (the baseline) |
| `extracts/council/` — versioned Council compromise texts | **`extracts/transposition-nl/`** — the Dutch implementing text (Wft / BW amendments), versioned across bill stages (submitted 36924-2 → post-nota → Staatsblad) |
| STATUS "Where each institution stands" — Commission proposer + Council/Parliament co-legislators | STATUS **two blocks**: (A) *EU instrument* — directive status + Commission-as-enforcer (infringement); (B) *Netherlands transposition* — Regering/MinFin · Tweede Kamer · Eerste Kamer · Implementatiebesluit (AMvB) · AFM |
| `docs/instruments/` — laws the regulation amends | CCD1 (repealed) + the **national** laws the transposition amends (Wft, BW Boek 7, Wet handhaving consumentenbescherming, Overgangswet NBW) |

The five-field per-actor skeleton (**Stage · Latest act · Owner · Position · Next**) is unchanged — it
fits the EU and NL actors directly. `tracker.yaml` carries directive-specific keys
(`instrument_type`, `directive:`, `transposition:`, `infringement:`) that the regulation template lacks;
these are the candidate additions to fold back upstream when the template grows directive support.

## Three-layer architecture (the big picture)

The repo separates three layers so links stay stable as the file evolves:

| Layer | Folder | What it is | Mutability |
|---|---|---|---|
| **Primary sources** | `sources/` | Register of every official document; committed copies where useful | append-only |
| **Operative-text extracts** | `extracts/` | Diffable plain-text transcriptions of the operative articles, one set per version | versioned |
| **Analysis** | `docs/` | Human-readable analysis linking *down* into the two layers above | living |

`extracts/directive/` is the **base text** (adopted CCD2 — the diff baseline), grouped by the directive's
own **chapters** (Ch I–XV): all 50 articles + all recitals + the annexes. `extracts/transposition-nl/`
holds the Dutch implementing text grouped by the bill's own **ARTIKEL** structure (`36924_artikel-I-wft`,
`-II-bw7`, `-III-whc`, `-IV-overgangswet`, `36924_slotbepalingen`, plus the draft `amvb_implementatiebesluit`).
A later version of the bill (after a nota van wijziging, or the enacted text) gets its own prefixed set so
`git diff` between versions is meaningful. The two layers' slice lists differ (directive ≠ Dutch numbering);
both are listed in [`tracker.yaml`](tracker.yaml) (`extract_slices`). Directive files anchor `article-N` /
`recital-N`; NL files anchor to Wft/BW provisions (`wft-4-34a`, `bw-7-75-4`, …).

> **Status:** operative-text extracts are **transcribed** — the directive verbatim from the EUR-Lex English
> text (CELEX 32023L2225) and the Dutch bill verbatim from the official Kamerstukken XML
> (`kst-36924-2`). The AMvB (`amvb_implementatiebesluit.md`) is a **draft summary** flagged `[DRAFT]` —
> refresh from the Staatsblad once the Implementatiebesluit is published.

## Single sources of truth (don't hand-edit downstream copies)

- **`data/documents.yaml`** is the canonical document register. `sources/README.md` is its *rendered*
  table — edit the YAML, then update the table to match. Never edit the table alone.
- **`data/tracker-state.yaml`** is **auto-maintained by the daily tracker** (a scheduled agent that
  hashes the watched sources T1-*/T2-*). "Do not edit by hand." `STATUS.md`'s **As of** = its `last_run`.
- **`data/positions.csv`** backs the provision comparison (columns: directive requirement vs NL
  implementation); keep it in sync with `docs/provisions/*` and `docs/what-changed.md`.
- **`STATUS.md`** is the one-screen current snapshot (format spec: `docs/reporting-standard.md`);
  **`TIMELINE.md`** is the full sourced chronology; **`docs/what-changed.md`** is the authority on what
  CCD2 changes vs current Dutch/CCD1 law.

## Adding a new transposition stage (the core workflow)

When a new Kamerstuk lands (nota naar aanleiding van het verslag, nota van wijziging, the adopted wet, or
the final Implementatiebesluit in the Staatsblad):

1. **Register the document** with the `register-document` skill — it appends to `data/documents.yaml` and
   renders `sources/README.md`.
2. **Update the pipeline** — `docs/transposition/nl-pipeline.md` stage table, `STATUS.md` (NL block +
   dashboard), `TIMELINE.md`.
3. **If the operative text changed** (a nota van wijziging or the enacted text), add/update the matching
   slice files under `extracts/transposition-nl/` so `git diff` against the prior version shows what moved,
   and refresh the affected `docs/provisions/*` and `data/positions.csv` rows.

> **Tooling note.** The template's `transcribe-council-extract` skill is built around `pdf_changes.py`
> (PyMuPDF) to recover tracked changes from Council PDFs. **PyMuPDF is unavailable on this host and the NL
> instruments are not tracked-change PDFs** — transcribe the Dutch text manually from the Kamerstukken
> HTML/text and the directive from EUR-Lex HTML. The pure-Python `linkcheck.py` driver still works and is
> the CI check.

## Conventions

- **Relative links only** inside the repo — never `github.com/...` absolute URLs (they break on rename/fork/mirror).
- **Deep-link into `extracts/*.md#anchor`, not `sources/*.pdf#page`** — GitHub's PDF viewer can't anchor to a page.
- **Document naming:** `<INSTITUTIONAL-ID>_<short-description>_<ISO-date>.<ext>` — stable number first, ISO
  date last (e.g. `36924-6_verslag_2026-05-21.pdf`, `32023L2225_directive_2023-10-30.pdf`).
- **Link checking (CI):** `lychee` runs on push/PR/weekly via `.github/workflows/link-check.yml`,
  configured by `lychee.toml`. Internal links are checked strictly; bot-blocking hosts (EUR-Lex,
  tweedekamer.nl, eerstekamer.nl, rijksoverheid.nl, overheid.nl, afm.nl, internetconsultatie.nl) are
  excluded. Run locally: `python3 .claude/skills/transcribe-council-extract/linkcheck.py .` (must end "0 broken").

## Disclaimer

Personal project, **not legal advice**, not any employer's view (`DISCLAIMER.md`). Original commentary is
CC BY 4.0 (`LICENSE`); EU documents and third-party works are not (`NOTICE`). Extracts are working
transcriptions — verify against the authoritative source before relying on them.
