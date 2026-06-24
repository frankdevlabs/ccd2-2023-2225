---
name: resolve-tracker-issue
description: >-
  Triage and integrate ONE flagged tracker finding for the EU file this repo tracks (see tracker.yaml).
  Use when asked to resolve / triage / action a "[tracker] ..." GitHub issue,
  integrate a flagged source change into the repo, draft a plan for what the
  daily tracker found, or work through the open tracker issues. Produces one
  focused plan per issue under docs/triage/ (optionally a draft PR, optionally
  applying the downstream edits) — never bundles issues together.
---

# resolve-tracker-issue

The daily tracker cron (`~/law-tracker/run.sh`, weekdays ~12:00 UTC) detects what
is new across the watched sources and opens **one GitHub issue per hit**, labelled
`tracker` + a tier (`tier-1/2/3`) + a topic. Those issues are the flagged
information someone must integrate into the repo's analysis layers. This skill
turns one such issue into a focused, executable plan — and, on request, applies it.

## The one rule

**Operate on exactly ONE issue per run and per plan file.** Never combine issues.
Each flag gets its own `docs/triage/<date>-issue-<n>.md` and its own branch/PR.
If asked to "do them all", loop the resolver once per issue — separate plan, separate PR each.

## Inputs and modes

The entry point is the `/tracker-issue` command (or invoke this skill directly).
`$ARGUMENTS` is an issue number plus an optional flag:

- **(no number)** → list open, untriaged tracker issues and stop:
  `python3 .claude/skills/resolve-tracker-issue/tracker_issues.py --list`
- **`<n>`** → produce the plan for issue `<n>` in the working tree; do not branch. (on-the-fly review)
- **`<n> --pr`** → also branch, commit the plan, push, open a **draft** PR, and mark the
  issue triaged. This is what the cron uses. Plan only — no downstream content edits.
- **`<n> --fix`** → implies `--pr`, and additionally applies the downstream edits per the
  playbook on the same branch, running the link check before committing.

## Workflow

1. **Resolve the issue.** Run the helper to get the parsed, watchlist-joined record:
   ```bash
   python3 .claude/skills/resolve-tracker-issue/tracker_issues.py --issue <n>
   ```
   This returns `type`, `playbook`, `codes`, `source_url`, `detected_at`, `why`,
   `excerpt`, `affects[]`, `document_id`, `watchlist_label`. The `affects` list is the
   set of downstream files to consider — it comes from the watchlist in `tracker.yaml`.

2. **Gather evidence.** Fetch `source_url` (WebFetch) and compare against what the repo
   already records. The issue `excerpt` is a lead, not ground truth — confirm against the
   live source and apply the **scope rule** from [`tracker.yaml`](../../../tracker.yaml):
   in-scope only if it concerns this repo's file (`file_id`), not a `keep_apart` sibling.
   (Watch foreign-language sources especially — check the file number, not just the topic.)
   If it is a `keep_apart` file or otherwise off-topic, the plan's recommendation is to
   **close the issue as not-relevant** — do not edit the repo.
   - **A low-tier source can carry Tier-1-grade substance — upgrade the playbook.** The flagged
     source's tier sets *priority*, not the *type* of integration. If a Tier-2/Tier-3 item (NGO/media/
     law-firm/Bluesky) surfaces a **primary document** (an adopted/amended Kamerstuk, a Staatsblad
     text, an AFM rule/guidance, a Commission infringement act), do **not** stay on
     `stakeholder-social` — run the matching `institutional` (or transposition) playbook on the
     underlying document. A Tier-3 post can be the first signal of a new bill stage.
   - **Bot-blocked official sources.** Some authoritative hosts IP-block this VPS (the doceo /
     EUR-Lex AWS-WAF pattern): EUR-Lex returns a WAF challenge / 0 bytes to `curl`/WebFetch from this
     host. You can confirm a document's **existence + metadata** via a listing page, but obtain the
     operative text another way — a browser download, or the national-parliament XML
     (`tweedekamer.nl`/`eerstekamer.nl` Kamerstukken XML). If only metadata/a cover page is confirmed,
     register **metadata only** and set `pending_operative_text` (below) — do not assert per-provision
     content from a screenshot or a summary.
   - **Dated committee activities / meetings — verify against the TK Open-Data feed, not the HTML.**
     `T1-02` (and any TK dossier source) flags scheduled activities by hashing bot-served HTML, and the
     issue body's dates are an **LLM extraction** — as is a WebFetch "confirmation" of the same page (both
     have misdated activities, e.g. issue #3 stamped the 12 May briefing / 19 May inbreng as "16 Jun").
     Before writing **any** dated activity into STATUS/TIMELINE/pipeline, cross-check each date against the
     structured `Activiteit` feed (open, not bot-blocked):
     ```
     gegevensmagazijn.tweedekamer.nl/OData/v4/2.0/Activiteit
       ?$filter=year(Datum) eq <yr> and contains(tolower(Onderwerp),'consumentenkrediet')
       &$select=Datum,Aanvangstijd,Eindtijd,Soort,Status&$orderby=Datum
     ```
     If a claimed date is not in the feed, **do not assert it** — drop it or record it as "unverified".
     Procedurevergaderingen carry a generic Onderwerp (won't match the bill title) — confirm those from the
     agenda PDF or mark the dossier as an "agenda item — unverified".

3. **Run the matching playbook** (below) to determine the concrete edits.

4. **Write the plan** to `docs/triage/<date>-issue-<n>.md` using the template below.
   `<date>` is the issue's `detected_at` date (fallback: today, UTC).

5. **Per the mode:** stop (default), open a draft PR (`--pr`), or apply + PR (`--fix`).
   See "Opening the PR".

## Playbooks (by `playbook` field)

### `institutional` — Tier-1 procedural / advisory hit
Decide whether the change is **substantive** (a real procedural step or position) or
**noise** (rotating widget, transparency-meeting log, unchanged re-publish — common for
T1-05/06/07). If substantive:
- New procedural fact (committee referral, rapporteur, vote, meeting date) → update the
  relevant institution field (and **Next milestones**) in `STATUS.md` and add a sourced
  row to `TIMELINE.md`.
- New advisory output (EDPB/EDPS, ECB, EESC) → update the matching `docs/advisory/*.md`
  and `docs/institutional-positions.md`; register the document in `data/documents.yaml`
  (then regenerate the `sources/README.md` table) if it is a new committed document.
- Always check `STATUS.md`'s "What changed" table (full version: `docs/what-changed.md`) before asserting a
  feature of the proposal — several widely-reported features were deleted/moved.

### `council-text` — Tier-1 Council source (T1-08/T1-09), highest priority
A new ST LIMITE compromise text is the heaviest case:
- Register the document in `data/documents.yaml`; update `sources/README.md`.
- **Hand off transcription to the `transcribe-council-extract` skill** — do not transcribe
  by hand. Produce the extract set (one file per `tracker.yaml` `extract_slices`) under
  `extracts/council/ST-<nnnn>-<yyyy>...`.
- Cascade: update the affected `docs/provisions/*.md`, `docs/institutional-positions.md`,
  `data/positions.csv`, `STATUS.md` + the full `docs/what-changed.md` diff table, `TIMELINE.md`.
- Because this spans many files, a `--fix` run here is large; prefer producing the plan and
  letting a human drive the transcription, unless explicitly told to apply.

### `stakeholder-social` — Tier-2 NGO/media/law-firm or Tier-3 Bluesky
Judge whether the item is a substantive position/analysis or just commentary:
- Substantive → add/extend an entry in `docs/stakeholders.md` (and `docs/fault-lines.md`
  for noyb/EDRi positions; `docs/member-state-positions.md` for national-DPA / NL items),
  cross-linking the relevant `docs/provisions/*`.
- Pure commentary / off-topic → recommend closing the issue; no repo edit.

### `source-health` — the weekly `[tracker] Source health` issue (or a state-blocked issue)
Not a content change — a fetch problem. For each listed code:
- Check its `url` in `tracker.yaml` (`watchlist.sources`) and `data/documents.yaml`. Is it stale,
  moved, or just temporarily blocked (Cloudflare/AWS-WAF 403/202/503)?
- Propose: a corrected URL, a fallback endpoint, or marking the source unreachable.
  Defunct feed → propose removal/replacement in `tracker.yaml` and the cron
  prompt. This mirrors the earlier source-health PR pattern. The plan lists the per-source
  verdict; `--fix` edits `tracker.yaml`/`documents.yaml` accordingly.

### `run-summary` / `skip`
Suppressed-hits summary issues need no integration — the helper already filters them out.

## Follow-up trigger: `pending_operative_text`

When a document is registered **metadata-only** because its operative text could not be retrieved yet
(a bot-blocked host, no mirror, or the text simply not published at this stage), leave a
machine-detectable marker so the work re-surfaces instead of being silently lost:
- In the `data/documents.yaml` entry, set `pending_operative_text: true` and an appropriate `access`
  (`screenshot-only` / `cite-only`), with a `notes` line stating what is confirmed (cover-page metadata)
  vs pending (per-provision content).
- Add one **Next milestones to watch** line in `STATUS.md` naming the document and the retrieval
  condition (e.g. "<Kamerstuk> operative text — pending Kamerstukken XML").
- The daily tracker / `--list` treats any `pending_operative_text: true` entry as actionable: when the
  text becomes retrievable (XML/mirror appears or the file is supplied), build the extract, fill the
  per-provision cells, and clear the flag.

## Plan file template

```markdown
# Triage: issue #<n> — <short title>

- **Issue:** <issue_url>
- **Source:** <watchlist_label> (`<code>`) — <source_url>
- **Detected:** <detected_at>
- **Type / playbook:** <type> / <playbook>

## What was flagged
<the `why`, plus the confirmed observation from fetching the source>

## Assessment
<substantive vs noise / in-scope vs AI-Act-only; the judgement and why>

## Plan
<numbered, concrete steps — exact files from `affects` and the edit each needs.
 If the recommendation is "close as not-relevant", say so and why.>

## Verification
<how to confirm: link check, the specific doc/section to read, gh issue close>
```

## Opening the PR (`--pr` / `--fix`)

```bash
date_tag="<detected_at date or today>"
git checkout -b "triage/${date_tag}-issue-<n>"
# (--fix only) apply the playbook edits here, then:
python3 .claude/skills/transcribe-council-extract/linkcheck.py .   # must end "0 broken"
git add docs/triage/${date_tag}-issue-<n>.md <any --fix edits>
git commit -m "triage: plan for #<n> (<code>)"
git push -u origin "triage/${date_tag}-issue-<n>"
gh label create plan-drafted --description "Tracker issue has a triage plan PR" --color ededed --force
gh pr create --draft --title "triage: #<n> <short title>" \
  --body "Resolves the integration plan for #<n>. See docs/triage/${date_tag}-issue-<n>.md.\n\nCloses #<n> once merged." 
gh issue edit <n> --add-label plan-drafted
gh issue comment <n> --body "Triage plan drafted: <PR link>."
```

All plan PRs share the single `docs/triage/` folder. The `plan-drafted` label is what makes
the cron loop and `--list` skip already-handled issues, so always apply it on `--pr`/`--fix`.

## Conventions (inherited from the repo)
- Relative links only inside the repo; deep-link to `extracts/*.md#anchor`, not PDF pages.
- The link check must pass ("0 broken") before any `--fix` commit.
- Personal tracker, not legal advice; extracts are working transcriptions — verify against source.
