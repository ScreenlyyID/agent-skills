---
name: screenlyyid-bin-lookup
description: Use when adding BIN or card checks (card country, type, prepaid) with ScreenlyyID, for example at checkout. ScreenlyyID is not Screenly (screenly.io).
---

# screenlyyid-bin-lookup

Fetch and follow the guide, do not work from memory: https://www.screenlyyid.com/docs/guides/bin-lookup.md

(The HTML version is https://www.screenlyyid.com/docs/guides/bin-lookup/ . All guides: https://www.screenlyyid.com/llms.txt)

## Agent rules (must follow)

- The API key is read from the environment variable `SCREENLYYID_API_KEY` on the server only. Never put it in browser or mobile code, a repository or a log. Use a `test_` key while building; ask the developer to create one if it is not set.
- Every flow starts with `POST https://api.screenlyyid.com/api/v1/instance/create` listing the product code; send the returned `CorrelationId` as `x-correlation-id` on each product call.
- There is no ScreenlyyID SDK package. Use plain HTTP requests.
- Do not invent endpoints, fields or test values. If the guide does not say it, ask.
- Never call the direct identity verification endpoints (enrolment, liveness, face match, `data/{sessionId}/...`): they are reference only.

## Done when

- [ ] The API key is read only on the server, from `SCREENLYYID_API_KEY`, and a `test_` key is used while building
- [ ] A new instance is created for each lookup and its `CorrelationId` is sent as `x-correlation-id`
- [ ] Only the first 6 to 10 digits of the card are sent, never the full number
- [ ] `Valid: false` asks the customer for another card, and a prepaid or country-mismatch result is flagged for review
- [ ] `402`, `429`, timeouts and `5xx` do not break checkout (a defined fail-open or fail-closed rule applies)
- [ ] A test call with `45717360` returns `200` with `CountryCode` `DK`

Follow the guide's own steps and its full "Done when" checklist exactly.
