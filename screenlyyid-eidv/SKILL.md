---
name: screenlyyid-eidv
description: Use when adding electronic identity verification (eIDV, 1x1 or 2x2, no document or selfie) with ScreenlyyID. ScreenlyyID is not Screenly (screenly.io).
---

# screenlyyid-eidv

Fetch and follow the guide, do not work from memory: https://www.screenlyyid.com/docs/guides/electronic-identity-verification.md

(The HTML version is https://www.screenlyyid.com/docs/guides/electronic-identity-verification/ . All guides: https://www.screenlyyid.com/llms.txt)

## Agent rules (must follow)

- The API key is read from the environment variable `SCREENLYYID_API_KEY` on the server only. Never put it in browser or mobile code, a repository or a log. Use a `test_` key while building; ask the developer to create one if it is not set.
- Every flow starts with `POST https://api.screenlyyid.com/api/v1/instance/create` listing the product code; send the returned `CorrelationId` as `x-correlation-id` on each product call.
- There is no ScreenlyyID SDK package. Use plain HTTP requests.
- Do not invent endpoints, fields or test values. If the guide does not say it, ask.
- Never call the direct identity verification endpoints (enrolment, liveness, face match, `data/{sessionId}/...`): they are reference only.

## Done when

- [ ] The API key is read only on the server
- [ ] The instance lists `eidv1` or `eidv2` in `GlobalScreening` and the `CorrelationId` is sent as `x-correlation-id`
- [ ] The right endpoint is called for the product listed (1x1 for `eidv1`, 2x2 for `eidv2`)
- [ ] The decision uses `Codes`, and a non-match leads to step-up or review, not an automatic rejection
- [ ] No document or selfie capture is built: that only goes through the hosted flow
- [ ] A test call returns `200` with a `test_` key

Follow the guide's own steps and its full "Done when" checklist exactly.
