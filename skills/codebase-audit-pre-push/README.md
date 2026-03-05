# Pre-Push Codebase Audit

Deep code review before pushing to GitHub. Checks every file for junk, dead code, security issues, and optimization problems.

## What It Does

1. Removes junk files (OS files, logs, build artifacts, secrets)
2. Fixes .gitignore
3. Eliminates dead code (unused imports, functions, variables)
4. Improves code quality (naming, magic numbers, debug statements)
5. Finds security vulnerabilities (injections, exposed secrets, auth issues)
6. Checks scalability (N+1 queries, missing indexes, pagination)
7. Reviews architecture (folder structure, separation of concerns)
8. Verifies performance optimizations
9. Checks documentation (README, comments)
10. Validates tests

## Usage

```
Use @codebase-audit-pre-push to review everything before I push to GitHub
```

The skill will go through every file, fix issues immediately, and provide a detailed report.

## Output

You get a complete audit report showing:
- Files removed and why
- Code changes per file
- Security issues found and fixed
- Scalability improvements
- Final status (Clean/Blocked/Caution)
- Scores for security, code quality, and scalability

## Key Features

- Actually reads and fixes code (not just lists issues)
- Focuses on real problems, not nitpicks
- Blocks critical security issues
- Tests after changes to prevent regressions
- Production-ready standards
