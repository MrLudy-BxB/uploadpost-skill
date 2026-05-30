---
source_id: llm-section:docs/api/upload-status.md
public_url: https://docs.upload-post.com/api/upload-status
raw_source: llm.txt
collected: 2026-05-30
---

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

in\_progress: Some platform results have been recorded but not all.

completed: All platforms have finished successfully.

failed: All platforms have failed, or no activity has been recorded for over 1 hour.

not\_found: No upload request was found with the provided ID (returned with HTTP 404).

Individual platform results may have their own status:

queued: The platform upload is waiting to be processed.

processing: The platform upload is currently being processed.

completed: The platform upload finished successfully.

failed: The platform upload failed permanently.

retryable: The platform upload failed but is eligible for automatic retry.

Example Request

For async uploads:

For scheduled posts:

Example Response

For async uploads (request\_id) — in progress:

For scheduled posts (job\_id):

Failed upload:

Not found:

Responses

| Status | Description |
|--------|-------------|
| 200 OK | Success. Response includes request\_id or job\_id depending on which parameter was used. |
| 400 Bad Request | Missing both request\_id and job\_id: {"error":"request\_id or job\_id is required"} |
| 401 Unauthorized | Invalid or expired token |
| 404 Not Found | No upload request found with the provided ID |
| 500 Internal Server Error | Server error with details |

SDK Examples

Python

JavaScript/Node.js

Polling Best Practices

The status endpoint uses internal caching. Polling faster than the cache refresh interval returns the same result and wastes your rate limit budget.

| Status | Cache TTL | Recommended poll interval |
|--------|-----------|--------------------------|
| queued / pending | 2 seconds | Every 5–10 seconds |
| processing | 3 seconds | Every 10 seconds |
| completed / failed | 5 minutes | Stop polling — result is final |

For high-volume integrations, consider using webhooks instead of polling — you'll receive an instant POST notification when the upload completes.

See the full Rate Limits & Polling guide for detailed recommendations.

Related

Text uploads

Video uploads

Photo uploads

Schedule posts

Rate Limits & Polling
