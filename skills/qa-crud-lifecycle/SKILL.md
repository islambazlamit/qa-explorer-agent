---
name: qa-crud-lifecycle
description: Use when a feature lets users create, view, edit, delete or manage records, files or settings and each stage of the record life must be proven.
---

# QA CRUD Lifecycle

Run each data feature through its whole life. Use the `QA-` name prefix from the safety skill.

## Create
- Minimal valid record, then a maximal one (all optional fields, longest allowed text).
- Duplicate name or unique field. Missing required fields. Cancel mid way.
- File and image upload: allowed types, wrong type, empty file, oversized file, odd file names (spaces, unicode), several files, replace, remove.
- After create: confirmation, correct redirect, record appears in lists, counts update.

## Read
- Detail view matches what was entered (casing, whitespace, line breaks, dates, timezones, currency, numbers).
- Lists: empty state, one item, many items, pagination edges (first, last, beyond last), page size change.
- Search: exact, partial, case, no match, special characters. Sort: each column both directions, stable ties. Filter: each filter alone and combined, reset.

## Update
- Edit one field, edit all fields, clear an optional field.
- Save, then reload, then re-login: values persist.
- Edit in two tabs: last write wins or conflict warning?
- Inline edit, bulk edit, status transitions allowed and not allowed.

## Delete
- Confirmation dialog present? Cancel keeps the record. Confirm removes it.
- Gone from lists, search, counts, related records, direct URL (shows a proper not found).
- Undo or trash/restore if offered. Delete a record that others depend on. Bulk delete, select all.

## Practical tips
- Native `confirm()` dialogs are invisible to screenshots. Before clicking delete, wrap `window.confirm` to record the message and return false (test cancel), then true (test confirm). Report vague messages such as a bare "Confirm?" that do not name the record.
- After create, edit and delete, check the list updates without a manual reload (wait a few seconds), then reload and check again. A stale list is a bug.
- Icon only buttons (pencil, trash) need an accessible name; check `aria-label` or `title`.
- Check that action columns are reachable at narrow widths, not clipped by `overflow: hidden`.
- Re-read the row text before every click on a row action so you never act on a seeded record.
- Typed text is safest checked through the DOM (`textContent`, no injected tags) and after reload.

- When a save "does nothing", look for a message at the top of the page and at other scroll positions, check the network tab for a request, and reload to see whether anything persisted. Report a silent failure as a bug even when the cause is bad test data.
- Before creating test data, check that a delete or deactivate path exists. If none does, create at most one record, tell the owner it needs manual cleanup, and list it as a leftover in the ledger.
- If a flow needs a prerequisite record (customer for an invoice, employee for payroll), test how the user can create that prerequisite first.
- Required fields should also be tested against a fresh dropdown or select with no options (empty data states).

## Cross feature
Import and export (round trip equals), copy/duplicate, share links, permissions on each action, notifications triggered, audit trail or history.

## Close out
Whatever remains from the cleanup ledger gets deleted through the UI last, which doubles as one more delete test.
