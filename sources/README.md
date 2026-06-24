# Document register

> This is the *rendered* table of [`data/documents.yaml`](../data/documents.yaml) — the single source of
> truth. **Edit the YAML, then mirror the table here; never edit the table alone.** The
> `register-document` skill does both. NL parliamentary documents are hosted by the Tweede/Eerste Kamer;
> we link the authoritative copy rather than committing PDFs (the hosts bot-block — see `lychee.toml`).
> Link the markdown extracts (`extracts/...md#anchor`), not source pages.

## EU instrument

| ID | Title | Date | Body | Provenance |
|---|---|---|---|---|
| `CELEX-32023L2225` | Directive (EU) 2023/2225 (CCD2) | 2023-10-18 | EP & Council | [EUR-Lex / ELI](https://eur-lex.europa.eu/eli/dir/2023/2225/oj) |
| `NIM-32023L2225` | National transposition measures (NIM index) | live | European Commission | [EUR-Lex NIM](https://eur-lex.europa.eu/legal-content/EN/NIM/?uri=CELEX:32023L2225) |
| `INF-26-115` | Letter of formal notice to NL (CCD2 transposition) | 2026-01-30 | European Commission | [press corner](https://ec.europa.eu/commission/presscorner/detail/en/inf_26_115) |

## NL transposition — the wet (Kamerstuk 36924)

| ID | Title | Date | Body | Provenance |
|---|---|---|---|---|
| `36924-2` | Voorstel van wet | 2026-04-01 | Ministerie van Financiën | [TK dossier](https://www.tweedekamer.nl/kamerstukken/wetsvoorstellen/detail?qry=wetsvoorstel%3A36924&cfg=wetsvoorsteldetails) |
| `36924-3` | Memorie van toelichting | 2026-04-01 | Ministerie van Financiën | [TK download](https://www.tweedekamer.nl/downloads/document?id=2026D15852) |
| `36924-4` | Advies Raad van State + nader rapport | 2026-04-02 | Raad van State | [TK download](https://www.tweedekamer.nl/downloads/document?id=2026D15855) |
| `36924-5` | Brief regering — Bescherming consument bij achteraf betalen (BNPL) | 2026-04-02 | Ministerie van Financiën | [TK download](https://www.tweedekamer.nl/downloads/document?id=2026D15849) |
| `36924-6` | Verslag (vaste commissie voor Financiën) | 2026-05-21 | Tweede Kamer | [TK dossier](https://www.tweedekamer.nl/kamerstukken/wetsvoorstellen/detail?qry=wetsvoorstel%3A36924&cfg=wetsvoorsteldetails) |
| `36924-7` | Brief regering — Reactie op verzoek commissie over de implementatie | 2026-06-23 | Ministerie van Financiën | [TK download](https://www.tweedekamer.nl/downloads/document?id=2026D31983) |

## NL transposition — the Implementatiebesluit (AMvB)

| ID | Title | Date | Body | Provenance |
|---|---|---|---|---|
| `AMvB-WGK026387` | Implementatiebesluit (internetconsultatie) | 2025-12-22 | Ministerie van Financiën | [internetconsultatie](https://www.internetconsultatie.nl/implementatiebesluitrichtlijnconsumentenkrediet/b1) |

## Supervisory & sector

| ID | Title | Date | Body | Provenance |
|---|---|---|---|---|
| `AFM-BNPL-MARKTUPDATE-2025` | AFM — Marktupdate BNPL 2025 | 2025-06-01 | AFM | [AFM report (PDF)](https://www.afm.nl/~/profmedia/files/rapporten/2025/rapport-marktupdate--bnpl-2025-ned.pdf) |
| `VFN-CONSULTATIEREACTIE-2025` | VFN — consultatiereactie CCD II | 2025-05-12 | VFN | [VFN (PDF)](https://www.vfn.nl/wp-content/uploads/2025/05/VFN_20250512_Consultatiereactie-Wetsvoorstel-CCD-II_Definitief-1.pdf) |
| `BNPL-GEDRAGSCODE-2025` | Government statement — BNPL rules / Gedragscode | 2025-10-31 | Ministerie van Financiën | [rijksoverheid](https://www.rijksoverheid.nl/actueel/nieuws/2025/10/31/nieuwe-regels-voor-buy-now-pay-later) |

## NL parliamentary scrutiny — Kamervragen (BNPL / achteraf betalen)

> Tweede Kamer written questions + Aanhangsel answers on the BNPL / Klarna / achteraf-betalen conduct CCD2
> brings into scope. `ID` = the answer document (Aanhangsel); the originating `kv-tk-…` question and any
> *Uitstel beantwoording* are in the `data/documents.yaml` notes. Auto-tracked via watchlist `T1-09`.
> CCD2 link is thematic — verify the answer text before citing a specific government position.

| ID | Subject (asker) | Answer date | Body | Provenance |
|---|---|---|---|---|
| `ah-tk-20222023-1339` | Buy Now, Pay Later-betaaldiensten (Kathmann) | 2023-01-25 | Tweede Kamer | [officielebekendmakingen](https://zoek.officielebekendmakingen.nl/ah-tk-20222023-1339.html) |
| `ah-tk-20232024-2019` | Verplichte leeftijdscontrole bij BNPL (Mohandis, Ceder) | 2024-06-20 | Tweede Kamer | [officielebekendmakingen](https://zoek.officielebekendmakingen.nl/ah-tk-20232024-2019.html) |
| `ah-tk-20242025-1089` | 'Einde Buy Now Pay Later stap dichterbij' (Inge van Dijk) | 2025-01-21 | Tweede Kamer | [officielebekendmakingen](https://zoek.officielebekendmakingen.nl/ah-tk-20242025-1089.html) |
| `ah-tk-20242025-1088` | 'Kan kabinet niet optreden tegen buy now pay later?' (Inge van Dijk) | 2025-01-21 | Tweede Kamer | [officielebekendmakingen](https://zoek.officielebekendmakingen.nl/ah-tk-20242025-1088.html) |
| `ah-tk-20242025-1087` | Achteraf betalen in fysieke winkels (Kisteman, Van Eijk, Aukje de Vries) | 2025-01-21 | Tweede Kamer | [officielebekendmakingen](https://zoek.officielebekendmakingen.nl/ah-tk-20242025-1087.html) |
| `ah-tk-20242025-2274` | Klarna verdient aan incassokosten (Welzijn) | 2025-05-22 | Tweede Kamer | [officielebekendmakingen](https://zoek.officielebekendmakingen.nl/ah-tk-20242025-2274.html) |
| `ah-tk-20252026-1015` | 'Koop nu, baal later': Klarna-incassotrajecten (Inge van Dijk, Hamstra; Ceder) | 2026-02-03 | Tweede Kamer | [officielebekendmakingen](https://zoek.officielebekendmakingen.nl/ah-tk-20252026-1015.html) |
| `ah-tk-20252026-1198` | Financiële risico's van achteraf betalen voor jongeren (Inge van Dijk, Hamstra) | 2026-03-02 | Tweede Kamer | [officielebekendmakingen](https://zoek.officielebekendmakingen.nl/ah-tk-20252026-1198.html) |
| `ah-1251487` | Kifid-uitspraak incassodienstverlening Klarna (Ceder) | 2026-06-02 | Tweede Kamer | [officielebekendmakingen](https://zoek.officielebekendmakingen.nl/ah-1251487.html) |
