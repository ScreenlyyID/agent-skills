---
name: screenlyyid-aml-screening
description: Use when adding AML, sanctions or PEP screening of a person or entity with ScreenlyyID.
---

# screenlyyid-aml-screening

Fetch and follow the guide, do not work from memory: https://www.screenlyyid.com/docs/guides/aml-screening.md

(The HTML version is https://www.screenlyyid.com/docs/guides/aml-screening/ . All guides: https://www.screenlyyid.com/llms.txt)

## Agent rules (must follow)

- The API key is read from the environment variable `SCREENLYYID_API_KEY` on the server only. Never put it in browser or mobile code, a repository or a log. Use a `test_` key while building; ask the developer to create one if it is not set.
- Every flow starts with `POST https://api.screenlyyid.com/api/v1/instance/create` listing the product code; send the returned `CorrelationId` as `x-correlation-id` on each product call.
- There is no ScreenlyyID SDK package. Use plain HTTP requests.
- Do not invent endpoints, fields or test values. If the guide does not say it, ask.
- Never call the direct identity verification endpoints (enrolment, liveness, face match, `data/{sessionId}/...`): they are reference only.

## Done when

- [ ] The API key is read only on the server
- [ ] The instance lists `aml` in `GlobalScreening` and the `CorrelationId` is sent as `x-correlation-id`
- [ ] `potential_match` stops the flow and creates a manual review, never an automatic rejection
- [ ] `incomplete` is retried or reviewed, never treated as `clear`
- [ ] The full response is stored as the audit record
- [ ] A test call with the example above returns `potential_match`

Follow the guide's own steps and its full "Done when" checklist exactly.
