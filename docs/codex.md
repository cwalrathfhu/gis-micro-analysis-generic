## REQUIRED: First Steps for Every Session

Before doing anything else, you MUST:

1. **Read `ARCHITECTURE.md`** — the authoritative reference for app structure, components, state, functions, charts, and UI layout. (NOTE: This does not exist yet. We will create it later)
2. **Read `CHANGELOG.md`** — session-by-session history of all changes. Review to understand what has already been built, recently changed, or intentionally removed. (NOTE: this does not exist yet. We will create it later)
3. **Notify the user** which branch you are on and that it includes all prior work (see Branch Management below).

D

## Developer Context

**User Experience Level**: Beginner/non-coder
- Limited experience with Git, GitHub, and project development
- Primarily using web Codex interface and committing to GitHub manually, not terminal-based development
- May use VS Code primarily for running `npm run dev` and pulling from Git but needs instructions on doing so if asked
- Requires clear, step-by-step instructions with explicit file paths

---

## Communication Guidelines

- Use plain language, avoid jargon where possible
- Always specify full file paths (e.g., `src/App.jsx` not "the main file")
- Explain *where* code changes are happening before making them
- Verify branch state before implementing features
- Show git commands explicitly: `git status`, `git pull`, `git checkout branch-name`
- Explain deployment implications (what happens when code is pushed)
- Confirm which branch should be used as base before starting work
- Use specific line numbers when referencing code locations

## Common Issues to Prevent

- User misunderstanding how branches work
- Features reverting due to unclear git state
- Changes made to wrong files
- User confusion about what version is "live"

- User not knowing a new branch was created or how to pull or deploy it
