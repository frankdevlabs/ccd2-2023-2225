# CCD2 (Directive (EU) 2023/2225) & its Dutch implementation — tracker

A personal, open tracker for the **Consumer Credit Directive II (CCD2)** — *Directive (EU) 2023/2225* —
and, principally, its **transposition in the Netherlands** (Implementatiewet herziene richtlijn
consumentenkrediet, Kamerstuk **36924**, + the Implementatiebesluit AMvB). Strand: consumer credit —
BNPL / credit cards / authorised overdrafts brought into scope, the creditworthiness test recast, a ban
on credit to minors, and tighter advertising rules.

> **Built from the EU legislative-tracker template.** The file this repo follows is set once in
> [`tracker.yaml`](tracker.yaml); `python3 bootstrap.py` fills the identity tokens in this README,
> `CLAUDE.md` and `STATUS.md`.
>
> **First directive in the family.** The template is shaped for a *regulation* in the Ordinary
> Legislative Procedure. CCD2's EU process is already **finished** (adopted 18 Oct 2023, in force 19
> Nov 2023), so this repo inverts the model: the **adopted directive is the base text** and the
> **Dutch implementing instruments are the versioned layer**. The live tracking is national
> transposition + EU infringement. See [`CLAUDE.md`](CLAUDE.md) → *Directive adaptation*.

> **Scope.** This repo tracks **CCD2 / Directive (EU) 2023/2225** and its NL transposition **only**. It
> is **not** `Directive 2008/48/EC (CCD1)` — the predecessor CCD2 repeals from 20 Nov 2026 — nor the
> adjacent NL files (NPL Implementatiewet kredietservicers en kredietkopers; Wetsvoorstel
> kredietregistratie WGK025409). See [`tracker.yaml`](tracker.yaml) (`file_id` vs `keep_apart`).

**Current status (high level):** see the full snapshot in **`STATUS.md`**. As of the last update: NL
**missed** the 20 Nov 2025 transposition deadline; the Commission sent a **letter of formal notice** on
30 Jan 2026; bill 36924 is in Tweede Kamer written preparation (verslag 36924-6, 21 May 2026); the AMvB
has been consulted but not finalised; application date 20 Nov 2026.

---

## How this repo is organised

The repo deliberately separates three layers so that links stay stable over time:

| Layer | Folder | What it is | Mutable? |
|---|---|---|---|
| **Primary sources** | [`sources/`](sources/) | Register of every official document, with links to authoritative copies | append-only |
| **Operative-text extracts** | [`extracts/`](extracts/) | Diffable plain-text of the operative articles — [`directive/`](extracts/directive/) is the adopted CCD2 base text, [`transposition-nl/`](extracts/transposition-nl/) the Dutch implementing text across bill stages | versioned |
| **Analysis** | [`docs/`](docs/) | The human-readable analysis that links *to* the layers above | living |

### Navigation

- **`STATUS.md`** — where transposition stands right now (one screen).
- **[`TIMELINE.md`](TIMELINE.md)** — full chronology, every entry linked to a source.
- **[`docs/summary.md`](docs/summary.md)** — the plain-language summary.
- **[`docs/directive.md`](docs/directive.md)** — digest of the adopted CCD2 (identification, key dates, scope).
- **[`docs/transposition/nl-pipeline.md`](docs/transposition/nl-pipeline.md)** — the NL parliamentary pipeline (bill 36924).
- **[`docs/transposition/implementatiebesluit.md`](docs/transposition/implementatiebesluit.md)** — the AMvB (BGfo / Besluit kredietvergoeding).
- **[`docs/transposition/supervision-afm.md`](docs/transposition/supervision-afm.md)** — AFM as supervisor, BNPL market context.
- **[`docs/infringement.md`](docs/infringement.md)** — EU enforcement (letter of formal notice).
- **[`docs/fault-lines.md`](docs/fault-lines.md)** — the contested issues.
- **[`docs/stakeholders.md`](docs/stakeholders.md)** — sector / industry / NGO positions.
- **[`docs/provisions/`](docs/provisions/)** — one file per tracked substantive change.
- **[`docs/instruments/`](docs/instruments/)** — CCD1 + the national laws the transposition amends.
- **[`docs/what-changed.md`](docs/what-changed.md)** — provision-by-provision: current law → CCD2 → Dutch bill.
- **[`sources/README.md`](sources/README.md)** — the document register.
- **[`extracts/directive/`](extracts/directive/)** — base-text extracts of CCD2 (the diff baseline).
- **[`extracts/transposition-nl/`](extracts/transposition-nl/)** — operative-text extracts of the Dutch implementing instruments.

---

## Linking conventions

1. **Relative links only.** Never link to `github.com/...` absolute URLs inside the repo — relative
   paths survive renames, forks and a move to a private mirror.
2. **Deep-link into extracts, not PDFs.** GitHub's PDF viewer does not support reliable `#page=`
   anchors, so to point at a specific article use the markdown extract's heading anchor, e.g.
   `extracts/directive/scope-art2.md#article-2` or `extracts/transposition-nl/36924_creditworthiness.md#wft-4-34a`.
3. **Source documents are committed in `sources/`.** The register records the document number, the
   authoritative URL, and any national-parliament record or mirror so provenance stays traceable. See
   [`NOTICE`](NOTICE) §2. The markdown extracts remain the *linkable* working copies.

## Naming convention for documents

`<INSTITUTIONAL-ID>_<short-description>_<ISO-date>.<ext>` — the stable document number first, ISO date
last, so chronological sort always works (e.g. `36924-6_verslag_2026-05-21.pdf`,
`32023L2225_directive_2023-10-30.pdf`).

## Contributing / adding a document

Open an issue using [`.github/ISSUE_TEMPLATE/new-document.md`](.github/ISSUE_TEMPLATE/new-document.md),
or run the `register-document` skill: it adds the entry to [`data/documents.yaml`](data/documents.yaml)
and regenerates the register row in [`sources/README.md`](sources/README.md).

---

## Disclaimer & licence

This is a **personal project** published in a personal capacity. It is **not legal advice** and does
**not** represent the views of any employer. See **[`DISCLAIMER.md`](DISCLAIMER.md)**.

- Original analysis and commentary in this repo: licensed under **CC BY 4.0** — see [`LICENSE`](LICENSE).
- EU documents and third-party works are **not** covered by that licence — see [`NOTICE`](NOTICE).
