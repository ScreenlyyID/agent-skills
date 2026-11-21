---
name: screenlyyid-phone-validation
description: Use when validating phone numbers at sign-up or checkout with ScreenlyyID. ScreenlyyID is not Screenly (screenly.io).
---

# screenlyyid-phone-validation

Fetch and follow the guide, do not work from memory: https://www.screenlyyid.com/docs/guides/phone-validation.md

(The HTML version is https://www.screenlyyid.com/docs/guides/phone-validation/ . All guides: https://www.screenlyyid.com/llms.txt)

## Agent rules (must follow)

- The API key is read from the environment variable `SCREENLYYID_API_KEY` on the server only. Never put it in browser or mobile code, a repository or a log. Use a `test_` key while building; ask the developer to create one if it is not set.
- Every flow starts with `POST https://api.screenlyyid.com/api/v1/instance/create` listing the product code; send the returned `CorrelationId` as `x-correlation-id` on each product call.
- There is no ScreenlyyID SDK package. Use plain HTTP requests.
- Do not invent endpoints, fields or test values. If the guide does not say it, ask.
- Never call the direct identity verification endpoints (enrolment, liveness, face match, `data/{sessionId}/...`): they are reference only.

## Done when

- [ ] The API key is read only on the server, from `SCREENLYYID_API_KEY`
- [ ] A new instance is created per check and its `CorrelationId` is sent as `x-correlation-id`
- [ ] `Valid: false` blocks the form with a message that lets the customer fix the number
- [ ] `InternationalNumber` is what you store
- [ ] Errors and timeouts do not block sign-up (a defined fallback applies)
- [ ] A test call returns `200` with a `test_` key

Follow the guide's own steps and its full "Done when" checklist exactly.
