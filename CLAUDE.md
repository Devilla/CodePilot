# CLAUDE.md

## Project Overview

CodePilot — A native desktop GUI client for Claude Code, built with Electron + Next.js.

## Release Checklist

**Before each release, update the version number:**

1. The `"version"` field in `package.json`
2. The corresponding version in `package-lock.json` (running `npm install` will sync it automatically)
3. Build commands: `npm run electron:pack:mac` (macOS) / `npm run electron:pack:win` (Windows)
4. Upload artifacts to GitHub Release and write release notes (format below under Release Notes)

## Development Rules

**Thorough testing before committing:**
- Before every commit, fully test all changed functionality in the development environment and confirm no regressions
- For frontend UI changes, actually start the app to verify (`npm run dev` or `npm run electron:dev`)
- For build/packaging changes, run a full pack flow once to verify artifacts work
- For cross-platform changes, consider differences between platforms

**Thorough research before adding features:**
- Before adding a feature, research the approach, API compatibility, and community best practices
- For Electron APIs, confirm support in the target version
- For third-party libraries, confirm compatibility with existing dependencies
- For Claude Code SDK, confirm what the SDK actually supports and how to call it
- For uncertain technical points, validate with a POC first; do not trial directly in main code

## Release Notes

Every GitHub Release must include the following:

**Title format:** `CodePilot v{version}`

**Body structure:**

```markdown
## New Features / Bug Fixes (choose heading based on content)

- **Feature/Fix title** — Brief description of the change and rationale

## Downloads

- **CodePilot-{version}-arm64.dmg** — macOS Apple Silicon (M1/M2/M3/M4)
- **CodePilot-{version}-x64.dmg** — macOS Intel

## Installation

1. Download the DMG for your chip architecture
2. Open the DMG and drag CodePilot into the Applications folder
3. On first open, if you see a security prompt, go to **System Settings → Privacy & Security** and click "Open Anyway"
4. Configure your Anthropic API Key or environment variables on the Settings page

## Requirements

- macOS 12.0+
- Anthropic API Key or `ANTHROPIC_API_KEY` environment variable configured
- For code-related features, Claude Code CLI is recommended

## Changelog (since v{previous version})

| Commit | Description |
|--------|-------------|
| `{hash}` | {commit message} |
```

**Notes:**
- Major releases (feature updates): use `## New Features` and `## Bug Fixes` sections
- Minor releases (fixes only): `## Bug Fix` is enough
- Always include Downloads, Installation, and Requirements for new users
- Changelog table lists all commits since the previous version

## Build Notes

- macOS build produces DMG (arm64 + x64); Windows produces NSIS installer or zip
- `scripts/after-pack.js` explicitly recompiles better-sqlite3 for the Electron ABI during pack, ensuring native module compatibility
- Clean before building with `rm -rf release/ .next/` to avoid stale artifacts
- After building the Windows package, run `npm rebuild better-sqlite3` to restore the local dev environment
- Cross-compiling Windows from macOS requires Wine (may not work on Apple Silicon); zip can be used instead of NSIS
