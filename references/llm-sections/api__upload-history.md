---
source_id: llm-section:docs/api/upload-history.md
public_url: https://docs.upload-post.com/api/upload-history
raw_source: llm.txt
collected: 2026-05-30
---

Upload History

Retrieve a paginated list of your past uploads across platforms.

Endpoint

Headers

| Name | Value | Description |
|------|-------|-------------|
| Authorization | Apikey your-api-key | Required.|

Query Parameters

| Name | Type | Required | Default | Allowed | Description |
|------|------|----------|---------|---------|-------------|
| page | Integer | No | 1 | >= 1 | Page number |
| limit | Integer | No | 10 | 10, 20, 50, 100 | Page size |

Responses

200 OK

history: array of history items (most recent first)

total: total number of records for the user

page: requested page

limit: requested limit

400 Bad Request: { "error": "Invalid page" } or { "error": "Invalid limit" }

401 Unauthorized: { "success": false, "message": "Invalid or expired token" }

500 Internal Server Error: { "error": "Failed to retrieve upload history", "details": "..." }

History Item Schema

Typical fields (not all fields are guaranteed on every record):

user\_email: string

profile\_username: string

platform: string (e.g., tiktok, instagram, linkedin, youtube, facebook, x, threads, pinterest, google\_business)

media\_type: string (video | photo | text)

upload\_timestamp: string (ISO-8601)

success: boolean

platform\_post\_id: string | array | null

post\_url: string | null (present when success is true)

error\_message: string | null

media\_size\_bytes: number | null

post\_title: string | null

post\_caption: string | null

is\_async: boolean | null

job\_id: string | null (present when the upload originated from a scheduled job)

dashboard: any | null

video\_was\_transcoded: boolean | null

changes: object | null

prevalidation\_metadata: object | null

request\_id: string | null

request\_total\_platforms: number | null

Note: When you schedule a post, the resulting history items will include job\_id. Use this to correlate the scheduled job with the eventual publish record in history.

Example Request

Example 200 Response (truncated)

See also

Upload Status

Manage Scheduled Posts

Upload Text

Upload Video

Upload Photos
