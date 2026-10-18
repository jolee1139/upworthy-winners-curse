# Scope Document — Measuring the Winner's Curse in Upworthy Headline Tests

**Phase 1 of 6.** Drafted October 5, 2026.

---

## 1. Research Question

Across all usable headline tests in the Upworthy Research Archive, I define the
apparent winner as the headline with the highest observed click-through rate
(CTR) and its apparent margin as the percentage-point difference between its CTR
and the average CTR of the other headlines. Its underlying—or "true"—margin is
the difference we would expect without the random sampling noise in a single
test. This project estimates the winner's curse: the number of percentage points
by which the apparent margin exceeds that underlying margin, with an uncertainty
interval obtained by resampling entire tests.

*"Usable" is defined by the data-quality criteria in §4.*

---

## 2. Why It Matters

Editors and product teams select the headline with the highest observed CTR, but
choosing the maximum causes sampling noise to inflate the winner's measured
advantage on average. If that inflation is large, headline-testing programs
overstate the lift they deliver—and therefore overstate the business case for
running them. If it is small, that is equally useful: the archive's winning
headlines mostly reflect underlying differences, so little or no discount is
needed.

---

## 3. Locked Analytical Choices

These six choices define the measured quantity. Each carries the tradeoff it
accepts, not only the argument for it.

| # | Choice | My defense |
|---|---|---|
| 1 | **Winner − mean of other arms** | If an editor without test results is effectively choosing from the candidate set by judgment, the mean of the other arms represents the no-testing counterfactual, and keeping that comparison set unselected concentrates the selection problem in the chosen winner; the cost is that this does not answer how the winner compares with a specific incumbent or the runner-up. |
| 2 | **Percentage points** | Percentage points make the overstatement an interpretable absolute CTR difference and remain stable when baseline CTR is small; the cost is that averaging across tests with different baselines produces an average absolute overstatement rather than a proportional one. |
| 3 | **Observed minus underlying margin** | The difference directly answers how many percentage points should be discounted, while a ratio would be unstable precisely where the hypothesis places most underlying margins—near zero; the cost is that the estimate does not express inflation relative to the size of the underlying effect. |
| 4 | **Highest observed CTR, with no significance bar** | Choosing the observed maximum matches the selection mechanism that creates the winner's curse and keeps close, noisy tests in the population; the cost is that the estimate applies to selection by rank and does not describe organizations that deploy a winner only after it clears a significance threshold. |
| 5 | **Quality filters only; no mechanism filters** | Quality filters remove tests that cannot support a valid calculation without selecting cases based on how much inflation they are expected to show; the cost is a more heterogeneous population, potentially wider uncertainty, and an average that may conceal meaningful differences between types of tests. |
| 6 | **Resampling entire tests** | Because the claim concerns performance across tests like those in the archive, tests—not individual clicks—are the population units, and resampling them preserves each experiment's internal arm structure; the cost is that the interval captures resampling variability across the observed Upworthy tests, not model misspecification or generalization beyond the archive. |

---

## 4. Analysis Population

**Included:** all tests in the archive surviving data-quality exclusion.

**Excluded (quality filters only):** records with missing impression or click
counts, zero impressions, clicks exceeding impressions, or duplicated records.
Exclusion counts and reasons are reported, along with a check of whether
exclusions cluster systematically (e.g. by date).

**Not excluded:** nothing is dropped for being small or for arm count. Both are
mechanically related to inflation, so filtering on them would be selection on the
mechanism being measured. The estimator is expected to handle noisy small tests
rather than deleting them.

**Reported instead:** inflation stratified by impression count and by number of
arms. An impressions-floor sensitivity check is a robustness item, not a
population definition.

---

## 5. Deliverables

The repository README links first to the one-page results memo and the
pre-registration, so non-technical readers can find the main result without
navigating the notebooks.

- **One-page results memo (`RESULTS_MEMO.pdf`)** — Reports the winner's-curse
  estimate and uncertainty interval in percentage points, explains the practical
  implication of a large or near-zero result, and states the main limitations.
  For editors, product teams, and other non-technical decision-makers.

- **Data-preparation notebook (`00_prepare_data.ipynb`)** — Imports the archive,
  applies the usability rules, constructs the analysis variables, and reports
  exclusions with reasons. For technical reviewers auditing the analysis
  population.

- **Exploratory notebook (`01_exploratory_analysis.ipynb`)** — Uses only the
  exploratory data to document the reasoning behind the estimator, diagnostic
  checks, and thresholds later fixed in the pre-registration. For reviewers who
  want to see how the analysis plan was developed and verify that exploratory
  work remained separate from confirmation.

- **Pre-registration (`PREREGISTRATION.md`)** — A timestamped, git-committed plan
  finalized before the confirmatory data are examined. It fixes the population,
  exclusions, selection rule, estimand, estimator, resampling procedure, subgroup
  analyses by impression count and number of arms, and the treatment of a null
  result. For anyone evaluating the credibility of the confirmatory analysis.

- **Confirmatory notebook (`02_confirmatory_analysis.ipynb`)** — Runs the locked
  analysis once on the confirmatory data, without changing the estimator or
  thresholds, and produces the final estimate, uncertainty interval, diagnostics,
  and preregistered subgroup results. For analysts reproducing or reviewing the
  primary result.

- **Results package (`results/`)** — Contains the test-level estimates, the
  overall summary estimate and interval, stratified summaries by impression count
  and number of arms, and figures showing the overall estimate with its interval
  and the preregistered subgroup estimates. For readers who want to inspect or
  reuse the outputs without rerunning the notebooks.

---

## 6. Known Limitations

To be stated plainly in the memo rather than discovered by a reader.

- **Mildest form of the phenomenon.** Argmax-alone selection produces less
  inflation than a significance-gated rule would. The estimate does not speak to
  teams who ship only on significance.
- **Arm count drives the quantity.** The maximum of 12 arms is more inflated than
  the maximum of 2. Handled by stratification, not exclusion, but the dependence
  is real and is reported.
- **The interval does not cover generalization.** One publisher, one editorial
  culture, Jan 2013 – Apr 2015. It is not evidence about headline tests in
  general.
- **The interval does not cover model misspecification.** Resampling reflects
  variability across tests, not the possibility that the estimator's assumptions
  are wrong.
- **Broad population.** "All usable tests" is heterogeneous; the average must not
  be read as describing a typical high-traffic test.
- **Scope of the claim.** A large inflation estimate means measured lift is
  overstated — not that true lift is zero or that testing has no value.

---

## 7. Out of Scope

### 7a. Possible follow-up projects

Legitimate extensions, but not deliverables for this one-week project:

- Analyzing headline wording, sentiment, topic, length, or other textual
  features.
- Predicting winning headlines or generating new ones.
- Analyzing image variants or headline–image interactions.
- Estimating changes in inflation over time.
- Studying sequential testing, stopping rules, multi-armed bandits, or
  alternative experiment designs.
- Building a financial model of the costs and returns of headline testing.
- Generalizing the findings to other platforms, periods, or audiences by adding
  another data source.
- Extending the primary estimand beyond the overall average and preregistered
  strata, including modeling the full distribution of inflation or presenting
  per-test inflation as a test-level finding.

### 7b. Confirmatory analysis guardrails

Not optional extensions; these would compromise the confirmatory analysis:

- Changing exclusions, thresholds, the primary estimator, the resampling
  procedure, or subgroup definitions after examining the confirmatory results.
- Adding an alternative estimator because the primary result looks surprising or
  disappointing.
- Rerunning the confirmatory analysis after making an outcome-driven adjustment.
- Reporting only the analyses that produce a preferred conclusion.

A robustness estimator may be reported only if it is fully specified in the
pre-registration before the confirmatory data are examined; it remains secondary
and cannot replace the primary estimator. Any unavoidable departure caused by a
verified code or data error will be documented explicitly rather than treated as
part of the original confirmatory analysis.

---

## 8. Phase Plan

| Phase | Status |
|---|---|
| 1. Scope document | Complete |
| 2. Data orientation — exploratory release only | Next |
| 3. Exploratory analysis — estimator and thresholds | |
| 4. Pre-registration — lock and commit with timestamp | |
| 5. Confirmatory analysis — one pass, no deviations | |
| 6. Writeup — notebooks and one-page memo | |

**Standing rules.** The confirmatory release is not touched until phase 4 is
committed. Once committed, phase 4's choices are not revisited.
