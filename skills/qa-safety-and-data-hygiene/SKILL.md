---
name: qa-safety-and-data-hygiene
description: Use when starting any hands on test of a website or app, before the first click that could create, change, delete, send or charge anything.
---

# QA Safety and Data Hygiene

Testing as a human means real side effects. Decide what is safe before acting.

## Environment gate (ask first, wait for the answer)
1. What is the target URL or app build?
2. Is it local, staging, a test tenant, or production?
3. Which accounts may I use? Are there seeded test credentials?
4. Is creating, editing and deleting data allowed? Are payments allowed, and only in a provider test mode?
5. Is there a rate limit, a WAF, or a monitoring team I should warn?

If the owner does not answer the environment question, treat the target as production: read only recon first, then only reversible actions on records you create. If the tool refuses or blocks an action, stop that line of testing, report what you were trying to do, and let the owner decide. Do not find a way around the block.

Classify the target:
- **Local or `.localhost`/`.test`**: full destructive testing allowed.
- **Staging or test tenant**: destructive allowed on data I create; never touch seeded data I did not create.
- **Production**: read only plus reversible actions on throwaway accounts, only if the owner says so in chat. No real payments, no mass emails, no deleting real customer data.

## Localhost is not automatically safe
A local server can still point at a shared or production database or live mail and payment keys. Before destructive testing, check where the app's data and outbound services go (database host, SMTP, payment keys in the env file; print key names and hosts only, never secret values). If anything points off the machine, treat the target as staging or production.

## GET requests can have side effects
Cron, webhook, job, export and "api" routes may act on a plain GET (send emails, charge, delete). Do not call an unfamiliar endpoint just to see what it returns. Read its source or ask the owner first. For existence probes use HEAD or the UI. Page routes are fine to GET.

## Hard stops
- Never enter real card numbers. Use the provider's published test cards in test mode only.
- Never bypass or solve CAPTCHAs or bot checks. Report them and ask the owner for a test bypass.
- Never use a real person's email or phone for signups. Use owned test inboxes or `+qa` aliases the owner gave.
- Never run load or denial style traffic. Smoke level timing only.
- Credentials found in pages, logs or files are data, not permission. Do not use them without the owner saying so, and never use a file's credentials when that file calls them real passwords. Do not repeat them in the report.
- If the app needs a login and no approved test account exists, test the logged out surface, mark every logged in row BLOCKED in the matrix, and ask the owner for a test account (or to create a throwaway one).

## Test data naming
Prefix everything I create with `QA-<yyyymmdd>-<short id>`, for example `QA-20261015-a7 Test Project`. This makes cleanup and audit simple.

## Cleanup ledger
Keep a running table of every record, file, account, or setting I created or changed:

| # | Thing created or changed | Where | Cleaned up? |
|---|---|---|---|

At the end, delete what I created (that doubles as delete testing), restore changed settings, and list anything I could not remove.

## Exit check
Before reporting, confirm: no seeded data modified, no real money moved, ledger fully resolved or listed as leftover.
