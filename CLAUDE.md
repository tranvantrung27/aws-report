# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Hugo static site for Trần Văn Trung's AWS Workforce Bootcamp internship report (April 20 – July 13, 2026).
Live site: https://tranvantrung27.github.io/aws-report/

## Common Commands

```bash
# Serve locally with live reload
hugo server

# Serve locally (include draft content)
hugo server -D

# Build for production
hugo --minify

# Build and specify environment
hugo --minify --environment production
```

## Architecture

**Stack:** Hugo static site generator + `hugo-theme-learn` (git submodule at `themes/hugo-theme-learn/`)

**Bilingual content:** Every page has two files — `_index.md` (English) and `_index.vi.md` (Vietnamese). Language is set in `config.toml` with `defaultContentLanguage = "en"`.

**Content sections** (under `content/`):
- `1-Worklog/` — 12 weekly reports (1.1-Week1 … 1.12-Week12)
- `2-Proposal/` — IoT Weather Platform project proposal (AWS CDK, IoT Core, MQTT)
- `3-BlogsTranslated/` — Translated AWS/technical posts
- `4-EventParticipated/` — Events attended
- `5-Workshop/` — Workshop notes and steps
- `6-Self-evaluation/` — Self-assessment
- `7-Feedback/` — Feedback/sharing section

**Customizations** (override theme defaults):
- `layouts/partials/` — `logo.html`, `custom-footer.html`, `menu-footer.html`
- `layouts/shortcodes/` — `tabs.html`, `tab.html`, `ghcontributors.html`
- `static/images/` — all image assets (organized by section)

## Configuration

Single config file: `config.toml`. Key settings:
- `baseURL` = `https://tranvantrung27.github.io/aws-report/`
- Theme variant: `themeVariant = "workshop"`
- Markup: `unsafe = true` (allows raw HTML in markdown)
- Outputs include `JSON` for Hugo search functionality

## Deployment

GitHub Actions (`.github/workflows/hugo.yml`) builds with Hugo 0.134.3 extended and deploys to the `gh-pages` branch on every push to `main`. The `force_orphan` option is set, so only the latest build is kept on that branch.

## Content Authoring Notes

- Use `weight` front matter to control page order in the sidebar.
- Use the `tabs` / `tab` shortcodes for multi-tab code examples.
- Images go in `static/images/<section>/`; reference them as `/aws-report/images/<section>/filename.ext`.
- Draft pages: set `draft: true` in front matter; they are excluded from `hugo --minify` builds.
