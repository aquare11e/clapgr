# clapgr

Upgrade [claude-code](https://github.com/anthropics/claude-code) via Homebrew with automatic release note fetching and AI-powered summaries.

## Why?

claude-code updates **a lot**. Staying current means running brew upgrade, checking what version you got, finding the release notes, and reading through them — every time. That's a lot of manual steps for something that should be one command.

**clapgr** automates all of it. One command: upgrade, fetch release notes for every version in between, and optionally summarize them using Claude itself. Full context, zero friction.

## Installation

### Homebrew (recommended)

```bash
brew tap aquare11e/clapgr
brew install clapgr
```

### Manual

```bash
curl -sL https://raw.githubusercontent.com/aquare11e/clapgr/main/clapgr -o /usr/local/bin/clapgr
chmod +x /usr/local/bin/clapgr
```

## Usage

```bash
clapgr
```

That's it. clapgr will:

1. Check whether Homebrew knows of a newer claude-code
2. Run `brew upgrade claude-code` — and, if an update was detected, fetch release notes from GitHub at the same time
3. Detect if the version changed and pick out the notes for all versions between old and new
4. Summarize release notes using `claude` CLI (if available)
5. Log everything to `~/.claude-upgrade-logs/`

### Options

```
-h, --help        Show help message
-v, --version     Show version
--stable          Switch to the stable channel (Homebrew cask: claude-code)
--latest          Switch to the latest channel (Homebrew cask: claude-code@latest)
--channel <name>  Switch to a channel: stable | latest
--skip            Upgrade only: no release notes, no summary
-b, --background  Detach, write everything to the log file, print its path
```

### Upgrade only

`--skip` stops right after the upgrade — no GitHub request, no Claude summary. Useful when you only want the new version, or when the API rate limit is spent:

```bash
clapgr --skip
```

### Background

`-b` / `--background` re-runs clapgr detached from the terminal. The prompt comes back immediately, and everything that would have been printed goes to the log file:

```bash
$ clapgr --background
clapgr running in background (pid 41288)
Log: /Users/you/.claude-upgrade-logs/20260921_101134.log
```

Arguments are forwarded, so `clapgr -b --latest` or `clapgr -b --skip` work as expected. Bad arguments still fail in the foreground, before anything is detached.

### Channels

With no channel option, clapgr upgrades whichever `claude-code` cask is currently installed.

A channel option swaps the Homebrew cask if you're not already on it (uninstalls the other, installs the target), then upgrades:

```bash
clapgr --latest   # switch to the latest (rolling) channel
clapgr --stable   # switch back to the stable channel
```

> **Nightly** is not available via Homebrew. Install it with npm instead:
> ```bash
> npm install -g @anthropic-ai/claude-code@nightly
> ```

### GitHub API rate limit

Release notes come from the public GitHub API, which allows **60 requests/hour per IP** unauthenticated. When that runs out, clapgr reports the limit and skips the notes (the upgrade itself still happens). Raise the limit to 5000/hour by exporting a token:

```bash
export GITHUB_TOKEN=$(gh auth token)   # or any personal access token
```

`GH_TOKEN` works too.

## Dependencies

- **Required**: bash, curl, jq, Homebrew
- **Optional**: [claude CLI](https://github.com/anthropics/claude-code) (for AI-powered summaries)

Install jq if you don't have it:

```bash
brew install jq
```

## Example output

```
Current version: 1.0.18
Running brew upgrade claude-code...
claude-code 1.0.18 -> 1.0.20
Fetching release notes...
Summarizing release notes with Claude...

═══ Summary: claude-code 1.0.18 → 1.0.20 ═══
• ...
```

## License

[MIT](LICENSE)