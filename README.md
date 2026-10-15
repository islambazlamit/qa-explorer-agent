# QA Explorer

**An AI tester that uses your website or app like a real person, and proves it covered everything.**

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
![Works with Claude Code](https://img.shields.io/badge/works%20with-Claude%20Code-6b4fbb)

QA Explorer is an agent for Claude Code (and any tool that can load a markdown agent file). Give it a URL or an app, and it will click the buttons, fill the forms, create things, edit them, delete them, go back, refresh, and break things the way real users do. When it finishes, you get a report with bugs, evidence and a coverage count.

> **Warning:** This agent performs real actions in the system you test. Use it only on systems you own or have written permission to test, and start on a test copy. I am not responsible for any misuse or any problem that comes from using it. Read the [DISCLAIMER](DISCLAIMER.md) first.

## Why use it

Most test tools check what you told them to check. QA Explorer starts from what is actually on the screen.

- **It acts like a human.** Real clicks, real typing, real navigation, in a real browser.
- **It does not skip things.** It builds a site map and a coverage table of every page, control and state. A row only counts as tested when it has an action, a result and evidence.
- **It is honest about gaps.** The report lists what was not tested and why. It never says "whole site tested" unless the table is full.
- **It is careful.** It asks which environment it is testing before it changes anything, uses `QA-` prefixed test data, and cleans up after itself.
- **It writes useful bugs.** Each bug has steps, expected result, actual result, how often it happens, severity (P0 to P3) and a screenshot or log.

## What it tests

| Area | Examples |
|---|---|
| Every page and control | Links, buttons, menus, tabs, modals, toggles, drag and drop, keyboard |
| Data lifecycle | Create, read, edit, delete, search, sort, filter, upload, persistence after reload |
| Forms | Empty, long, unicode, emoji, markup, boundaries, double submit |
| Accounts | Signup, login, logout, reset, roles, direct URL access |
| Hidden failures | Console errors, failed and slow requests, silent save errors |
| Screens | Phone to desktop widths, dark mode, zoom, orientation |
| Accessibility | Keyboard only run, labels, focus, contrast (quick pass) |
| When things go wrong | Offline, expired links, back button, two sessions, 404 pages |
| Native iOS apps | Taps, swipes, permissions, interruptions in the iOS Simulator |

## Install

### Claude Code

Copy the agent and the skills into your Claude folder.

```bash
git clone https://github.com/islambazlamit/qa-explorer-agent.git
cd qa-explorer-agent
mkdir -p ~/.claude/agents ~/.claude/skills
cp agents/qa-explorer.md ~/.claude/agents/
cp -R skills/* ~/.claude/skills/
```

For one project only, use `.claude/agents/` and `.claude/skills/` inside that project instead.

### Other tools

Paste the contents of [agents/qa-explorer.md](agents/qa-explorer.md) into the tool's agent or system prompt. The agent file already holds a short summary of every skill, so it works even when the tool cannot load skills.

## What you need

The agent must be able to drive a browser or a simulator. Any one of these works:

- Claude in Chrome (uses your real browser and logged in sessions)
- The built in browser of the Claude desktop app
- Playwright MCP, or Playwright scripts
- The iOS Simulator tool, for native iOS apps

If no tool can click and type, the agent will say so and stop. It will not pretend.

## Use it

Start with the target and the rules. A few examples:

```text
Use the QA Explorer agent to test http://localhost:3000 end to end.
This is my local dev copy, so you can create and delete data.
```

```text
Use QA Explorer on https://staging.example.com.
Staging, test account: tester@example.com (I am already signed in).
Do not publish or send anything that other people can see.
```

```text
Use QA Explorer to retest the checkout flow and confirm bugs 3, 5 and 7 are fixed.
```

### What it does, in order

1. **Safety gate.** Asks for the environment, accounts and what it may change.
2. **Recon.** Maps every page, route and feature.
3. **Coverage table.** Lists each page, control and state to test.
4. **Execution.** Happy path first, then wrong inputs, odd navigation and edge cases.
5. **Cross checks.** Console and network errors, screen sizes, accessibility, resilience.
6. **Cleanup.** Deletes the test data it created and lists anything it could not remove.
7. **Report.** Verdict, coverage numbers, bugs by severity, untested list, next steps.

### Sign in

The agent never types your password. Sign in yourself in the browser, then tell it you are ready. It will continue from your session.

## The report

You get a markdown report like this:

```text
Verdict: SHIP WITH FIXES
Coverage: 37 routes probed, 26 pages read, 3 full create/edit/delete flows
Bugs: 2 P1, 5 P2, 6 P3
Not tested: user creation (not allowed), payroll (no way to delete test data)
Cleanup: 3 of 4 test records deleted, 1 leftover listed
```

Each bug looks like this:

```text
BUG-007 | P1 | Checkout
Title: Invalid promo code clears the cart total
Steps: 1. Add an item. 2. Open checkout. 3. Enter ZZZ in promo and press Enter
Expected: Clear error, total unchanged
Actual: Total shows 0.00 and no message
Frequency: always (3 of 3)
Evidence: shots/BUG-007-step3.png, console TypeError at promo.js:88
```

## Skills

The agent loads these from the [skills](skills) folder. Each one is a small, focused guide.

| Skill | Use it for |
|---|---|
| `qa-safety-and-data-hygiene` | Environment gate, test data prefix, cleanup, hard stops |
| `qa-recon-and-site-map` | Finding every page, route and feature |
| `qa-coverage-matrix` | Tracking page, control and state until each has evidence |
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

- Asks for the environment (local, staging or production) before it changes anything.
- Treats the target as production if you do not answer.
- Checks where a local app really sends its data, mail and payments.
- Uses `QA-` prefixed test data and keeps a cleanup list.
- Never enters real payment details, never solves or bypasses CAPTCHAs, never types passwords.
- Does not call unknown API, cron or webhook URLs just to see what happens.
- Does not create accounts, send messages to real people, or publish to other users without your clear yes.
- If a tool blocks an action, it stops and asks you instead of finding a way around.

These are instructions to an AI. They lower risk but cannot remove it. See the [DISCLAIMER](DISCLAIMER.md).

## Limits

- It tests what a browser or simulator can reach. It is not a load tester, a penetration test or a security audit.
- It cannot test what it is not allowed to touch (real payments, account creation, messages to real people).
- It can make mistakes. Check the evidence in the report before you act on it.
- Coverage depends on the accounts and roles you give it. Use one account per role for full coverage.

## Contributing

Ideas and fixes are welcome. Open an issue or a pull request. Keep skills short, practical and written in plain words.

## Credits

Methods adapted, not copied, from public work by others:

- Anthropic, [webapp-testing skill](https://github.com/anthropics/skills) (look first, then act)
- testdino, [playwright-skill](https://github.com/testdino-hq/playwright-skill) (MIT)
- VoltAgent, [awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) (test design ideas)

This project is not affiliated with or endorsed by them or by Anthropic.

## License and copyright

Copyright (c) 2026 Islam Albazlamit. Released under the [MIT License](LICENSE). You may use, copy, change and share it, as long as you keep the copyright and license notice. The software comes as is, without warranty, and the author is not liable for any claim or damage. See the [DISCLAIMER](DISCLAIMER.md).
