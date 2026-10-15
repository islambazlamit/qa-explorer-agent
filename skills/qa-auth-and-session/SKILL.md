---
name: qa-auth-and-session
description: Use when an app has accounts, login, roles or private areas that need testing as a real user would use them.
---

# QA Auth and Session

Use only test accounts approved in the safety gate. Never type a real person's password. If sign in needs a real password or a CAPTCHA, stop and ask the owner for a test account or bypass.

## Signup
Happy path, existing email, weak password, mismatched confirmation, terms checkbox, email verification link (valid, used twice, expired), welcome state, redirect.

## Login
Correct credentials, wrong password, unknown user (message should not reveal which), empty fields, case in email, whitespace, remember me, social login buttons (note only whether they start, do not use real identities), lockout after repeated failures (warn the owner before triggering).

## Logout and session
- Logout clears access: Back button must not reveal private pages.
- Session persists across refresh and new tab; ends after the stated timeout.
- Login in two tabs, logout in one, act in the other.
- Expired session during a form submit: is data lost, is the user sent to login, then returned?

## Password reset and account
Request reset (known and unknown email show the same message), link single use, link expiry, new password rules, old password stops working. Change email, change password, delete account, export data, two factor setup if present (use the owner's test method).

## Authorization
- Open private URLs while logged out: redirect to login, then back to the intended page.
- Each role: can see and do exactly what it should. Try admin URLs as a normal user. Try another user's record URL by changing the ID in the address bar (read only check).
- UI hides what the server also blocks: note where the button is hidden but the direct URL works.

## Record
Role × feature table with expected and actual access, plus evidence for each denial and success.
