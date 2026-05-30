---
source_id: llm-section:docs/api/instagram-comments.md
public_url: https://docs.upload-post.com/api/instagram-comments
raw_source: llm.txt
collected: 2026-05-30
---

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
| limit    | Integer | No       | Comments per page, between 1 and 50 (Meta's hard cap for this edge). If omitted, Instagram's default applies (~25).        |
| after    | String  | No       | Cursor returned by Meta in the previous page (pagination.next\_cursor). Use it to fetch the next page.                        |

Ordering: Meta returns comments newest-first (reverse-chronological) on Graph API v3.2 and later. There is no query parameter to change this — the order is fixed by Meta.

Example Requests

Single page (default):

Paginate through all comments (50 per page):

Loop while pagination.has\_next is true, passing pagination.next\_cursor as after each time.

Responses

200 OK

When the last page is reached, pagination is {"next\_cursor": null, "has\_next": false}.

400 Bad Request

400 Bad Request (invalid limit)

400 Bad Request (invalid post URL)

500 Internal Server Error

Note: When using a post URL, the API automatically resolves the shortcode to a media ID by scanning the account's recent posts. The resolved IDs are cached for subsequent requests. The post must belong to the authenticated account.

Rate limiting: Calls to this endpoint are subject to your account's global API rate limits, which scale with your plan. There is no per-post throttle, so you can walk all pages of a viral post without artificial waits.

Reply to Comment (Private Reply)

Send a private reply (DM) to the author of a comment on your post. This sends a direct message to the commenter.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Body Parameters (JSON)

| Name       | Type   | Required | Description                                                  |
|------------|--------|----------|--------------------------------------------------------------|
| platform   | String | Yes      | The platform (e.g., "instagram").                          |
| user       | String | Yes      | Profile username (as configured in Upload-Post).             |
| comment\_id | String | Yes      | The ID of the comment to reply to (from Get Post Comments).  |
| message    | String | Yes      | The private reply message text.                              |

Example Request

Responses

200 OK (reply sent successfully)

400 Bad Request (missing fields)

429 Too Many Requests (daily DM limit exceeded)

500 Internal Server Error

Reply to Comment (Public Reply)

Post a public reply to a comment on your Instagram post. The reply appears as a visible comment under the original comment.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Body Parameters (JSON)

| Name       | Type   | Required | Description                                                  |
|------------|--------|----------|--------------------------------------------------------------|
| platform   | String | Yes      | The platform (e.g., "instagram").                          |
| user       | String | Yes      | Profile username (as configured in Upload-Post).             |
| comment\_id | String | Yes      | The ID of the comment to reply to (from Get Post Comments).  |
| message    | String | Yes      | The public reply message text.                               |

Example Request

Responses

200 OK (reply posted successfully)

400 Bad Request (missing fields)

429 Too Many Requests (daily limit exceeded)

500 Internal Server Error

Important Notes

7-day window for private replies: Some platforms only allow private replies to recent comments (e.g., Instagram requires comments less than 7 days old). Public replies do not have this restriction.

Comment must be on your post: You can only reply to comments on posts owned by the authenticated account.

Daily limits: Upload-Post enforces a configurable daily limit per user. Both private and public replies count toward this limit. When exceeded, the API returns a 429 status code.

Public vs. private replies: Use /comments/reply to send a private DM to the comment author. Use /comments/public-reply to post a visible reply under the original comment. Both require the comment\_id from the Get Post Comments endpoint.

Using comment data for DMs: Each comment includes the commenter's user ID. You can use this ID with the Direct Messages endpoint to send follow-up DMs directly.
