# Changelog

All changes that affect how the agent behaves are listed here. Each one should add or update a scenario in the [validation plan](docs/validation-plan.md) regression suite.

## 2.0.0
Rewrite of the agent instructions for honesty and safety.

- No bug quota. Zero defects is a valid result. Removed the "find at least a few issues" stance
- Added a tool capability check (`AVAILABLE`, `PARTIAL`, `UNAVAILABLE`) and a rule to never imply capture the tools cannot do
- Coverage statuses are now `TODO`, `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, `EXCLUDED_BY_RULE`, `NOT_APPLICABLE`
- Coverage is bounded: the agent never claims everything was covered, and reports numerators and denominators
- Defect reports include confidence and verification, and unconfirmed candidates are listed apart from confirmed defects
- Release verdicts are given only when asked, and are provisional
- Safety gate expanded: environment, side effect destinations, hard stops, "localhost is not safe", no GET on unknown endpoints, unanswered environment means production
- Mutation ledger with original state capture and cleanup verified by a fresh look
- Kept practical lessons from real runs: stale click references, hidden `confirm()` dialogs, silent failures, orphan pages, checking for a delete path before creating test data
- Added a validation plan and a starter fixture

## 1.0.0
First release. Human style end to end testing with 13 skills, a coverage table, safety rules and a report template.
