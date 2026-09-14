# RavenStack — Business Questions: Rationale and Defence

Version: 1.1  
Updated: 14 September 2026

This document explains why the five analytical questions matter, how they would be investigated and what the results could support. See [business_requirements.md](business_requirements.md) for the fictional business scenario and stakeholder roles.

These are proposed analyses. The working measurement rules are documented in [metric_definitions.md](metric_definitions.md); source fields and implementation feasibility remain unverified. All numerical examples below are hypothetical illustrations, not findings from RavenStack data. GBP is an example currency; the reporting currency remains subject to source validation.

## Shared Reasoning

Under a consistent MRR definition and currency:

**Net MRR change (net additions) = Ending MRR − Opening MRR**

With complete movement coverage and no other adjustments:

**Net MRR additions = New + Expansion + Reactivation − Contraction − Churned MRR**

**Ending MRR = Opening MRR + Net MRR additions**

**Monthly MRR growth rate = Net MRR additions / Opening MRR × 100%**

The examples record Contraction and Churned MRR as positive loss amounts and subtract them. With zero opening MRR, report the growth rate as N/A and retain the absolute change. Verified opening and ending balances can establish net change even when gross movements cannot be recovered from the available history.

A lower growth rate can coexist with increasing MRR. Counts, rates and monetary amounts answer different questions, so the analysis must connect customer behaviour to changes in MRR over time. MRR movements identify financial contributions; further evidence is needed to explain the underlying customer or operational causes.

Customer counts use distinct accounts. Monetary movements sum qualifying event amounts; an account can have more than one event in a period. A distinct-customer count multiplied by average MRR at a single churn event is therefore not a general formula for gross Churned MRR.

Feature and support cohorts may overlap across features or observation windows. Attribute monetary events once within each intended breakdown and use aligned period boundaries when reconciling to company MRR. Do not sum overlapping cohort losses as if they were independent contributions.

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

The Head of Growth can use the breakdown to prioritise investigation. Falling New MRR warrants checking the number of first-time paying accounts and their average initial MRR. Rising Churned MRR warrants checking distinct affected accounts, churn-event counts and the MRR lost at each event.

For example, 10 new customers averaging £1,000 contribute £10,000, while 12 averaging £500 contribute £6,000. New MRR can fall even when the number of new paying customers increases.

The breakdown does not establish why customers churn or expand. It also does not establish profitability without cost information.

### Interview Defence

> I would reconcile the monthly MRR movements and compare their contributions to net additions and the growth rate. This would identify whether weaker additions, greater losses or a larger opening MRR base explain the slowdown numerically. The result would guide Growth's next investigation, while further evidence would be needed to establish the underlying business causes.

## 2. Customer Segments

**Question:** How has churn changed across customer segments over time?

### Why and How

Company-wide churn can hide deterioration in a particular customer group. I would use clearly defined segments supported by the data, such as company size or country, and compare churn counts, churn rates and Churned MRR over complete months.

The customer churn rate uses the fixed cohort paying at the opening cutoff: its numerator is the distinct accounts that lose all paid subscriptions at least once during the month. An account that churns and returns still enters this event-based numerator. I would assign these accounts to their opening segment and keep the assignment fixed for the period.

I would report opening-cohort churn counts, rates and Churned MRR together, then show Churned MRR from accounts outside that cohort separately when reconciling to the company total. Group sizes, recurring revenue exposure and changes in customer mix remain part of the interpretation.

### Worked Example

For this example, each churned account has one churn event and all churn events belong to the opening cohort.

| Segment | Customers at the start | Churned customers from that starting group | Customer churn rate | Opening-cohort Churned MRR |
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

**Question:** How is feature usage frequency over a 30-day period associated with paid-customer retention at the end of the following 30 days?

### Why and How

This question investigates whether earlier feature usage is associated with later retention. I would start with accounts paying at cutoff T, measure feature active days during the preceding 30 complete reporting dates and assess paid status at T + 30 days. Valid usage-event counts would provide supporting volume context. Eligible observations require adequate feature-access and tracking coverage and an observable day-30 paid status.

Usage bands remain subject to feature semantics and source validation. Within each feature and observation period, I would compare groups' endpoint retention, eligible-account counts and subsequent Churned MRR. If relevant, I would separately examine Contraction MRR among customers who remain paying. An account that leaves and returns by day 30 is retained at the endpoint, so endpoint retention alone does not recover every churn event.

### Worked Example

Suppose Churned MRR from the lower-usage group for one feature rises from £2,000 to £5,000 across two comparable observation periods. If Churned MRR from all other accounts and all other MRR movements are unchanged, net additions over those same boundaries are £3,000 lower. Contributions to a calendar-month slowdown must be reconciled using the events' effective dates in that calendar month.

That does not prove more customers churned. Each churned account has one churn event in this example:

| Period | Churned customers | Average MRR at churn | Churned MRR |
| --- | ---: | ---: | ---: |
| Previous period | 2 | £1,000 | £2,000 |
| Current period | 1 | £5,000 | £5,000 |

Here fewer customers churned, but the lost MRR increased. Both the number of churned customers and their recurring value matter.

### Decision and Limitations

Product can investigate onboarding, usability or customer needs in the affected group. A confirmed onboarding problem could justify clearer guidance; a confirmed usability problem could justify a targeted product change.

Low usage alone does not establish low value or cause MRR loss. A useful monthly feature may only need monthly use. Account size, tenure, feature access and incomplete tracking can affect usage comparisons; missing events must not automatically be treated as zero usage.

Usage must be measured before the outcome. Observations with incomplete follow-up or unknown endpoint status must be reported separately and excluded from the currently measurable day-30 rate, with counts and reasons disclosed. Even with sufficient coverage, an existing intention to leave could reduce usage before cancellation. Temporal ordering alone does not prove causation.

### Interview Defence

> I would measure usage before the retention outcome and compare clearly defined usage groups. I would then track their size and lost MRR over time to connect any retention differences to the slowdown. Product could use this to investigate specific problems, but I would not present low usage as proof of low value or as a demonstrated cause of churn.

## 4. Support Experience

**Question:** How is support CSAT associated with paid-customer retention 30 days after the selected CSAT response, and how has this relationship changed over time?

### Why and How

CSAT is a specific measure of reported satisfaction with support. For each account and reporting month, I would select the first valid CSAT response recorded while the account is paying. Its recorded timestamp is T; paid status at T + 30 days is the retention outcome. This response timestamp is the operational post-support anchor and is not assumed to equal ticket resolution time.

I would define score groups using the validated source scale, then compare day-30 paid retention, eligible-account counts and subsequent Churned MRR over time. Later responses remain in the source data but do not replace the selected response or create another observation for that account in the same reporting month. Monetary comparisons with the slowdown must use aligned reporting periods and avoid duplicate events across observations.

### Worked Example

| CSAT group | Eligible paying customers | Paying at day 30 | Retention |
| --- | ---: | ---: | ---: |
| Low | 100 | 80 | 80% |
| High | 200 | 160 | 80% |

Both groups have 80% retention. Comparing 80 retained customers with 160 without considering the group sizes would be misleading. This example does not demonstrate lower retention in the low-CSAT group.

Customer retention also does not capture every revenue loss. If 100 customers continuously remain paying but 10 reduce their recurring subscription amounts, customer retention is 100% and the lost recurring amount is Contraction MRR.

### Decision and Limitations

Customer Success can investigate response times, resolution times, repeated issues or unclear handovers where the data supports these checks. Long resolution times alone do not justify adding support staff or engineers: delays could arise from ownership, dependencies, issue complexity or another bottleneck.

Low CSAT does not prove an issue remains unresolved. Survey respondents may differ from non-respondents, and severe issues may affect both satisfaction and retention. The selected first score does not represent every support experience during the month. Accounts with multiple tickets must not have their customer counts or MRR duplicated through joins. Require observable paid status at day 30 and disclose observations excluded because their outcome is not observable.

### Interview Defence

> I would select one qualifying CSAT response per account and reporting month, then compare paid status 30 days later across score groups. I would examine whether related Churned MRR increased during the slowdown using aligned periods and distinct monetary events. Customer Success could use the findings to investigate specific support problems. Group sizes, ticket severity, repeated observations and selective survey responses would limit interpretation; the result would remain an association.

## 5. Expansion by Subscription Plan

**Question:** How has Expansion MRR changed across Basic, Pro and Enterprise over the latest three complete months available in the dataset?

### Why and How

The MRR movement question identifies whether Expansion contributes to the slowdown. This question locates that change across subscription plans.

I would use the latest three complete months in the dataset and attribute each qualifying Expansion event to the plan immediately before the event. Within each plan/month, I would sum the Expansion amounts by account, count distinct expanding accounts and calculate their average total contribution. Accounts without expansion do not enter this average. I would also consider the eligible customer base when interpreting differences.

Expansion measures the increase in recurring value. For a single expansion from £100 to £150 MRR, the expansion is £50. If the account stays at £150 next month, it continues contributing MRR but creates no new expansion.

### Worked Example

| Scenario | Accounts with expansion | Average total Expansion MRR per expanding account | Expansion MRR |
| --- | ---: | ---: | ---: |
| Previous month | 10 | £500 | £5,000 |
| Current month, possibility A | 4 | £500 | £2,000 |
| Current month, possibility B | 10 | £200 | £2,000 |

Possibility A reflects fewer expanding accounts; B reflects smaller average increases. The same total can arise through different mechanisms.

If Expansion MRR attributed to Pro falls from £5,000 to £2,000 while Expansion from all other plan groups and all other MRR movements remain unchanged, net additions fall by £3,000. This does not establish that total Pro MRR or the total number of Pro customers declined. Downgrades belong to Contraction, which is held unchanged in this example.

### Decision and Limitations

Growth can focus on the affected plan and investigate verified expansion needs, such as additional seats or features, if the pricing model supports them. Higher usage does not guarantee an upgrade: the current plan may still meet the customer's needs.

A Basic-to-Pro change from £100 to £150 MRR creates £50 Expansion attributed to Basic under the pre-event plan rule. Expansion can also occur through recurring seats or add-ons without a change of plan name. Keep missing or ambiguous pre-event plan assignments in Unclassified plan, preserving the monetary amount. Three months may be insufficient to establish a longer-term trend or rule out seasonality.

Multiple events on the same account can increase its monthly contribution without increasing its count within that plan/month. An account can contribute to different plan groups through separate events; per-plan distinct-account counts are therefore not necessarily additive. Calculate company totals from distinct accounts across all qualifying events, and reconcile monetary totals including Unclassified plan.

### Interview Defence

> I would attribute Expansion to the plan before each event and compare total Expansion, distinct expanding accounts and their average total contribution across complete months. This would show which starting-plan groups weakened and how that affected net additions. Growth could investigate those groups' needs and expansion opportunities. I would account for missing plan history and overlapping account counts, and avoid assuming that high usage necessarily warrants an upgrade.

## Source Validation and Implementation Dependencies

The working business rules are specified in [metric_definitions.md](metric_definitions.md). The following checks are still required before implementation:

| Area | Required validation |
| --- | --- |
| Grain and joins | Confirm whether each source represents an account, subscription, user, event or ticket; prevent duplicated customer counts and MRR. |
| MRR rules | Map monthly normalisation and account-level effective movements to source fields. Confirm currency and treatment of discounts and other adjustments; assess whether event history supports gross movements and reconciliation. |
| Retention and churn | Validate paid-state semantics and effective dates. Establish whether the fixed opening cohort's churn events and the required retention endpoints are observable; preserve unknown outcomes and incomplete follow-up. |
| Group definitions | Validate historical opening-segment and pre-event plan membership, feature access and first-response selection. Set data-dependent usage bands and CSAT thresholds explicitly after source review. |
| Data coverage | Verify the required fields and periods exist. Document missing values, tracking gaps and unsupported questions before changing scope. |
