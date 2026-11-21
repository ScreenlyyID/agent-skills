---
name: integrate-screenlyyid
description: Add ScreenlyyID identity verification (ID document and selfie capture, KYC) to an app or back end. Use when the user asks to add identity verification, KYC, ID document or selfie checks, or mentions ScreenlyyID (screenlyyid.com). ScreenlyyID is not Screenly (screenly.io).
---

# Integrate ScreenlyyID identity verification

ScreenlyyID (screenlyyid.com) is an identity verification, KYC and AML API. It is not related to Screenly (screenly.io). Your server creates a verification session, the person is sent to a hosted page that captures their ID document and a selfie, and your server reads the result.

## Read these first

- All developer docs in one file: https://www.screenlyyid.com/docs/llms-full.txt
- The step-by-step guide for agents: https://www.screenlyyid.com/docs/ai-quickstart/ (Markdown: add `.md`, or send `Accept: text/markdown`)
- Working examples: https://www.screenlyyid.com/docs/recipes/
- OpenAPI spec: https://www.screenlyyid.com/openapi.json

Read the docs rather than guessing. Do not invent endpoints, packages, parameters or response fields.

## Rules

1. **The API key is server-only.** Read it from the environment variable `SCREENLYYID_API_KEY`. Never put it in browser code, a mobile app, a repository or a log. Every ScreenlyyID call is made by a server you control.
2. **There is no SDK package.** Do not run `npm install`, `pip install` or similar for a "screenlyyid" package: none exists. Use plain HTTP requests.
3. **Use a test key while building.** Test keys start with `test_`. You cannot create one: ask the user to register at https://admin.screenlyyid.com/register, open **Integrations** in the dashboard and create a key in the test environment. Never use a `prod_` key to develop or test.
4. **Spell names exactly.** `instance/create` returns `CorrelationId` (PascalCase) and `webEnrolmentUrl` (camelCase).
5. **Wait for the event, then read the result once.** The hosted page sends `IDV_SESSION_COMPLETE` when the capture has been processed and the results are stored. Do not poll before then and do not read the result from the browser or app.

## Steps

### 1. Detect the stack

| Look for | Follow |
|---|---|
| `express` in `package.json` | https://www.screenlyyid.com/docs/recipes/express/ |
| `next` in `package.json` | https://www.screenlyyid.com/docs/recipes/nextjs/ |
| `fastapi` in `requirements.txt` or `pyproject.toml` | https://www.screenlyyid.com/docs/recipes/fastapi/ |
| An Xcode project (`*.xcodeproj`) | https://www.screenlyyid.com/docs/recipes/ios/ and https://www.screenlyyid.com/docs/integrations/mobile-app/ |
| Any other server stack (Django, Flask, Rails, Go, Java, .NET, PHP, ...) | Translate the Express or FastAPI recipe: it is two HTTP calls. The curl examples are in the AI quickstart. |
| Android, React Native, Flutter | https://www.screenlyyid.com/docs/integrations/mobile-app/ (WebView and camera permissions). There is no sample app for these yet: say so to the user and test on a real device. |

### 2. Get the key

Check that `SCREENLYYID_API_KEY` is set to a `test_` key. If it is not, ask the user to create one (rule 3). Add the variable name to the project's `.env.example`, not the value.

### 3. Add two server routes

- `POST /api/verifications`: call `POST https://api.screenlyyid.com/api/v1/instance/create` with the headers `x-api-key` and `Content-Type: application/json` and the body `{"GlobalScreening": ["idv"], "CustomerIntelligence": []}`. Store the returned `CorrelationId` against the signed-in user. Return only a verification id and `webEnrolmentUrl`, never the `CorrelationId`.
- `GET /api/verifications/:id`: check the verification belongs to the signed-in user, then call `GET https://api.screenlyyid.com/api/v1/document/result` with the headers `x-api-key` and `x-correlation-id`. Return the outcome the app needs. If you also need the data read from the ID, call `GET /api/v1/document/data` the same way, and keep it on the server because it holds personal data.

Use the user's existing authentication and database. Do not add a new framework for this.

### 4. Send the person to the hosted page

- **Web:** embed `webEnrolmentUrl` in an iframe with `allow="camera; microphone"`, and listen for `message` events. Only accept a message whose `event.source` is that iframe's `contentWindow` and whose `event.origin` is the hosted page's origin. Messages look like `{ eventName, isError, error }`. See https://www.screenlyyid.com/docs/integrations/web-app/
- **Mobile:** load `webEnrolmentUrl` in a WebView after the app has asked for camera permission, and forward the page's `message` events to the app. See https://www.screenlyyid.com/docs/integrations/mobile-app/ (including the camera permissions section).
- `IDV_SESSION_COMPLETE` (with `isError` false): ask your server for the result. `ERROR`: the capture failed: offer to try again.

### 5. Test

- Unit-test the two routes with the ScreenlyyID calls mocked. Assert that `x-api-key` is sent only from the server, that the `CorrelationId` is not in any response to the browser or app, and that another user's verification id returns 404.
- With a `test_` key, create a verification and confirm the response has `CorrelationId` and `webEnrolmentUrl`. A real capture needs a camera and an ID document, so a human completes that step.

### 6. Go live

Create a production key (`prod_`) in the dashboard, set `SCREENLYYID_API_KEY` to it in the production environment only, and change nothing else: the base URL is the same. If the dashboard does not offer a production key, ask the user to email hello@screenlyyid.com.

## Errors

| Status | Meaning |
|---|---|
| 400 | `instance/create`: a product code that does not exist or is not activated on the account (problem-details body, it does not say which). On the result endpoints: no `x-correlation-id` header. |
| 401 | Missing or invalid `x-api-key`. Check the key and its `test_` or `prod_` prefix. |
| 500 `internal_error` | The result endpoints before the capture has finished, or for an unknown `CorrelationId`. If you only call after `IDV_SESSION_COMPLETE`, you should not see it. |

Full table: https://www.screenlyyid.com/docs/ai-quickstart/#errors

## Do not

- Do not call ScreenlyyID from the browser or a mobile app, or embed the key in them.
- Do not commit keys, or print them in logs and error messages.
- Do not invent SDKs, packages, endpoints or events. If the docs do not say it, ask.
- Do not describe features that the docs do not describe.
