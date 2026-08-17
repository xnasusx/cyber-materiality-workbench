# Cyber Materiality Workbench

Work a cybersecurity incident through an SEC **Item 1.05** materiality determination — both legs of
the analysis, the four-business-day clock, and the contemporaneous memo that is the thing you will
actually be asked for later.

**Live:** https://rootcawsllc.github.io/cyber-materiality-workbench/

![Steps one and two of the workbench: the discovery and determination dates, then the quantitative screen showing an $11M expected loss and $28M reasonably likely downside against a $6.0M five-percent-of-pre-tax-income threshold, marked crossed at P50. Below it the benchmark cross-check, with a US financial-services data-breach shard selected and labelled "benchmark review candidate · module governed · 6 medium confidence", its published per-event range of $152,000 / $329,000 / $11,500,000, a note that the entered P50 sits between the central estimate and the ceiling while the P90 sits above it, a warning that this is corroboration for a class rather than evidence about the incident, and the six cited sources each with its own limitation](preview.png)

## Why this exists

Item 1.05 of Form 8-K gives a registrant four business days to disclose a material cybersecurity
incident. Two things about that sentence get misread constantly:

1. **The clock starts at determination, not discovery.** The four days run from the date you
   determine the incident is material. The determination itself must be made without unreasonable
   delay after discovery — which makes the gap between the two dates independently reviewable.
2. **There is no dollar threshold.** Materiality still runs on *TSC Industries* and *Basic*: would a
   reasonable investor consider it important, given the total mix of information? SAB No. 99 then
   makes the harder point — a quantitatively small number can still be material once qualitative
   factors are weighed.

Most calculators in this space return a score. A score is exactly the wrong output, because the
artefact that gets examined afterwards is the reasoning, not the number.

## What it does

**Step 1 — the two dates.** Capture discovery and determination separately. The tool computes the
elapsed gap and flags a long one, because "without unreasonable delay" is a standard someone will
eventually ask you to defend.

**Step 2 — the quantitative screen.** Four loss components sum to an expected (P50) exposure, and a
configurable multiplier produces the reasonably likely downside (P90) — Item 1.05 reaches impact
that is *reasonably likely*, not only impact already realised. Both are shown against 5% of pre-tax
income, with 0.5% of revenue alongside it because pre-tax income is a poor anchor near break-even.
Dollar fields accept shorthand: `500K`, `1.2M`, `3B`.

An optional **benchmark cross-check** sits at the foot of this step. Pick a comparable country,
sector and threat and the tool shows the published per-event loss range for that population, where
your entered P50 and P90 fall against it, and the sources behind every figure. It deliberately does
**not** fill anything in: a shard carries one loss range, this step wants four separate components,
and splitting one into four would invent an allocation nothing in the source supports. Only
USD-denominated shards are offered, because a determination reported in dollars should not quietly
borrow a figure in another currency.

Each shard shows its maturity status, its provenance tier, and how many of its parameters sit at
each confidence level. Where a shard has no practitioner "not good for" statement, the panel and
the memo both say the statement is missing rather than printing nothing — a memo should not imply
a caveat was considered and found unnecessary.

**Step 3 — the total mix.** Nine qualitative factors, each rated none / limited / moderate /
significant, covering data sensitivity, operational disruption, customer and contractual impact,
litigation exposure, reputational harm, ICFR impact, strategic information loss, concealment or
management conduct, and whether the incident masks a trend.

**Step 4 — the memo.** A plain-text determination memorandum recording the inputs, both legs of the
analysis, the conclusion and its basis, and the filing deadline. Copy it straight into your case
management system.

## How the determination logic works

The verdict is deliberately **not** an average of the two legs. Either one can carry it on its own:

| Condition | Result |
| --- | --- |
| Expected (P50) loss crosses the quantitative screen | Material |
| Any single qualitative factor rated *significant* | Material |
| P90 crosses the screen **and** qualitative weight ≥ 6 | Presumed material |
| Qualitative weight ≥ 10 with no dominant factor | Presumed material |
| P90 crosses, or any moderate factors present | Too close to call — escalate |
| Neither leg approached | Not material on these facts |

That asymmetry is the whole point of SAB No. 99. A $40K incident against $120M of pre-tax income
reads "not material" — until one factor is rated significant, at which point it is material and the
dollar figure never mattered.

Every determination can be overridden manually, and the override is recorded in the memo as a filer
judgment. The tool assists the analysis; it does not make the call.

## The four-business-day clock

The deadline calculation excludes weekends **and** US federal holidays, with the Saturday-observed-
Friday and Sunday-observed-Monday rules applied. This matters more than it sounds: a determination
made Wednesday 1 July 2026 is not due Tuesday 7 July, because Independence Day falls on a Saturday
that year and is observed Friday 3 July. The correct deadline is **Wednesday 8 July 2026**.

## Running locally

Single self-contained `index.html` — React 18 via UMD CDN, no build step, no dependencies.

```bash
python -m http.server 8000
```

Then open http://localhost:8000.

**Nothing you type leaves the browser.** There is no backend, no storage, and no telemetry: the
incident facts, the loss figures, and the memo exist only in the page.

The page does make three outbound requests, none of which carry anything you entered — React from
a CDN, the webfonts, and a parameterless `GET` for the benchmarks file used by the optional
cross-check. If your incident response policy prohibits third-party requests from a machine
handling incident data, serve the page from your own host and the cross-check will report itself
unavailable while everything else works unchanged.

## Scope and limits

This is an analysis aid, **not legal advice**, and it does not substitute for review by securities
counsel. The 5% screen is a rule of thumb SAB No. 99 describes as a starting point and expressly
declines to bless as a safe harbour — every threshold here is configurable because none of them is
authoritative.

Item 1.05 is also not the whole obligation. Item 106 of Regulation S-K covers the annual description
of cybersecurity risk management, strategy, and governance, and a determination of "not material"
today does not settle what happens as the facts develop. Non-US regimes — NIS2, DORA, GDPR
Article 33 — run their own clocks on their own triggers, and they are not modelled here.

**The benchmark cross-check is corroboration for a class, not evidence about your incident.** A
shard describes a comparable population; a determination rests on the facts of the event in front
of you. It never changes the verdict — selecting one and clearing it again leaves the call and the
memo identical apart from the cited paragraph. The shards are governed starters rather than
benchmark-grade figures, several bridge a frequency from another country where no local rate is
published, and each carries its own statement of what it will not support. If you quote one in a
memo, quote its limitation in the same paragraph; the tool writes both.

## Attribution

Benchmark data comes from [risk-benchmarks](https://github.com/RootCawsLLC/risk-benchmarks), which
derives it from [RiskShard](https://github.com/raviaxo/RiskShard) by
[raviaxo](https://github.com/raviaxo), AGPL-3.0. Ranges, limitations and "not good for" statements
are RiskShard's, carried through unchanged; the underlying publications are cited, not reproduced.

## License

Copyright (c) 2026 RootCaws LLC.

[GNU AGPL v3 or later](LICENSE). If you modify this and run it as a network service, the AGPL requires you to offer your users the modified source under the same terms.
