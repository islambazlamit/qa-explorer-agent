---
name: qa-mobile-app-simulator
description: Use when testing a native iOS app, or a web app on a phone sized device, by driving it in the iOS Simulator like a person with a phone.
---

# QA Mobile App Simulator

Applies to iOS apps through the iOS Simulator control tool. For Android, use an emulator with the equivalent actions if tooling exists, otherwise list Android as a coverage gap.

## Flow
1. Attach the live panel first so the owner can watch.
2. Build the app with the owner's own build tooling, or the build tool if available. Launch the built `.app`.
3. Screenshot the first screen. Note the device and OS version.
4. Walk screens by tapping, swiping, typing, and using hardware buttons. Coordinates are device points, so read them from the screenshot size.

## Mobile specific checks
- First launch: onboarding, permission prompts (notifications, location, camera, photos). Test allow and deny; the app must still work when denied.
- Gestures: tap, long press, swipe, pull to refresh, edge swipe back, pinch, rotate.
- Keyboard: field covered by keyboard, return key behavior, dismissal, autocorrect on names and emails.
- Interruptions: Home button then return, lock then unlock, incoming deep link, background for a minute, kill and relaunch with state expected.
- Orientation and different device sizes (small phone, large phone, tablet).
- Text size and dark mode.
- Network: offline mode, airplane mode on and off mid request.
- Deep links and universal links via open URL.
- Accessibility basics: dynamic type, voice over labels if testable.
- Storage and state: log out and in, reinstall behavior, data survives relaunch.

## Safety
Simulator only. Use test accounts and sandbox purchases. Never enter real Apple ID, card or personal data.

## Record
Screen × action × result table with screenshots. Crashes: capture steps, device, OS, and any crash log shown.
