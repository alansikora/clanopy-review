# clanopy-review

GitHub Action for clanopy PR reviews. This is a thin wrapper — all review logic lives in the [clanopy CLI](https://github.com/alansikora/clanopy).

## Project structure

```
action.yml                          # Composite GitHub Action definition
.github/workflows/review.yml       # Reusable workflow for consumer repos
.github/workflows/release-tag.yml  # Auto-updates v1 tag on push to main
README.md                           # Usage docs
```

## Architecture

- **Composite action** — shell steps only, no JavaScript/Docker
- **Reusable workflow** — consumers call this via `workflow_call` instead of inlining filter/review logic
- Downloads the clanopy CLI binary via `install.sh` from the main clanopy repo
- Installs Claude Code via npm
- Generates a GitHub App installation token via OIDC exchange with `token.clanopy.workers.dev`
- Runs `clanopy review`

## Rules

- Keep this repo minimal. Review logic belongs in the clanopy CLI, not here.
- The action interface (inputs/outputs) should change rarely. Bump the major version tag (`v1` → `v2`) only when inputs/outputs have breaking changes.
- This repo is versioned independently from the clanopy CLI. The `v1` tag here does not correspond to clanopy `v1`.
