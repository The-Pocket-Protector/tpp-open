# How The Pocket Protector ranks plans

This documentation explains how our Medicare Advantage comparison engine uses your answers, available plan information, and decision rules to order plans and identify a recommendation.

Start with [How we rank plans](how-we-rank-plans.md). For the calculation structure and exceptions, read the [technical methodology](technical-methodology.md). The [examples](examples.md) illustrate how the rules work. See the [change history](CHANGELOG.md) for this documentation's scope and review date.

## What this documentation covers

This edition covers the Medicare Advantage comparison engine reviewed on September 16, 2026. Standalone Part D recommendations, Medigap comparisons, employer coverage comparisons, advisor matching, and plan-page benefit ratings have separate methods and are outside this edition.

We publish the factors, calculation structure, ordering rules, and limitations. Exact calibration values—including numerical weights, default usage assumptions, valuation tables, and recommendation thresholds—are not included. These documents are not sufficient to reproduce every result independently.

This repository contains methodology documentation. It does not contain the application source code or a complete executable scoring model.

## Using the results

A recommendation depends on the information you enter and the plan data available to us. A plan's position is not a CMS rating or a guarantee of coverage, eligibility, or future costs. Confirm the details that matter to you with the plan before enrolling.

You can also compare coverage options using [Medicare Plan Compare](https://www.medicare.gov/plan-compare/).
