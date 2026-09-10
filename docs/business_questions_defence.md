# RavenStack — Business Questions: Rationale and Defence

This document explains why the five analytical questions matter, how they would be investigated and what the results could support. See [business_requirements.md](business_requirements.md) for the fictional business scenario and stakeholder roles.

These are proposed analyses. Source fields and implementation rules have not yet been verified. All numerical examples below are hypothetical teaching examples, not findings from RavenStack data.

## Shared Reasoning

Under a consistent MRR definition, using one currency and assuming no other adjustments:

**Net MRR additions = New + Expansion + Reactivation − Contraction − Churned MRR**

**Ending MRR = Opening MRR + Net MRR additions**

**Monthly MRR growth rate = Net MRR additions / Opening MRR × 100%**

The examples record Contraction and Churned MRR as positive loss amounts and subtract them. The growth-rate formula requires positive opening MRR; a zero opening balance needs separate treatment.

A lower growth rate can coexist with increasing MRR. Counts, rates and monetary amounts answer different questions, so the analysis must connect customer behaviour to changes in MRR over time. MRR movements identify financial contributions; further evidence is needed to explain the underlying customer or operational causes.

## 1. MRR Movements

**Question:** How have changes in New, Expansion, Reactivation, Contraction and Churned MRR contributed to the slowdown in RavenStack's monthly MRR growth rate?

### Why and How

Total MRR alone does not identify which sources of growth or loss changed. I would reconcile opening MRR, the five movements and ending MRR for each complete month, then compare net additions and growth rates. I would examine both the monetary amounts and each movement relative to opening MRR.

### Worked Example

Amounts are in £1,000; loss amounts are shown as positive values.

| Measure | Previous month | Current month |
| --- | ---: | ---: |
| Opening MRR | 100 | 110 |
| New MRR | 10 | 8 |
| Expansion MRR | 5 | 5 |
| Reactivation MRR | 2 | 2 |
| Contraction MRR | 1 | 1 |
| Churned MRR | 6 | 10 |
| Net MRR additions | 10 | 4 |
| Ending MRR | 110 | 114 |
| Monthly growth rate | 10.00% | 3.64% |

New MRR fell by £2,000 and Churned MRR rose by £4,000. With other movements unchanged, net additions fell by £6,000. MRR still increased from £110,000 to £114,000.

The growth rate fell by approximately 6.36 percentage points. The £6,000 change in net additions is a monetary change, not a percentage-point change.

Separately, unchanged net additions of £10,000 would produce 10.00% growth on a £100,000 opening balance and 9.09% on £110,000. A larger opening balance can reduce the growth percentage without reducing New MRR.

### Decision and Limitations

The Head of Growth can use the breakdown to prioritise investigation. Falling New MRR warrants checking the number of new paying customers and their average initial MRR. Rising Churned MRR warrants checking churned-customer counts and their MRR at churn.

For example, 10 new customers averaging £1,000 contribute £10,000, while 12 averaging £500 contribute £6,000. New MRR can fall even when the number of new paying customers increases.

The breakdown does not establish why customers churn or expand. It also does not establish profitability without cost information.

### Interview Defence

> I would reconcile the monthly MRR movements and compare their contributions to net additions and the growth rate. This would identify whether weaker additions, greater losses or a larger opening MRR base explain the slowdown numerically. The result would guide Growth's next investigation, while further evidence would be needed to establish the underlying business causes.

## 2. Customer Segments

**Question:** How has churn changed across customer segments over time?

### Why and How

Company-wide churn can hide deterioration in a particular customer group. I would use clearly defined segments supported by the data, such as company size or country, and compare churn counts, churn rates and Churned MRR over complete months.

Rates require a consistent eligible customer base. I would examine group sizes, recurring revenue exposure and changes in customer mix, then reconcile the segment-level losses to the company total.

### Worked Example

| Segment | Customers at the start | Churned customers from that starting group | Customer churn rate | Churned MRR |
| --- | ---: | ---: | ---: | ---: |
| A | 100 | 10 | 10% | £1,000 |
| B | 100 | 5 | 5% | £5,000 |

Segment A has a higher customer churn rate, while B loses more MRR. The MRR problem therefore cannot be prioritised using customer churn rates alone.

This single-month comparison does not establish a trend. I would check whether each segment's MRR loss increased during the slowdown and how much it contributed to the change in net additions.

### Decision and Limitations

Growth and Customer Success can prioritise investigating groups with worsening retention or increasing MRR losses. Any action should depend on the cause found within that group.

Small samples, differences in plan or tenure, and changing segment composition can affect comparisons. Within one segment breakdown, categories must be mutually exclusive to reconcile to the total; missing segment values must remain visible.

### Interview Defence

> I would compare churn counts, rates and Churned MRR across segments over time. This would locate where losses are increasing and distinguish customer frequency from financial impact. It would help Growth and Customer Success focus their investigation, while group size, revenue exposure and customer mix would remain part of the interpretation.

## 3. Product Usage

**Question:** How is feature usage frequency over a 30-day period associated with paid-customer retention over the following 30 days?

### Why and How

This question investigates whether earlier feature usage is associated with later retention. I would define eligible paying accounts with access to the feature, measure usage during the preceding 30 days and observe retention over the following 30 days.

Usage groups must be defined explicitly. I would compare their retention, group size and subsequent Churned MRR across observation periods. If relevant, I would separately examine Contraction MRR among customers who remain paying.

### Worked Example

Suppose Churned MRR from the lower-usage group rises from £2,000 to £5,000. With all other MRR movements unchanged, net additions are £3,000 lower.

That does not prove more customers churned:

| Period | Churned customers | Average MRR at churn | Churned MRR |
| --- | ---: | ---: | ---: |
| Previous month | 2 | £1,000 | £2,000 |
| Current month | 1 | £5,000 | £5,000 |

Here fewer customers churned, but the lost MRR increased. Both the number of churned customers and their recurring value matter.

### Decision and Limitations

Product can investigate onboarding, usability or customer needs in the affected group. A confirmed onboarding problem could justify clearer guidance; a confirmed usability problem could justify a targeted product change.

Low usage alone does not establish low value or cause MRR loss. A useful monthly feature may only need monthly use. Account size, tenure, feature access and incomplete tracking can affect usage comparisons; missing events must not automatically be treated as zero usage.

Usage must be measured before the outcome, with complete follow-up data. Even then, an existing intention to leave could reduce usage before cancellation. Temporal ordering alone does not prove causation.

### Interview Defence

> I would measure usage before the retention outcome and compare clearly defined usage groups. I would then track their size and lost MRR over time to connect any retention differences to the slowdown. Product could use this to investigate specific problems, but I would not present low usage as proof of low value or as a demonstrated cause of churn.

## 4. Support Experience

**Question:** How is support CSAT associated with paid-customer retention 30 days after support, and how has this relationship changed over time?

### Why and How

CSAT is a specific measure of reported satisfaction with support. I would identify eligible support interactions and paying accounts, define score groups using the actual scale and compare paid-customer retention 30 days after the agreed support reference point.

I would track group sizes and subsequent Churned MRR over time to assess their contribution to the slowdown. The reference event, handling of multiple tickets and aggregation of scores must be defined before calculation.

### Worked Example

| CSAT group | Eligible paying customers | Still paying after 30 days | Retention |
| --- | ---: | ---: | ---: |
| Low | 100 | 80 | 80% |
| High | 200 | 160 | 80% |

Both groups have 80% retention. Comparing 80 retained customers with 160 without considering the group sizes would be misleading. This example does not demonstrate lower retention in the low-CSAT group.

Customer retention also does not capture every revenue loss. If 100 customers continuously remain paying but 10 reduce their recurring subscription amounts, customer retention is 100% and the lost recurring amount is Contraction MRR.

### Decision and Limitations

Customer Success can investigate response times, resolution times, repeated issues or unclear handovers where the data supports these checks. Long resolution times alone do not justify adding support staff or engineers: delays could arise from ownership, dependencies, issue complexity or another bottleneck.

Low CSAT does not prove an issue remains unresolved. Survey respondents may differ from non-respondents, and severe issues may affect both satisfaction and retention. Accounts with multiple tickets must not have their customer counts or MRR duplicated through joins. Follow-up must cover the full 30 days.

### Interview Defence

> I would compare 30-day paid-customer retention across CSAT groups and examine whether their Churned MRR increased during the slowdown. Customer Success could use the findings to investigate specific support problems and choose actions based on the identified cause. I would account for group sizes, ticket severity, repeated tickets and selective survey responses, and would describe the result as an association.

## 5. Expansion by Subscription Plan

**Question:** How has Expansion MRR changed across Basic, Pro and Enterprise over the last three months?

### Why and How

The MRR movement question identifies whether Expansion contributes to the slowdown. This question locates that change across subscription plans.

I would use the latest three complete months in the dataset and a consistent plan-attribution rule. Within each plan, I would separate the number of accounts with expansion from their average incremental MRR and consider the eligible customer base.

Expansion measures the increase in recurring value. For a single expansion from £100 to £150 MRR, the expansion is £50. If the account stays at £150 next month, it continues contributing MRR but creates no new expansion.

### Worked Example

| Scenario | Accounts with expansion | Average incremental MRR | Expansion MRR |
| --- | ---: | ---: | ---: |
| Previous month | 10 | £500 | £5,000 |
| Current month, possibility A | 4 | £500 | £2,000 |
| Current month, possibility B | 10 | £200 | £2,000 |

Possibility A reflects fewer expanding accounts; B reflects smaller average increases. The same total can arise through different mechanisms.

If Expansion MRR attributed to Pro falls from £5,000 to £2,000 while other movements remain unchanged, net additions fall by £3,000. This does not establish that total Pro MRR or the total number of Pro customers declined. Downgrades belong to Contraction, which is held unchanged in this example.

### Decision and Limitations

Growth can focus on the affected plan and investigate verified expansion needs, such as additional seats or features, if the pricing model supports them. Higher usage does not guarantee an upgrade: the current plan may still meet the customer's needs.

A Basic-to-Pro movement must be assigned consistently, for example by the plan before or after the change, without being counted in both within the same breakdown. Expansion can occur without a change of plan name. Three months may be insufficient to establish a longer-term trend or rule out seasonality.

The worked example assumes one expansion per expanding account in each month. Multiple events require a consistent aggregation rule.

### Interview Defence

> I would compare Expansion MRR across plans, separating the number of expanding accounts from the average incremental MRR and considering the eligible base. This would identify where expansion weakened and how it reduced net additions. Growth could investigate the affected group's needs and expansion opportunities. I would define plan attribution consistently and avoid assuming that every customer with high usage should upgrade.

## Implementation Decisions to Resolve

These decisions must be documented and verified before the questions are implemented:

| Area | Required decision or validation |
| --- | --- |
| Grain and joins | Confirm whether each source represents an account, subscription, user, event or ticket; prevent duplicated customer counts and MRR. |
| MRR rules | Define monthly normalisation, discounts, currencies, movement timing, and how multiple changes in a period are classified. Reconcile the chosen movements to opening and ending MRR. |
| Retention and churn | Define eligibility, effective churn dates, the retention outcome, treatment of reactivation and complete observation windows. |
| Group definitions | Define segment timing, feature access, usage thresholds, CSAT groups, multiple-score handling and plan attribution. |
| Data coverage | Verify the required fields and periods exist. Document missing values, tracking gaps and unsupported questions before changing scope. |

