# AI Proposal Triage Design

Reference: MATACS-TH-2026-02. Configuration: **commercial-solar-demo-v1**.

## Scope, assumptions and scoring

The system assists a reviewer; a human awards the contract. Admins configure category requirements, weights and scoring targets rather than hard-coding them. Each category selects a versioned extraction template/schema. Weights must total 100%.

Mandatory submission evidence: a bidder-held Ontario electrical contractor licence, WSIB clearance and CAD 5M liability insurance. A future subcontractor does not satisfy the bidder-licence rule. As an explicit design interpretation, CAD 260,000 and 16 weeks are hard upper limits.

After extraction quality is established, confirmed failure takes precedence: mark `ineligible` and retain other warnings. Complete, readable submissions missing mandatory proof are ineligible; unreadable or ambiguous proof requires review. Otherwise, unresolved critical facts or an embedded ranking instruction cause `flagged` pending human review. Missing critical values are unknown, not zero. Complete, eligible records proceed to scoring.

The following targets are configurable assumptions chosen before scoring: 15 qualifying projects, CAD 200,000 preferred price, 10-week preferred completion, 25-year panel and 15-year workmanship warranty. Preferred targets are not eligibility thresholds. Each raw score is capped at 100:

| Criterion | Raw score | Weight |
|---|---|---:|
| Experience | `100 * min(qualifying_projects / 15, 1)` | 30% |
| Price | `100 * min(200000 / price_CAD, 1)` | 25% |
| Timeline | `100 * min(10 / weeks, 1)` | 15% |
| Warranty | `50 * min(panel_years / 25, 1) + 50 * min(workmanship_years / 15, 1)` | 15% |
| Team/approach | 25 each for evidenced lead, crew, safety plan and monitoring plan | 15% |

Validate units, positive price/weeks, and nonnegative counts/durations first. Monitoring may be documented progress or performance monitoring. Missing checklist evidence earns no points, without asserting the capability is absent. Missing a whole required scoring input triggers review. Targets reward sufficiency and cap exceptional values; fixed benchmarks prevent another bidder changing existing scores.

Sum raw scores multiplied by weights. Keep full precision for ordering; show two decimals. Break ties by experience score, lower price, then shorter timeline; unresolved ties require a recorded human decision before final positions.

For this worked example only, assume A's 14 and E's 8 installations fall within the chosen evaluation date's previous five years. Their year-only descriptions do not prove that. These are sample analyst assumptions, not extracted evidence or completed verification; a real run flags unresolved dates for either proposal. Also assume quoted sample prices cover the project, dollar-denominated insurance is CAD, and stated completion periods run from award. The extractor preserves omitted qualifiers as unknown; real reviewers resolve them. The appendix note preceding D is interpreted as part of C. C's nine-project count remains unverified.

| Proposal | Experience points | Price | Timeline | Warranty | Team | Total |
|---|---:|---:|---:|---:|---:|---:|
| A | 28.00 | 21.01 | 12.50 | 12.50 | 15.00 | **89.01** |
| E | 16.00 | 20.62 | 11.54 | 13.50 | 15.00 | **76.66** |

A ranks first under those assumptions: its experience, price and timeline outweigh E's stronger workmanship warranty. B is ineligible as submitted, not declared legally unlicensed. C is flagged for the embedded instruction and experience dates; its claimed pre-approval earns nothing and does not automatically disqualify it. D is flagged because completion is unspecified, not proven late. Unranked, unscored records have null rank/score. If C's nine projects are confirmed and its instruction review is cleared, its conditional score is 72.96, with progress monitoring credited and safety-plan evidence unawarded; that is not its current result.

## 1. AI versus rules

Upload and category selection lead to PDF text extraction/OCR. AI returns typed facts, evidence references and uncertainty flags, never eligibility decisions or scores. Document content is untrusted data, including alleged pre-approval. Deterministic rules validate fields, apply gates and calculate the rubric; arithmetic and policy should not depend on model opinion. Human reviewers resolve uncertain extraction and documents containing manipulation attempts before eligible records proceed.

## 2. Consistency and explainability

Freeze the evaluation date, input hash, extraction snapshot, accepted facts and configuration version for each run. Cache accepted extraction; low temperature alone does not guarantee identical AI output. Repeat scoring on identical accepted inputs produces identical totals. Show each fact's source, criterion points, weights, gate outcomes and rank reason. Fresh extraction or corrections create a new run. Use arithmetic-derived explanations rather than letting AI invent reasons.

## 3. Human review and audit

Review unreadable text, critical unknowns, conflicting claims and embedded instructions. A reviewer can correct extracted facts or confirm documented evidence, then re-run validation and scoring. Keep original and corrected values, reviewer, timestamp, reason and evidence. Log document/configuration hashes, model and prompt versions, evaluation date, flags, calculations and output. Preserve system ranking alongside any separately recorded award override; an override does not silently rewrite the scores or waive requirements. No actual verification or award is claimed in this submission.

## 4. Changing criteria

Save edits as a new configuration version. Mark older rankings superseded, revalidate and rescore every affected proposal under the same new version, and retain prior runs. Reuse accepted facts where sufficient; extract/review additional facts if a new criterion needs them. Never mix versions in one ranking. Highlight eligibility/rank changes for human review before the revised result is accepted.

## 5. Risks and safeguards

- **Extraction mistakes/invented facts:** preserve source evidence, validate types/units and route unknowns or conflicts to review; never guess.
- **Manipulation:** isolate untrusted text, validate structured output, block document-driven tool actions and flag embedded instructions; detection is supplementary, not the trust boundary.
- **Bias:** apply the published rubric consistently; do not score company names, persuasive wording or unsupported claims.
- **Cost/failure:** limit document size, cache extraction, cap retries and route failures to review rather than fabricate a result.
- **Data exposure:** restrict access and retention; use only fictional samples here, keep source text out of the deliverables and never commit secrets.
