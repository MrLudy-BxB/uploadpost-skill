---
source_id: llm-section:docs/api/instagram-dms.md
public_url: https://docs.upload-post.com/api/instagram-dms
raw_source: llm.txt
collected: 2026-05-30
---

Direct Messages

Send direct messages (DMs) and retrieve conversations on connected social media accounts.

Send a Direct Message

Send a DM directly to a user using their platform-specific User ID.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Body Parameters (JSON)

| Name         | Type   | Required | Description                                                                 |
|--------------|--------|----------|-----------------------------------------------------------------------------|
| platform     | String | Yes      | The platform to send the DM on (e.g., "instagram").                      |
| user         | String | Yes      | Profile username (as configured in Upload-Post).                            |
| recipient\_id | String | Yes      | The platform-specific User ID of the recipient.                            |
| message      | String | Yes      | The text message to send.                                                   |

Example Request

Responses

200 OK (DM sent successfully)

400 Bad Request (missing fields, invalid account)

429 Too Many Requests (daily DM limit exceeded)

500 Internal Server Error

Get Conversations

Retrieve the list of DM conversations for an account, including participants and recent messages.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Query Parameters

| Name     | Type   | Required | Description                                                      |
|----------|--------|----------|------------------------------------------------------------------|
| platform | String | Yes      | The platform to retrieve conversations from (e.g., "instagram"). |
| user     | String | Yes      | Profile username (as configured in Upload-Post).                 |

Example Request

Responses

200 OK

400 Bad Request

500 Internal Server Error

Important Notes

Messaging policies vary by platform: Each platform has its own messaging rules. For example, Instagram requires the recipient to have messaged your account first (24-hour window).

Daily DM limits: Upload-Post enforces a configurable daily DM limit per user to prevent accidental overuse. When the limit is reached, the API returns a 429 status code.

Where to find the recipient ID

The recipient's User ID can be obtained from:

The Get Conversations endpoint above (each participant has an id field).

The Get Post Comments endpoint (GET /api/uploadposts/comments) where each comment includes the commenter's user.id.

Difference from Comment Replies

| Feature | Comment Reply (/comments/reply) | Direct Message (/dms/send) |
|---|---|---|
| Recipient | comment\_id — replies to a specific comment | recipient\_id — sends to a user by ID |
| Use case | Auto-reply to post engagement | Customer support, follow-up conversations |
