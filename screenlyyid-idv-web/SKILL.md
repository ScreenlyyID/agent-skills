---
name: screenlyyid-idv-web
description: Use when adding identity verification (ID document and selfie) to a web app with ScreenlyyID. ScreenlyyID is not Screenly (screenly.io).
---

# screenlyyid-idv-web

Fetch and follow the guide, do not work from memory: https://www.screenlyyid.com/docs/guides/identity-verification-web.md

(The HTML version is https://www.screenlyyid.com/docs/guides/identity-verification-web/ . All guides: https://www.screenlyyid.com/llms.txt)

## Agent rules (must follow)

- MUST create the instance on the server (`POST /api/v1/instance/create`, `GlobalScreening: ["idv"]`) and store the `CorrelationId` per user.
- MUST NOT put the API key in the app or browser, and MUST NOT call the result APIs from the app or browser: only the back end calls `GET /api/v1/document/data` and `GET /api/v1/document/result`.
- MUST use the exact event names `IDV_SESSION_COMPLETE` and `ERROR` with the `{eventName, isError, error}` format, and treat `ERROR` as a restartable failure.
- MUST NOT build a custom camera, upload, liveness or face-match flow, and MUST NOT call the direct identity verification endpoints (enrolment, liveness, face match, `data/{sessionId}/...`) even if asked: they are reference only.
- MUST NOT call the result endpoints before `IDV_SESSION_COMPLETE`, and MUST NOT poll. Do not invent an SDK: there is none.
- Use a `test_` key; ask the developer to create one. The API key is read from `SCREENLYYID_API_KEY` on the server only.
- MUST embed `webEnrolmentUrl` in an iframe with `allow="camera; microphone"` and check `event.source` and `event.origin` on every message.

## Done when

- [ ] The instance is created on the server and the API key never reaches the browser
- [ ] The `webEnrolmentUrl` is embedded in an iframe with `allow="camera; microphone"` over HTTPS
- [ ] The message handler checks `event.source` and `event.origin` and handles `IDV_SESSION_COMPLETE` and `ERROR` once
- [ ] On `IDV_SESSION_COMPLETE` the server calls `document/data` and `document/result` once
- [ ] `HIGH`, `MEDIUM`, `LOW` map to approved, review, rejected; `ERROR` restarts the flow
- [ ] Verified in a browser with a `test_` key

Follow the guide's own steps and its full "Done when" checklist exactly.
