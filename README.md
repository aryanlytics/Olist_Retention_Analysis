# Olist_Retention_Analysis

## Project Overview
This project diagnoses the root cause of the $68\%$ first-time customer churn at Olist, isolating the key drivers to provide a strategic, constraint-aware recommendation. The solution targets an estimated $3.6 Million Annual GMV opportunity.

## The Problem Statement
The Crisis: 
Olist is facing an unsustainable $68\%$ first-purchase churn rate.

The Objective: 
Determine if the failure is Operational (fixable problems like late delivery) or Structural (intrinsic product mix issues).

The Constraint: 
The analysis respects the critical limitation that Olist cannot control logistics/delivery carriers.

## Analytical Findings: Structural Dominance is the Proof
The Logistic Regression model isolated the causal effects of each factor, proving the solution lies in the product mix.

The Damage (Operational Factors):
Late Delivery (is_late): This factor has an Odds Ratio ($\text{OR}$) of $3.09$. This means a late delivery triples the odds of a customer churning (a $209\%$ increase).

Review Score Paradox: A one-point increase in review score slightly increases churn odds ($\text{OR} = 1.49$), suggesting that high expectations in certain categories are quickly penalized if not met.


The Fix (Structural Factors):
The Best Categories (Computers): This category has an $\text{OR}$ of $0.248$. This means belonging to this category reduces the odds of churn by $75.2\%$ (relative to the baseline category).


The Conclusion (The Comparison):
The positive impact of the Structural Fix (the $75.2\%$ churn reduction) is $\sim 24\times$ greater than the negative impact of the worst operational damage (the $209\%$ increase from late delivery). The strategic focus must be on fixing the category mix.


## Final Recommendation & Business Impact
### Recommendation:

Immediate shift in Seller Recruitment Strategy to aggressively target high-retention categories.

Focus Areas: 
Direct resources toward acquiring sellers in top-retaining categories like Computers, Health/Beauty, and Furniture Decor.

De-prioritize: 
Stop actively onboarding sellers in high-churn, low-repeat categories (e.g., Music, Art).

Quantified Value:
By raising the customer repeat rate from $32\%$ to a conservative $45\%$, this strategy unlocks $3.6 Million in additional Gross Merchandise Volume (GMV) annually.





