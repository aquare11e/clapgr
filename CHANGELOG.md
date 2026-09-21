# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [0.2.2] - 2026-09-21

### Added

- `--skip` upgrades and stops there: no GitHub release-note request, no Claude summary
- `-b` / `--background` re-execs clapgr detached from the terminal; the parent prints the log path and returns immediately, and all output goes to the log file. Other arguments are forwarded, and argument validation still happens in the foreground

## [0.2.1] - 2026-09-11

### Fixed

- Release-note fetching crashed with `jq: error ... Cannot index string with string "tag_name"` when the GitHub API answered with an error object instead of an array (most often the 60 req/hour unauthenticated rate limit). The response is now validated (HTTP status + JSON shape) and the API message is reported instead

### Added

- `GITHUB_TOKEN` / `GH_TOKEN` is sent to the GitHub API when set, raising the rate limit from 60 to 5000 requests/hour
- Rate-limit responses print a hint on how to set a token
- 20s timeout on the GitHub request; network failures degrade to "release notes unavailable" instead of an opaque error

## [0.2.0] - 2026-06-03

### Added

- Release channel switching via Homebrew casks: `--latest` (claude-code@latest), `--stable` (claude-code), and `--channel <name>`
- Channel switch swaps casks when needed (uninstall the other, install the target), then upgrades
- Cask-aware version detection and upgrades — default mode now upgrades whichever `claude-code` cask is installed
- `--nightly` errors with guidance to use npm (no Homebrew nightly cask exists)

## [0.1.0] - 2026-04-23

### Added

- Upgrade claude-code via Homebrew with version change detection
- Fetch release notes from GitHub API for all versions between old and new
- AI-powered release note summaries via Claude CLI (optional)
- Colored terminal output
- Persistent logging to `~/.claude-upgrade-logs/`
- `--version` and `--help` flags