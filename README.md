# 🏁 Pit Wall — Flaky CI Detective

**Created by:** Ishu & Prateek

> Flaky tests quietly burn developer time and CI money. Pit Wall catches them red-handed.

---

## The Problem

Flaky tests — tests that fail, then pass on re-run with **no code change** — are one of the most underrated sources of waste in modern software teams:

- Developers re-run pipelines "just to be sure," burning compute minutes.
- Real bugs get ignored because "oh, that test is just flaky."
- Nobody has a number to point to, so nobody prioritizes fixing it.

Most teams *feel* like their CI is flaky. Almost none of them can say **how flaky, which jobs, or how much it's costing them.** Pit Wall makes that visible.

---

## What Pit Wall Does

Give Pit Wall a GitHub repository, and it:

1. **Pulls recent GitHub Actions run history** for that repo.
2. **Finds jobs that both failed and passed on the exact same commit** — this is direct, undeniable evidence of flakiness (no code changed, yet the outcome flipped).
3. **Quantifies the damage** in wasted CI compute time.
4. **Diagnoses the worst offenders** using AI log analysis.
5. **Hands you a fix** — in the form of a ready-to-post GitHub issue.

In short: Pit Wall turns "our tests feel flaky" into a dashboard, a dollar/hour figure, and an actionable ticket.

---

## Features

### 🔢 Headline Number: Wasted CI Hours This Month
The single most important metric on the dashboard — total compute time spent re-running jobs that later turned out to be flaky (not genuine failures). Designed to be the number you show your engineering manager.

### 🏆 Flakiness Leaderboard
Ranks jobs/tests by flake frequency, with a trend chart showing whether flakiness is getting better or worse over time. Instantly spots the repeat offenders.

### 🕵️ AI Root-Cause Analysis
For the worst offending job, Pit Wall reads the actual failure logs and classifies the likely cause:
- **Timing / race conditions**
- **Network flakiness** (timeouts, DNS, rate limits)
- **Test ordering / shared state issues**
- Other (with reasoning)

### 📝 Suggested Fix + Ready-to-Post GitHub Issue
No more staring at a red build wondering what to do. Pit Wall drafts a GitHub issue with:
- The suspected root cause
- Evidence (linked failed/passed run pairs on the same commit)
- A suggested remediation approach (e.g., add retry logic, isolate test state, mock the flaky network call)

---

## How It Works (Under the Hood)

```
GitHub Actions API
       │
       ▼
Fetch recent workflow runs (per commit SHA)
       │
       ▼
Group runs by commit → detect commits with both PASS and FAIL outcomes
       │
       ▼
Flag job as "flaky" + compute wasted compute-minutes
       │
       ▼
Rank jobs → Leaderboard + Trend Chart
       │
       ▼
Pull failure logs for #1 flakiest job → feed to LLM
       │
       ▼
LLM classifies cause → drafts fix + GitHub issue
```

**Core signal:** same commit SHA + differing job outcome (pass vs. fail) across runs = flaky, not a real regression.

---

## Demo Strategy

The headline number is the whole pitch — it needs to land. Before demoing:
- Scan a handful of real, active open-source repos ahead of time.
- Pick one with **visible, non-trivial flakiness** (ideally a project with heavy CI usage — large test suites, integration/E2E tests, or network-dependent tests tend to flake more).
- Good candidates to check first (based on typical characteristics — verify current flakiness before demo day):
  - Large monorepos with extensive E2E/integration suites
  - Projects with browser-based test automation (Selenium/Playwright/Cypress)
  - Projects with heavy external API/network dependency in tests
  - Popular repos with high PR/CI volume (more data = more visible patterns)

> 💡 Tip: The best demo repo is one where the leaderboard chart has an obvious spike or a clear repeat offender — makes for a much stronger story than a flat, boring graph.

---

## Suggested Repo Names for Pit Wall Itself

| Name | Vibe |
|---|---|
| **pit-wall** | Racing pit crew watching telemetry — matches the "detective at the wall" theme |
| **flake-finder** | Direct, descriptive, easy to search |
| **ci-detective** | Clear positioning as an investigator |
| **retest-radar** | Emphasizes catching re-run patterns |
| **red-flag-ci** | Plays on red/failed builds |
| **flakeout** | Short, punchy, memorable |
| **same-commit-different-day** | Cheeky, describes the core signal literally |
| **ci-forensics** | Sounds serious/technical, good for enterprise pitch |
| **checkered-flag** | Racing theme continuation of "Pit Wall" |
| **flake-tax** | Frames flakiness as a cost — ties directly to the headline metric |

**Top pick:** `pit-wall` — it's short, on-theme, and doubles as a nice tagline ("keep an eye on the pit wall").

---

## Tech Stack (Suggested)

- **Data source:** GitHub Actions REST/GraphQL API (workflow runs, jobs, logs)
- **Backend:** Python or Node.js — scheduled job to pull & diff run data
- **Storage:** Lightweight DB (SQLite/Postgres) to track historical run data for trend charts
- **AI Layer:** LLM call over failure logs for root-cause classification + issue drafting
- **Frontend:** Dashboard (headline number, leaderboard, trend chart) — React or a simple HTML/JS dashboard for MVP
- **Output:** GitHub Issues API integration to post the drafted fix directly

---

## Roadmap Ideas (Post-MVP)

- [ ] Slack/Discord alert when a new flaky job is detected
- [ ] Auto-tag PRs blocked by known-flaky tests
- [ ] Cost estimation using actual GitHub Actions runner pricing
- [ ] Multi-repo / org-wide dashboard
- [ ] Historical flake-rate regression detection (is a test getting *more* flaky over time?)
- [ ] One-click "quarantine this test" PR generator

---

## Authors

Built by **Ishu** and **Prateek**.
