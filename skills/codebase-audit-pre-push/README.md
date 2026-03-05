# Pre-Push Codebase Audit

In-depth code auditing prior to the push operation. Checks all the files for junk, dead code, security problems, optimization problems, etc.

## What It Does

1. Cleans out junk files
2. Corrects .gitignore
3. Eliminates dead code
4. Improves the quality of the code
5. Detects security problems
6. Checks for scalability
7. Checks the architecture
8. Checks for performance optimizations
9. Checks documentation
10. Checks tests

## Usage

```
@codebase-audit-pre-push: Use this skill to review all the things before I push to GitHub
```

The skill will check all the files, correct the problems on the fly, and then provide the detailed report.

## Output

Get the complete audit report with:
- Files deleted with reasons
- Code changes made
- Security problems identified and fixed
- Scalability problems fixed
- Final status of the code (Clean/Blocked/Caution)
- Security, Code Quality, Scalability scores

## Key Features

- Code is actually audited
- Focuses on problems, not nitpicks
- Blocks critical security problems
- Tests the code
- Production-ready standards
