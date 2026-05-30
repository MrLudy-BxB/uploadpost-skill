# Instagram Media, Comments, DMs, and AutoDM

Collected: 2026-05-30

## Source Traceability

| Source | Public URL |
| --- | --- |
| `docs/api/instagram-media.md` | https://docs.upload-post.com/api/instagram-media |
| `docs/api/instagram-comments.md` | https://docs.upload-post.com/api/instagram-comments |
| `docs/api/instagram-dms.md` | https://docs.upload-post.com/api/instagram-dms |
| `docs/api/autodms.md` | https://docs.upload-post.com/api/autodms |
| `openapi.json` | https://docs.upload-post.com/openapi.json |

## Endpoint Routing

| Method | Path | Operation | Summary | Required fields |
| --- | --- | --- | --- | --- |
| `GET` | `/uploadposts/comments` | `getPostComments` | Get Instagram post comments | platform, user |
| `POST` | `/uploadposts/comments/reply` | `replyToComment` | Reply to comment (private reply DM) | comment_id, message, platform, user |
| `GET` | `/uploadposts/dms/conversations` | `getDmConversations` | Get Instagram DM conversations | platform, user |
| `POST` | `/uploadposts/dms/send` | `sendDm` | Send Instagram DM | message, platform, recipient_id, user |
| `POST` | `/api/uploadposts/autodms/start` | `docs-autodms-post-start` | Autodms | - |
| `GET` | `/api/uploadposts/autodms/status` | `docs-autodms-get-status` | Autodms | - |
| `GET` | `/api/uploadposts/autodms/logs` | `docs-autodms-get-logs` | Autodms | - |
| `POST` | `/api/uploadposts/autodms/pause` | `docs-autodms-post-pause` | Autodms | - |
| `POST` | `/api/uploadposts/autodms/resume` | `docs-autodms-post-resume` | Autodms | - |
| `POST` | `/api/uploadposts/autodms/stop` | `docs-autodms-post-stop` | Autodms | - |
| `POST` | `/api/uploadposts/autodms/delete` | `docs-autodms-post-delete` | Autodms | - |

## Practical Notes

- Instagram AutoDM monitor endpoints are present in the docs pages and may not be present in the OpenAPI spec.
- AutoDM monitors are Instagram-only, expire after 15 days, and have per-profile/per-plan DM limits.

## Source Excerpts

### docs/api/instagram-media.md

Media List

Retrieve a list of recent media (posts, reels, videos, pins, tweets, etc.) from a connected social media account. Supports all major platforms: Instagram, TikTok, YouTube, LinkedIn, Facebook, X (Twitter), Threads, Pinterest, Bluesky, and Reddit.

Useful for building post selectors, displaying recent content, or getting media IDs for other API calls.

Get User Media

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Query Parameters

| Name     | Type   | Required | Description                                                      |
|----------|--------|----------|------------------------------------------------------------------|
| platform | String | Yes      | The platform to retrieve media from. See Supported Platforms. |
| user     | String | Yes      | Profile username (as configured in Upload-Post).                 |
| page\_urn | String | No       | LinkedIn only. Selects which LinkedIn page to fetch posts from. Accepts a numeric organization ID (e.g., 12345), a full URN (e.g., urn:li:organization:12345), or me to force the connected member's personal profile. If omitted, the endpoint targets the page that was active when the account was linked — for accounts connected as an organization admin, the first administered organization is auto-resolved; otherwise the personal profile is used. Use the LinkedIn Pages endpoint to list available organizations. |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/instagram-comments.md

Comments

Retrieve comments from social media posts, send private replies (DMs) to commenters, or post public replies visible under the original comment.

Get Post Comments

Retrieve all comments on a specific post. Accepts either a numeric media ID or a full post URL.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Query Parameters

| Name     | Type    | Required | Description                                                                                                                    |
|----------|---------|----------|--------------------------------------------------------------------------------------------------------------------------------|
| platform | String  | Yes      | The platform to retrieve comments from (e.g., "instagram").                                                                  |
| user     | String  | Yes      | Profile username (as configured in Upload-Post).                                                                               |
| post\_id  | String  | Yes\*    | Numeric media ID. Use post\_id or post\_url (one is required).                                                               |
| post\_url | String  | Yes\*    | Full post URL (e.g., https://www.instagram.com/p/ABC123/). Alternative to post\_id.                                         |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/instagram-dms.md

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

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/autodms.md

AutoDM Monitors

Set up persistent monitors that automatically send private DMs to users who comment on your Instagram posts. Monitors run in the background 24/7 — no need to keep polling manually.

Start a Monitor

Create a new AutoDM monitor for an Instagram post. The monitor will check for new comments at regular intervals and send a private reply (DM) to each new commenter.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Body Parameters (JSON)

| Name                | Type           | Required | Description                                                                                     |
|---------------------|----------------|----------|-------------------------------------------------------------------------------------------------|
| post\_url            | String         | Yes      | The Instagram post URL to monitor for comments.                                                |
| reply\_message       | String         | Yes      | The DM message to send to each matching commenter.                                             |
| profile\_username    | String         | Yes      | Profile username (as configured in Upload-Post). Must have Instagram connected.                |
| monitoring\_interval | Integer        | No       | Minutes between comment checks. Default: 15. Minimum: 15.                                 |

[Excerpt truncated in this topic reference; see the source URL for full page context.]
