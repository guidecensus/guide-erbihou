# L3 closeout — corpus-wide retraction / withdrawal check

**Date of check:** 2026-11-06 (statuses as recorded in PubMed / Europe PMC on this date).
**Scope:** every unique citation classified in L3 batches 1–5 for which a PMID could be
established from production artefacts, plus a pivotal-completion pass for pivotals no
artefact identified. **No production file was modified.** Machine-readable results:
`retraction_check_results.json` (per-PMID status, signals, evidence, doc/ref/item mapping).

## Headline counts

| Quantity | n |
|---|---|
| Classified citations in designs files (30 documents; ARIA2025R1 has zero citations) | 8,660 entries |
| (doc, ref) pairs with an established PMID | 4,989 (57.6%) |
| Unique PMIDs in the corpus mapping | 4,605 |
| Records checked in total (incl. 12 supplementary identity / cited-version checks) | 4,615 |
| CLEAR | 4,116 |
| CORRECTION_ONLY (erratum/correction/update links only — counted, not a flag) | 493 |
| WITHDRAWN | **3** |
| RETRACTED | **1** |
| EXPRESSION_OF_CONCERN | **2 records = 1 citation** (the notice + the paper it concerns) |
| IS_RETRACTION_NOTICE (a cited record that is itself a retraction notice) | 0 |
| CHECK_FAILED (no status determinable after all retries) | **0** |

**Bottom line: 5 flagged citations** — 3 withdrawn, 1 retracted, 1 under an expression of
concern. They touch 7 item-attachments; **3 are pivotals**. Every pivotal in the corpus was
checked except 14 item-instances (12 refs) whose PMIDs could not be established (listed
under Coverage).

## Flagged citations (with item / pivotal trace)

### WITHDRAWN — all three are the previously known flags, re-verified (see below)
1. **PMID 30184244** — "WITHDRAWN: Topical and systemic antifungal therapy for the
   symptomatic treatment of chronic rhinosinusitis" (Cochrane, CD008263.pub3).
   Cited as AAO2025SIN2 ref 370 → **pivotal of AAO2025SIN2-KAS12**.
2. **PMID 25924708** — "WITHDRAWN: Zinc for the common cold" (CD001364.pub5).
   Cited as EPOS2020 ref C4-217 → **pivotal of EPOS2020-16**.
3. **PMID 27110699** — "WITHDRAWN: Nasal saline irrigations for the symptoms of chronic
   rhinosinusitis" (CD006394.pub3). Cited as EPOS2020 ref C7-130 → **pivotal of
   EPOS2020-69**; also cited as ICAR2021RS ref 1051 → attachment in ICAR2021RS-21
   (not pivotal there).

### RETRACTED — new find
4. **PMID 37062296** — Jiang F et al., "Association between hearing aid use and all-cause
   and cause-specific dementia: an analysis of the UK Biobank cohort", *Lancet Public
   Health* 2023. PubMed pubtypes include **'Retracted Publication'**; CommentsCorrections
   carries **RetractionIn → notice PMID 38101424**. Cited as AAO2024ARHL ref 3 →
   attachments in **AAO2024ARHL-KAS02 and AAO2024ARHL-KAS06**; **not a pivotal**.
   Note: the ARHL guideline's own reference list already prints this citation with a
   "RETRACTED:" prefix, and the batch-1 designs entry's stored pubtypes already contained
   'Retracted Publication' — the status was visible in production artefacts but had never
   been flagged until this check.

### EXPRESSION OF CONCERN — one citation, two records
5. **PMID 11087881** — Bombardier C et al. (VIGOR Study Group), "Comparison of upper
   gastrointestinal toxicity of rofecoxib and naproxen in patients with rheumatoid
   arthritis", *N Engl J Med* 2000. The paper's own PubMed record carries
   **ExpressionOfConcernIn → PMID 16339408** (the NEJM expression-of-concern notice,
   itself pubtyped 'Expression of Concern'). Cited as AAO2021OPI ref 140 → attachment in
   **AAO2021OPI-KAS06**; **not a pivotal**. (Production's title match for this ref had
   latched onto the notice record 16339408 — see Identity slips; both records were
   checked, and the concern attaches to the cited paper either way.)

## The three mandatory re-verifications — current status with evidence

- **(a) PMID 30184244 (SIN2 KAS12 pivotal): WITHDRAWN.** PubMed title: "WITHDRAWN: Topical
  and systemic antifungal therapy for the symptomatic treatment of chronic rhinosinusitis";
  Cochrane Database Syst Rev, DOI 10.1002/14651858.CD008263.pub3. Europe PMC pubtypes
  concur (Meta-Analysis / Systematic Review / systematic-review).
- **(b) PMID 25924708 (EPOS C4-217, zinc CD001364): WITHDRAWN.** Title "WITHDRAWN: Zinc
  for the common cold" (CD001364.pub5). Abstract opens: "September 2016 updated withdrawal
  notice — This Cochrane Review was withdrawn in April 2015, and this withdrawal notice
  was updated in September 2016. The review was withdrawn as result of comments submitted
  via the Cochrane Library by Harri Hemilä in February 2015. Hemilä identified multiple
  errors …"
- **(c) PMID 27110699 (EPOS C7-130, saline CD006394): WITHDRAWN.** Title "WITHDRAWN:
  Nasal saline irrigations for the symptoms of chronic rhinosinusitis" (CD006394.pub3).
  Abstract: "Review withdrawn from Issue 4, 2016. Review replaced by 'Saline irrigation
  for chronic rhinosinusitis' (Chong 2016). The editorial group responsible for this
  previously published document have withdrawn it from publication."

## Check-failed disposition

Final CHECK_FAILED = **0**. Four PMIDs (20301525, 20301607, 24049842, 29261934) were
absent from all NCBI efetch batch responses and from individual efetch retries; all four
were recovered through the Europe PMC fallback channel and classified CLEAR from their
Europe PMC records (each entry's `checked_via` says so). No fetch failure was ever read
as evidence of status.

## How the PMID corpus was built (provenance, in priority order)

| Source | (doc, ref) pairs |
|---|---|
| RS strict re-verification records (`rs_reverify_results.json`, supersedes designs metadata) | 139 |
| Structured `pmid` fields in designs files (top-level or `evidence`) | 751 |
| Adjudication pack-N files (EPOS + batch-5 candidate fetches) | 24 |
| Batch-3 pivotal-pack fetches (classification checkpoints added no new pairs) | 378 |
| SSNHL pilot Europe PMC file | 7 |
| PMIDs printed in final-rows / adjudication text | 179 |
| Exact-title resolution of stored matched-record titles (Europe PMC) | 3,441 |
| Pivotal-completion pass (strict gates on the reflist cite; see below) | 70 |
| **Total** | **4,989** |

Title resolution: 3,576 unique stored titles queried (OR batches of 8, then individual
retry); **3,401 resolved (95.1%)**, 141 ambiguous (≥2 records share the exact title —
excluded), 34 unresolved. Acceptance required an exactly equal normalised title and a
single carrying PMID.

Pivotal completion: of 760 pivotal item-instances, 662 were already PMID-covered;
a targeted pass on the remaining 82 unique (doc, ref) pairs — guideline-printed
[PubMed:] ids, DOI lookup, TITLE phrase and keyword routes, all under the settled strict
gates (title overlap ≥ 0.85 both ways, first-author agreement, year ± 1) — resolved 70.
**Final pivotal coverage: 746/760 instances (98.2%).** The 12 unresolved pairs
(14 instances): AAO2013BELL|54 (KAS03), AAO2016OME|7 (KAS01A/B), AAO2020EPI|5 (KAS01),
AAO2025SIN2|66 (KAS01B), ICAR2021RS|1780 (RS-69), ICAR2022OLF|1378 (OLF-27),
ICAR2022OLF|1504 (OLF-43), ICAR2023AR|1045 (AR-28), ICAR2023AR|2063 (AR-47/58),
ICAR2024TUM|1372 (TUM-48), ICAR2024TUM|1677 (TUM-59), ICAR2024TUM|2103 (TUM-76).
These 14 pivotals' retraction status is **unknown**, not clear.

## Coverage gaps (honest accounting)

The 3,671 designs entries without a PMID are concentrated where production never
established a record identity: batch-3 table-design refs (ICAR2023AR 883/1,076 unmapped,
ICAR2024TUM 762/859, ICAR2019ESBS 384/467, ICAR2022OLF 268/415, ICAR2021RS 200/695),
SSNHL (177/257 — most SSNHL designs survive only as bare strings in the merged file),
and refs whose stored titles were ambiguous/unresolved (175 titles corpus-wide).
Per-document mapped shares range from 88% (EAACI2018) to 11% (ICAR2024TUM).
Re-matching those citations from scratch solely for this check would have repeated
exactly the weak-route matching the RS re-verification audit discredited, so they are
reported as a gap, not filled. Non-pivotal attachments in those documents are therefore
only partially covered; all pivotals are covered except the 14 instances above.

## Side findings (for parent adjudication — nothing was changed)

### 1. RS designs file carries stale record metadata for re-verified refs (39 conflicts)
For 39 of the 148 strictly re-verified RS refs, `ICAR2021RS_designs.json` still holds the
OLD wrong record's `pmid`/`title` (the design values were corrected; the record metadata
was not, where the design itself did not change). This check used the re-verification
records as authoritative. Any future consumer of the designs file's `pmid`/`title` fields
for those refs would inherit the wrong identity.

### 2. EPOS C4-203 cited-version split (1 conflict, benign)
The EPOS designs file matches C4-203 to the 2015 update (PMID 26387658, checked CLEAR);
the adjudicated cited version is the 2009 original (PMID 19588387), checked
supplementarily: CORRECTION_ONLY (an UpdateIn link only). Both records are clean of
retraction signals.

### 3. Identity slips: 11 citations whose production match is a notice/wrong record
Detected by scanning checked records for notice-type titles and adjudicating each
against the guideline's own reflist cite. In every case the actually-cited paper was
then checked (all CLEAR or CORRECTION_ONLY — no additional retraction flags), and
**none of the 11 refs is a pivotal anywhere**. Six slips have a design consequence
(stored design 'other' where the true record is an RCT or SR/MA — attachment counts
in the listed items are potentially understated by one):

| Cited as | Checked (wrong) record | Actually cited (checked) | Stored design vs true type | Items containing the ref |
|---|---|---|---|---|
| AAO2024AIT ref 64 | 28932383 (erratum to the review's *protocol*) | **28493631** Dhami 2017 ARC SR/MA — CLEAR | other vs **SR/MA** | KAS01, KAS05, KAS12 |
| AAO2024AIT ref 149 | 28932386 (erratum to the protocol) | **28543086** Dhami 2017 asthma SR/MA — CLEAR | other vs **SR/MA** | KAS04 |
| AAO2021OPI ref 140 | 16339408 (EoC notice) | **11087881** VIGOR RCT — EXPRESSION_OF_CONCERN | other vs **RCT** | KAS06 |
| AAO2016OME ref 200 | 20307908 (comment) | **19819563** Casselbrant 2009 — CORRECTION_ONLY | other vs **RCT** | KAS12A, KAS12B |
| AAO2022TYMP ref 217 | 31214828 (correction) | **31127413** Hao 2019 — CORRECTION_ONLY | other vs **RCT** | KAS11 |
| ICAR2021RS ref 1856 | 32200284 (corrigendum) | **28494895** El-Shmaa 2017 — CORRECTION_ONLY | other vs **RCT** | RS-60 |

Design-neutral slips (stored 'other' is also the true record's class, or production had
already overridden the design): AAO2017NECK ref 74 (comment 22943354 checked CLEAR;
true paper 22613304 CORRECTION_ONLY; items KAS02B/KAS02C), ICAR2021RS ref 2111 (erratum
25613294; true paper 24972542; item RS-74), AAO2026TIN2 ref 98 (corrigendum 27612990;
true review 27246985 CLEAR; design already SR/MA via hand-cite; item KAS02),
AAO2024AIT ref 63 (stored title is an unrelated 1990 reply, PMID 2370386 CLEAR; true
cite is the NAEPP 2020 focused-updates report, PMID 33280709 CORRECTION_ONLY; items
KAS01/KAS02A/KAS03), AAO2024AIT ref 113 (stored title is a correction to an unrelated
2026 *Lancet Respir Med* paper, PMID 42710509; true cite is the 2023 GINA report
summary, PMID 37302397 CLEAR; item KAS03).
One apparent case was **not** a slip: AAO2019SHL ref 136 — the guideline's reflist itself
cites the correction notice (Pasquini 2018), so the checked record is the cited one.

Whether to correct the six design values (and the two AIT SR/MA attachment counts) via
the amendment process is a parent decision; this check only reports them.

## Method

- **Primary channel:** NCBI efetch XML, batches of 100, pace 0.5 s, 4-attempt backoff.
  Signals read per record: PublicationTypeList, CommentsCorrectionsList (RefType/RefPMID),
  title, abstract head. NCBI esearch was not used (it was HTTP-500-broken on 2026-11-05);
  efetch was healthy throughout — every batch returned, no rate-limit incident.
- **Cross-check channel:** Europe PMC core records for every flagged record, every
  efetch-missing record, and the three mandatory flags (pace 0.36 s). Europe PMC
  pubtypes concurred on all five flagged citations.
- **Category rules:** WITHDRAWN = title carries the WITHDRAWN prefix or the abstract
  opens with a withdrawal statement (the Cochrane withdrawn state); RETRACTED = pubtype
  'Retracted Publication' or a RetractionIn link; EXPRESSION_OF_CONCERN = EoC pubtype or
  an ExpressionOfConcernIn link; CORRECTION_ONLY = erratum/correction/update links only.
  All raw signals are preserved per record in the results JSON, so any precedence choice
  can be re-adjudicated from the file.
- **Boundary:** statuses are those recorded by PubMed/Europe PMC on 2026-11-06.
  Retraction Watch was not consulted; a record those two sources have not yet annotated
  would read CLEAR here.
