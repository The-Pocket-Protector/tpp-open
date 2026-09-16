# Medicare Advantage ranking methodology

Reviewed September 16, 2026. This document describes the active Medicare Advantage comparison engine, including how it distinguishes list order, the highlighted plan, and a recommendation to stay or switch. It is a structural explanation, not an executable specification.

## 1. Candidate selection

Location, plan year, requested plan types, prescription-coverage path, service availability, and special-needs screening determine the initial candidates. Validation, duplicate removal, and any strict doctor-network policy can reduce that list. A current plan may be retained for comparison under exceptions that do not apply to new candidates.

The active comparison flow ranks the remaining candidate pool before limiting the returned list. Other callers can request a narrower care-match group and are outside this edition's default-flow description.

An earlier scoring and preference-ordering stage still matters to duplicate selection and to the ordering carried into final ties. The final ranking is therefore not fully described by a single weighted average or by alphabetical tie-breaking.

## 2. Ordering the candidates

The active ordering compares these criteria in sequence. A later criterion does not overturn an earlier difference:

1. Higher number of carrier-confirmed doctor matches.
2. Higher combined number of doctor and prescription matches.
3. Higher doctor-match count, then higher prescription-match count.
4. Higher number of available doctor cross-reference checks.
5. A usable internal cost calculation before an unavailable one, then a lower internal cost measure.
6. A known language-target match, then a higher CMS star rating when both compared plans have numeric ratings.
7. The incoming order when the preceding comparisons tie.

Doctor match totals can include confirmed matches, likely matches, and acknowledged cross-reference matches. They are capped at the number of doctors entered. An available cross-reference check is a separate, weaker signal than an acknowledged match. Unknown counts contribute no match credit; they do not establish a confirmed coverage loss.

When both plans lack a cost calculation within an otherwise equal coverage comparison, language and star comparisons can still distinguish them. An unrated plan is neutral in the pairwise star comparison; a missing rating is not a zero-star rating.

## 3. Internal cost calculation

For a plan with the necessary data, the structure is:

**Ranking measure = round(N + V + R + B + U)**

| Term | Meaning |
| --- | --- |
| N | Annual plan premium plus estimated annual covered prescription cost, less an applicable annual Part B premium reduction. |
| V | Modeled medical visit costs for the care-use category. |
| R | An internal adjustment for medical out-of-pocket exposure. |
| B | Modeled unmet supplemental-benefit needs, after any eligible shared-allowance adjustment. |
| U | Estimated annual cash cost of uncovered prescriptions. |

Annualizing a monthly amount multiplies it by twelve. The underlying plan-cost rollup and individual components have their own rounding before the final whole-number rounding. An applicable Part B reduction can make N negative; that does not mean total Medicare spending is negative. The calculation does not apply that reduction for a member identified as having Medicaid.

This measure excludes the standard Part B premium and does not model every medical service. R is an internal ranking adjustment, not projected spending. The sum must not be presented as an all-inclusive annual cost estimate or a guaranteed saving.

### Medical use and risk

**V = sum of modeled service counts × usable filed cost-sharing amounts**, with an inpatient term for the highest care-use category. A computable upper bound can be used when filed cost sharing is a range. A missing amount for a service with modeled use prevents a complete cost calculation. An unanswered care-use question uses a default category.

**R = round(w(u) × M)**, where M is the plan's medical out-of-pocket limit and w(u) is a calibration weight for care-use category u. This is a ranking policy, not a claim that a person has that probability of reaching the limit. Plans specifically identified as having zero-dollar D-SNP cost sharing receive a zero adjustment. An unknown limit otherwise prevents a complete calculation.

Exact w(u) values and default service counts are not published here.

### Supplemental benefits

For an allowance-based benefit, the basic structure is:

**Unmet need = max(0, modeled need − usable coverage)**

Usable coverage depends on the benefit's scope, period, limits, and cost-sharing structure. Dental calculations can account for routine care drawing on a shared dental maximum, or estimate the member's share from procedure costs. Some structures use fallback assumptions when direct pricing is not possible. Hearing and vision can depend on product choices and replacement frequency. Over-the-counter benefits depend on a stated spending category and usable allowance.

Requesting that a benefit exist is distinct from stating a priceable need. Coverage-only intent creates no monetary charge for that benefit. An omitted or uncertain answer does not have one universal effect: for example, some hearing product selections use a default valuation once a priceable need is established. Benefits without a spending question do not receive an invented spending value.

For qualifying shared allowances:

**Shared offset = min(usable shared annual allowance, sum of eligible unmet needs)**

**B = sum of benefit-level unmet needs − shared offset**

The shared offset is applied once. Coverage cannot create a negative unmet need or offset unrelated premiums. Missing benefit facts may leave a full modeled need uncovered, while some incompletely specified structures use a fallback member-share assumption. Numerical valuation tables, procedure-mix weights, cadence defaults, and fallback calibration values are not published here.

### Prescription costs and missing information

Covered prescription estimates are already included in N. Cash estimates for uncovered prescriptions enter U separately. Where a prescription has already been priced through a covered generic substitution, it is not charged again as an uncovered prescription in this calculation.

Unpriceable uncovered prescriptions, missing cash estimates for known uncovered prescriptions, or an explicit indication that covered prescription costs could not be estimated prevent a complete cost calculation. So do a missing core rollup, required medical cost sharing, or required out-of-pocket limit. An unavailable cost calculation sorts after usable calculations within the same coverage comparison. It does not erase the plan's coverage position.

## 4. Selecting the highlighted plan

The highlighted plan is generally the first ranked candidate with a usable cost calculation that passes the applicable selection checks:

- A quality check based on a star-rating threshold and a low-performing indicator. Unrated plans remain eligible but are flagged.
- A language check: known targeting that conflicts with the session language can make a plan ineligible for the default recommendation. No classification is neutral.
- A vision check: when a member asks for vision coverage without a priceable need, confirmed absence of routine exam coverage can make a plan ineligible. Unknown exam coverage does not trigger that exclusion.

These checks affect selection rather than list order. The quality check relaxes if every candidate fails it; the language and vision checks have corresponding fallback rules if no candidate survives the preceding checks and the added check. A surviving candidate with incomplete cost data can still prevent a check from relaxing.

The returned list may be shortened. The engine preserves the selected candidate in that list when needed, so displayed position and full-pool rank can differ.

## 5. Comparing against a current plan

The current plan uses the same cost-calculation structure. With a current plan available, confirmed coverage evidence is evaluated before a cost-only switching decision:

- A confirmed loss of entered doctors or prescriptions on the current plan, combined with a higher total care-match count on the selected candidate, can support switching independently of the cost threshold.
- If the current plan matches all entered care, every candidate's coverage is known, and none matches all entered care, the engine favors staying.
- Otherwise, when both cost measures are available, switching on cost requires the current plan's measure minus the candidate's measure to reach a materiality threshold.

Unknown coverage alone does not establish a confirmed loss. Without adequate cost data, the cost comparison can be inconclusive. Without a current plan, the engine can identify a candidate but cannot calculate improvement over that current plan.

The numerical quality and switching thresholds are not included in this edition.

## 6. Interpretation and limits

This documentation describes the normal active engine path. Incomplete data and fallback processing can limit a result. Estimates and provider evidence require verification against plan information. TPP's ranking is distinct from [CMS plan ratings and Medicare's comparison tools](https://www.medicare.gov/basics/get-started-with-medicare/using-medicare/helpful-tools).

Publishing the calculation structure makes the reasoning easier to examine. Withholding calibration values means this documentation does not enable exact independent reproduction. It also does not guarantee that parameters cannot be inferred from observable product behavior.
