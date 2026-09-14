# RavenStack — Metric definitions

Version: 1.1  
Updated: 14 September 2026  
Status: Business specification. Source validation and implementation are pending.

All monetary examples in this document are hypothetical. They are not findings about RavenStack. GBP is used for examples only; the reporting currency must be established from the sources.

## 1. Purpose and analytical scope

Primary question: **What is driving the slowdown in RavenStack's MRR growth?**

See [business_requirements.md](business_requirements.md) for the business context and [business_questions_defence.md](business_questions_defence.md) for the question rationale and illustrative analyses.

The working business scenario is that MRR continues to increase while its monthly growth rate slows. This remains a hypothesis to investigate with data.

| Business question | Required measures |
| --- | --- |
| How have changes in New, Expansion, Reactivation, Contraction and Churned MRR contributed to the slowdown in RavenStack's monthly MRR growth rate? | Opening and Ending MRR; New, Expansion, Reactivation, Contraction and Churned MRR; net MRR additions; monthly MRR growth rate. |
| How has churn changed across customer segments over time? | Opening paid-account count; distinct opening-cohort accounts that churn; customer churn rate; opening-cohort Churned MRR and separate losses outside that cohort; segment size. |
| How is feature usage frequency over a 30-day period associated with paid-customer retention at the end of the following 30 days? | Feature active days; feature usage-event count as supporting context; eligible-account count; paid-customer retention at day 30. |
| How is support CSAT associated with paid-customer retention 30 days after the selected CSAT response, and how has this relationship changed over time? | First qualifying CSAT response per account/reporting month; eligible-account count; paid-customer retention 30 days after that response. The response timestamp is the post-support observation anchor; see Section 7. |
| How has Expansion MRR changed across Basic, Pro and Enterprise over the latest three complete months available in the dataset? | Expansion MRR; distinct accounts with expansion; average Expansion MRR per expanding account; an explicit plan-attribution rule. |

Use the three most recent complete months available in the dataset for the last question. Do not substitute the current calendar month or infer a long-term trend from three months alone.

## 2. Shared measurement rules

- **Business entity:** an account represents a customer business. An account may have multiple subscriptions. Aggregate the relevant subscriptions to account MRR before classifying account-level movements.
- **MRR:** recurring subscription revenue from effective paid subscriptions, normalised to one month. It measures a recurring revenue level, not profit, cash receipts or a growth percentage.
- **Units:** MRR levels and movements are expressed in reporting currency per month; counts use accounts, events or days as explicitly named; rates use percentages.
- **Timing:** use subscription-effective dates, rather than assuming signup, payment or cancellation-request dates determine MRR. Month-end snapshots must use one consistent cutoff and time zone.
- **Signs:** New, Expansion and Reactivation are positive additions. Contraction and Churned MRR are positive loss amounts that are subtracted in the bridge. Convert source signs once to avoid subtracting a negative loss twice.
- **Missing versus zero:** missing history, unobserved usage and an incomplete follow-up window must not silently become zero. Preserve valid information and label the unavailable measure separately.
- **Zero denominators:** report an undefined rate as N/A with its reason, rather than 0%. Preserve the underlying count or monetary change where known.
- **Aggregation:** add monetary movements across compatible events and accounts. Recalculate rates from their numerators and denominators; do not sum segment rates or average them without the correct weighting.

### MRR inclusions and exclusions

Include the monthly recurring component of effective paid subscriptions, including recurring seats or add-ons where supported by the pricing model. Exclude one-off setup fees and free subscriptions or free trials. Do not subtract operating costs from MRR.

For a 12-month subscription, normalised MRR equals the recurring fee for those 12 months divided by 12. The cash payment may occur once rather than monthly.

Example: ten accounts at GBP 100 per month plus one account paying GBP 1,200 for 12 months contribute GBP 1,100 MRR. An additional GBP 300 one-off setup fee does not increase MRR.

Treatment of discounts, taxes, credits, variable usage charges, pauses and payment failures still requires explicit source and business-rule review. A zero amount or a status label alone must not be treated as proof that all paid subscriptions ended.

## 3. MRR levels and growth

| Metric | Definition / formula | Reporting grain |
| --- | --- | --- |
| Account MRR at a cutoff | Sum of eligible subscription MRR for that account at the cutoff. | Account and cutoff. |
| Opening MRR | Total account MRR at the opening boundary, consistent with the preceding month's Ending MRR. | Company or defined segment and month. |
| Ending MRR | Total account MRR for eligible subscriptions effective at the month-end cutoff. | Company or defined segment and month. |
| Net MRR change (net additions) | Ending MRR - Opening MRR. With complete movement coverage and no other adjustments, this equals New + Expansion + Reactivation - Contraction - Churned MRR. | Company or defined segment and month. |
| Monthly MRR growth rate | (Ending MRR - Opening MRR) / Opening MRR x 100%. | Company or defined segment and month. |

MRR is a level at a cutoff: summing successive month-end MRR values does not establish accounting revenue for those months.

At company level, when coverage, currency and definitions align and there are no other adjustments, the reconciliation is:

    Ending MRR = Opening MRR + New + Expansion + Reactivation - Contraction - Churned MRR

An unexplained difference is an issue to investigate, not an amount to force into one movement. Report unclassified changes and any out-of-scope adjustments explicitly.

Verified opening and ending balances can establish net MRR change even when event history is insufficient to split that change into movements. For a segment-level bridge, specify membership consistently and account for transfers between segments; the company-level formula alone does not explain a changing segment's balance.

Examples:

- GBP 1,000 opening and GBP 1,100 ending: net increase GBP 100; growth rate 10%.
- GBP 0 opening and GBP 500 ending: net increase GBP 500; growth rate N/A because the denominator is zero. This alone does not prove the company has just launched.
- A subscription that ceased to be effective on 20 September contributes no MRR at the end of 30 September.
- A cancellation requested on 20 September, with paid service remaining effective through 31 October, does not remove the subscription from September Ending MRR.

Growth can slow because additions weaken, losses increase, or the opening MRR base grows. MRR growth does not establish profit growth. Compare changes in rates in percentage points when appropriate.

## 4. Account-level MRR movements

The preferred working definition measures each effective change in total account MRR. Let B be MRR immediately before the change and A be MRR immediately after it. Classify the account after considering all its subscriptions.

| Movement | Business condition | Amount |
| --- | --- | --- |
| New MRR | Account starts its first-ever effective paid subscription: B = 0, A > 0, with sufficient history to establish first paid activation. | A. |
| Reactivation MRR | Account previously had paid subscriptions, subsequently ceased paid service and reached zero MRR, then resumes paid service: B = 0, A > 0. | A. |
| Expansion MRR | Existing paying account increases its recurring monthly amount: A > B > 0. | A - B. |
| Contraction MRR | Account reduces MRR while retaining paid service: B > A > 0. | B - A. |
| Churned MRR | Account loses its final paid subscription and total MRR falls from B > 0 to A = 0. | B. |

Signup is not necessarily first paid activation. A previously free account first taking a GBP 2,400 annual paid subscription contributes GBP 200 New MRR, even if it signed up in an earlier month. A returning former paying account belongs in Reactivation, not New.

A plan-name change is not required for Expansion or Contraction. Additional recurring seats can increase MRR within the same named plan.

### Multiple changes within a month

Sum separately classified event amounts. Month-end differences alone show net change and cannot recover all gross movements.

| Account path | Expansion | Contraction | Net change |
| --- | ---: | ---: | ---: |
| GBP 200 to GBP 260 to GBP 230 | GBP 60 | GBP 30 | GBP 30 |
| GBP 200 to GBP 150 to GBP 200 | GBP 50 | GBP 50 | GBP 0 |

If a source only shows GBP 200 at both month ends, net change is zero. Expansion and Contraction remain unknown without adequate history; they must not automatically be reported as zero.

### Multiple subscriptions on an account

An account with subscriptions worth GBP 100 and GBP 50 has GBP 150 total MRR. Ending the GBP 100 subscription creates GBP 100 Contraction because GBP 50 remains. If the final GBP 50 subscription later ends, that second event creates GBP 50 Churned MRR, not GBP 150.

### Incomplete history

An account first appearing in an extract is not automatically a new customer. If prior paid history cannot be established, label the New/Reactivation classification as unknown. Keep a verified current MRR contribution in Ending MRR. If even the pre-change balance is unknown, do not invent a movement amount from the ending balance alone.

Keep incomplete classifications visible in reconciliation. Verify historical coverage and simultaneous subscription-change handling before implementing the event model.

## 5. Customer churn and month-end retention

These measures count accounts, not subscriptions, tickets or money. Let C0 be the fixed cohort of distinct accounts paying at the opening cutoff; N0 is its size.

| Metric | Numerator | Denominator |
| --- | --- | --- |
| Monthly customer churn rate, event-based | Distinct accounts in C0 that lose all paid subscriptions at least once during the month. Count each account once, even if it churns repeatedly. | N0. |
| Paid-customer retention at month end | Accounts in C0 paying at month end, including those that churned and reactivated before the cutoff. | N0. |
| Opening-cohort non-paying share at month end, supporting diagnostic | Accounts in C0 not paying at month end. | N0. |

Multiply numerator / denominator by 100% for each rate. With no opening cohort, report N/A. If missing records prevent establishing a required event history or endpoint status for the fixed cohort, report the affected rate as unavailable and disclose the number of accounts with unknown outcomes. Preserve known counts. Do not silently reduce N0 or treat an unknown status as a non-paying account.

Event-based churn requires evidence of whether each account lost all paid subscriptions during the month. Opening and closing status alone cannot establish that no churn occurred between those cutoffs. Endpoint retention requires observable paid status at the endpoint; it does not require uninterrupted paid service.

For these customer-count rates by segment, assign accounts to their segment at the opening cutoff and keep that assignment fixed for the period. Keep unknown segment membership visible as a separate group.

New customers acquired during the month do not enter C0. They can increase the closing paid-account count without increasing retention of the opening cohort.

Example: 100 opening accounts, 10 that churn and do not return, plus 20 new paying accounts gives 10% customer churn, 90% opening-cohort retention and 110 closing paying accounts.

Reactivation example: of 100 opening accounts, 10 churn and two of those return before month end. Event-based churn is 10/100 = 10%; endpoint retention is 92/100 = 92%; the endpoint non-paying share is 8/100 = 8%.

The 8% share complements endpoint retention. Event-based churn does not necessarily complement it. Retention at the endpoint is not proof of uninterrupted paid service throughout the period.

Churned MRR and opening-cohort customer churn have different populations: monetary movements include qualifying events across accounts, while this customer churn rate restricts its numerator to C0. An account newly acquired and lost within the same month therefore requires care when comparing the two measures.

When reporting segment churn rates alongside monetary losses, label losses restricted to C0 as opening-cohort Churned MRR and use the same fixed opening-segment assignments. Show losses from accounts outside C0 separately when reconciling to total company Churned MRR. The company-level movement definition still includes all qualifying churn events.

## 6. Feature usage and retention 30 days later

### Usage measures

For each account, feature and observation cutoff T, measure usage over the 30 complete reporting dates immediately before T. For this analysis, T is aligned to the start of a reporting day; activity at or after T is excluded. Confirm the reporting time zone before implementation.

| Measure | Definition | Unit |
| --- | --- | --- |
| Feature active days in the preceding 30 days | Number of distinct reporting dates with at least one valid use of that feature. This is the primary proposed frequency measure. | Days, from 0 to 30 for a complete 30-day window. |
| Feature usage events in the preceding 30 days | Total valid uses of that feature. Supporting measure of volume. | Events. |

For example, 100 uses on one day give one active day and 100 events; one use per day across 30 days gives 30 active days and 30 events.

Zero usage requires adequate tracking and feature eligibility. Missing tracking must be marked unknown. Confirm how to treat partial feature access, newly created accounts and incomplete exposure before grouping accounts. Account size and tenure can influence usage. A low-frequency feature may still be valuable.

Usage bands are not fixed yet. Select and document them after examining feature semantics, access and coverage; do not invent thresholds or choose them merely to maximize an observed retention difference.

### Paid-customer retention at day 30

Start with accounts paying at T. Fix their usage-group membership using information available by T. Assess paid status at T + 30 days.

    Day-30 paid retention = eligible starting accounts paying at T + 30 days
                            / eligible starting accounts x 100%

For each feature/group/cohort, eligible starting accounts must be paying at T, have sufficient prior usage and feature-exposure coverage, and have an observable paid status at T + 30 days. Count each eligible account once in both the denominator and, if retained, the numerator. Report the starting-account count, eligible-account count and exclusions by reason. With no eligible accounts, report N/A.

Keep observations with incomplete follow-up or unknown endpoint status separately with their reasons. They do not enter the currently measurable day-30 rate and must not be assigned a churn outcome. Report observation cutoffs and coverage alongside comparisons so that differences in follow-up are visible.

If data stops at T + 20, day-30 retention is not yet observable. For 50 starting accounts with 48 paying at day 20, 96% describes day 20, not day 30. Label the day-30 result as unavailable due to incomplete follow-up.

This is endpoint retention, so a customer can leave and return before day 30 and still be retained at the endpoint. Usage must precede the retention outcome. Associations alone do not establish that a feature prevents churn.

## 7. Support CSAT and day-30 paid retention

CSAT is a customer's assessment of satisfaction with support. Retain the source scale and distinguish a valid score from a missing survey response. The source scale, valid values and any low/neutral/high thresholds have not been verified.

### Observation rule

For each account and calendar reporting month, select its **first valid CSAT response recorded while the account is paying**. Use that response's score to assign the CSAT group. An account without a qualifying response has no CSAT observation for that month; a missing response is not a low score.

**T is the timestamp when the selected response is recorded and available for analysis.** T is a time, not the score. Assess paid status at T + 30 days. Use the same reporting time zone throughout and verify that the source provides a usable response timestamp. Do not silently substitute a ticket creation or resolution date.

This defines retention 30 days after the selected CSAT response. The response is a practical post-support anchor, not proof of the exact time support ended; report that distinction when answering the support business question.

Keep subsequent responses in the source data, but do not replace the selected score or create another account observation within the same reporting month. If qualifying responses share the earliest timestamp, use a documented, stable response-identifier order once the source is known. The selection rule must not depend on a later score or retention outcome.

### Selection and follow-up are separate

Example: an account is paying when it gives a score of 2/5 on 5 September, its first qualifying response that month. It gives 5/5 on 20 September.

- Select the 2/5 response; T is its recorded time on **5 September**.
- The day-30 endpoint is the corresponding time on **5 October**, not 20 September. The 15-day gap between responses does not define the retention window.
- If paid-status coverage only reaches 20 September, preserve the selected observation and label its day-30 outcome as incomplete follow-up. Exclude it from the currently measurable day-30 rate and disclose the exclusion; do not mark it churned or assign a zero outcome.
- If coverage reaches the endpoint on 5 October, use paid status at that endpoint to determine retention. A second CSAT response is not required to observe paid retention.

The selected score describes the first qualifying response, not satisfaction throughout the month or the latest experience. In this example, the later 5/5 response directly contradicts a claim that the account gave low ratings throughout September. Waiting until day 30 does not make the first score representative of every response.

Use the same endpoint-retention calculation within each CSAT group and reporting month: accounts with a qualifying selected response while paying at T and observable paid status at T + 30 days form the denominator; those paying at that endpoint form the numerator. Feature-usage coverage from Section 6 is not an eligibility requirement for this CSAT analysis. Report starting observations, eligible-account counts and exclusions by reason; use N/A when no accounts are eligible. Multiple tickets must not multiply an account's weight or duplicate its MRR in a group.

Example: account A has three low-CSAT tickets and is paying at day 30; account B has one low-CSAT ticket and is not paying at day 30. Both accounts are paying at their selected T, their selected first responses place them in the same low-CSAT group, and their day-30 statuses are observable. Account retention is 1/2 = 50%, not 3/4 = 75%. More tickets from A do not change account retention.

Limitations include survey non-response, ticket severity, account tenure and the information omitted by selecting only the first response. The same account may appear in different reporting months; these are repeated account-month observations, not independent new customers. Low CSAT does not prove an issue was unresolved, and an association with churn is not evidence of causation. Use a consistent observation rule when comparing periods.

## 8. Expansion by plan

Use complete reporting months. Attribute each qualifying account-level Expansion event to the **plan immediately before that event**. This answers which starting plan the expanding account came from. It does not measure the destination plan's MRR balance.

Example: an account moves from Basic at GBP 100 MRR to Pro at GBP 150 MRR. Its GBP 50 Expansion is attributed to **Basic**. Expansion attributed to Basic can also come from recurring seats or add-ons while the account remains on Basic; it does not necessarily imply an upgrade to another plan.

When the pre-event plan cannot be established, or the account holds multiple plans without an unambiguous attribution, retain the verified Expansion amount in an **Unclassified plan** bucket with a reason. Do not guess a primary plan or duplicate the amount. Verify the plan history and any required mapping during source validation.

First aggregate the attributed events to account/plan/month, then count accounts and calculate the average.

| Metric | Formula / definition |
| --- | --- |
| Expansion MRR by plan and month | Sum of qualifying positive Expansion amounts attributed to that plan and month. |
| Accounts with expansion | Distinct accounts contributing positive Expansion MRR to that plan/month. |
| Average Expansion MRR per expanding account | Expansion MRR for the plan/month divided by its distinct accounts with expansion. |

The average excludes accounts with no expansion. Multiple expansion events on one account increase its monetary contribution, not its account count. With no expanding accounts, the average is N/A; the count can legitimately be zero when coverage is complete.

Example, all accounts remaining on Pro for the month:

| Account | Expansion events | Total Expansion MRR | Included in expanding-account count? |
| --- | --- | ---: | --- |
| A | GBP 20, GBP 30 and GBP 10 | GBP 60 | Yes, once. |
| B | GBP 100 | GBP 100 | Yes, once. |
| C | None | GBP 0 | No. |

Total Expansion is GBP 160; there are two expanding accounts; the average is GBP 80 per expanding account. The check is GBP 160 = 2 x GBP 80.

Compare both the account count and average contribution over time. The same total can arise from different combinations. A paid subscription's full post-change MRR is not its Expansion amount.

An account can contribute to more than one plan group in a month if it has separate Expansion events originating from different plans. Count it once within each contributing plan/month, but calculate the company account count from distinct accounts across all events. Per-plan account counts are therefore not necessarily additive. Attributed monetary amounts, including Unclassified plan, must reconcile to total Expansion MRR without duplication.

## 9. Measurement decisions and validation requirements

### Measurement decisions

| Decision | Measurement rule | Interpretation / limitation |
| --- | --- | --- |
| Plan attribution for Expansion | Use the plan immediately before each qualifying Expansion event. Keep missing or ambiguous assignments in Unclassified plan. | Describes the plan the expanding account came from. Monetary amounts are additive; distinct accounts across plan groups may overlap. |
| Support observation and CSAT score selection | Per account/reporting month, select the first valid response recorded while paying. T is that response's recorded timestamp; evaluate paid status at T + 30 days. | One observation per account/month prevents ticket volume from weighting accounts. The selected score does not describe the entire month, and outcome coverage must be checked separately. |

Document any revisions to these definitions if source validation shows that the available data cannot support a rule.

### Source validation requirements

- Account/subscription identifiers, relationship cardinality, source grain and complete lifecycle history.
- Valid paid states, effective start/end semantics, timestamp ordering, reporting time zone and consistent month boundaries.
- Reporting currency and treatment of discounts, taxes, refunds/credits, variable charges, pauses and failed payments.
- Whether event history supports gross movements or only net snapshot comparisons; handling simultaneous changes and missing opening balances.
- Feature-event meaning, tracking completeness, entitlement/exposure and the suitability of active-day frequency and grouping bands.
- CSAT scale, response and support timestamps, stable response identifiers, valid scores, missing responses and usable group thresholds. Verify response coverage from the reporting month's start through T before identifying a response as the first qualifying one.
- The ability to observe paid status at every required day-30 endpoint without treating an incomplete follow-up as churn.
- Historical plan and segment membership, ambiguous multi-plan accounts and overlap in per-plan distinct counts. A segment-level MRR bridge also needs a consistent membership rule and explicit treatment of transfers between segments.

Until these checks are complete, the definitions are a proposed business specification. They do not establish source feasibility, successful implementation or verified analytical results.
