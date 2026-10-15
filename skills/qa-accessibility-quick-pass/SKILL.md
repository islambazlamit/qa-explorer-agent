---
name: qa-accessibility-quick-pass
description: Use when you need a fast human style accessibility check during end to end testing; escalate to a full WCAG audit.
---

# QA Accessibility Quick Pass

This is a smoke level pass for a tester. For a formal audit, hand off to an accessibility specialist or a full WCAG audit tool.

## Keyboard only run
Put the mouse aside. On every key page:
- Tab reaches every interactive element in a logical order; Shift+Tab reverses.
- Focus is always visible and never trapped (except inside an open modal, where Escape must release it).
- Skip link present and works.
- Enter/Space activate buttons; arrows work in menus, tabs, radios, selects.
- After closing a modal or menu, focus returns to its trigger.

## Semantics and labels
- One `h1`, headings in order, landmarks (`header`, `nav`, `main`, `footer`).
- Every input has a visible label tied to it; placeholders are not labels.
- Buttons and links have meaningful names; icon buttons have aria labels.
- Images: informative ones have alt text, decorative ones empty alt.
- Page `title` and `lang` set and updated on navigation.

## Visual
- Text contrast at least 4.5:1 (3:1 for large text and UI borders).
- Information is not color only (errors, charts, status).
- Layout works at 200 percent zoom.
- Motion: auto playing media has pause; no flashing content.

## Dynamic content
Errors are announced and linked to fields. Toasts and live regions use `aria-live` or equivalent. Loading and success states are communicated.

## Tools
Run an automated scan (axe, Lighthouse) if available, then confirm findings by hand. Automated tools find only part of the problems; say so in the report.

## Record
Issue, element, WCAG criterion if known, evidence, severity.
