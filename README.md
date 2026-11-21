# Delivery-route-problem-Investigation

Prepared by:  Felix Adebayo Bakare, Operations Analytics
Presented to:  Senior Management Team
Date:  November 2026
Dataset:  DataCo Global Supply Chain Dataset (Kaggle) — 180,519 orders, 53 variables
Tools used:  Python (pandas, matplotlib), Power BI

Executive Summary
We ship thousands of orders every day across multiple regions and shipping methods. The data tells a clear story: more than half of those orders are arriving late. That is not a rounding error. It is a structural problem that is costing us in refunds, in customer trust, and in repeat business we are not getting back.
I was asked to find out where the problem is coming from. Rather than reviewing 180,000 individual orders, I ran a full Python analysis against the complete order dataset. What came back was specific enough to act on. Five routes are responsible for a disproportionate share of everything going wrong. Three of those five have a 78 to 100 percent late delivery rate. That means nearly every single order on those routes is arriving late.
This report walks through the findings, explains what is driving them, and sets out a prioritized set of actions. The recommendation is not to fix everything at once. It is to fix the right things first, in the order that will have the biggest measurable impact.
1.  The Problem
Late deliveries are not just an operations metric. They show up in the business in several ways at once. A customer who waits two extra days for an order does not necessarily call to complain. They just do not come back. When they do complain, there is a service cost. When we refund shipping, there is a direct cost. And when it happens at scale, the aggregate damage to the brand is real even if it is hard to put a single number on it.
The question I was asked to answer was simple: where is the problem concentrated? The working assumption going in was that delays were spread fairly evenly across regions and shipping methods. The data says otherwise. The problem is highly concentrated in a small number of route and shipping mode combinations, and within those, in specific carrier and scheduling failures that can be addressed.
Overall Performance at a Glance
Metric	Value
Total orders analyzed	180,519
Orders arriving late	Approx. 99,000 (55% of total)
Unique routes analyzed	40+
Routes with 100% late rate	1 (Central America, First Class)
Routes with 78%+ late rate	3
Top 5 routes share of late orders	Over 60% of all late deliveries

2.  What the Data Shows
The Top 5 Worst Routes
After cleaning the data and removing cancelled and fraudulent orders, I grouped every remaining order by its route (region plus shipping method) and calculated how many arrived late. The table below shows the five routes that need attention most urgently.
Route	Total Orders	Late Orders	Late Rate
Central America — Standard Class	16,382	6,571	40.1%
Western Europe — Standard Class	15,143	6,089	40.2%
Western Europe — Second Class	5,206	4,236	81.4%
Central America — First Class	4,185	4,185	100.0%
Central America — Second Class	5,250	4,111	78.3%

The numbers on the right column are the ones that should stop the room. Central America First Class is running at 100 percent. Every order on that route is late. Western Europe Second Class is at 81 percent. Central America Second Class is at 78 percent. These are not performance gaps. They are complete failures of those specific route configurations, and they have been accumulating at scale.
The Pareto Picture
The Pareto analysis makes the prioritization straightforward. These five routes together account for more than 60 percent of all late orders across the entire network. The remaining 35+ routes share the other 40 percent between them. That means fixing these five routes gets us most of the way to a materially better on-time rate without touching anything else yet.
This is the core insight the analysis was designed to surface. We are not dealing with a network-wide problem that requires a network-wide solution. We are dealing with a concentrated problem that has specific causes and specific fixes.
Breakdown by Shipping Mode
Looking across all routes by shipping mode, Second Class and Standard Class consistently show the highest late rates. First Class performs better on most routes except Central America, where the 100 percent late rate suggests a carrier-specific or scheduling issue rather than a mode issue. Same Day delivery performs best overall, though it represents a much smaller volume.
Breakdown by Region
Central America and parts of Western Europe show the highest concentration of late deliveries. South America and Southeast Asia also appear in the mid-tier. Western Europe is interesting because it has high order volume combined with a split performance picture: Standard Class performs at 40 percent late while First Class on the same region performs significantly better. That gap points to a carrier or lead time issue specific to Standard Class in that region, not a regional infrastructure problem.
3.  What Is Driving This
Late delivery data alone tells you where the problem is. It does not automatically tell you why. Based on the patterns in the data and what we know about how these routes operate, there are three likely root causes worth investigating in parallel.
Carrier Performance Gaps
The 100 percent late rate on Central America First Class is almost certainly a carrier issue, not a demand or scheduling one. When an entire route class fails completely, the most likely explanation is a carrier that is not meeting its SLA commitments. First Class is a premium service. If it is performing worse than Standard Class on the same region, that is a contract and accountability problem.
Lead Time Misconfiguration
On the two high-volume Standard Class routes (Central America and Western Europe), a 40 percent late rate at that scale suggests the promised delivery windows are not realistic for the actual transit times. Orders are being promised in five days when the route consistently takes seven. That gap between what is promised and what is achievable produces late orders structurally, regardless of how well the carrier performs.
Shipping Mode Selection Logic
Some of the late orders on Second Class routes may be a result of customers selecting a cheaper shipping option for orders that, by size, weight, or destination, cannot realistically meet Second Class timelines. If the system does not flag this at the time of order, the customer gets a late delivery that could have been prevented by routing them to a more appropriate option or by being transparent about realistic timelines upfront.
4.  Recommendations
The recommendations below are ordered by expected impact. The first two can be acted on quickly and should produce measurable improvement within 30 to 60 days. The third and fourth are medium-term structural fixes.
1.  Audit and replace the Central America First Class carrier immediately
A 100 percent late rate on a premium service is not a performance issue to manage. It is a contract issue to resolve. Pull the SLA agreement, document the breach, and begin conversations with alternative carriers for this lane. Until a replacement is in place, consider suspending First Class as an option for Central America or being explicit with customers that timelines cannot be guaranteed.
2.  Recalibrate Standard Class lead times for Central America and Western Europe
Work with logistics to establish actual average transit times on these routes over the past six months. Update the promised delivery windows in the system to reflect reality. A customer who is promised seven days and gets seven days is not a dissatisfied customer. The 40 percent late rate on these routes is at least partly a promise problem, not just a delivery problem.
3.  Put Second Class routes under carrier performance review
Western Europe Second Class at 81 percent and Central America Second Class at 78 percent both need a carrier-level performance review. These are not close to acceptable. Identify whether the issue is carrier-specific or route-specific, then decide whether to renegotiate, replace, or restructure the service offering on those lanes.
4.  Build a weekly route performance dashboard
The analysis I ran was a one-time look at historical data. What we need going forward is a live view of late delivery rates by route that updates weekly and flags any route crossing a threshold, say 25 percent late, automatically. I have already built the Power BI framework for this. It can be connected to live data with minimal additional work and would give operations leadership an early warning system rather than a retrospective problem report.
5.  What This Is Costing Us
Exact financial impact depends on refund policy, customer lifetime value assumptions, and service cost data that this analysis did not have access to. But the order of magnitude is clear enough to frame the urgency.
Assumption	Estimate
Approximate late orders across top 5 routes	25,000+
Average refund or credit per late order (est.)	$8 to $15
Direct refund cost range	$200,000 to $375,000
Customer service handling cost per complaint (est.)	$5 to $8
Repeat business lost per dissatisfied customer (est.)	$150 to $300 annually
Projected annual revenue at risk from churn	Significant — requires CRM data to quantify

Even at conservative estimates, the direct cost of late deliveries on these five routes alone is in the range of two to four hundred thousand dollars annually before accounting for lost repeat business. The fixes recommended above cost a fraction of that to implement.
6.  Proposed Next Steps
Action	Owner	Timeline
Review Central America First Class carrier contract	Logistics / Procurement	Week 1
Suspend or flag CA First Class until resolved	Operations	Week 1
Pull actual transit time data for CA and WE Standard	Analytics	Week 1 to 2
Update delivery window promises in ordering system	Product / IT	Week 2 to 3
Initiate carrier review for WE and CA Second Class	Logistics	Week 2 to 4
Deploy weekly route performance dashboard in Power BI	Analytics	Week 3 to 4
Set threshold alerts for routes crossing 25% late rate	Analytics	Week 4
30-day review: measure improvement against baseline	Management	Day 30

Appendix: Methodology Notes
The analysis used the DataCo Global Supply Chain Dataset, available publicly on Kaggle. The dataset contains 180,519 order records with 53 variables covering shipping mode, region, product category, order value, delivery status, and more.
•	Cancelled and suspected fraud orders were removed before analysis. These orders never entered the fulfillment pipeline and cannot be late in the operational sense.
•	Routes with fewer than 200 orders were excluded to avoid drawing conclusions from statistically thin samples.
•	The late delivery indicator used throughout is the dataset's Late_delivery_risk field, which flags orders where delivery occurred after the promised date.
•	The Pareto chart ranks routes by total volume of late orders rather than late rate percentage, because a route with 100 orders and a 100% late rate matters less operationally than a route with 16,000 orders and a 40% late rate.
•	All code, data outputs, and the Power BI dashboard specification are available in the project GitHub repository for review and replication.

Felix Adebayo Bakare  |  Operations Analytics  |  November 2026
