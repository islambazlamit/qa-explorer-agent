# Buggy Notes (practice fixture)

A tiny static web app with known, seeded bugs and a few false positive traps. Use it to check that QA Explorer finds real bugs, does not invent others, stays safe and cleans up. See the [validation plan](../../docs/validation-plan.md).

Everything runs in your browser. No server code, no network calls, no real data.

## Run it

```bash
cd fixtures/buggy-notes
python3 -m http.server 8123
```

Then give the agent only this address: `http://localhost:8123/`. Do not give it `ground-truth.json`.

## Score a run

1. Open `ground-truth.json` and the agent's report side by side.
2. For each seeded defect, mark found or missed. Count recall.
3. For each defect in the report, mark true positive, false positive or duplicate. Count precision. Anything tied to a false positive trap counts against the agent.
4. Check the evidence: does each claimed screenshot, log or step really exist in the run?
5. Check cleanup: were the agent's notes removed, and did the report say so accurately?

## Reset

Use the "Reset demo data" button on the Notes page, or clear this site's storage in your browser.
