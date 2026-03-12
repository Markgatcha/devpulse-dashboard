# GitHub Actions Workflows

This directory is reserved for CI/CD workflows.

## Planned Workflows

### `weekly-update.yml` (coming soon)
Automates the weekly Monday dashboard refresh:
- Triggers every Monday at 09:00 UTC
- Calls the DevPulse analysis script
- Commits updated README.md and archives report

### Status Badges
Once the workflow is active, add these to the README:

```markdown
![Weekly Update](https://github.com/Markgatcha/devpulse-dashboard/actions/workflows/weekly-update.yml/badge.svg)
```

---

*DevPulse Dashboard — [github.com/Markgatcha/devpulse-dashboard](https://github.com/Markgatcha/devpulse-dashboard)*
