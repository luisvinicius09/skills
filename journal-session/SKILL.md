---
name: journal-session
description: Write a structured session journal to docs/sessions/ capturing what changed, why, and problems encountered. Use when user wants to log or journal the current session, save session context, or invokes /journal-session.
---

# Session Journal

Write a session summary to `docs/sessions/` in the current project.

## Steps

1. **Gather context** by running these in parallel:
   - `git diff HEAD~$(git rev-list --count HEAD --since="today.midnight")..HEAD --stat` to see what files changed this session
   - `git log --since="today.midnight" --oneline` to see commit messages
   - Review your conversation history for decisions, reasoning, and problems

2. **Ask the user** for a short description (2-4 words) to use in the filename. Example: "auth-refactor", "fix-payment-bug".

3. **Create the file** at `docs/sessions/{YYYY-MM-DD}_{description}.md` using this template:

```markdown
# Session: {description}

**Date**: {YYYY-MM-DD}

## Summary

{2-3 sentence high-level summary of what was accomplished}

## Changes

{Bulleted list of what changed — files, features, fixes. Keep it concise, not a full diff}

## Decisions & Reasoning

{Key decisions made during the session and why. This is the most valuable section for future context}

## Problems Encountered

{Issues hit during the session and how they were resolved. Skip if none}

## Context for Next Session

{Anything the next session should know — loose ends, next steps, things to watch out for}
```

4. **Create** `docs/sessions/` directory if it doesn't exist.

5. **Show the user** the file path when done.

## Rules

- Keep the journal concise — it's a reference, not a transcript
- Focus on the *why* over the *what* — code diffs already show the what
- Only include problems that would be useful context for future work
- Don't fabricate details — if you're unsure about something from the session, ask
