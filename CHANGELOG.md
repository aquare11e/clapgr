# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [0.1.0] - 2026-04-23

### Added

- Upgrade claude-code via Homebrew with version change detection
- Fetch release notes from GitHub API for all versions between old and new
- AI-powered release note summaries via Claude CLI (optional)
- Colored terminal output
- Persistent logging to `~/.claude-upgrade-logs/`
- `--version` and `--help` flags