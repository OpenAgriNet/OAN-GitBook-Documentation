# Type A

## BharatVistaar Type A Integration Guide

BV Type A can be implemented in two ways: **iFrame embedding** or **REST API integration**.

The iFrame approach is the quicker option, as it reuses Vistaar's own hosted chat interface instead of the State building a front-end on their own. This frontend is allowed to be white-labelled as well based on the State's requirements provided there is a UI element mentioning **"Powered by BharatVistaar"**.

***

### iFrame Integration

An iFrame (inline frame) is an HTML element that loads one web page inside another. The embedded page runs in its own isolated context but appears seamlessly within ours. Here, it loads Vistaar's live chat from `chat-vistaar.da.gov.in` into a panel on the State's website / app page — the user gets the full Vistaar experience without leaving the State site, and without any back-end or UI work.

#### Pre-requisite — URL whitelisting (required on both ends)

For the frame to load, access must be permitted on both sides:

* **On the State's end** — add `chat-vistaar.da.gov.in` to the `frame-src` directive of the state's Content Security Policy, otherwise the browser blocks it.
* **On BharatVistaar's end** — it must allow the State's domain in its `frame-ancestors` (or `X-Frame-Options`), otherwise the page refuses to be embedded.

Hence, the state must share the application URL that can be whitelisted at BV's end.

#### Code snippet

html

```html
<iframe
  src="https://chat-vistaar.da.gov.in/chat"
  width="100%"
  height="600"
  style="border:none;"
  title="Vistaar Chat"
  allow="clipboard-write; microphone; camera"
></iframe>
```

***

### REST API Integration

> **client\_code**: A unique identifier assigned to your organization when you onboard (use this value on every token request).

#### 1. Environment base URLs

All paths below are appended to the API base (no trailing slash). API prefix is `/api`.

| Environment | API base URL                    |
| ----------- | ------------------------------- |
| Development | `https://dev-vistaar.da.gov.in` |
| Production  | `https://vistaar.da.gov.in`     |

| Capability | Development                                                 | Production                                              |
| ---------- | ----------------------------------------------------------- | ------------------------------------------------------- |
| Issue JWT  | `POST https://dev-vistaar.da.gov.in/api/token/api-key`      | `POST https://vistaar.da.gov.in/api/token/api-key`      |
| Chat (SSE) | `GET https://dev-vistaar.da.gov.in/api/chat/`               | `GET https://vistaar.da.gov.in/api/chat/`               |
| Feedback   | `POST https://dev-vistaar.da.gov.in/api/telemetry/feedback` | `POST https://vistaar.da.gov.in/api/telemetry/feedback` |
| Error      | `POST https://dev-vistaar.da.gov.in/api/telemetry/error`    | `POST https://vistaar.da.gov.in/api/telemetry/error`    |

> Question and answer telemetry are sent by the API when chat runs. Do **not** call a separate observability URL from the app.

***

#### 2. API key

**Requesting your API key**

API keys are not self-service. To integrate, contact the Bharat OAN platform team and request credentials for your organization.

When you onboard, we will:

1. Register your organization as a provider
2. Assign a unique `client_code` for your app or service
3. Issue a unique, static API key for the authentication endpoints — one key per provider, per environment (development and production are separate)

Send your request to the platform team with:

| Detail                       | Description                                                                  |
| ---------------------------- | ---------------------------------------------------------------------------- |
| Organization / provider name | Legal or product name of your organization                                   |
| App or service name          | Name of the application integrating with Bharat OAN                          |
| Contact email                | Technical point of contact for credential delivery and support               |
| Environment(s) needed        | Development, production, or both                                             |
| Expected usage               | Brief description of the integration (mobile app, web portal, gateway, etc.) |

We will provision credentials and share them through a secure channel. **Do not** share your API key publicly or commit it to source control.

**Using your API key**

* Send it as header `X-API-Key` on `POST /api/token/api-key` **only**
* Keep it on a trusted backend (e.g. your gateway). Do not embed it in a public app build
* Typical pattern: `client app → your backend → Bharat OAN API with X-API-Key`
* Each provider receives a dedicated static key — keys are not shared across organizations

***

#### 3. Authentication — short-lived JWT (API key)

**Endpoint**

```
POST /api/token/api-key
Content-Type: application/json
X-API-Key: <your-api-key>
```

**Request body**

| Field            | Required | Description                                                                |
| ---------------- | -------- | -------------------------------------------------------------------------- |
| `client_code`    | Yes      | Your assigned provider code                                                |
| `fingerprint_id` | Yes      | Device / install id, or a unique user id when no device fingerprint exists |
| `user_id`        | No       | Optional end-user id when different from the device id                     |
| `name`           | No       | Display name                                                               |
| `role`           | No       | Role                                                                       |
| `metadata`       | No       | Extra object or JSON string (app version, region, etc.)                    |

**Example (production)**

bash

```bash
curl -s -X POST "https://vistaar.da.gov.in/api/token/api-key" \
  -H "Content-Type: application/json" \
  -H "X-API-Key: <your-api-key>" \
  -d '{
    "client_code": "<your-client-code>",
    "fingerprint_id": "fp_device_abc123",
    "user_id": "user_456",
    "metadata": { "app_version": "1.0.0", "platform": "android" }
  }'
```

**Response**

json

```json
{
  "token": "<JWT>",
  "expires_in": 900
}
```

* JWT is RS256, typically valid \~15 minutes (`expires_in` in seconds, e.g. 900)
* Refresh by calling `/api/token/api-key` again before expiry
* Use on every subsequent call: `Authorization: Bearer <JWT>`

**Common errors**

| HTTP | Meaning                                              |
| ---- | ---------------------------------------------------- |
| 401  | Wrong or missing API key, or invalid/expired JWT     |
| 400  | Missing `client_code` or invalid body                |
| 500  | Token service unavailable — contact platform support |

***

#### 4. Correlation IDs (use across chat + telemetry)

| ID           | Who generates                                              | Used for                                                      |
| ------------ | ---------------------------------------------------------- | ------------------------------------------------------------- |
| `session_id` | UI (once per conversation, e.g. UUID; reuse on every turn) | Chat history and telemetry — required on chat and relays      |
| `qid`        | API (one per chat request)                                 | Same id on chat, feedback, and error for that turn — required |

The API assigns a new `qid` for every `GET /api/chat/` call. The client does **not** generate or send `qid` on chat — it comes from the chat response (`X-QID`).

After you open the chat SSE request, read the response header:

```
X-QID: <uuid>
```

The API also sends `Access-Control-Expose-Headers: X-QID` so browser clients can read this header cross-origin.

**You must:**

1. Read `X-QID` as soon as the chat response starts (before or while consuming the SSE stream).
2. Store that value for the current turn.
3. Pass the same `qid` (and `session_id`) to `POST /api/telemetry/feedback` and `POST /api/telemetry/error` for that question.

> If feedback or error uses a different `qid` than the chat turn, telemetry will not correlate in dashboards.

***

#### 5. Chat

**Endpoint**

```
GET /api/chat/
Authorization: Bearer <JWT>
```

**Query parameters**

| Parameter     | Required | Default | Description                                                     |
| ------------- | -------- | ------- | --------------------------------------------------------------- |
| `query`       | Yes      | —       | User message (URL-encoded)                                      |
| `session_id`  | Yes      | —       | Generate in the UI (e.g. UUID); reuse for the same conversation |
| `source_lang` | No       | `hi`    | Source language code                                            |
| `target_lang` | No       | `hi`    | Response language code                                          |
| `latitude`    | No       | —       | Optional GPS                                                    |
| `longitude`   | No       | —       | Optional GPS                                                    |

> Do not send `qid` on chat. Use the `X-QID` header returned by the API (see §4).

**Response headers**

| Header                          | Description                                            |
| ------------------------------- | ------------------------------------------------------ |
| `Content-Type`                  | `text/event-stream`                                    |
| `X-QID`                         | Question id for this turn — use for feedback and error |
| `Access-Control-Expose-Headers` | Includes `X-QID` for browser clients                   |

**Response body**

Streamed text until complete.

* Do **not** send question/answer telemetry from the client — the API handles it
* End-user identity for telemetry comes from the JWT (set at token time via `fingerprint_id` and optional `user_id`)

**Example (development)**

bash

```bash
TOKEN="<JWT>"
SESSION="$(uuidgen)"

# -D - saves response headers so you can read X-QID
curl -N -D /tmp/chat-headers.txt \
  "https://dev-vistaar.da.gov.in/api/chat/?query=$(python3 -c 'import urllib.parse; print(urllib.parse.quote("your question here"))')&session_id=${SESSION}&source_lang=hi&target_lang=hi" \
  -H "Authorization: Bearer ${TOKEN}"

QID="$(grep -i '^x-qid:' /tmp/chat-headers.txt | awk '{print $2}' | tr -d '\r')"
echo "Use qid=${QID} for feedback and error on this turn"
```

> **Browser / mobile note:** With `fetch` or your HTTP client, read `response.headers.get('X-QID')` when the response is available, then consume the SSE body. With native clients, read `X-QID` from response headers before streaming.

**Image in chat (optional)**

Upload via `POST /api/image/upload`, then include the returned `image_id` in the chat query. Use the same Bearer token.

***

#### 6. Feedback

**Endpoint**

```
POST /api/telemetry/feedback
Authorization: Bearer <JWT>
Content-Type: application/json
```

**Request body**

| Field           | Required | Description                                                |
| --------------- | -------- | ---------------------------------------------------------- |
| `qid`           | Yes      | Same `qid` from `X-QID` on the chat response for this turn |
| `session_id`    | Yes      | Same `session_id` as chat                                  |
| `feedback_type` | Yes      | e.g. `like`, `dislike`                                     |
| `feedback_text` | No       | Free text                                                  |
| `question_text` | No       | Original question (recommended)                            |
| `answer_text`   | No       | Reply shown to the user                                    |

**Example**

bash

```bash
curl -s -X POST "https://dev-vistaar.da.gov.in/api/telemetry/feedback" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "qid": "'"${QID}"'",
    "session_id": "'"${SESSION}"'",
    "feedback_type": "like",
    "feedback_text": "",
    "question_text": "your question here",
    "answer_text": "<assistant reply>"
  }'
```

**Response `200`**

json

```json
{ "status": "accepted" }
```

***

#### 7. Error reporting

For client-side failures (network, UI, parsing). Failures during chat on the server are recorded by the API.

**Endpoint**

```
POST /api/telemetry/error
Authorization: Bearer <JWT>
Content-Type: application/json
```

**Request body**

| Field           | Required | Description                                                |
| --------------- | -------- | ---------------------------------------------------------- |
| `qid`           | Yes      | Same `qid` from `X-QID` on the chat response for this turn |
| `session_id`    | Yes      | Same `session_id` as chat                                  |
| `error_text`    | Yes      | Error summary                                              |
| `question_text` | No       | Question being processed                                   |

**Example**

bash

```bash
curl -s -X POST "https://dev-vistaar.da.gov.in/api/telemetry/error" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "qid": "'"${QID}"'",
    "session_id": "'"${SESSION}"'",
    "error_text": "SSE connection reset",
    "question_text": "your question here"
  }'
```

**Response `200`**

json

```json
{ "status": "accepted" }
```

***

#### 8. End-to-end flow

1. Request API key and `client_code` from the platform team (see §2)
2. `POST /api/token/api-key` (`client_code`, `fingerprint_id`, `X-API-Key`; optional `user_id`) → store JWT + `expires_in`
3. User asks a question → `GET /api/chat/?query=...&session_id=...` (Bearer) → read `X-QID` from response headers → consume SSE stream
4. User gives feedback → `POST /api/telemetry/feedback` (same `qid` from `X-QID`, `session_id`, Bearer)
5. Client error on that turn → `POST /api/telemetry/error` (same `qid` from `X-QID`, `session_id`, Bearer)
6. Before JWT expires (\~15 min) → repeat step 2

***

#### 9. HTTP status reference

| Code | Meaning                                  |
| ---- | ---------------------------------------- |
| 200  | Success                                  |
| 401  | Missing/invalid API key or JWT           |
| 400  | Missing `client_code` or invalid body    |
| 500  | Service error — contact platform support |
