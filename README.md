# QA Explorer

**An AI tester that explores your website or app like a real user, and tells you exactly what it covered and what it did not.**

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
![Works with Claude Code](https://img.shields.io/badge/works%20with-Claude%20Code-6b4fbb)
![Version](https://img.shields.io/badge/agent-v2.0.0-blue)

QA Explorer is an agent for Claude Code (and any tool that can load a markdown agent file). Give it a URL or an iOS simulator app, and it will click, type, create, edit, delete, go back and refresh the way real users do. It keeps an auditable ledger of the scope it discovered and agreed on, verifies each bug, and reports coverage gaps openly. You get a report with bugs, evidence and numbers you can check.

> **Warning:** This agent performs real actions in the system you test. Use it only on systems you own or have written permission to test. Use an isolated test copy and synthetic data, never real customer data. I am not responsible for any misuse or any problem that comes from using it. Read the [DISCLAIMER](DISCLAIMER.md) first.

## What it does and does not promise

It does:
- Explore the user facing parts it can reach, in order of risk
- Keep a coverage ledger with one row per test, each with an action, an expected result, an observation and evidence
- Verify bugs before it reports them, and list unconfirmed candidates separately
- Clean up the test data it created, and prove it
- Report what was not tested and why

It does not:
- Guarantee exhaustive coverage or bug free software. No finite run can
- Find a fixed number of bugs. Zero bugs is a valid result
- Replace specialists for penetration testing, load testing, formal accessibility conformance, API contract testing or broad cross device testing

Results depend on the target, the accounts and roles you give it, the tools it has, how much evidence those tools can capture, and the time or action budget.

## What it tests

| Area | Examples |
|---|---|
| Pages and controls | Links, buttons, menus, tabs, dialogs, toggles, drag and drop, keyboard |
| Data lifecycle | Create, read, edit, delete, search, sort, filter, upload, persistence after reload |
| Forms | Empty, long, unicode, emoji, markup, boundaries, double submit |
| Accounts | Login and logout, roles and direct URL access (you sign in, it never types your password) |
| Hidden failures | Console errors, failed and slow requests, silent save errors |
| Screens | Selected widths, dark mode, zoom, orientation |
| Accessibility | Quick keyboard, label and contrast check (smoke level, not a conformance audit) |
| When things go wrong | Offline, expired links, back button, 404 pages, two sessions |
| Native iOS apps | Taps, swipes, permissions, interruptions in the iOS Simulator |

## What tools it needs

The agent works only as well as its tools. It checks what it has at the start of every run and reports it as available, partial or unavailable. It will not imply that it saw console logs, network calls or screenshots if the tools cannot capture them.

Any one of these lets it drive an app:
- Claude in Chrome (your real browser and signed in sessions)
- The built in browser of the Claude desktop app
- Playwright MCP, or Playwright scripts
- The iOS Simulator tool, for native iOS apps

If no tool can click and type, it says so and gives you a test plan instead.

## Install

> Keep the warning above in mind. Start on a local or staging copy.

### Claude Code

```bash
git clone https://github.com/islambazlamit/qa-explorer-agent.git
cd qa-explorer-agent
mkdir -p ~/.claude/agents ~/.claude/skills
cp agents/qa-explorer.md ~/.claude/agents/
cp -R skills/* ~/.claude/skills/
```

For one project only, use `.claude/agents/` and `.claude/skills/` inside that project.

### Other tools

Paste [agents/qa-explorer.md](agents/qa-explorer.md) into the tool's agent or system prompt. The agent file holds the core rules, so it works even when the tool cannot load skills.

## Use it

> Safety first: tell the agent the environment, which accounts it may use, and what it must not touch. Use synthetic data. If you do not say, it treats the target as production and will only do read only checks and reversible actions on its own `QA-` test records.

Examples:

```text
Use the QA Explorer agent to test http://localhost:3000.
This is my local dev copy with a test database, so it is fine to create and delete data.
```

```text
Use QA Explorer on https://staging.example.com.
Staging, I am already signed in as a test user.
Do not publish or send anything other people can see. Budget: about 60 minutes.
```

```text
Use QA Explorer to retest the checkout flow and confirm bugs 3, 5 and 7 are fixed.
```

### How a run goes

1. **Charter.** Environment, authorization, roles, allowed and prohibited actions, critical flows, budget.
2. **Recon.** Maps the pages, routes and features it can reach, including pages missing from the menu.
3. **Risk plan.** Ranks the checks and builds the coverage ledger.
4. **Execution.** Happy path first, then wrong input, odd navigation and edge cases, with evidence for each result.
5. **Signals.** Console, network, screen size and accessibility observations.
6. **Verify.** Repeats each candidate bug when it is safe, and separates confirmed from unconfirmed.
7. **Cleanup.** Removes what it created and checks that it worked.
8. **Report.** Coverage numbers with numerators and denominators, bugs by severity, untested list, limits.

### Sign in

The agent never types your password. Sign in yourself in the browser, then tell it you are ready.

## The report

```text
Recommendation (provisional): SHIP WITH FIXES
Scope: 37 routes probed, 26 pages read, 3 full create, edit and delete flows
Coverage: 58 of 71 obligations executed (82 percent of the recorded scope, not of the whole product)
Bugs: 2 P1, 5 P2, 6 P3 (4 more unconfirmed)
Not tested: user creation (not allowed), payroll (no way to delete test data)
Cleanup: 3 of 4 test records deleted and verified, 1 leftover named
```

The numbers above are an illustration of the format, not a benchmark.

Each bug looks like this:

```text
BUG-007 | P1 | confidence: high | reproduced 2 of 2
Title: Invalid promo code clears the cart total
Steps: 1. Add an item. 2. Open checkout. 3. Enter ZZZ in promo and press Enter.
Expected: Clear error, total unchanged
Actual: Total shows 0.00 and no message
Evidence: shots/BUG-007-step3.png, console TypeError at promo.js:88
```

## Skills

The agent loads these from the [skills](skills) folder. Each is a short, focused guide.

| Skill | Use it for |
|---|---|
| `qa-safety-and-data-hygiene` | Environment gate, test data prefix, cleanup, hard stops |
| `qa-recon-and-site-map` | Finding every page, route and feature |
| `qa-coverage-matrix` | Tracking page, control and state until each has evidence or a reason |
| `qa-human-interaction-sweep` | Touching every clickable and typeable thing |
| `qa-crud-lifecycle` | Create, read, update, delete, search, sort, filter, upload |
| `qa-form-and-input-validation` | Value classes, error messages, double submit |
| `qa-auth-and-session` | Accounts, sessions, roles, direct URL access |
| `qa-console-network-watch` | Console errors and failing or slow requests |
| `qa-responsive-and-browser-matrix` | Widths, orientation, themes, browsers |
| `qa-accessibility-quick-pass` | Keyboard and semantics smoke check |
| `qa-mobile-app-simulator` | iOS Simulator app testing |
| `qa-edge-and-resilience` | Offline, errors, expired links, two sessions |
| `qa-bug-report-and-final-report` | Severity scale, bug template, final report |

## Built in safety rules

- Asks for the environment (local, staging or production) before it changes anything, and treats the target as production if you do not answer.
- Checks where a local app really sends its data, mail and payments (names and hosts only, never secrets).
- Uses `QA-` prefixed test data and keeps a cleanup list. Checks that a delete path exists before creating data.
- Never enters real payment details, never solves or bypasses CAPTCHAs, never types passwords, never creates accounts.
- Does not call unknown API, cron or webhook URLs just to see what happens.
- Does not send messages to real people or publish to other users without your clear yes.
- If a tool blocks an action, it stops and asks you instead of finding a way around.

These are instructions to an AI. They lower risk but cannot remove it. See the [DISCLAIMER](DISCLAIMER.md).

## Is it any good? Validation

No benchmark results are published yet, and I will not claim any until they have been run. The [validation plan](docs/validation-plan.md) describes how to measure defect recall, precision, reproduction rate, unsupported claims, safety violations and cleanup success. A small practice app with seeded bugs and false positive traps is in [fixtures/buggy-notes](fixtures/buggy-notes) so you can run the plan yourself.

## Changes

See the [CHANGELOG](CHANGELOG.md). Every change that affects behavior should come with a regression scenario.

## Contributing

Ideas and fixes are welcome. Open an issue or a pull request. Keep skills short, practical and in plain words. If you change behavior, add a line to the changelog and a scenario to the validation plan.

## Credits

Methods adapted, not copied, from public work by others:

- Anthropic, [webapp-testing skill](https://github.com/anthropics/skills) (look first, then act)
- testdino, [playwright-skill](https://github.com/testdino-hq/playwright-skill) (MIT)
- VoltAgent, [awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) (test design ideas)

This project is not affiliated with or endorsed by them or by Anthropic.

## License and copyright

Copyright (c) 2026 Islam Albazlamit. Released under the [MIT License](LICENSE). You may use, copy, change and share it, as long as you keep the copyright and license notice. The software comes as is, without warranty, and the author is not liable for any claim or damage. See the [DISCLAIMER](DISCLAIMER.md).
