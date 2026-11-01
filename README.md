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

# UK planning appeal outcome (pins-planning-appeal-outcome)

A binary prediction task built from the UK Planning Inspectorate's Casework Database
(GOV.UK bulk open data, the pinned 2026-10-22 edition), published under the Open
Government Licence v3.0.

**Question.** For a planning-permission-family appeal (PINS casework type `Planning
Appeal` or `Householder (HAS)`) validated by the Planning Inspectorate, will the final
decision be **Allowed** rather than **Dismissed** - predicted from only what is known
at validation: the casework type, LPA name and ONS code, received date, procedure,
development type, appeal reason fields, site descriptors and designation flags, and
LPA-level prior-outcome history computed strictly from decisions earlier than the
validation instant?

- **Prediction instant:** the appeal's **Valid Date** (`event_time`) - the instant PINS
  validates the appeal. Present on 100% of the 186,785 measured decided rows; received
  to valid is within 30 days for 85.9% of cases and valid to decision is 91-365 days
  for 74%, so the outcome window follows the instant. Every feature field is recorded
  at submission/registration and carries a publisher per-field data-quality grade.
- **Target:** `allowed` = 1 iff the publisher's Decision field is `Allowed`, 0 iff
  `Dismissed`. The Decision field and its date do not ship in any split.
- **Population:** decided s78_core appeals with a binary decision - 145,133 s78_core
  rows reduced by 2,757 non-binary decisions, 3,110 cross-file composite duplicates
  (2021 decisions appear in both publisher files) and 2 decided-before-validation rows,
  each counted, never silently dropped: **139,264 scored rows**. This is the
  publisher's own decided-cases-only population (in progress, withdrawn and turned
  away cases are not in the extract) - an outcome-conditioned population recorded as
  a property of the source.
- **Splits:** temporal split on the **validation instant** (the shipped `valid_date`
  column) because the outcome date cannot ship: train = validated before 2020-01-01
  (54,834 rows, 31.09% positive), eval = validated 2020-2021 (28,104 rows, 30.13%),
  holdout = validated on or after 2022-01-01 (56,326 rows, 31.43%, through
  2026-08-21). Disjoint windows, both classes in every window. Slow cases put a
  split's labels after the next split's earliest instants, so **training-label timing
  is UNVERIFIED** and the evaluation contract is retrospective research only.
- **Cross-row exposure, declared:** the LPA-history features embed other rows'
  labels (`lpa_last_outcome` is the previous same-LPA decided case's label verbatim).
  Honest evaluation therefore **requires isolated rows** - one row presented to the
  solver at a time. The shipped illustrative runner does NOT enforce this (it passes
  the whole eval feature table to the predictor); the requirement is stated as a
  requirement, never as an enforcement claim.

## Measured evidence (identical eval rows, after gate acceptance)

| Reference / model | Eval AUC (28,104 rows, 8,469 positives) |
| --- | --- |
| Full model - the stock XGBoost runner baseline, verbatim | **0.6108** |
| Procedure-type rate (train-fitted positive rate per procedure) | 0.5551 |
| LPA history rate (train-fitted rate per LPA, the named headroom reference) | 0.5414 |
| Shipped rate feature (the artifact's own `lpa_allow_rate_prior` column) | 0.5422 |
| Persistence (previous same-LPA decided case's outcome) | 0.5045 |
| Calendar-identity control | degenerate - collapsed onto the LPA rate exactly |
| Majority-class accuracy on this split | 0.6987 |

Paired differences (150 resamples, row-level and LPA-clustered bootstrap, job
`pins-planning-baseline-002`): model - procedure_type **+0.0558** [0.0467, 0.0626]
row / [0.0462, 0.0654] LPA-clustered; model - lpa_hist **+0.0694** [0.0622, 0.0766] /
[0.0597, 0.0792]; model - shipped_rate_feature **+0.0686** [0.0619, 0.0756] /
[0.0596, 0.0800]. Every interval excludes zero under both protocols: **demonstrated
headroom over every measured simple reference under this protocol** - an observed
result with recorded scope (one model configuration, no re-tuning; two validation
years of eval coverage; the calendar-identity control collapsed onto the LPA rate
because eval years have no train-era valid-year support, so it is recorded as a
collapsed control, not independent confirmation). Persistence is measured, not
declared not-applicable: 92%+ of eval rows' LPAs have a train-era decided-case history.

All figures were computed on a temporary worker after exact-byte acceptance of
artifact `c861bd8f4acd9bbe267bdb58e1168996fdb359c540aaf4030030a632d693c527` (gate
1.5.0 on the full artifact, failed_checks empty). The holdout was never read by any
scoring job and stays sealed. Labeled eval is development/validation evidence, never
an untouched final test.

## Layout

```
public/train.csv    train split   (validated before 2020-01-01)
public/eval.csv     eval split    (validated 2020-2021)
private/holdout.csv holdout split (validated 2022-01-01 onwards)
meta.json           runner descriptor
quality.json        qualification descriptor (timing, splits, evaluation contract)
code/               construction (build.py), the gate, references, run order
baseline/           the benchmark's own runner files + reproduction script
get_dataset.py      downloads the release assets, verifies digests, restores layout
```

GitHub release assets are flat (no `/` allowed); `get_dataset.py` restores the layout
above. `python3 get_dataset.py --dest ./task` does the download, digest verification
and layout in one step.

## Licence

Our code and documentation: MIT (`LICENSE-MIT.txt`). Our rights in the derived
compilation: CC0-1.0 (`LICENSE-CC0-1.0.txt`), scoped to our contribution only. The
underlying Planning Inspectorate Casework Database keeps its own designation and
remains subject to the Open Government Licence v3.0, whose attribution condition
carries through to every copy: *Contains public sector information licensed under the
Open Government Licence v3.0. Source: Planning Inspectorate Casework Database
(Planning Inspectorate), Casework_Database_-_2610.xlsx and
Older_Casework_Data_-_2610.xlsx, 2026-10-22 edition.* See `LICENSE.md` for the full
terms, scope, required attribution and the publisher's own data caveats. This is a
derived research dataset - not a Planning Inspectorate product.

## Provenance

- Source: the GOV.UK publication *Planning Inspectorate Casework Database*
  (https://www.gov.uk/government/publications/planning-inspectorate-appeals-database),
  2026-10-22 edition:
  `Casework_Database_-_2610.xlsx` (22,207,115 B, sha256
  `dff80bd0f17cbc640dc205009a9411269c8e58462b6df71af903c592af3c20fa`) and
  `Older_Casework_Data_-_2610.xlsx` (12,325,866 B, sha256
  `baf28d80c5cd5ad266f8a9e62c25517b2ec9d4c5d951239ab0638666bc092a4b`), two anonymous
  GETs. The build fails if these bytes change; the publisher republishes the rolling
  database monthly-to-quarterly, so the pin is the vintage.
- Constructed by worker job `pins-planning-002` (the single permitted rebuild after
  the first runner-contract failure; `--built-at 2026-10-31T00:00:00Z`); qualified by
  the same job with `code/qualify_dataset.py` (gate 1.5.0) and exact-byte accepted
  before any metric existed. The construction, baseline and diagnostic jobs all ran
  on temporary workers, since destroyed with provider receipts; the bulk bytes never
  passed through the small coordinator.
- Frame fidelity cross-checked in-build: the per-decision-year fixture reproduces the
  discovery probe exactly for all 11 decision years (kept + counted exclusions ==
  probe n; kept_allowed + excluded_allowed == probe allowed), and the flag columns'
  presence is verified by direct letter-based counts
  (`measurements.json`).
- The gate report for exactly these bytes ships as `qualification.json`.
