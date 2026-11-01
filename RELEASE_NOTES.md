<!-- dataset-factory:links -->
**Dataset Factory · v2026.10**

[Dataset website](https://earino.github.io/pins-planning-appeal-outcome/versions/v2026.10/) · [GitHub release](https://github.com/earino/pins-planning-appeal-outcome/releases/tag/v2026.10) · [Hugging Face](https://huggingface.co/datasets/earino/pins-planning-appeal-outcome/tree/v2026.10) · [Catalogue](https://earino.github.io/dataset-factory-site/)

Artifact: c861bd8f4acd9bbe267bdb58e1168996fdb359c540aaf4030030a632d693c527

### Suitability and evaluation requirements

Retained for retrospective model-development research. Each row is one decided s78_core planning-permission-family appeal whose features are its own registration attributes plus LPA prior-outcome history computed strictly from decisions earlier than the row's validation instant; the prediction instant is the validation event (Valid Date, event_time) and no point-in-time or operational claim is made. The publisher serves a rolling database (pinned here at the 2026-10-22 edition) and no historical vintages, so training-label timing and full-row historical availability are unverified and none is claimed. The evaluation contract requires isolated rows: the LPA-history features embed other rows' labels (lpa_last_outcome is the previous same-LPA decided case's label), a declared and probed cross-row exposure, while the shipped illustrative runner passes the whole eval feature table to the predictor - unrestricted-access scores overstate honest performance and the requirement is a requirement, never an enforcement claim. Measured finding carried beside every figure: the stock runner baseline (eval AUC 0.6108) exceeds every measured simple reference on the identical 28,104 eval rows - procedure-type rate 0.5551 (+0.0558, row CI95 [0.0467, 0.0626], LPA-cluster CI95 [0.0462, 0.0654]), LPA history rate 0.5414 (+0.0694, [0.0622, 0.0766] / [0.0597, 0.0792]), shipped rate feature 0.5422 (+0.0686, [0.0619, 0.0756] / [0.0596, 0.0800]) - every paired interval excluding zero under both the row-level and the LPA-clustered protocol: demonstrated headroom over these references under this protocol, with its scope recorded (one model configuration, no re-tuning; two validation years of eval coverage). Persistence is measured at 0.5045 (92%+ of eval rows' LPAs have train-era history; the -1 sentinel marks the rest). The calendar-identity control collapsed onto the LPA rate exactly (eval years have no train-era valid-year support) and is recorded as a degenerate control, never as independent confirmation.

**Supported uses:** Retrospective research and method comparison on the pinned 2026-10-22 edition, predicting row-wise from the shipped validation-instant columns under an isolated-rows protocol; Agent evaluation with the labeled eval split as development/validation data and the holdout kept outside agent development until a design is frozen; Study of LPA-level and procedure-level prevalence and of which validation-instant attributes carry outcome signal

**Not supported:** Operational or live validation-instant screening claims: no historical operational-availability evidence exists and none is claimed; Point-in-time historical reconstruction: the publisher serves a rolling current vintage; a later re-fetch is a different vintage, not what was knowable earlier; Quoting the model AUC (0.6108) without the reference comparison (procedure-type 0.5551, LPA rate 0.5414, shipped rate 0.5422, persistence 0.5045, with the paired row-level and LPA-clustered intervals) beside it; Scoring the eval split with the whole feature table and presenting the result as an isolated-rows figure: the LPA-history features make cross-row label recovery measurably possible, so unrestricted-access scores overstate honest performance; Treating the split as certified decision-time separation: the split key is the validation instant and slow cases put some training labels after the eval window's earliest instants, so label timing is unverified

**Evaluation access:** isolated rows. The labeled eval split is development/validation data. Keep the final holdout outside agent development; prevent later rows from influencing earlier predictions.

**Historical availability:** unverified. **Training-label timing:** unverified. **Vintage:** current snapshot. **Selection:** outcome conditioned.

**Cross-row audit:** recovery demonstrated. Gate 1.5.0's own probe over the full artifact (check contract.cross_row_probe_0, job pins-planning-002): the within-LPA donor join covers 36,643 train / 21,593 eval / 43,747 holdout rows, and on the covered rows the later row's lpa_last_outcome feature is an earlier shipped row's label verbatim exactly when that previous case was already decided before the donor's instant (measured mismatches 15,428 / 8,803 / 18,430 are rows whose previous case was still pending at the donor's instant or where another same-LPA case was decided in between; 12,705 / 6,085 / 12,143 ambiguous same-day keys). The exposure is inherent to any prior-outcome feature and unmitigated at runtime by the shipped contract, which passes the whole eval feature table to the predictor: quality.json declares isolated_rows as a REQUIREMENT for honest evaluation and claims no enforcement. Any evaluation must present one row at a time to the solver; unrestricted-access AUCs are not comparable to isolated-rows AUCs and must not be presented as such.

Structural qualification and baseline scores do not certify operational forecasting validity.

- Single current-snapshot vintage (the 2026-10-22 edition, sha256-pinned for both source files); the publisher republishes the rolling database monthly-to-quarterly and retains no historical vintages, so no historical operational-availability claim is possible
- Training-label timing and full-row historical availability are unverified; the split key is the validation instant because the outcome date cannot ship, so the task is retrospective research only
- All publisher-sourced columns are current-snapshot vintage: values are as revised in the pinned edition, not as first published at past instants
- The population is the publisher's own decided-cases-only extract (in progress, withdrawn and turned away cases are absent) further restricted to binary decisions - outcome-observability conditioning recorded as a limitation, never resolved by relabelling
- The cross-row exposure over LPA-history features is declared and requires an isolated-rows protocol; the shipped runner does not enforce it
- The headroom finding's scope: one model configuration with no re-tuning, two validation years of eval coverage; the calendar-identity control was degenerate (collapsed onto the LPA rate), so seasonality/identity headroom is not independently established; the model's per-validation-year AUC and Brier were not computed; beating the measured references does not enumerate all simple rules and does not prove a causal signal
- The holdout was never evaluated; no holdout figure exists and its first use must follow a frozen design

Review: 2026-10-31; applies to v2026.10 / `c861bd8f4acd9bbe267bdb58e1168996fdb359c540aaf4030030a632d693c527`. Frozen data and tags are unchanged.
<!-- /dataset-factory:links -->

# Release notes - v2026.10

## What this is

`pins-planning-appeal-outcome` is a binary prediction task: for a
planning-permission-family appeal (casework type `Planning Appeal` or
`Householder (HAS)`) validated by the Planning Inspectorate, will the final decision
be `Allowed` rather than `Dismissed`, predicted from what is known at validation -
casework type, LPA identity, received date, procedure, development type, appeal
reason fields, site descriptors and designation flags, and LPA-level prior-outcome
history computed strictly from decisions earlier than the validation instant. This
is the first packaged release; there is no prior version.

The prediction instant is the **validation event** (`event_time`, the appeal's Valid
Date). The task is a clearly-labelled retrospective construction over the pinned
2026-10-22 edition of the publisher's rolling database: the publisher exposes the
current vintage only, no historical vintages exist, and none are reconstructed.
Training-label timing is UNVERIFIED by construction (slow cases put some training
labels after the eval window's earliest instants), so the evaluation contract in
`quality.json` is retrospective research only - never a certified historical
forecasting setup. The LPA-history features embed other rows' labels through a
declared cross-row probe, so honest evaluation **requires isolated rows**; the shipped
illustrative runner does not enforce that, and the requirement is stated as a
requirement, never as an enforcement claim.

## Artifact identity

- Artifact version (gate digest over the five files):
  `c861bd8f4acd9bbe267bdb58e1168996fdb359c540aaf4030030a632d693c527`
- Gate version: 1.5.0 on the full artifact, failed_checks empty, exact-byte
  `--accept` ACCEPTED on the worker (job `pins-planning-002`) and re-accepted in job
  `pins-planning-baseline-002`, which re-fetched the five files from private staging
  and verified every sha256 on arrival before running anything.
- Source snapshot: the 2026-10-22 edition of the Planning Inspectorate Casework
  Database - `Casework_Database_-_2610.xlsx` (22,207,115 B, sha256
  `dff80bd0f17cbc640dc205009a9411269c8e58462b6df71af903c592af3c20fa`) and
  `Older_Casework_Data_-_2610.xlsx` (12,325,866 B, sha256
  `baf28d80c5cd5ad266f8a9e62c25517b2ec9d4c5d951239ab0638666bc092a4b`).
- Splits (validation-instant windows, scored rows / positives / positive rate):
  train, validated before 2020-01-01: **54,834 / 17,048 / 0.310902**; eval, validated
  2020-2021: **28,104 / 8,469 / 0.301345**; holdout, validated on or after
  2022-01-01: **56,326 / 17,705 / 0.314331** (through 2026-08-21). Exclusions
  counted per the eligibility frame: 2,757 non-binary decisions, 3,110 cross-file
  composite duplicates, 2 decided-before-validation rows.

## Measured evidence (identical eval rows, after acceptance)

All figures on the same 28,104 eval rows; the holdout was never read and stays
sealed.

| Reference / model | Eval AUC | Brier |
| --- | --- | --- |
| Full model (stock runner XGBoost, verbatim) | **0.6108** | not recorded |
| Procedure-type rate (train-fitted, dominant simple reference) | 0.5551 | 0.2080 |
| LPA history rate (train-fitted, the named headroom reference) | 0.5414 | 0.2107 |
| Shipped rate feature (`lpa_allow_rate_prior`) | 0.5422 | - |
| Persistence (previous same-LPA decided outcome) | 0.5045 | 0.4150 |
| Calendar-identity control | degenerate (collapsed onto the LPA rate) | - |
| Majority-class accuracy on this split | 0.6987 | - |

Paired differences (150 resamples, job `pins-planning-baseline-002`; row-level and
LPA-clustered bootstrap - appeal outcomes of one planning authority are correlated):

- model - procedure_type_rate: **+0.0558**, row CI95 [0.0467, 0.0626], LPA-cluster
  CI95 [0.0462, 0.0654] - excludes zero under both protocols
- model - lpa_hist_rate: **+0.0694**, row CI95 [0.0622, 0.0766], LPA-cluster
  CI95 [0.0597, 0.0792] - excludes zero under both protocols
- model - shipped_rate_feature: **+0.0686**, row CI95 [0.0619, 0.0756], LPA-cluster
  CI95 [0.0596, 0.0800] - excludes zero under both protocols

Per validation year the references hold their ordering (2020, 13,797 rows:
procedure 0.5616, LPA rate 0.5473, persistence 0.5076; 2021, 14,307 rows: procedure
0.5486, LPA rate 0.5358, persistence 0.5013) and prevalence is flat across the two
years (0.2990 -> 0.3036), so the model's advantage is not a prevalence artefact; the
model's own per-year AUC was not computed and is not claimed. The calendar-identity
control is **degenerate and recorded as such**: eval validation years have no
train-era valid-year support, so every eval row fell back to its LPA's train rate
and the control collapsed onto `lpa_hist_rate` exactly (paired diff 0.0, CI [0,0]) -
a collapsed control, never independent confirmation.

**Interpretation.** Demonstrated headroom over every measured simple reference under
this protocol. The scope is explicit: one model configuration with no re-tuning, two
validation years of eval coverage, row-level and LPA-clustered intervals only. This
is an observed result, not a bound on other models and not an operational-validity
claim.

## What is not claimed

- No point-in-time or historical-vintage claim: the publisher's current edition is
  the only vintage, none is reconstructed, and training-label timing stays
  unverified.
- No claim that beating these references proves a causal signal; other simple rules
  were not exhaustively enumerated, and the calendar control was degenerate rather
  than independent.
- No agent/harness comparison was run; the runner-contract baseline says nothing
  about how a coding agent would score on the task.
- The labelled holdout ships as an asset but stays outside evaluated agents'
  workspaces; no job in this record has read it.
- Isolated-rows evaluation is a requirement the shipped illustrative runner does not
  enforce; unrestricted-access scores overstate honest performance.
- The derived dataset is not a Planning Inspectorate product, and the publisher's
  own data caveats carry through unchanged (subjective categorisation of some
  appeals; the data can change between publications; decided cases only; and the
  Planning Inspectorate will not entertain a challenge to the appointment of any
  Decision Maker on the basis of this data).

## Provenance and licence

Source terms preserved (OGL-UK-3.0, attribution required); our code MIT; our
compilation contribution CC0-1.0 - see `LICENSE.md`. Frame fidelity was
cross-checked in-build: the per-decision-year fixture reproduces the discovery probe
exactly for all 11 decision years, and flag-column presence was verified by direct
letter-based counts.
