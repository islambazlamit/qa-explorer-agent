---
name: qa-human-interaction-sweep
description: Use when exercising a page or screen the way a curious human would, touching every clickable, typeable, draggable and scrollable thing.
---

# QA Human Interaction Sweep

Act like a person who is new to the product, impatient, and a little careless.

## Per page loop
1. Load fresh. Note first paint, layout shifts, visible errors.
2. Scroll top to bottom slowly. Check sticky headers, lazy loading, back to top, infinite scroll end.
3. Click every link (note where it goes, whether it opens a new tab, whether the label matches).
4. Click every button, including icon only ones. Read tooltips and aria labels.
5. Open every menu, dropdown, accordion, tab, carousel, modal, drawer, popover. Close each three ways: close button, Escape, click outside.
6. Toggle every switch, checkbox, radio, slider. Confirm the change persists and the label state matches.
7. Hover for tooltips and menus. Focus with Tab; confirm focus is visible.
8. Right click, double click, long press on mobile where plausible.
9. Drag and drop, reorder, resize, crop, draw if offered.
10. Media: play, pause, seek, mute, fullscreen, captions.
11. Copy and paste into and out of fields. Select all. Undo and redo.
12. Keyboard: Enter, Space, arrows, Escape, Tab and Shift+Tab, shortcuts shown in the UI.

## Human behaviors to inject
- Double click a submit button. Click while loading. Click rapidly.
- Press Back, Forward, Refresh in the middle of a flow. Reopen the URL in a new tab.
- Resize the window while a modal is open.
- Leave a form half filled and navigate away; check for a warning and for lost data.
- Use the app at a slow pace (wait 2 minutes mid flow) then continue.

## Tool tips
- Take a screenshot before any coordinate click, and use the coordinate frame the tool reports. Prefer `find` or refs for buttons. After resizing the window, take a new screenshot first.
- A click by element reference can land on a stale position after scrolling. Scroll the element into view, take a screenshot, then click by the coordinates shown. For simple buttons a script `.click()` is a fair fallback; set React inputs with the native value setter and dispatch `input` and `change` events.
- If a click seems to do nothing, check for an overlay (open sidebar, modal) before calling it a bug, then retry once.
- Test at the pane's real width and at a wide desktop width; some controls only break at narrow widths.

## What to watch on each action
Visible feedback (spinner, toast, disabled state), correct result, URL change, title change, console errors, failed network calls, layout break, text overflow or truncation, typos, dead ends, broken images.

## Record
For each control: action, result, evidence. Update the coverage matrix as you go.
