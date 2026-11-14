# Trust Fall

**Which fix types from an ops agent have earned a skip-the-review-queue lane?** A Bayesian Beta-Binomial Monte Carlo decision prototype.

- **Live demo:** https://shriyashekatkar25-byte.github.io/trust-fall/
- **Gallery:** [APM Lab](https://shriyashekatkar25-byte.github.io/apm-lab/)
- By Shriya Shekatkar · UCLA · Stats 102C Monte Carlo Methods

## The launch that inspired it

On **November 12, 2026**, Databricks moved **Genie ZeroOps** to Beta ([release notes, Nov 2026](https://docs.databricks.com/aws/ja/release-notes/product/2026/november)). Per the [ZeroOps docs](https://docs.databricks.com/aws/en/zeroops/), it is a background agent that monitors jobs, pipelines and tables, finds failures and stale data, diagnoses the root cause and proposes a fix. It doesn't change production data or code without human review, and it can push a fix to a Git branch only after you ask and confirm. The [launch blog post](https://www.databricks.com/blog/introducing-genie-zeroops) (June 2026) describes testing fixes in an isolated sandbox before anything is applied.

## The PM decision

Every proposed fix waits for a human today, and at 2am the wait *is* the outage. The hypothetical question: if we offered an **opt-in, admin-controlled “earned lane”** where a fix category can sandbox-test and merge its own PR with one-click revert, **which categories (if any) have the evidence to qualify?**

- **Gate:** a category graduates when P(true clean-fix rate ≥ bar | review history) ≥ confidence.
- **Success metric:** median time-to-recovery for broken jobs/pipelines, −40% vs a review-only control in 8 weeks.
- **Guardrails:** bad auto-merged fixes ≤ 2%; p90 time-to-revert ≤ 15 min; zero fixes that rewrite production data rows; < 10% of admins turn the lane off within 30 days.
- **Stakeholders:** platform/data engineering leads, workspace admins and security, ops-agent engineering and applied AI, governance PM, support and field engineering.

With the default settings (97% bar, 90% confidence, 60-fix prior), only *retries after upstream blips* graduate; schema-drift columns sit just under the line (88%), which is exactly the kind of near-miss a shadow-mode test should settle.

## What the prototype shows about product judgment

| Databricks APM skill | Where it shows up |
|---|---|
| Customer problem framing | The 2am review queue, framed as recovery time rather than “more automation” |
| Prototyping | Interactive simulation with seeded RNG, canvas animation, live KPIs |
| SQL / Python | Track-record view in SQL plus a posterior gate job in Python |
| Data-driven decisions | Graduation is a posterior probability, with break-even economics and demotion rules |
| Clear writing | A director memo and a one-sentence VP summary that rewrite themselves from the simulation |

## Methods

| Method (Stats 102C) | Used for |
|---|---|
| Ch2 Bayesian: Beta prior + Binomial data → Beta posterior | Each category's clean-fix rate, with prior shrinkage toward the pooled rate |
| Ch2 posterior mean as weighted average; flat prior → MAP = MLE | Prior-strength slider, small-sample protection |
| Ch2 quantile credible intervals | 90% credible interval per category |
| Ch1 Monte Carlo estimation (sample means of indicators) | P(rate ≥ bar), net hours saved, chance the lane loses hours |
| Ch5 composition (Box-Muller normals → Gamma → Beta) | Drawing posterior samples in the browser |

All numbers are synthetic.

## Design baseline

Four pins from Shriya's Pinterest home feed, each redrawn in SVG/CSS/canvas (no pin images used):

- **Hero:** [“the sun will always rise again” (aanvi)](https://www.pinterest.com/pin/654710864623796967/), for the wavy radial gold filaments, #FFD45C → #B57406 glow and soft lowercase sans headline.
- **Cards & buttons:** [“treasure” crow sketch (fel)](https://www.pinterest.com/pin/579134833373374069/), for the gray paper, wobbly double ink borders and hand-lettered thought-bubble labels.
- **KPI badges:** [“the E in my name…” pink poster (awandemadlalaa)](https://www.pinterest.com/pin/1072560467532173353/), for the hot pink blocks with butter-yellow ultra-condensed caps and a star.
- **Simulation room:** [“All negative energy sent in my direction…” green lettering (tarotreaderpeter)](https://www.pinterest.com/pin/790944753356020726/), for the neon green marker caps with black outline on white.

---

Unofficial concept prototype by Shriya Shekatkar. All data is simulated. Not affiliated with any company.
