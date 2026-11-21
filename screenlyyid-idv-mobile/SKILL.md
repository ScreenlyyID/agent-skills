---
name: screenlyyid-idv-mobile
description: Use when adding identity verification (ID document and selfie) to an iOS, Android, React Native or Flutter app with ScreenlyyID.
---

# screenlyyid-idv-mobile

Fetch and follow the guide, do not work from memory: https://www.screenlyyid.com/docs/guides/identity-verification-mobile.md

(The HTML version is https://www.screenlyyid.com/docs/guides/identity-verification-mobile/ . All guides: https://www.screenlyyid.com/llms.txt)

## Agent rules (must follow)

- MUST create the instance on the server (`POST /api/v1/instance/create`, `GlobalScreening: ["idv"]`) and store the `CorrelationId` per user.
- MUST NOT put the API key in the app or browser, and MUST NOT call the result APIs from the app or browser: only the back end calls `GET /api/v1/document/data` and `GET /api/v1/document/result`.
- MUST use the exact event names `IDV_SESSION_COMPLETE` and `ERROR` with the `{eventName, isError, error}` format, and treat `ERROR` as a restartable failure.
- MUST NOT build a custom camera, upload, liveness or face-match flow, and MUST NOT call the direct identity verification endpoints (enrolment, liveness, face match, `data/{sessionId}/...`) even if asked: they are reference only.
- MUST NOT call the result endpoints before `IDV_SESSION_COMPLETE`, and MUST NOT poll. Do not invent an SDK: there is none.
- Use a `test_` key; ask the developer to create one. The API key is read from `SCREENLYYID_API_KEY` on the server only.

## Done when

- [ ] The instance is created on the server, the `CorrelationId` stored per user, and the API key is nowhere in the app
- [ ] The app asks for camera permission before loading the page
- [ ] The `webEnrolmentUrl` opens in a WebView with JavaScript, inline media and camera access
- [ ] The app receives `IDV_SESSION_COMPLETE` and `ERROR` (exact names) and acts once
- [ ] On `IDV_SESSION_COMPLETE` the back end calls `document/data` and `document/result` once with `x-api-key` and `x-correlation-id`
- [ ] `HIGH`, `MEDIUM`, `LOW` map to approved, review, rejected; `ERROR` restarts the flow
- [ ] Verified on a real device with a `test_` key

Follow the guide's own steps and its full "Done when" checklist exactly.
