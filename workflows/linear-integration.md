---
title: "Linear → Claude Code → Cursor → Mintlify Workflow"
description: "Complete guide to the Modern Agentic Stack development workflow"
---

# Modern Agentic Stack: Complete Workflow Guide

This guide explains how to use the integrated **Linear → Claude Code → Cursor 2.0 → Mintlify** workflow for development in the Metacogna repository.

## Overview

The Modern Agentic Stack creates a seamless development pipeline:

1. **Linear** - Source of truth for all work (issues, features, bugs)
2. **Claude Code** - Autonomous development agent (heavy lifting)
3. **Cursor 2.0** - IDE with Linear integration (tactical refinement)
4. **Mintlify** - Automated documentation (auto-sync from Linear + code)
5. **GitHub** - Version control + CI/CD (auto-updates Linear)

## Setup

### 1. Linear API Key

Get your Linear API key:
1. Go to https://linear.app/settings/api
2. Create a new Personal API Key
3. Copy the key

Set the environment variable:
```bash
# Add to your ~/.bashrc or ~/.zshrc
export LINEAR_API_KEY='your-linear-api-key-here'
```

### 2. Cursor MCP Configuration

The repository includes `.cursor/mcp.json` which configures Linear MCP integration.

**Verify it's working:**
1. Open Cursor IDE
2. Type `@Linear` in chat
3. You should see Linear options appear

### 3. GitHub Secrets (for CI/CD)

Add to repository settings → Secrets and variables → Actions:
- `LINEAR_API_KEY` - Your Linear API key
- `MINTLIFY_API_KEY` - Mintlify deployment key (optional)

## Complete Workflow: Building a Feature

### Step 1: Create Issue in Linear

**Option A: Web Interface**
1. Go to https://linear.app/pratejra
2. Click "+ New Issue"
3. Fill in:
   - Title: Clear, actionable description
   - Description: Requirements and acceptance criteria
   - Project: Timeless Love / Shadow Agent / Metacogna Platform
   - Labels: Feature, backend/frontend, priority
4. Assign to yourself

**Option B: Via Cursor**
```
@Linear create issue
  title: "Add family invitation system"
  description: "Users should be able to invite family members via email"
  project: "Timeless Love Service"
  labels: ["Feature", "backend", "frontend"]
```

**Option C: Via CLI**
```bash
./scripts/claude-linear-helper.sh create-issue \
  "Add family invitation system" \
  "Users should be able to invite family members via email" \
  "Timeless Love"
```

**Result:** Issue created, assigned ID like `TL-123`

### Step 2: Start Work with Claude Code

**In your terminal:**

```bash
# Method 1: Ask Claude Code to fetch and implement
claude "Fetch Linear issue TL-123 and implement the feature"

# Method 2: More specific instructions
claude "Read Linear issue TL-123. Implement the backend API endpoints, then create the frontend components. Run all tests and fix any errors."
```

**What Claude Code will do:**
1. ✅ Fetch issue details via Linear MCP
2. ✅ Read requirements and acceptance criteria
3. ✅ Understand codebase context (reads CLAUDE.md files)
4. ✅ Scaffold implementation:
   - Backend: Models, schemas, endpoints, services
   - Frontend: Components, services, types
5. ✅ Write tests
6. ✅ Run tests and fix errors autonomously
7. ✅ Create commits with Linear issue reference:
   ```
   [TL-123] feat: add invitation model and schema
   [TL-123] feat: implement invitation endpoints
   [TL-123] feat: add frontend invitation UI
   [TL-123] test: add invitation flow tests
   ```
8. ✅ Update Linear issue with progress

**You can walk away!** Claude Code runs autonomously until tests pass.

### Step 3: Refine in Cursor 2.0

Once Claude Code completes, open Cursor for refinement:

**1. Review changes:**
```bash
git status
git diff
```

**2. Verify Linear issue requirements:**
```
@Linear get issue TL-123
```

Check that all acceptance criteria are met.

**3. Polish the implementation:**
- **UI/UX refinement:** Adjust layouts, colors, spacing
- **Edge cases:** Handle error states, loading states
- **Accessibility:** Add ARIA labels, keyboard navigation
- **Performance:** Optimize queries, add caching

**4. Use Cursor Tab for quick fixes:**
- Cursor Tab suggests code completions
- Knows Linear context via MCP

**5. Final testing:**
```bash
# Backend
cd backend && pytest

# Frontend
cd frontend && bun test
```

### Step 4: Create Pull Request

**Option A: GitHub CLI**
```bash
gh pr create \
  --title "[TL-123] Feature: Family invitation system" \
  --body "Implements Linear issue TL-123

## Changes
- Added invitation model and API endpoints
- Created frontend invitation UI
- Added comprehensive tests

## Linear
Fixes: https://linear.app/pratejra/issue/TL-123

## Testing
- [x] Backend tests passing
- [x] Frontend tests passing
- [x] Manual testing completed"
```

**Option A: Via Cursor**
```
@Linear update issue TL-123 status:"In Review"
```

Then create PR normally on GitHub.

### Step 5: Automatic Processing

**When you create the PR:**
1. ✅ GitHub Actions detects Linear ID in PR title
2. ✅ Updates Linear issue with PR link
3. ✅ Adds comment to Linear: "🔗 PR opened: [link]"
4. ✅ Moves Linear issue to "In Review"
5. ✅ Runs full test suite (backend + frontend)
6. ✅ Updates Linear with test results

**When PR is merged:**
1. ✅ Linear issue automatically moves to "Done"
2. ✅ Deployment to Railway (backend) + Cloudflare Pages (frontend)
3. ✅ Linear issue gets deployment comment: "🚀 Deployed to production"
4. ✅ Mintlify docs auto-update:
   - Changelog generated from Linear issue
   - API reference regenerated from code
   - Feature docs updated

**You don't need to do anything!** It's all automated.

## Quick Reference

### Commit Message Format

**REQUIRED:** All commits must include Linear issue ID

```bash
[TL-123] feat: add authentication endpoints
[SA-456] fix: resolve WASM memory leak
[MP-789] docs: update API reference
```

### Valid Commit Types

- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation
- `refactor` - Code refactoring
- `test` - Tests
- `chore` - Maintenance

### Linear Issue Prefixes

- `TL-###` - Timeless Love issues
- `SA-###` - Shadow Agent issues
- `MP-###` - Metacogna Platform issues

### Cursor Commands

```
# List your issues
@Linear list issues assignee:"me"

# Get specific issue
@Linear get issue TL-123

# Update issue status
@Linear update issue TL-123 status:"In Progress"

# Create new issue
@Linear create issue title:"Feature name" project:"Timeless Love"
```

### CLI Helpers

```bash
# Create issue
./scripts/claude-linear-helper.sh create-issue "Title" "Description" "Project"

# List your issues
./scripts/claude-linear-helper.sh list-my-issues

# Update status
./scripts/claude-linear-helper.sh update-issue TL-123 "In Progress"

# Link PR
./scripts/claude-linear-helper.sh link-pr TL-123 https://github.com/...
```

## Example Workflows

### Scenario 1: Bug Fix

```bash
# 1. User reports bug, you create Linear issue
@Linear create issue title:"Token refresh fails" label:["Bug","urgent"]

# Result: SA-456 created

# 2. Claude Code implements fix
claude "Fix Linear issue SA-456. Debug the token refresh logic and add regression tests."

# 3. Claude creates commits
[SA-456] fix: resolve token refresh race condition
[SA-456] test: add token refresh regression test

# 4. You create PR
gh pr create --title "[SA-456] Fix: Token refresh race condition"

# 5. Automatic processing
# - PR created → Linear updated
# - Tests run → Linear updated with results
# - PR merged → Linear moved to "Done"
# - Deployed → Linear notified
```

### Scenario 2: Large Feature (Multi-day)

```bash
# Day 1: Backend implementation
claude "Implement backend for Linear issue TL-789. Focus on database models, API endpoints, and tests. Don't implement frontend yet."

# Result: Backend complete, tests passing

# Day 2: Frontend implementation
claude "Implement frontend for Linear issue TL-789. The backend is already done. Create the UI components and integrate with the API."

# Result: Full feature complete

# Day 3: Polish in Cursor
# Open Cursor, refine UI/UX, fix edge cases

# Day 4: Create PR and merge
gh pr create --title "[TL-789] Feature: Advanced analytics dashboard"
```

### Scenario 3: Documentation Update

```bash
# 1. Create docs issue
@Linear create issue title:"Update API documentation" label:["docs"]

# Result: MP-101 created

# 2. Update docs
# Edit markdown files manually or with Claude

# 3. Commit with Linear ID
git commit -m "[MP-101] docs: update API reference for new endpoints"

# 4. Push to main
git push origin main

# 5. Automatic processing
# - Mintlify auto-syncs
# - Linear issue marked done
# - Changelog updated
```

## Troubleshooting

### "Linear ID not found in commits"

**Problem:** GitHub Actions can't find Linear issue reference

**Solution:**
- Ensure commit message starts with `[PROJ-123]`
- Check issue exists in Linear
- Verify you have access to the project

### "Claude Code can't fetch Linear issue"

**Problem:** `@Linear` not working in Claude Code

**Solution:**
1. Check `LINEAR_API_KEY` environment variable is set
2. Restart Claude Code session
3. Verify issue ID is correct

### "Mintlify not updating"

**Problem:** Documentation not syncing after merge

**Solution:**
1. Check GitHub Actions ran successfully
2. Verify `MINTLIFY_API_KEY` secret is set
3. Check Mintlify dashboard for errors

### "PR not linking to Linear"

**Problem:** Linear issue not getting PR link

**Solution:**
1. Ensure PR title includes Linear ID: `[TL-123]`
2. Check GitHub Actions workflow logs
3. Verify `LINEAR_API_KEY` secret is set in repository

## Best Practices

### ✅ DO

- **Always** reference Linear issues in commits
- **Keep** Linear issues updated with progress
- **Use** descriptive commit messages
- **Link** PRs to Linear issues in description
- **Update** Linear when deployments happen
- **Close** Linear issues when work is complete

### ❌ DON'T

- **Don't** commit without Linear issue reference
- **Don't** create PRs without Linear link
- **Don't** leave Linear issues stale
- **Don't** use generic commit messages
- **Don't** skip acceptance criteria
- **Don't** forget to update Linear after merge

## Advanced Usage

### Multiple Issues in Single PR

If your PR addresses multiple issues:

```bash
# PR title
[TL-123][TL-456] Feature: Complete authentication system

# PR body
Fixes: TL-123, TL-456

This PR implements:
- TL-123: JWT authentication
- TL-456: Password reset flow
```

### Emergency Hotfix Without Linear

For urgent production fixes where creating a Linear issue adds delay:

```bash
# 1. Create commit with HOTFIX prefix
git commit -m "HOTFIX: resolve critical production bug"

# 2. Create PR
gh pr create --title "HOTFIX: Critical production fix"

# 3. After merge, create Linear issue retroactively
@Linear create issue title:"Document hotfix" description:"Retroactive issue for hotfix [link-to-PR]"
```

### Working Offline

If Linear/GitHub are unavailable:

```bash
# 1. Work normally, use placeholder
git commit -m "[TODO-123] feat: implement feature"

# 2. When online, amend commits
git rebase -i HEAD~3
# Edit commits to add real Linear IDs

# 3. Force push (if not yet pushed)
git push --force-with-lease
```

## Metrics & Analytics

The workflow automatically tracks:

- **Cycle time:** Issue created → Issue done
- **Lead time:** First commit → Deployment
- **Deployment frequency:** How often you ship
- **Change failure rate:** Issues reopened
- **Linear issue velocity:** Issues completed per sprint

View metrics in:
- Linear: Team → Insights
- GitHub: Repository → Insights → Actions

## Support & Resources

- **Linear Documentation:** https://linear.app/docs
- **Cursor MCP Guide:** `.cursor/rules/linear-workflow.mdc`
- **GitHub Actions Workflows:** `.github/workflows/`
- **Helper Scripts:** `scripts/`
- **Project CLAUDE.md:** Root repository guidance

---

**Remember:** The goal is **autonomous development** with **automated documentation**. Let the tools do the work!
