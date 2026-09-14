# RavenStack — Business Requirements

## 1. Company Context

RavenStack is a fictional B2B SaaS company offering Basic, Pro and Enterprise subscription plans. Its customers are businesses that pay recurring subscription fees to access its software.

## 2. Business Problem

In the project scenario, RavenStack's monthly recurring revenue (MRR) continues to increase, but its month-over-month MRR growth rate is slowing. The business needs to understand what is driving this slowdown before deciding which actions to prioritise. This is the project's starting assumption and must be checked against the dataset before being presented as an analytical finding.

## 3. Analysis Objective

The analysis aims to explain the slowdown by examining New, Expansion, Reactivation, Contraction and Churned MRR, alongside opening MRR as the denominator of the monthly growth rate. It will investigate differences across customer segments and subscription plans, and assess how product usage and support experience are associated with subsequent retention, where the available data supports these analyses. The findings will help the Head of Growth, Head of Product and Customer Success prioritise further investigation and consider actions supported by evidence.

## 4. Stakeholders and Decision Needs

The roles below represent the intended stakeholders in this fictional scenario. No stakeholder interviews have been conducted.

| Stakeholder | Analysis needs | Decisions the analysis could support |
| --- | --- | --- |
| Head of Growth | Understand which MRR movements contribute to the slowdown. | Prioritise acquisition, retention or expansion investigations based on their contribution to the slowdown. |
| Head of Product | Understand how feature usage and adoption relate to subsequent retention. | Investigate onboarding, usability or feature fit, then consider product changes where evidence supports them. |
| Customer Success | Understand how support experience relates to subsequent retention and recurring revenue losses. | Investigate high-risk accounts and support bottlenecks, then consider changes based on the identified cause. |

## 5. Business Questions

These are analytical questions to be answered using data. In a real engagement, they would be reviewed with stakeholders to confirm their usefulness, priorities and scope.

### Primary Question

What is driving the slowdown in RavenStack's MRR growth?

### Secondary Questions

1. **MRR movements:** How have changes in New, Expansion, Reactivation, Contraction and Churned MRR contributed to the slowdown in RavenStack's monthly MRR growth rate?
2. **Customer segments:** How has churn changed across customer segments over time?
3. **Product usage:** How is feature usage frequency over a 30-day period associated with paid-customer retention at the end of the following 30 days?
4. **Support experience:** How is support CSAT associated with paid-customer retention 30 days after the selected CSAT response, and how has this relationship changed over time?
5. **Subscription plans:** How has Expansion MRR changed across Basic, Pro and Enterprise over the latest three complete months available in the dataset?

## 6. Initial Scope and Limitations

The questions and analysis windows are proposed requirements, subject to source-data availability. Working metric definitions and observation rules are documented in [metric_definitions.md](metric_definitions.md). Source mapping, the availability of customer segments and data-dependent choices such as usage bands and CSAT thresholds remain subject to validation.

Paid-customer retention is measured at a specified endpoint and can include accounts that leave and return before that endpoint. For the support analysis, the selected CSAT response's recorded timestamp starts the 30-day follow-up; it is an operational post-support anchor rather than a verified ticket-resolution time. The three-month plan comparison uses the latest three complete months available in the dataset.

Associations between product usage or support experience and retention will not, on their own, establish causation. Any identified pattern must be connected to MRR movements over time before being presented as evidence that it contributes to the growth slowdown.

The rationale, illustrative calculations and interpretation limits for each question are documented in [business_questions_defence.md](business_questions_defence.md). No source feasibility, completed implementation or analytical result is established by these documents alone.
