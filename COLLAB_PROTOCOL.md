# COLLAB_PROTOCOL.md - How We Work Together

**Last Updated:** 2026-02-20
**Team:** Atlas (COO) + Tyler (Lead Engineer) + Codex (Reviewer)

---

## Culture: Productive, Not Just Busy

**Principle:** Don't do things for the sake of looking busy. Every action should:
- Fix something broken
- Improve something working
- Build something needed
- Learn something useful

**If there's nothing valuable to do:** Say so. It's better to be honest than to create busywork.

**Before starting a task, ask:**
1. Does this fix a real problem?
2. Does this move the business forward?
3. Is this the highest-impact thing I could be doing right now?

If the answer is no → find something better or rest.

---

## Code Change Workflow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌──────────┐
│   Tyler     │ ──► │   Codex     │ ──► │   Atlas     │ ──► │  MERGED  │
│ (implement) │     │  (review)   │     │ (approve)   │     │          │
└─────────────┘     └─────────────┘     └─────────────┘     └──────────┘
```

### Step 1: Tyler Implements
- Receives task from Bossio or Atlas
- Writes code fix/feature
- Runs `npm run build` / `npx tsc --noEmit` to verify
- Pushes to feature branch
- Posts branch name in #ai_workspace

### Step 2: Codex Reviews
- Tyler runs Codex on the branch:
  ```bash
  codex "Review the changes in branch fix/xxx for bugs, edge cases, and improvements"
  ```
- Codex provides feedback
- Tyler addresses any issues
- Re-runs Codex if significant changes made

### Step 3: Atlas Approves
- Reviews PR diff (code + Codex feedback)
- Checks business context (does this align with PRD/goals?)
- Approves and merges to main
- Vercel auto-deploys

---

## Roles

| Role | Owner | Responsibilities |
|------|-------|------------------|
| **Implement** | Tyler | Write code, fix bugs, build features |
| **Review** | Codex | Technical cross-check, catch edge cases |
| **Approve** | Atlas | Final review, business context, merge |
| **Direct** | Bossio | Prioritize, decide, steer |

---

## Communication

- **Channel:** Discord #ai_workspace
- **Handoffs:** Update `HANDOFF.md` before/after work
- **Tagging:** 
  - Atlas: `<@1470102290152161570>`
  - Tyler: `<@1474231358040572149>`

---

## Branch Naming

```
fix/short-description     # Bug fixes
feat/short-description    # New features
chore/short-description   # Cleanup, refactoring
security/short-description # Security fixes
```

---

## PR Requirements

Before requesting review:
- [ ] TypeScript compiles (`npx tsc --noEmit`)
- [ ] Build passes (`npm run build`)
- [ ] No console.log in production code
- [ ] Codex review completed
- [ ] Branch pushed to GitHub

---

## Quick Commands

**Tyler - After implementing:**
```bash
# Verify build
npm run build

# Push branch
git push origin fix/my-fix

# Request Codex review
codex "Review the diff between main and fix/my-fix for bugs and improvements"
```

**Atlas - To merge:**
```bash
# Review diff
gh pr diff <number>

# Merge
gh pr merge <number> --squash
```

---

## When to Skip Codex Review

- Typo fixes
- Documentation updates
- Config changes (non-logic)
- Emergency hotfixes (note: still review after)

For all code logic changes, run Codex review.
