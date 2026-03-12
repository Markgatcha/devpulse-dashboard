# 🔬 DevPulse Analysis Methodology

This document describes how the DevPulse Dashboard collects, processes, and scores repository health data each week.

---

## Data Sources

| Source | What We Pull |
|--------|-------------|
| **GitHub REST API** | Stars, open issues, open PRs, last push timestamp, primary language |
| **GitHub Trending** | Weekly trending repos per language (Python, JS, Rust) |
| **Web Search** | Community buzz, new project announcements, ecosystem context |

---

## Repository Selection

Each week, the top 20 repositories are selected from:
1. GitHub's trending page for **Python**, **JavaScript**, and **Rust** (≈7 repos each)
2. Previous week's repos that remain relevant (continuity tracking)
3. Notable new entries with >100 new stars/day

Repos must be **open-source** (OSI-approved license) and **actively used** (not abandoned forks).

---

## Health Score Formula

The Health Score is a 0–100 composite metric:

```
Health Score = Commit Recency + Issue Response Rate + PR Merge Velocity + Star Momentum
```

### Commit Recency (max 30 pts)
Measures how recently the repository received a code push.

| Last Commit | Points |
|-------------|--------|
| Today (0d)  | 30     |
| Within 7d   | 20     |
| Within 30d  | 10     |
| Older       | 0      |

### Issue Response Rate (max 25 pts)
Open issues signal backlog and maintainer responsiveness.

| Open Issues | Points |
|-------------|--------|
| < 10        | 25     |
| 10 – 49     | 15     |
| 50 – 99     | 5      |
| 100+        | 0      |

### PR Merge Velocity (max 25 pts)
How quickly PRs get reviewed and merged on average.

| Avg Merge Time | Points |
|----------------|--------|
| < 7 days       | 25     |
| 7 – 29 days    | 15     |
| 30 – 59 days   | 5      |
| 60+ days       | 0      |

### Star Momentum (max 20 pts)
New stars gained in the past 7 days, indicating community excitement.

| New Stars/Week | Points |
|----------------|--------|
| > 500          | 20     |
| 101 – 500      | 15     |
| 11 – 100       | 10     |
| ≤ 10           | 5      |

---

## Score Interpretation

| Range  | Label   | Meaning |
|--------|---------|---------|
| 80–100 | ✅ Excellent | Highly active, low issues, fast PRs, growing |
| 60–79  | ✅ Healthy | Good activity with minor concerns |
| 40–59  | ⚠️ Fair  | Some signals are weak — monitor closely |
| 20–39  | ⚠️ At Risk | Multiple red flags — may be stagnating |
| 0–19   | 🚨 Critical | Likely abandoned or severely backlogged |

---

## Weekly Update Process

Every Monday, the automated agent:

1. Searches web + GitHub trending for top repos in Python, JS, Rust
2. Pulls live stats via `gh api repos/{owner}/{repo}`
3. Calculates Health Scores using the formula above
4. Archives the current README to `/reports/YYYY-MM-DD.md`
5. Rewrites `README.md` with fresh data and timestamp
6. Commits with message: `📊 Weekly dashboard update - [date]`
7. Checks for any repo that dropped >20 health points — adds a ⚠️ Notable Drops section if found

---

## Limitations

- **Open PRs count** is approximate (GitHub API returns up to 1 when pagination is used; full counts require pagination)
- **Weekly star gains** are estimated from GitHub's trending page signals and community data; exact figures require GitHub Archive or third-party APIs
- **PR merge velocity** is estimated from public merge history sampling — not computed from every historical PR
- Repos with private issue trackers (common for large orgs) will show lower issue counts than reality

---

*Methodology maintained by [DevPulse](https://github.com/Markgatcha/devpulse-dashboard) | Powered by Perplexity Computer + Claude Sonnet 4.6*
