---
name: screenlyyid-device-ip
description: Use when checking IP addresses, VPN or proxy use, blocklists, host reputation or user agents with ScreenlyyID. ScreenlyyID is not Screenly (screenly.io).
---

# screenlyyid-device-ip

Fetch and follow the guide, do not work from memory: https://www.screenlyyid.com/docs/guides/device-ip-intelligence.md

(The HTML version is https://www.screenlyyid.com/docs/guides/device-ip-intelligence/ . All guides: https://www.screenlyyid.com/llms.txt)

## Agent rules (must follow)

- The API key is read from the environment variable `SCREENLYYID_API_KEY` on the server only. Never put it in browser or mobile code, a repository or a log. Use a `test_` key while building; ask the developer to create one if it is not set.
- Every flow starts with `POST https://api.screenlyyid.com/api/v1/instance/create` listing the product code; send the returned `CorrelationId` as `x-correlation-id` on each product call.
- There is no ScreenlyyID SDK package. Use plain HTTP requests.
- Do not invent endpoints, fields or test values. If the guide does not say it, ask.
- Never call the direct identity verification endpoints (enrolment, liveness, face match, `data/{sessionId}/...`): they are reference only.

## Done when

- [ ] The API key is read only on the server
- [ ] The client IP comes from your server (the real remote address or your proxy's trusted header), never from a field the browser can set
- [ ] A new instance is created per check and its `CorrelationId` is sent
- [ ] VPN and proxy results lead to review, not an automatic block
- [ ] Errors and timeouts do not break sign-up or checkout
- [ ] A test call returns `200` with a `test_` key

Follow the guide's own steps and its full "Done when" checklist exactly.
