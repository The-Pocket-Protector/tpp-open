# Examples of the ranking rules

These examples are invented to illustrate individual rules. They are not real plans, quotes, production outputs, or a complete simulation of the engine.

## Confirmed doctor evidence can decide before cost

Suppose you enter two doctors. Plan A has two carrier-confirmed matches. Plan B has two likely matches but no carrier-confirmed matches. Plan A comes first on the confirmed-doctor criterion even if Plan B has a lower internal cost measure. Both kinds of evidence still require you to verify participation before enrolling.

## A larger allowance does not create cash savings

Assume a benefit's modeled annual need is $600 and usable annual coverage is $400. For this allowance-only example, unmet need is max(0, $600 − $400), or $200. If usable coverage is $900 instead, unmet need becomes $0. The unused $300 does not reduce premiums or another benefit's costs.

These round numbers are illustrative inputs, not TPP's valuation assumptions.

## A shared allowance counts once

Suppose two eligible benefit needs have $250 and $200 remaining, and one verified shared allowance can cover $300 across those needs. The combined offset is min($300, $450), or $300. The combined unmet amount is $150. The engine does not subtract $300 separately from each need.

## Missing prices are not free care

Suppose two plans are equal on every coverage criterion. One has the information needed to calculate its internal cost measure; the other lacks a cost-sharing amount for a medical service included in modeled use. The plan with usable cost data sorts first at the cost comparison. The missing amount is not assumed to be zero.

## A ranking and a recommendation can differ

A plan can appear earlier in the ordered list but fail a check for becoming the highlighted recommendation. For example, when the quality check remains active, a plan that fails it stays in the ranked list while another eligible, priceable plan may be highlighted.

[Return to the methodology](technical-methodology.md)
