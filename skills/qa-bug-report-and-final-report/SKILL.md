---
name: qa-bug-report-and-final-report
description: Use when logging a defect found during testing, or when writing the final QA report with coverage and a ship decision.
---

# QA Bug Report and Final Report

## Severity
- **P0**: data loss, security exposure, payment error, app unusable, core flow blocked for everyone.
- **P1**: major feature broken or silent failure, no workaround, or wrong data shown.
- **P2**: feature works with a workaround, or notable UX, a11y or layout defect.
- **P3**: cosmetic, copy, minor polish.

## Bug template
```
ID: BUG-### | Severity: P_ | Area: <page or feature>
Title: <what is wrong, in one line>
Environment: <URL/build, browser or device, viewport, role, date>
Steps to reproduce:
1. ...
Expected:
Actual:
Frequency: always / sometimes (n of m)
Evidence: <screenshot path, console line, request and status>
Notes: <workaround, suspected cause, related bugs>
```
Reproduce each bug a second time from a clean state before filing. If it does not repeat, file it as intermittent with counts.

## Evidence rules
No evidence, no bug. Every finding has a screenshot or log. Name files `BUG-###-step.png`. Do not include real personal data or secrets in evidence; blur or omit.

## Final report structure
1. Verdict: SHIP / SHIP WITH FIXES / DO NOT SHIP, and the one reason.
2. Scope and environment tested, and what was not (with reasons).
3. Coverage numbers from the matrix: total rows, PASS, FAIL, BLOCKED, SKIPPED-BY-RULE, percent.
4. Bugs by severity, top 5 first, each with ID and link to full entry.
5. Cross cutting results: console and network, responsive, accessibility, edge cases.
6. Cleanup ledger result.
7. Untested list and what would unblock it.
8. Suggested regression checks for the fixes.

Be specific, never vague praise. If nothing was found in an area, say what was tried so the claim can be checked.
