# 95th Percentile Billing Explained: How the 5% Rule Works, What a 100 Mbps Commit Really Delivers, and Which Plan Fits

95th percentile billing charges you for the highest bandwidth level you hold for all but about 36 hours of the month. Your provider samples the port every 5 minutes, collects 8,640 readings in a 30-day month, discards the top 432 of them (5%), and bills the highest value that survives. A 24-hour spike vanishes into that discard pile. A plateau that lasts a week lands on your invoice at full price.

Everything else in this article follows from that one rule: how to turn a 95th percentile number into terabytes, how your traffic curve sets the real price per GB, what to ask a provider before signing, and where SixtyNet's 95th-billed line sits among its other plans.

> **Quick pick:** size the commit to the level you hold, not the peak you touch. If your traffic sits above a given Mbps for more than roughly 36 hours a month, that level is your bill. Of the plans on SixtyNet's VPS hosting page, only the Los Angeles China Unicom Premium line is advertised with 95th percentile billing (100M basic network, 3× burst rate) at **$2,500/yr**, down from $3,000. 👉 [Check the China Unicom Premium plan's current price and terms](https://sixtynet.net/aff.php?aff=39)

## SixtyNet's current plans, and which one this math applies to

SixtyNet sells VPS plans on AMD EPYC hardware with NVMe storage, and nearly all of them are metered by monthly traffic quota rather than by bandwidth commit. The exception is the China Unicom Premium line in Los Angeles, which lists a 100M basic network, a 3× burst rate and 95th billing. That is the plan where the arithmetic below applies directly.

All prices are USD, shown as the annual rate on the site's hosting page. Residential plans also display monthly starting prices of $25, $45, $85 and $165 respectively.

| Plan | vCPU / RAM / NVMe | Network & traffic | Annual price | Order |
| --- | --- | --- | --- | --- |
| Residential-STARTER | 2 / 2 GB / 40 GB | 2G premium network, 5 TB, residential IP | $300 → **$260** | [Order Residential-STARTER](https://sixtynet.net/aff.php?aff=39) |
| Residential-MICRO | 4 / 4 GB / 80 GB | 4G premium network, 10 TB, residential IP | $540 → **$460** | [Order Residential-MICRO](https://sixtynet.net/aff.php?aff=39) |
| Residential-MEDIUM | 8 / 8 GB / 160 GB | 10G premium network, 20 TB, residential IP | $1,020 → **$860** | [Order Residential-MEDIUM](https://sixtynet.net/aff.php?aff=39) |
| Residential-LARGE | 16 / 16 GB / 320 GB | 10G premium network, 40 TB, residential IP | $1,980 → **$1,660** | [Order Residential-LARGE](https://sixtynet.net/aff.php?aff=39) |
| LAX-STARTER | 2 / 2 GB / 40 GB | 2G premium network, 5 TB, DDoS protection | $240 → **$200** | [Order LAX-STARTER](https://sixtynet.net/aff.php?aff=39) |
| LAX-MICRO | 4 / 4 GB / 80 GB | 4G premium network, 10 TB, DDoS protection | $480 → **$400** | [Order LAX-MICRO](https://sixtynet.net/aff.php?aff=39) |
| LAX-MEDIUM | 8 / 8 GB / 160 GB | 10G premium network, 20 TB, DDoS protection | $960 → **$800** | [Order LAX-MEDIUM](https://sixtynet.net/aff.php?aff=39) |
| LAX-LARGE | 8 / 16 GB / 320 GB | 10G premium network, 40 TB, DDoS protection | $1,920 → **$1,600** | [Order LAX-LARGE](https://sixtynet.net/aff.php?aff=39) |
| Irvine Pro Starter | 2 / 2 GB / 40 GB | 2G premium network, AS9929 China-optimized, 5 TB, residential IP | $420 → **$350** | [Order Irvine Pro Starter](https://sixtynet.net/aff.php?aff=39) |
| LA China Unicom Premium | Not listed on the page | 100M basic network, 3× burst, 95th billing, AS9929, free BGP session setup | $3,000 → **$2,500** | [Order the China Unicom Premium line](https://sixtynet.net/aff.php?aff=39) |

Three things on that page change how you use these plans:

- **Residential IP plans do not open ports 80 and 443.** SixtyNet states this as an IP risk-control measure, so anything that has to answer HTTP on the standard ports needs a LAX DDoS-protected plan instead.
- **Irvine Pro refunds are issued as account balance only**, according to the plan page. That differs from a cash refund and matters if you are trialling the China-optimized routing.
- **Two configurations are not priced on the hosting page**: a larger Irvine Pro build (4 cores / 4 GB / 80 GB, 4G network, 10 TB) and the RAID 10 storage builds (4 cores / 4 GB / 300 GB + 200 GB, 10 TB; 8 cores / 8 GB / 700 GB + 300 GB, 15 TB). Checkout shows their current rates.

For the 95th-billed plan, the page states the base network and burst rate but does not publish the commit or the per-Mbps overage rate. Ask before you order; the questions later in this article are the ones to send.

## What the 5% rule actually forgives

The mechanics are the same at every provider that offers this billing model. A reading every 5 minutes gives 12 per hour, 288 per day, and 8,640 across a 30-day month. Sorting those readings and dropping the top 432 leaves 8,208. The largest of the survivors is your billable rate.

Discarding 5% of the samples is the same as not billing 36 hours per month, or about 1.2 hours per day. Grafana Cloud, which applies the identical rule to metrics usage rather than bandwidth, states it the same way: the top five percent of usage time in each monthly period is forgiven, "roughly the top 36 hours in a 720 hour month."

The 95th percentile is neither the average nor the maximum, and the gap between them is where budgets go wrong. In a month of CenturyLink data presented by Amie Elcan at NANOG 53 in October 2011, one interface produced a 95th percentile of 25 Mbps against a maximum of 31 Mbps and an average of 15 Mbps. A single day from the same data showed max 26.3, mean 14.6, 95th percentile 24.1, a 95th-to-mean ratio of 1.6. In that month, traffic above the 95th percentile line accounted for about 9% of total volume, and none of it was billed.

## Turning a 95th percentile number into terabytes

The number on your invoice is a rate in Mbps. The number you probably care about is how much data that rate buys. Converting one to the other takes one extra input: the ratio of your 95th percentile to your monthly mean.

Worked through with the CenturyLink example, a 100 Mbps commit and a 95th-to-mean ratio of 1.8:

1. 100 ÷ 1.8 = **55.6 Mbps** average for the month
2. 55.6 ÷ 8 = **6.9 MB/s**
3. 6.9 × 2,592,000 seconds (30 days) = **18,000,000 MB**
4. ÷ 1,024 = **17,578 GB, or about 17.2 TB**

At a hypothetical $3 per Mbps, the bill is 100 × $3 = $300, which works out to $300 ÷ 17,578 GB = **1.7 cents per GB**. That $3 figure and the $1–$5 range it comes from are 2011 unit prices used in the original presentation. They demonstrate the method; they are not a current market rate, and SixtyNet's China Unicom Premium pricing is quoted per plan, not per Mbps.

Run the same four steps with your own commit and your own measured ratio and you have a defensible cost-per-GB estimate before you sign anything.

## Peakedness sets your real price per GB

A 1 Mbps commit does not buy a fixed amount of traffic. It buys whatever your traffic curve allows, and the curve changes the answer by more than a factor of two.

| 95th-to-mean ratio | GB per month per 1 Mbps of commit | Efficiency |
| --- | --- | --- |
| 1.00 | 316 | 100% |
| 1.25 | 253 | 80% |
| 1.50 | 211 | 67% |
| 1.75 | 181 | 57% |
| 2.00 | 158 | 50% |
| 2.25 | 141 | 44% |

Natural traffic tends to land near 2:1, with daily peaks closer to 3:1. At a 2:1 ratio, half of what you pay for is headroom you never use. Combine the ratio with the per-Mbps rate and the cost per GB spans 0.32 cents to 3.56 cents across the $1–$5 range, at ratios from 1.0 to 2.25. Two providers quoting the same per-Mbps price can be 50% apart on cost per delivered GB once your curve is taken into account.

The lever you control is peakedness. Moving nightly backups off the evening peak, serving static and video assets through a CDN so the origin sees steadier load, rate-limiting bulk sync jobs, and shifting batch transfers into low-traffic hours all push the 95th percentile toward the mean. That is the entire cost strategy under this model: flatten the curve, or pay for the shape of it.

If your workload can be reshaped that way, the China Unicom Premium line's 3× burst headroom is the useful part of the offer, since a short burst you can absorb is exactly what the 5% discard is designed to ignore. 👉 [See what the 100M base / 3× burst configuration includes](https://sixtynet.net/aff.php?aff=39)

## Six questions to send before you sign

Provider documentation is uneven on these points, and each one changes your bill.

1. **Is the sampling interval 5 minutes?** The 36-hour forgiveness is a direct consequence of 8,640 samples per month. A shorter interval, or a different percentile, changes how much of your peak is exempt.
2. **What is the overage rate per Mbps above the commit?** A cheap commit with an expensive overage is a worse deal than a slightly larger commit at a flat rate.
3. **Is overage invoiced separately?** At Ingenuity Cloud Services, overage appears on a separate invoice from the usual monthly bill, itemized by pool.
4. **Do you send alerts when I approach the commit?** Ingenuity states plainly that it does not send email notifications when a pool looks likely to exceed its commit. Assume silence unless told otherwise and monitor the usage graph yourself.
5. **How are pools grouped?** The same provider's default puts every server in a datacenter into one bandwidth pool, with each server adding to the pool's commit, and the pool's overage rate equals the highest overage rate of any server inside it. One badly configured machine can raise the rate for its neighbours.
6. **Is the 95th percentile applied to bandwidth or to something else?** Grafana Cloud bills the 95th percentile of active series and data points per minute. If the unit is not Mbps, none of the conversions above apply.

## The same 95th percentile math shows up on your monitoring bill

Observability pricing now uses this model, so the arithmetic transfers. Grafana Cloud counts a series as active if it received data in the last 20 minutes, and bills the 95th percentile of the active series count across the period. Their own example: a stack normally running 6,000 active series that spikes to 30,000 for 24 hours is still billed close to the 6,000-series rate, because 24 hours falls inside the forgiven top 5% of a 720-hour month.

Two details from that documentation are worth carrying over to any 95th-billed contract:

- **Resolution multiplies the bill.** Usage is the higher of active series or total DPM divided by included DPM. Halving the scrape interval from 60 seconds to 30 seconds doubles billable usage at the same series count, so 50,000 series becomes 100,000 billable units.
- **Month-to-date dashboards mislead.** Grafana's cost panels show a cumulative month-to-date figure refreshed hourly, and the 95th percentile recalculates as the month fills, so the displayed value can fall. Deriving daily usage from it with `delta()` or `increase()` can produce negative numbers. That is expected behaviour, not a billing error.

The transferable lesson: a brief spike doesn't set your bill, but a sustained level does. Cardinality that stays high for a week costs money the same way a week-long traffic plateau does.

## When 95th percentile billing is the wrong deal

This model rewards bursty traffic and punishes steady traffic. Three cases where it works against you:

- **Your traffic is flat and close to the commit.** You get no benefit from the 5% discard and every extra Mbps is a straight cost. A quota-based plan with a generous traffic allowance is usually simpler, and SixtyNet's residential and LAX plans are priced that way, at 5 TB to 40 TB per month.
- **Your peaks last longer than 36 hours a month.** A nightly backup window of two hours is 60 hours a month, which is well past the exemption. Either move the job or buy the commit to cover it.
- **You cannot measure your own curve.** Without 30 days of 5-minute data you are guessing at both the commit and the ratio, and the ratio is what determines your cost per GB.

If you answer yes to "brief, infrequent peaks, steady baseline," 95th percentile billing is usually the cheaper structure. If your baseline itself is the peak, it is not.

## Choosing between the options

Match the plan to the shape of your traffic:

- **Steady traffic, no meaningful peaks:** buy the commit that covers your baseline exactly, and treat the overage rate as the price of being wrong. On SixtyNet's list, quota-metered plans remove the question entirely.
- **Short peaks you can absorb (under 36 hours a month):** 95th percentile billing is the better structure, since those hours are free. Check that the burst allowance on the plan actually accommodates your peak rate.
- **Sustained peaks:** buy the commit at the level you sustain. A commit 20% below your plateau does not save 20%; it converts the difference into overage at whatever per-Mbps rate the provider sets, on a separate invoice, usually without a warning email.

Before you order, confirm two things in writing: the percentile and sampling interval used, and the per-Mbps overage rate with the invoicing method. Then set up your own usage check, because the provider may not send one.

SixtyNet's 95th-billed option is the Los Angeles China Unicom Premium line at **$2,500/yr** (list $3,000), with a 100M base network, 3× burst and free BGP session setup. 👉 [Check availability and the full configuration on SixtyNet](https://sixtynet.net/aff.php?aff=39)
