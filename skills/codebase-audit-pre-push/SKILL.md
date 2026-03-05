---
name: codebase-audit-pre-push
description: "Deep audit before GitHub push: removes junk files, dead code, security holes, and optimization issues. Checks every file line-by-line for production readiness."
category: development
risk: safe
source: community
date_added: "2026-03-05"
---

# Pre-Push Codebase Audit

You're a senior engineer doing final review before this code goes to GitHub. Check everything systematically and fix issues as you find them.

## When to Use This Skill

- User says "audit the codebase" or "review before push"
- Before pushing to GitHub for the first time
- Before making a repository public
- Pre-production deployment review
- User asks to "clean up the code" or "optimize everything"

## Your Job

Go through the entire codebase file by file. Actually read the code. Fix problems immediately. Don't just list issues—make the changes.

## Audit Process

### 1. Clean Up Junk Files

First, scan for files that shouldn't be on GitHub:

**Delete these immediately:**
- OS files: `.DS_Store`, `Thumbs.db`, `desktop.ini`
- Logs: `*.log`, `npm-debug.log*`, `yarn-error.log*`
- Temp files: `*.tmp`, `*.temp`, `*.cache`, `*.swp`
- Build output: `dist/`, `build/`, `.next/`, `out/`, `.cache/`
- Dependencies: `node_modules/`, `vendor/`, `__pycache__/`, `*.pyc`
- IDE files: `.idea/`, `.vscode/` (ask user first), `*.iml`, `.project`
- Backup files: `*.bak`, `*_old.*`, `*_backup.*`, `*_copy.*`
- Test artifacts: `coverage/`, `.nyc_output/`, `test-results/`
- Personal junk: `TODO.txt`, `NOTES.txt`, `scratch.*`, `test123.*`

**Critical - Check for secrets:**
- `.env` files (should never be committed)
- Files with: `password`, `api_key`, `token`, `secret`, `private_key`
- `*.pem`, `*.key`, `*.cert`, `credentials.json`, `serviceAccountKey.json`

If you find secrets in code, flag as CRITICAL BLOCKER.

### 2. Fix .gitignore

Check if `.gitignore` exists and is comprehensive. If missing or incomplete, update it to cover all junk file patterns above. Verify `.env.example` exists (with keys but no values).

### 3. Audit Every Source File

Go through each code file and check:

**Dead Code (remove immediately):**
- Commented-out code blocks
- Unused imports/requires
- Unused variables (declared but never used)
- Unused functions (defined but never called)
- Unreachable code (after `return`, inside `if (false)`)
- Duplicate logic (same code in multiple places—consolidate)

**Code Quality (fix as you go):**
- Vague names: `data`, `info`, `temp`, `thing` → rename to be descriptive
- Magic numbers: `if (status === 3)` → extract to named constant
- Debug statements: remove `console.log`, `print()`, `debugger`
- TODO/FIXME comments: either fix them or remove them
- TypeScript `any`: add proper types or comment why `any` is needed
- Use `===` not `==` in JavaScript
- Functions > 50 lines: consider splitting
- Nested code > 3 levels: refactor with early returns

**Logic Issues (critical):**
- Missing null/undefined checks
- Array operations on potentially empty arrays
- Async functions not awaited
- Promises without `.catch()` or try/catch
- Infinite loop possibilities
- Missing `default` in switch statements

### 4. Security Check (Zero Tolerance)

**Secrets:** Grep for hardcoded passwords, API keys, tokens. Must be in env vars.

**Injection vulnerabilities:**
- SQL: No string concatenation in queries—use parameterized queries only
- Command injection: No `exec()` with user input
- Path traversal: No file paths from user input without validation
- XSS: No `innerHTML` or `dangerouslySetInnerHTML` with user data

**Auth/Authorization:**
- Passwords hashed with bcrypt/argon2 (never MD5 or plain text)
- Protected routes check authentication
- Authorization checked server-side, not just UI
- No IDOR: verify user owns the resource they're accessing

**Data exposure:**
- API responses don't leak unnecessary data
- Error messages don't expose stack traces or DB details
- Pagination on list endpoints

**Dependencies:**
- Run `npm audit` or equivalent
- Flag critically outdated or vulnerable packages

### 5. Scalability Check

**Database:**
- N+1 queries: loops with DB calls inside → use JOINs or batch queries
- Missing indexes on WHERE/ORDER BY columns
- Unbounded queries: add LIMIT/pagination
- Avoid `SELECT *`: specify columns

**API Design:**
- Heavy operations (email, reports, file processing) → move to background queue
- Rate limiting on public endpoints
- Caching for read-heavy data
- Timeouts on external calls

**Code:**
- No global mutable state
- Event listeners cleaned up (no memory leaks)
- Large files streamed, not loaded into memory

### 6. Architecture Check

**Organization:**
- Clear folder structure
- Files in logical locations
- No "misc" or "stuff" folders

**Separation of concerns:**
- UI layer: only rendering
- Business logic: pure functions
- Data layer: isolated DB queries
- No 500+ line "god files"

**Reusability:**
- Duplicate code → extract to shared utility
- Constants defined once and imported
- Types/interfaces reused, not redefined

### 7. Performance

**Backend:**
- Expensive operations don't block requests
- Batch DB calls where possible
- Cache headers set correctly

**Frontend (if applicable):**
- Code splitting implemented
- Images optimized
- No massive dependencies for small utilities
- Lazy loading for heavy components

### 8. Documentation

**README.md must have:**
- What the project does
- How to install and run
- Required environment variables
- How to run tests

**Code comments:**
- Explain WHY, not WHAT
- Complex logic has explanations
- No comments that just restate code

### 9. Testing

- Critical paths have tests (auth, payments, core features)
- No `test.only` or `fdescribe` left in code
- No `test.skip` without explanation
- Tests actually test behavior, not implementation details

### 10. Final Verification

Run the app after all changes. Make sure nothing broke. Check that:
- App starts without errors
- Main features work
- Tests pass (if they exist)
- No regressions introduced

## Output Format

After auditing, provide a report:

```
CODEBASE AUDIT COMPLETE

FILES REMOVED:
- node_modules/ (build artifact)
- .env (contained secrets)
- old_backup.js (unused duplicate)

CODE CHANGES:
[src/api/users.js]
  ✂ Removed unused import: lodash
  ✂ Removed dead function: formatOldWay()
  🔧 Renamed 'data' → 'userData' for clarity
  🛡 Added try/catch around API call (line 47)
  
[src/db/queries.js]
  ⚡ Fixed N+1 query: now uses JOIN instead of loop

SECURITY ISSUES:
🚨 CRITICAL: Hardcoded API key in config.js (line 12) → moved to .env
⚠️ HIGH: SQL injection risk in search.js (line 34) → fixed with parameterized query

SCALABILITY:
⚡ Added pagination to /api/users endpoint
⚡ Added index on users.email column

FINAL STATUS:
✅ CLEAN - Ready to push to GitHub

Scores:
Security: 9/10 (one minor header missing)
Code Quality: 10/10
Scalability: 9/10
Overall: 9/10
```

## Key Principles

- Actually read the code, don't skim
- Fix issues immediately, don't just document them
- If you're unsure about removing something, ask the user
- Test after making changes
- Be thorough but practical—focus on real problems
- Security issues are blockers—nothing ships with critical vulnerabilities

## Related Skills

- `@security-auditor` - Deeper security review
- `@systematic-debugging` - Investigate specific issues
- `@git-pushing` - Push code after audit
