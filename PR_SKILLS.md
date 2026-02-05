# PR: vercel-labs/skills

## Title
fix: prevent hangs on invalid git repos and refine agent detection

## Body
Fixes #58
Fixes #186

### Changes

#### 1. Prevent hangs on invalid/private Git repos (#58)
Modified `src/git.ts` to set `GIT_TERMINAL_PROMPT: '0'`. This prevents `git clone` from hanging indefinitely by waiting for credential prompts when a user provides a typo in the repo name or a private repo they don't have access to. It now fails quickly with a descriptive error.

#### 2. Refine agent detection logic (#186)
Narrowed down the `detectInstalled` logic for several agents (`antigravity`, `github-copilot`, `opencode`, `replit`) that were using overly broad project-level directory checks (like `.github` or `.agent`). This reduces false positives where the CLI would detect 20+ agents not actually installed on the user's system.

### Verification
Verified with a test script for the git hang and by running `npx skills list` / `pnpm test`.

## Human-like Comment for @qual1337
Hey @qual1337, 

I've put together a few fixes for common CLI annoyances:
1. Fixed the hang on `add` when there's a typo in the repo name by disabling git terminal prompts.
2. Refined the agent detection logic to address #186. Some of the checks were a bit too broad (e.g., checking for `.github` in any project was triggering Copilot detection for everyone). 

Tested these locally and they seem to make the experience much smoother. Let me know if you need any adjustments!
