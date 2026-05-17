# Data Topology — Where Things Live

Last updated: 18 May 2026

---

## Summary

| Layer | URL / Path | What's there | Who can see it |
|---|---|---|---|
| Local dashboard | http://127.0.0.1:8787/web/ | Full data including contracts, financing, logs, offer ceilings | You only (Mac LAN) |
| GitHub Pages | http://prakharsingh.com/fieldnotes-7x9q2/ | Curated public export — no contracts, no financing details, no logs | Public (anyone with URL) |
| GitHub repo | github.com/prkhr-singh/fieldnotes-7x9q2 | Source for GitHub Pages — same curated subset | Public |

---

## Local Dashboard

**URL:** http://127.0.0.1:8787/web/  
**Served by:** macOS LaunchAgent → `web/serve-dashboard.sh`  
**Logs:** `logs/`  
**Source files:** `/Users/singh.prakhar/vibe_analysis/home-purchase/`

Full unredacted dataset. Includes everything:
- `contracts/` — full Section 32, Contract of Sale PDFs
- `financing.md` — cash position, income, ESPP/RSU details, deposit mechanics
- `log.md` — append-only inspection and decision log
- `stocks/` — stock strategy, CGT calculations
- `action-items.md` — current open actions
- `property-comparisons/<slug>/contract-review.md` — full contract analysis per property
- `tracker/` — email tracker output

**Not accessible remotely** unless Tailscale is set up (see dashboard README).

---

## GitHub Pages (Public Export)

**URL:** http://prakharsingh.com/fieldnotes-7x9q2/  
**Repo:** github.com/prkhr-singh/fieldnotes-7x9q2  
**Branch:** main, served from root `/`  
**Custom domain:** prakharsingh.com (via DNS CNAME)

Curated public subset — intentionally excludes sensitive files. Safe to share the URL.

### What IS published

| File / folder | Notes |
|---|---|
| `web/` | Dashboard HTML, CSS, JS |
| `property-comparisons/comparison-matrix.csv` | Scores only — no contract details |
| `property-comparisons/<slug>/report.md` | AI analysis, pre-visit assessment |
| `property-comparisons/<slug>/in-person.md` | Visit notes, scores, lifestyle fit |
| `criteria.md` | Buyer criteria and scoring rubric |
| `suburbs.md` | Suburb research |
| `suburb-analytics.md` | Suburb data analysis |
| `market-insights.md` | Pricing rules, auction mechanics |
| `analyses/` | Land value and market analyses |
| `agenda-*.md` | Inspection day agendas |
| `agent-calls-*.md` | Agent call scripts (added 18 May 2026) |

### What is NOT published (excluded from repo)

| Excluded | Reason |
|---|---|
| `contracts/` | Full PDFs — sensitive legal documents |
| `property-comparisons/<slug>/contract-review.md` | Offer ceilings, legal analysis |
| `property-comparisons/<slug>/purchase-readiness.md` | Offer strategy, financial breakdown |
| `financing.md` | Income, cash position, ESPP/RSU details |
| `log.md` | Decision log with negotiation details |
| `action-items.md` | Open actions with offer specifics |
| `stocks/` | Stock strategy and CGT calculations |
| `tracker/` | Email tracker |
| `assessments/` | Internal assessments |
| `h-and-l.md`, `h-and-l-land-search.md` | H&L financial model details |

---

## How to Push an Update to GitHub Pages

The GitHub repo is a **separate clone** — it is not the same git repo as the local workspace (local workspace has no git remote).

**Steps to push an update:**

```bash
# 1. Clone (first time) or pull (subsequent times)
git clone https://github.com/prkhr-singh/fieldnotes-7x9q2.git /tmp/fieldnotes-clone
# OR if clone already exists:
cd /tmp/fieldnotes-clone && git pull

# 2. Copy updated files from local workspace
cp ~/vibe_analysis/home-purchase/web/index.html /tmp/fieldnotes-clone/web/index.html
cp ~/vibe_analysis/home-purchase/web/app.js /tmp/fieldnotes-clone/web/app.js
cp ~/vibe_analysis/home-purchase/web/styles.css /tmp/fieldnotes-clone/web/styles.css
# Copy any new markdown files that are safe to publish:
cp ~/vibe_analysis/home-purchase/agent-calls-18-may-2026.md /tmp/fieldnotes-clone/

# 3. Commit and push
cd /tmp/fieldnotes-clone
git config user.email "prkhr.singh@gmail.com"
git config user.name "Prakhar Singh"
git add -A
git commit -m "describe what changed"
git push
```

GitHub Pages rebuilds within ~30 seconds of each push.

---

## Decision Rules — What to Publish vs Keep Local

| Type of content | Publish? | Reason |
|---|---|---|
| Property scores and AI analysis | ✅ Yes | No personal data |
| In-person visit notes (lifestyle, layout) | ✅ Yes | No financial detail |
| Suburb research and market insights | ✅ Yes | Public information |
| Inspection agendas and call scripts | ✅ Yes | Operational, no offer numbers |
| Offer ceilings and negotiation tactics | ❌ No | Agents could read this |
| Contract analysis (contract-review.md) | ❌ No | Contains offer strategy |
| Purchase readiness files | ❌ No | Contains offer ceilings |
| Financial position (income, cash, stocks) | ❌ No | Personal financial data |
| Full contract PDFs | ❌ No | Vendor personal details |
| Log with negotiation notes | ❌ No | Competitive information |

---

## GitHub Repos at a Glance

| Repo | Purpose | Pages URL |
|---|---|---|
| prkhr-singh/fieldnotes-7x9q2 | **Home purchase dashboard public export** | prakharsingh.com/fieldnotes-7x9q2/ |
| prkhr-singh/prkhr-singh.github.io | Personal website (Jekyll) | prakharsingh.com |
| prkhr-singh/gstack | Fork — Claude Code tooling | N/A |
