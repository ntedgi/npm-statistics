# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-script Node.js project that fetches total NPM download counts for the author's packages and updates `README.md` and `stats.json` with the results. It runs automatically on a daily cron schedule via GitHub Actions.

## Running

```bash
npm install
npm start        # node index.js — fetches stats and rewrites README.md
```

There are no tests and no lint step.

## How it works

`index.js` calls `npmtotal(key)` where `key` is the `npm-stats` field in `package.json` (set to `"naort"` — the npm username). It excludes a hardcoded list of packages that don't belong to the author. The results are sorted by download count, formatted as a Markdown table, and injected into `README.md` between the `<!-- AUTO-GENERATED-CONTENT:START (PACKAGES) -->` / `<!-- AUTO-GENERATED-CONTENT:END -->` comment markers using `markdown-magic`. It also writes the total download count into `stats.json` (used by the README badge).

## Automation

`.github/workflows/sync.yml` triggers daily at 00:01 UTC and on `workflow_dispatch`. It runs `npm start`, commits any changes to `README.md` and `stats.json`, and pushes back to the repo.

## Modifying the tracked packages

- To **exclude** additional packages: add their names to the `exclude` array in `index.js`.
- To **change the npm username**: update the `npm-stats` field in `package.json`.
