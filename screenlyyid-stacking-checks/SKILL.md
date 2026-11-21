---
name: screenlyyid-stacking-checks
description: Use when running several ScreenlyyID checks (BIN, phone, email, address, IP, eIDV, AML, identity verification) for one customer under one correlation id.
---

# screenlyyid-stacking-checks

Fetch and follow the guide, do not work from memory: https://www.screenlyyid.com/docs/guides/stacking-checks.md

(The HTML version is https://www.screenlyyid.com/docs/guides/stacking-checks/ . All guides: https://www.screenlyyid.com/llms.txt)

## Agent rules (must follow)

- The API key is read from the environment variable `SCREENLYYID_API_KEY` on the server only. Never put it in browser or mobile code, a repository or a log. Use a `test_` key while building; ask the developer to create one if it is not set.
- Every flow starts with `POST https://api.screenlyyid.com/api/v1/instance/create` listing the product code; send the returned `CorrelationId` as `x-correlation-id` on each product call.
- There is no ScreenlyyID SDK package. Use plain HTTP requests.
- Do not invent endpoints, fields or test values. If the guide does not say it, ask.
- Never call the direct identity verification endpoints (enrolment, liveness, face match, `data/{sessionId}/...`): they are reference only.
- Each product call is separate and shares the `CorrelationId`; there is no combined result endpoint: combine the per-product outcomes in your own code. A repeated product call with the same id can return `409`: create a new instance per check.

## Done when

- [ ] One instance lists every product used and one `CorrelationId` is sent on every call
- [ ] Each product is called once per instance
- [ ] A failed or timed-out check is handled by an explicit fail rule and never silently allowed
- [ ] Per-product outcomes are combined into one decision; AML `incomplete` is never treated as clear
- [ ] Any stack that includes `idv` follows the identity verification guide for the capture
- [ ] Per-product results are stored against the `CorrelationId`

Follow the guide's own steps and its full "Done when" checklist exactly.
