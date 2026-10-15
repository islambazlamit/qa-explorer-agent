---
name: qa-form-and-input-validation
description: Use when testing any form or input field, including signup, checkout, search, settings, comments and contact forms.
---

# QA Form and Input Validation

For every field, pick values from classes, not guesses. Benign probes only; this is not a penetration test.

## Value classes per field
- Valid typical. Valid at minimum and maximum length. One below min, one above max.
- Empty, spaces only, leading and trailing spaces, tabs, line breaks.
- Wrong type: letters in number fields, negative, zero, decimals, very large, scientific notation.
- Unicode: accents, right to left text, CJK, emoji, zero width characters.
- Symbols and markup: `<b>x</b>`, `"` and `'`, `&`, `%`, backslash, a harmless `<script>` text probe, a quote character that could break SQL. Expect safe display, never execution.
- Dates: past, future, today, leap day, month ends, invalid dates, locale formats.
- Email: missing `@`, double `@`, plus tags, long domains, uppercase.
- Phone, postal code, URL, card test numbers: valid, invalid, wrong country.
- Passwords: too short, no number, common, with spaces, very long, paste in confirmation.

## Behaviors per form
- Required vs optional markers match real behavior.
- Inline validation timing (on blur, on submit), message clarity, message placed near the field, focus moves to first error.
- Error text is human, specific, and does not leak internals.
- Submit with nothing, with one bad field, with all bad. Fix and resubmit.
- Double submit, Enter key submit, submit during slow network.
- Autofill, paste, browser back after submit, refresh after submit (resubmission prompt?).
- Disabled and loading states of the submit button. Success confirmation. Data persists correctly.
- Maxlength truncation silently losing data. Character counters accurate.
- Selects, radios, checkboxes, date pickers, sliders, file inputs, rich text editors, autocomplete, tag inputs.

## Record
Field, value class, result, expected vs actual, evidence. Bugs go to the bug report skill format.
