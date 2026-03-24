# Clanopy PR Review Action

GitHub Action for AI-powered PR reviews using [Claude Code](https://docs.anthropic.com/en/docs/claude-code) and [clanopy](https://github.com/alansikora/clanopy).

## Quick setup

The easiest way to set up is with the clanopy CLI:

```bash
clanopy review init
```

This installs the required GitHub App, configures authentication, and creates a PR with the workflow.

## Manual setup

1. Install the [Clanopy Review](https://github.com/apps/clanopy-review) GitHub App on your repo
2. Add a workflow:

```yaml
# .github/workflows/clanopy-review.yml
name: Clanopy Review
on:
  pull_request_target:
    types: [opened, reopened, synchronize]
  pull_request_review_comment:
    types: [created]

permissions:
  contents: read
  id-token: write
  pull-requests: write

jobs:
  pr:
    uses: alansikora/clanopy-review/.github/workflows/review.yml@v1
    with:
      config_path: .clanopy/review.yml
    secrets:
      claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
```

The `id-token: write` permission is required for OIDC-based token generation with the Clanopy Review app. `pull_request_target` is used instead of `pull_request` to support fork PRs.

## Inputs

| Input | Description | Default |
|---|---|---|
| `claude_code_oauth_token` | Claude access token (preferred) | |
| `anthropic_api_key` | Anthropic API key (alternative) | |
| `config_path` | Path to review config | `.clanopy/review.yml` |
| `post_comment` | Post findings as PR comment | `true` |
| `clanopy_version` | CLI version to install (`v0.3.0`, `canary`, etc.) | `latest` |
| `reply_only` | Evaluate thread replies only, skip new findings | `false` |

## How it works

1. Installs the [clanopy](https://github.com/alansikora/clanopy) CLI
2. Installs Claude Code
3. Obtains a GitHub token via OIDC exchange with the Clanopy Review app
4. Runs `clanopy review` against the pull request

Review rules are configured in `.clanopy/review.yml` — see the [clanopy docs](https://github.com/alansikora/clanopy#review-rules) for details.

## License

MIT
