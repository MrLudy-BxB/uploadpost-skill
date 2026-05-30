# Upload Status, History, and Scheduled Posts

Collected: 2026-05-30

## Source Traceability

| Source | Public URL |
| --- | --- |
| `docs/api/upload-status.md` | https://docs.upload-post.com/api/upload-status |
| `docs/api/upload-history.md` | https://docs.upload-post.com/api/upload-history |
| `docs/api/schedule-posts.md` | https://docs.upload-post.com/api/schedule-posts |
| `openapi.json` | https://docs.upload-post.com/openapi.json |

## Endpoint Routing

| Method | Path | Operation | Summary | Required fields |
| --- | --- | --- | --- | --- |
| `GET` | `/uploadposts/status` | `getUploadStatus` | Get upload status | - |
| `GET` | `/uploadposts/history` | `getUploadHistory` | Get upload history | - |
| `GET` | `/uploadposts/schedule` | `listScheduledPosts` | List scheduled posts | - |
| `DELETE` | `/uploadposts/schedule/{job_id}` | `cancelScheduledPost` | Cancel a scheduled post | - |
| `PATCH` | `/uploadposts/schedule/{job_id}` | `editScheduledPost` | Edit a scheduled post | - |

## Practical Notes

- Use the endpoint index for exact method/path lookup and this reference for task-level routing.

## Source Excerpts

### docs/api/upload-status.md

Upload Status

Check the status of asynchronous uploads initiated with async\_upload=true or scheduled posts.

Endpoint

Headers

| Name | Value | Description |
|------|-------|-------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| request\_id | String | Conditional | The request identifier returned by the upload endpoints when async\_upload=true. Use for async uploads. You can also provide your own request\_id when submitting the upload (via form field or X-Request-Id header) so you can poll status even if the upload response is lost due to a timeout. |
| job\_id | String | Conditional | The job identifier returned by scheduled posts. Use for posts with scheduled\_date. |

Note: At least one of request\_id or job\_id must be provided.

Behavior

Async uploads: When you submit an upload request with async\_upload=true, the API returns immediately with a request\_id. Use this to retrieve aggregated progress and results.

Scheduled posts: When you schedule a post with scheduled\_date, the API returns a job\_id. Use this to check the status after the scheduled time.

The top-level status field may be one of:

pending: The request has been accepted but no platform results have been recorded yet. For scheduled posts, this means the job has not executed yet.

queued: The upload is queued and waiting for a worker to begin processing.

processing: At least one platform is actively being processed while others are still queued or pending.

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/upload-history.md

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

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/schedule-posts.md

Manage Scheduled Posts

Schedule your uploads in advance and keep full control over them with our job management endpoints. This page covers how to list and cancel scheduled jobs created via the scheduled\_date parameter.

List Scheduled Posts

| |  |
|---|---|
| Endpoint | GET /api/uploadposts/schedule |
| Authentication | Required. Supply the Apikey in the Authorization header — e.g. Authorization: Apikey \<token> |
| Query / Body Params | None. The user is inferred from the access-token. |

Success Response 200 OK

Returns a JSON array where each element is a scheduled-job object:

| Field | Type | Description |
|-------|------|-------------|
| job\_id | string | Unique identifier of the scheduled job. Required to cancel it. |
| scheduled\_date | string | ISO-8601 date/time when the post will go live. Time is in UTC. |
| post\_type | string | One of video, photo, or text. |
| profile\_username | string | Upload-Post profile that will publish the content. |
| title | string | Title/caption of the post. |
| preview\_url | string \\| null | Short-lived signed URL to preview the media (first photo or video). null for text posts. |

Error Responses

| Status | Reason |
|--------|--------|
| 401 Unauthorized | Missing or invalid token. |

Cancel a Scheduled Post

| | |
|---|---|
| Endpoint | DELETE /api/uploadposts/schedule/\<job\_id> |

[Excerpt truncated in this topic reference; see the source URL for full page context.]
