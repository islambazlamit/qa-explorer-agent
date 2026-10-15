---
name: qa-responsive-and-browser-matrix
description: Use when checking that a website or app works across screen sizes, orientations, themes and browsers.
---

# QA Responsive and Browser Matrix

Layout bugs hide at specific widths. Test the main flows, not only the home page.

## Viewports
320 (small phone), 375 (phone), 768 (tablet), 1024 (small laptop), 1440 (desktop), 1920 (wide). Use resize tools or device emulation. Also test portrait and landscape on phone and tablet sizes.

## Per viewport check
- No horizontal scroll, no cut off text, no overlapping elements, no unreachable buttons.
- Navigation collapses correctly; menu opens, closes, traps focus when it should.
- Tap targets at least 44 by 44 px on touch sizes.
- Tables, charts, code blocks, long words and URLs wrap or scroll in their own container.
- Images scale, keep aspect, have no layout shift.
- Modals and sticky bars fit; the on screen keyboard does not hide the focused field (use real device or simulator when possible).
- Forms usable: correct keyboard type (`email`, `tel`, `number`), autocomplete attributes.

## Preferences and conditions
- Zoom 200 percent and text size increase.
- Dark mode and light mode; contrast on both; images and logos on dark backgrounds.
- Reduced motion; animations stop or soften.
- Print view for pages likely to be printed (invoices, receipts).
- Slow network and CPU throttling where tooling allows.
- Language and right to left layout if the product supports them.

## Browsers
Test in whatever is available: Chromium, Firefox, WebKit (Playwright can run all three). If only one browser exists, say so in the report as a coverage gap, do not claim cross browser results.

## Record
Page × viewport grid of PASS/FAIL, with a screenshot for every FAIL and one clean reference for each key page.
