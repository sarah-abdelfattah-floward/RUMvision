# RumVision Evaluation Report

---

## 1. Executive Summary

We want to improve our website's Core Web Vitals — the performance metrics Google uses as a ranking signal. Right now, we lack real-time visibility into how actual users experience our site. 

Our current performance analysis approach relies heavily on synthetic testing tools such as Lighthouse and PageSpeed Insights, these tools provide only partial visibility into the actual experience of real users in production environments. They do not fully capture the impact of:

- Real-world network conditions
- Device diversity
- Third-party scripts
- Production regressions
- Geographic differences
- User interaction delays
- Release-related performance degradation

In addition to that, Google Search Console also gives us data and visibility, but with a 28-day delay and no ability to diagnose root causes.

RumVision is a Real User Monitoring (RUM) platform that gives us instant, actionable performance data from every visitor session. It detects regressions within hours of a deploy, identifies which third-party scripts are hurting performance, and benchmarks us against competitors.

---

## 2. What Is Real User Monitoring (RUM)?

Real User Monitoring is a technique for measuring website performance by collecting data from actual visitors as they use the site. Every time a real person loads a page, clicks a button, or scrolls through content, a lightweight script records how long things took and whether anything went wrong.

This is fundamentally different from synthetic monitoring (covered in the next section). Instead of simulating a user in a controlled lab environment, RUM captures what actually happens in the real environment, across thousands of different devices, browsers, network connections, and geographic locations.

Here is how RUM data collection typically works:

```
┌─────────────┐      ┌──────────────────┐      ┌─────────────────┐
│  Real User  │──────│  JS Snippet on   │──────│  RUM Platform   │
│  visits     │      │  your website    │      │  (RumVision)    │
│  your site  │      │  collects timing │      │  aggregates &   │
│             │      │  data silently   │      │  analyzes data  │
└─────────────┘      └──────────────────┘      └─────────────────┘
                                                        │
                                                        ▼
                                               ┌─────────────────┐
                                               │  Dashboards,    │
                                               │  Alerts, and    │
                                               │  Reports        │
                                               └─────────────────┘
```

The data RUM collects includes:

- **Page load timing:** How long each phase of loading took (DNS, connection, server response, rendering)
- **Core Web Vitals scores:** LCP, INP, and CLS for every single page view
- **Device and browser context:** What hardware, OS, browser version, and screen size the visitor used
- **Geographic distribution:** Where visitors are connecting from and how network latency varies by region
- **Third-party script impact:** How external scripts (analytics, ads, chat widgets) affect the user's experience
- **Error and interaction data:** JavaScript errors, failed resource loads, and response times for user interactions

The key advantage of RUM is sample size and reality. We're not testing one scenario on one device, we're measuring millions of real scenarios across your actual user base. This means we see problems that only appear on certain devices, in certain regions, or under certain network conditions. Problems that synthetic testing will never catch.

---

## 4. Why Synthetic Monitoring Alone Is Not Enough

Most teams start with synthetic monitoring tools like Google Lighthouse or PageSpeed Insights. These tools simulate a page load in a controlled environment, a specific device, a specific network speed, a specific location, and report back a score. They're useful for catching obvious issues during development, but they have significant blind spots that make them insufficient as the only performance monitoring strategy.

**The core problem is that synthetic tests don't reflect reality.** They test one configuration at a time. The actual users bring thousands of configurations: old Android phones on 3G in Africa, iPads on hotel WiFi, desktop Chrome with 47 extensions installed, corporate networks with aggressive proxy servers. A synthetic test might show a perfect score while 20% of the real users experience a broken layout or a 6-second load time.

Here are the specific gaps that synthetic monitoring cannot cover:

**Device diversity.** Lighthouse typically tests on a simulated mid-tier device. But the real traffic includes everything from flagship iPhones to budget Android phones with 2GB of RAM. JavaScript that executes in 50ms on a fast phone might take 800ms on a slow one, and that difference shows up in INP scores. Synthetic tools don't test the long tail of devices your users actually have.

**Network variability.** Synthetic tests use a fixed simulated network speed (usually "Fast 3G" or "4G"). Real users experience everything from fiber connections to congested cellular networks, with packet loss, DNS failures, and CDN routing issues that change by the minute. A page that loads in 1.2 seconds on a synthetic test might take 4+ seconds for 15% of your users.

**Third-party script interference.** When you run Lighthouse, it loads your page in isolation. In reality, your users have ad blockers, browser extensions, and your own third-party scripts (analytics, A/B testing, chat widgets, consent managers) all competing for CPU time and network bandwidth. A consent banner that triggers a 300ms layout shift will never show up in a Lighthouse audit.

**Geographic and CDN effects.** Synthetic tests typically run from one or two locations. Your users are distributed globally. CDN cache misses, regional server issues, and cross-ocean latency all affect real performance in ways a single-location test cannot reveal.

**Temporal patterns.** Performance can degrade at specific times, during traffic spikes, when batch jobs run on shared infrastructure, or when a third-party script updates itself. Synthetic tests run at scheduled intervals and miss these transient issues that affect thousands of users.

The bottom line is that synthetic monitoring tells you how fast your site *could* be. RUM tells you how fast it *actually is* for your users. You need both, but if you only have one, RUM gives you the truth.

---

## 5. What Is RumVision

RumVision is a dedicated Real User Monitoring platform built specifically for tracking and improving Core Web Vitals. Unlike general-purpose APM tools (like Datadog or New Relic) that offer RUM as one feature among many, RumVision is purpose-built for web performance optimization. It's designed to be usable by both developers who need granular debugging data and non-technical teams (SEO, growth, product) who need high-level performance visibility.

**Company background.** RumVision is a European company (EU-based hosting and data processing). They position themselves as a specialist tool for teams that care deeply about Core Web Vitals, particularly e-commerce companies and content publishers where page speed directly impacts revenue. Their notable clients include Dexerto (a major gaming/entertainment publisher), Suzuki, Loop Earplugs, and Phone Arena.

**How it works.** We add a small JavaScript snippet to our website. This snippet collects performance data from every visitor session, anonymously, without collecting any personally identifiable information. The data flows to RumVision's EU-hosted servers where it's processed and made available through dashboards, alerts, and reports. Setup takes approximately 5 minutes for the basic implementation, and they offer a free 1-hour onboarding call with their performance experts to help configure it properly.

**Who it's for.** RumVision serves developers and engineers, it provides template-level performance breakdowns, third-party script attribution, and deployment regression detection. For SEO managers, growth teams, and leadership, it provides competitor benchmarks, weekly KPI reports, and clear pass/fail scores that don't require technical interpretation.

---

## 6. Features in Detail

### Real-Time Core Web Vitals Monitoring

The core of RumVision is continuous, real-time measurement of all Core Web Vitals metrics across the entire user base. Unlike Google Search Console which aggregates data over a rolling 28-day window (meaning we don't see the impact of a change for nearly a month), RumVision shows the performance data within hours. This means we can deploy a change in the morning and know by afternoon whether it helped or hurt.

The monitoring goes beyond simple averages. we can segment data by device type, browser, geographic region, connection speed, and, critically, by page template. This template-level view is particularly valuable because it lets we isolate performance issues to specific page types (e.g., "product detail pages are slow on mobile" vs. "the homepage is fine everywhere").

### Automated Issue Detection and Alerts

RumVision automatically identifies performance regressions and creates issues in its system. When a deployment causes any spikes, or when an interaction becomes slow, the platform detects it and can notify us via Slack or email. Crucially, these issues also auto-close when the problem resolves, which prevents alert fatigue from stale tickets.

The alert system is tied to deployments, meaning we can correlate performance changes with specific code releases. This dramatically reduces the time spent asking "what changed?" when metrics degrade.

### Third-Party Script Rating

Every external script on our site (Google Analytics, ad networks, chat widgets, A/B testing tools, consent managers) receives a performance rating: Good, Moderate, or Poor. This rating is based on the script's actual measured impact on real users, not a theoretical assessment. This feature is invaluable because third-party scripts are often the #1 cause of Core Web Vitals failures, and they change without our knowledge or consent. A third-party vendor pushing a bad update can tank our CLS score overnight, RumVision catches this and alerts us.

### Competitor Benchmarking

RumVision lets us track up to 10 competitors' Core Web Vitals performance using data from Chrome UX Report (CrUX). This gives us a clear picture of where we stand relative to our market. The historical data extends back 6 months, so we can see trends and identify when competitors improved (or degraded).

This feature is particularly useful for management reporting, it frames performance investment in competitive terms rather than abstract technical metrics.

### Weekly KPI Reports

The platform generates automated weekly performance reports that can be sent to our inbox or Slack channels. These reports summarize our Core Web Vitals status, flag any regressions from the previous week, and show progress toward targets. This is designed for stakeholders who don't log into the dashboard daily but need to stay informed.

### Privacy and GDPR Compliance

RumVision collects only anonymous performance data. No personally identifiable information is captured, no IP addresses, no user IDs, no cookies that track individuals. All data is processed and stored in the EU. This makes it GDPR-compliant by design and means it does not require cookie consent banners to operate. This is a meaningful advantage over some competing tools that require consent management integration.

---

## 7. Technical Integration

### How the JavaScript Snippet Works

RumVision uses a lightweight JavaScript snippet that is embedded in the `<head>` section of the HTML pages. The snippet initializes asynchronously (not blocking page rendering) and begins collecting performance timing data using standard browser APIs like the Performance Observer API and the PerformanceNavigationTiming API.

The data collected is batched and sent to RumVision's servers via beacon requests (using the `navigator.sendBeacon` API when available), which means the data transmission itself doesn't impact user experience. Beacons are low-priority requests that the browser handles in the background.

### Deployment Options

You have two main options for deploying the snippet:

| Option | Pros | Cons |
|--------|------|------|
| **Direct embed in HTML `<head>`** | Most accurate data collection; loads earliest | Requires a code deploy to add/update |
| **Google Tag Manager (GTM)** | No code deploy needed; marketing can manage | Slightly delayed loading; relies on GTM working correctly |

RumVision recommends the direct embed for the most accurate data, but GTM is supported as a viable alternative.

### Performance Impact

The snippet is designed to be lightweight. It uses standard browser performance APIs that are already running regardless of whether or not we collect the data. The data transmission uses beacons which are explicitly designed not to impact user experience. That said, any JavaScript added to a page has *some* cost as the script must be parsed and executed. RumVision does not publicly disclose the exact size of their snippet, which is something we should verify during a demo call with their team.

### Data Flow Architecture

```
┌──────────────┐     ┌──────────────────┐     ┌───────────────────┐
│   The Users  │────▶│  RumVision JS    │────▶│  EU-hosted        │
│  (browsers)  │     │  (in page head)  │     │  ingestion API    │
└──────────────┘     └──────────────────┘     └───────────────────┘
                                                       │
                                                       ▼
                                              ┌───────────────────┐
                                              │  Processing &     │
                                              │  Aggregation      │
                                              └───────────────────┘
                                                       │
                              ┌─────────────────────────┼─────────────────────┐
                              ▼                         ▼                     ▼
                     ┌──────────────┐        ┌──────────────┐      ┌──────────────┐
                     │  Dashboards  │        │  Alerts      │      │  Weekly      │
                     │  & Explorer  │        │  (Slack/     │      │  Reports     │
                     │              │        │   Email)     │      │              │
                     └──────────────┘        └──────────────┘      └──────────────┘
```

### Sampling Configuration

At the Scale plan level, RumVision supports custom sampling setup. This means you can choose to only collect data from a percentage of sessions (e.g., 10% of traffic) to stay within pageview limits while still getting statistically valid data. The Growth plan uses full collection by default.

### Setup Timeline

| Phase | Duration | Who's Involved |
|-------|----------|---------------|
| Snippet deployment | 2-3 hours | Frontend developer |
| Onboarding call with RumVision | 1 hour | Engineering lead + whoever will own the tool |
| Template configuration | 2-4 days | Frontend developer (one-time) |
| Alert setup (Slack integration) | 1 hour | DevOps or engineering |
| Full data collection begins | Immediate after deploy | Automatic |
| Meaningful trend data available | 1-2 weeks | — |

Total time from decision to value: approximately 2 weeks.

---

## 8. Pricing & Cost Breakdown

All prices below are in EUR, excluding VAT. Data sourced from rumvision.com as of May 2026.

### Plan Comparison

| | **Growth Plan** | **Scale Plan** |
|---|---|---|
| **Monthly pageviews included** | 1.5 million | 7.5 million |
| **Domains included** | 2 | 5 |
| **Data retention** | Standard | 13 months |
| **Custom sampling** | No | Yes |
| **Setup call** | 1 hour free | 1 hour free |
| **Monthly consultancy** | Included | Included |
| **Competitor tracking** | Up to 10 via CrUX | Up to 10 via CrUX |

### Pricing by Billing Cycle

| Billing Cycle | Growth Plan | Scale Plan |
|--------------|-------------|------------|
| **Monthly** | €150/month | €700/month |
| **Quarterly** (3 months) | €450 (€150/mo) | €2,100 (€700/mo) |
| **Annual** (10% discount) | €1,620/year (€135/mo) | €7,560/year (€630/mo) |

### Add-on Costs

| Add-on | Monthly | Quarterly | Annual |
|--------|---------|-----------|--------|
| Extra domain | €20/month | €60 | €216/year |
| Additional 1M pageviews | €75/month | €225 | €810/year |
| Autoscaling (per 1M overage) | €100/month | — | — |
| 1-hour consultancy session | Custom pricing | — | — |

Note: Autoscaling costs €25 more per million pageviews than pre-paid add-ons. If it is expected to regularly exceed the plan's limits, pre-purchasing additional pageviews is cheaper than relying on autoscaling.

### Cost Scenarios

Since I don't yet know our exact monthly pageview count, here are projections for different traffic levels:

| Scenario | Plan | Annual Cost | Monthly Equivalent |
|----------|------|-------------|-------------------|
| Under 1.5M pageviews, 1-2 domains | Growth (annual) | **€1,620/year** | €135/month |
| 2M pageviews, 2 domains | Growth + 1M extra (annual) | **€2,430/year** | €202/month |
| 3M pageviews, 3 domains | Growth + 2M extra + 1 domain (annual) | **€3,456/year** | €288/month |
| Under 7.5M pageviews, up to 5 domains | Scale (annual) | **€7,560/year** | €630/month |
| 10M pageviews, 5 domains | Scale + 2.5M extra (annual) | **€9,585/year** | €799/month |

### What's Included at No Extra Cost

Both plans include monthly support and consultancy calls, daily regression alerts, weekly KPI reports, competitor benchmarking, the initial setup call, and access to all dashboard and reporting features. There are no per-user fees and the entire team can access the platform.

---

## 9. Comparison Against Competitors

### vs. Free Tools

| Capability | Google Search Console | PageSpeed Insights | Chrome UX Report (CrUX) | **RumVision** |
|------------|----------------------|-------------------|------------------------|---------------|
| Real user data | Yes (28-day delayed) | No (lab only) | Yes (28-day delayed) | **Yes (real-time)** |
| Regression detection | No | No | No | **Yes (automated)** |
| Template-level breakdown | No | Single URL only | No | **Yes** |
| Third-party script attribution | No | Partial | No | **Yes (with ratings)** |
| Competitor benchmarking | No | No | Manual queries | **Yes (automated)** |
| Alerts on degradation | No | No | No | **Yes (Slack/email)** |
| Weekly reports | No | No | No | **Yes** |
| Cost | Free | Free | Free | €135-630/month |
| Setup effort | Already have it | None | API access needed | 5-minute snippet |

**The core gap with free tools:** Google's free tools give us a hint about the performance, they tell us what happened 28 days ago. RumVision gives you a comprehensive idea that it tells us what's happening right now. The free tools cannot tell us which deploy caused a regression, which third-party script is the problem, or how us compare to competitors over time. They also cannot alert us when something breaks.

The free tools are insufficient. They're useful as a verification source, but not as a working tool.

### vs. Paid RUM Competitors

| Capability | SpeedCurve | Datadog RUM | New Relic Browser | Raygun | **RumVision** |
|------------|-----------|-------------|-------------------|--------|---------------|
| **Focus** | Performance + synthetic | Full-stack APM | Full-stack APM | Error tracking + perf | Core Web Vitals specialist |
| **Core Web Vitals depth** | Good | Moderate | Moderate | Basic | **Excellent** |
| **Third-party script ratings** | Yes | No | No | No | **Yes** |
| **Competitor benchmarking** | Yes | No | No | No | **Yes** |
| **Automated issue detection** | Partial | Custom alerts | Custom alerts | Yes (errors) | **Yes (auto-close)** |
| **Setup complexity** | Moderate | High | High | Moderate | **Low (5 min)** |
| **GDPR by design** | Varies | US company | US company | NZ company | **EU-hosted, no PII** |
| **Starting price** | ~$200/mo | ~$15/host/mo (scales fast) | ~$0.25/GB (scales fast) | ~$20/mo | **€150/mo** |
| **Best for** | Performance-focused teams with dev resources | Teams already using Datadog for infra | Teams already using New Relic | Error-heavy apps | **Teams focused specifically on CWV and SEO** |

**Positioning summary:** RumVision sits in a specific niche. It's not trying to be a full-stack APM tool, those. If your primary goal is specifically improving Core Web Vitals for SEO and user experience, and we want a tool that non-technical stakeholders can also use, then RumVision offers more focused functionality at a lower price point than adding RUM to a full APM suite.

SpeedCurve is the closest direct competitor in terms of focus. It offers both synthetic and RUM monitoring with a strong performance focus. The key differentiators for RumVision are: lower setup effort, automated issue lifecycle (issues auto-close), GDPR-native design (EU hosting, no PII), and a price point that doesn't scale as aggressively with traffic.

---

## 10. Risks & Limitations

### What RumVision Does NOT Do

RumVision monitors performance, it does not fix it. It tells us what's slow and helps us identify why, but the actual optimization work still falls on the engineering team. It also does not provide:

- Server-side performance monitoring (still need APM for backend issues)
- Synthetic/lab testing (still need to use Lighthouse for pre-deploy checks)
- Error tracking (it's not a replacement for Sentry or similar tools)
- A/B testing of performance changes (it shows impact after the fact, not during)
- Full session replay or user behavior analytics

### Vendor Lock-in Risk

RumVision stores the historical performance data. If we later decide to switch tools, we lose access to that historical baseline. The data retention is 13 months on the Scale plan (unclear on Growth). Moving to a competitor means starting the historical data from scratch. This is a moderate risk, but it's worth noting that performance baselines are less sticky than, say, application logs, we could reconstruct trends from CrUX data if needed.

### Single-Vendor Dependency

RumVision appears to be a relatively small, specialized company (they're not a household name like Datadog or New Relic). This carries inherent risk: if the company pivots, gets acquired, or shuts down, we'd need to migrate. Their client list (Suzuki, Dexerto, Phone Arena) suggests they're established enough to be stable, but they lack the "too big to fail" safety of larger vendors.

### Data Accuracy Considerations

- **Ad blockers:** Some ad blockers and privacy extensions block RUM scripts. This means you're likely missing 10-30% of users (the ones most privacy-conscious). This is true of all RUM tools, not specific to RumVision.
- **Bot traffic filtering:** The accuracy of the data depends on RumVision's ability to filter out bot traffic. Bots don't experience performance the same way humans do.
- **Low-traffic pages:** For pages with very few daily visits, the sample size may be too small for statistically meaningful CWV scores.

### JavaScript Dependency

The tool requires JavaScript execution to collect data. This means:

- It cannot measure the experience of users with JavaScript disabled (a tiny minority, but worth noting)
- If their snippet has a bug, it could theoretically impact the page (rare but possible with any third-party JS)
- The snippet size and execution cost have not been publicly disclosed, we should confirm this during a demo

### Contractual Unknowns

From the public pricing page, we cannot determine:

- SLA guarantees (uptime commitment for their dashboard and data collection)
- Data processing agreement (DPA) details beyond "GDPR compliant"
- What happens to our data if we cancel
- Whether there's a minimum contract term
- Support response time guarantees

### Cost at Scale

If our traffic grows significantly, costs can escalate. Each additional 1M pageviews costs €75/month (€810/year). If we grew from 5M to 15M pageviews, the annual cost would jump from ~€7,560 to ~€14,640+. This isn't unreasonable, but it's worth modeling growth scenarios to avoid budget surprises.

---

## 11. Implementation Plan

### Phase 1: Evaluation (Week 1)

- Confirm our monthly pageview count across all domains we want to monitor
- Schedule a demo call with RumVision to ask clarifying questions (snippet size, SLA, contract terms, trial availability)
- Determine which domains are in scope
- Get budget approval based on the appropriate plan

### Phase 2: Setup (Week 2)

- Sign contract and provision account
- Deploy JavaScript snippet to staging environment first
- Verify data is flowing correctly
- Complete 1-hour onboarding call with RumVision team
- Configure page templates and URL groupings

### Phase 3: Production Launch (Week 2-3)

- Deploy snippet to production
- Configure Slack integration for alerts
- Set up weekly report recipients
- Add competitor domains for benchmarking (up to 10)
- Verify real data is appearing in dashboards

### Phase 4: Operationalize (Week 3-4)

- Establish baseline metrics (first 1-2 weeks of data)
- Set performance targets (e.g., "all templates pass CWV on mobile")
- Assign ownership: who monitors alerts, who triages regressions
- Onboard relevant team members on the dashboard
- Integrate into deploy workflow (check RumVision after every release)

### Ongoing

- Review weekly reports
- Use data to prioritize performance work
- Quarterly review: are we getting value? Should we expand coverage?

---

## 12. Bottom line

It is recommended to adopt RumVision. The core argument is straightforward: we cannot improve what we cannot measure in real-time. Google's free tools give us a 28-day-delayed view with no actionable detail. RumVision fills that gap at a reasonable cost (€135-630/month depending on our traffic) with minimal setup effort and strong GDPR compliance.

RumVision is the right choice over a general-purpose APM tool because our specific goal is Core Web Vitals improvement, not full-stack application monitoring. It's purpose-built for this problem, easier to set up, cheaper than adding RUM to an APM suite, and designed for both technical and non-technical stakeholders to use.

The risks are manageable: small vendor size is mitigated by their established client base, and vendor lock-in is limited because performance data is less sticky than application logs.


### Open Questions to Resolve Before Purchase

1. What is our exact monthly pageview count? (Determines Growth vs. Scale plan)
2. Does RumVision offer a free trial or proof-of-concept period (needs to be confirmed from their team)?
3. What is their SLA for data availability and dashboard uptime (probably high, but need to confirm)?
4. What is the exact size and performance cost of their JavaScript snippet?
5. Is there a minimum contract length, or can we cancel monthly?
6. Do they offer enterprise or custom pricing for larger organizations?

---
