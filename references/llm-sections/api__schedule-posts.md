---
source_id: llm-section:docs/api/schedule-posts.md
public_url: https://docs.upload-post.com/api/schedule-posts
raw_source: llm.txt
collected: 2026-05-30
---

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
| Authentication | Required. Either an Apikey (Authorization: Apikey \<token>) or a white-label profile JWT (Authorization: Bearer \<profile\_jwt>). When authenticated with a profile JWT, the job must belong to that profile and the profile must have readonly\_calendar: false. |
| URL Param | job\_id — ID obtained from the list endpoint. |

Success Response 200 OK

Error Responses

| Status | Body | Condition |
|--------|------|-----------|
| 401 Unauthorized |   | Invalid or missing token. |
| 404 Not Found | { "success": false, "error": "Job not found" } | The supplied job\_id does not exist or doesn't belong to the authenticated user. |
| 500 Internal Server Error |   | Unexpected failure while cancelling the job or deleting its assets. |

Edit a Scheduled Post

| | |
|---|---|
| Endpoint | PATCH /api/uploadposts/schedule/\<job\_id> |
| Authentication | Required. Either an Apikey (Authorization: Apikey \<token>) or a white-label profile JWT (Authorization: Bearer \<profile\_jwt>). When authenticated with a profile JWT, the job must belong to that profile and the profile must have readonly\_calendar: false. |
| URL Param | job\_id — ID obtained from the list endpoint. |
| Body | JSON object with one or more of the fields below. |

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| scheduled\_date | string | No | ISO-8601 date/time, e.g., "2025-10-05T10:30:00Z". Must be in the future and within 1 year. Interpreted as UTC unless timezone is provided. |
| timezone | string | No | IANA timezone identifier (e.g., "Europe/Madrid", "America/New\_York"). If provided, scheduled\_date is interpreted in this timezone. Defaults to UTC if omitted. See IANA Time Zone Database. |
| title | string | No | New post title/caption. |
| caption | string | No | New caption/description. |

Success Response 200 OK

Error Responses

| Status | Body | Condition |
|--------|------|-----------|
| 400 Bad Request | { "success": false, "error": "\<reason>" } | Invalid body; invalid/past date; job not editable; daily limit reached. |
| 401 Unauthorized |   | Invalid or missing token. |
| 403 Forbidden | { "success": false, "error": "Forbidden" } | The job does not belong to the authenticated user, or the profile JWT does not match the job's profile. |
| 403 Forbidden | { "success": false, "message": "This calendar is read-only…", "error\_code": "READONLY\_CALENDAR" } | Authenticated with a profile JWT for a profile that has readonly\_calendar: true. |
| 404 Not Found | { "success": false, "error": "Job not found" } | The supplied job\_id does not exist. |
| 500 Internal Server Error |   | Unexpected failure while editing the job. |

Example Request

See Also

Using scheduled\_date when uploading content – parameter description.

Upload Video, Upload Photos, Upload Text – endpoints that support scheduling.

Upload Status – Check the execution status of scheduled posts using the job\_id.
