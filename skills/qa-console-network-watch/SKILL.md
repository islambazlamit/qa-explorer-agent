---
name: qa-console-network-watch
description: Use when testing in a browser and you must catch errors the screen does not show, such as console errors, failed requests and slow calls.
---

# QA Console and Network Watch

The UI can look fine while the app is failing underneath. Watch both channels during every test pass.

## Setup
Open console and network capture before the first action (read_console_messages and read_network_requests style tools, or Playwright `page.on('console')` and `page.on('response')`). Clear before each flow so errors map to actions.

## Console
- Errors and unhandled promise rejections: always a finding unless known third party noise.
- Warnings: React key, deprecated API, CSP, mixed content, cookie SameSite: note and group.
- Logged secrets, tokens, personal data, stack traces: finding.

## Network
- Status 4xx and 5xx on any action. Note the endpoint, method, status, body message.
- Failed or cancelled requests, CORS errors, blocked resources.
- Slow calls: over 1s on interaction, over 3s on page load as smoke thresholds. Note, do not load test.
- Repeated or duplicate calls (double fetch, polling too fast).
- Large payloads and unoptimized images over 1 MB.
- Insecure `http://` resources on `https://` pages.
- Redirect chains. Missing caching on static assets.
- Requests carrying sensitive data in the URL query string.

## Correlate
For each hit, store: action that triggered it, URL, time, message, screenshot of the visible symptom (or "no visible symptom"). A silent failure with no user feedback is a higher severity bug than the same failure with a clear error message.

## Output
A table: Page | Action | Console or network event | Visible to user? | Severity.
