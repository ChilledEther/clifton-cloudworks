# Agent Developer Notes: Clifton Cloudworks

This file contains important project quirks, constraints, and instructions for any AI agents working on this repository in the future.

## 1. Ruby Environment & Cloudflare Builds
**Local Environment:** macOS running an outdated system Ruby (usually `v2.6.10`).
**Production Environment:** Cloudflare Pages running a modern Ruby (e.g., `v3.4.4+`).

### 🔴 Critical Rule: Do Not Downgrade Gems for Local Fixes
You will likely encounter `Gem::LoadError` or native extension compilation failures (like `ffi` or `jekyll-sass-converter`) when attempting to run `bundle install` or `bundle exec jekyll serve` locally. 

**DO NOT** add legacy version constraints to the `Gemfile` (such as `gem "ffi", "< 1.17.0"`) to "fix" local build errors.

Downgrading gems locally actively breaks the Cloudflare production deployment, which relies on modern global specs that have already been activated (e.g., `ffi 1.17.2`). 

### ✅ The Solution
- **If you must preview locally:** Use isolated static HTML previews or ensure you use `bundle install --path vendor/bundle` carefully without committing `Gemfile` downgrades.
- **Commit Policy:** Never commit down-level gem dependencies to the `feature` or `main` branches.

## 2. Staging Branch & Deployments
**Always target the `staging` branch for updates.** 
Whether you are creating a new pull request to merge changes or directly editing files, your work should always go into `staging`. Cloudflare is configured to fully automate deployments from this branch, so all automated and agentic work must funnel through it.
