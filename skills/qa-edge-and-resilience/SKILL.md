---
name: qa-edge-and-resilience
description: Use when testing how a website or app behaves when things go wrong, such as bad networks, errors, expired links, odd navigation and two users at once.
---

# QA Edge and Resilience

Real users hit the unhappy paths. Test them on purpose.

## Network
- Offline mid flow, then reconnect. Slow 3G. Request interrupted by a refresh.
- Submit while offline: queued, lost, or clearly errored?
- Page loaded then connection drops; stale data indicators.

## Errors and missing things
- 404 page: helpful, branded, with a way home. Mistyped URLs, trailing slashes, uppercase paths.
- Deleted or never existing record IDs in the URL.
- Server error display (if triggerable safely): no stack traces, retry option.
- Expired, used, or tampered links (reset, invite, share, verify).
- Broken images, missing translations, empty API responses.

## Navigation oddities
- Back after submit. Forward after back. Refresh on every step of a wizard.
- Open the same page in two tabs and act in both.
- Deep link into the middle of a flow while logged out.
- Bookmark a modal or step; reopen it.
- Browser autofill and password managers interfering with fields.

## Concurrency and state
- Two sessions editing the same record. Two users claiming the same limited item.
- Rapid repeat actions (double add to cart, double like, double pay button).
- Time sensitive features: expiry, timezone changes, daylight saving, midnight rollover.

## Limits
- Maximum items, maximum text, maximum upload, pagination of huge lists.
- Special account states: new account with no data, account with a lot of data.

## Record
Scenario, steps, expected resilient behavior, actual, evidence. A data loss or silent failure outcome is at least P1.
