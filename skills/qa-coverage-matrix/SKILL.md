---
name: qa-coverage-matrix
description: Use when you need proof that a whole website or app was tested, by tracking every page, control and state until each one has evidence or a stated reason.
---

# QA Coverage Matrix

"I tested everything" is a claim. The matrix is the proof.

## Build it from the site map
One row per testable item. Items are page × control × state.

| ID | Page | Control or flow | State | Test types | Status | Evidence | Bug IDs |
|---|---|---|---|---|---|---|---|

Test types per item: happy, invalid input, boundary, keyboard, responsive, error, permission, persistence.

## Status values
- `TODO`: not tried.
- `PASS`: acted, observed expected result, evidence saved.
- `FAIL`: acted, result wrong, bug filed.
- `BLOCKED`: could not act (needs credentials, third party, bot check). State the blocker.
- `SKIPPED-BY-RULE`: forbidden by the safety gate (real payment, production delete). State the rule.
- `N/A`: does not exist in this state. State why.

A row is PASS only with all three: the action I took, what I saw, a screenshot or log reference.

## Flows to add as extra rows
Signup to first value, login and logout, purchase or booking, create then edit then delete, search to result, settings change then verify elsewhere, password reset, contact form, newsletter, cookie banner, language or theme switch, error pages, empty states.

## Exit criteria
- Zero `TODO` rows.
- Every `BLOCKED` and `SKIPPED-BY-RULE` row has a reason and a suggested way to unblock.
- Coverage percent = (PASS + FAIL) / (all rows minus N/A). Report it, with the list of non tested rows. Never round up.
- Re-scan the live app once more for new elements; add rows and finish them.
