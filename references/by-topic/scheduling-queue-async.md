# Async Uploads, Scheduling, and Queue System

Collected: 2026-05-30

## Source Traceability

| Source | Public URL |
| --- | --- |
| `docs/guides/async-uploads.md` | https://docs.upload-post.com/guides/async-uploads |
| `docs/api/queue-system.md` | https://docs.upload-post.com/api/queue-system |
| `docs/guides/rate-limits.md` | https://docs.upload-post.com/guides/rate-limits |
| `openapi.json` | https://docs.upload-post.com/openapi.json |

## Endpoint Routing

| Method | Path | Operation | Summary | Required fields |
| --- | --- | --- | --- | --- |
| `GET` | `/uploadposts/queue/settings` | `getQueueSettings` | Get queue settings | - |
| `POST` | `/uploadposts/queue/settings` | `updateQueueSettings` | Update queue settings | profile_username |
| `GET` | `/uploadposts/queue/preview` | `getQueuePreview` | Preview queue slots | - |
| `GET` | `/uploadposts/queue/next-slot` | `getNextQueueSlot` | Get next available queue slot | - |

## Practical Notes

- Poll status at documented intervals instead of polling faster than cache TTLs.
- Prefer webhooks for high-volume integrations when possible.
- Queue settings determine automatic post slot selection.

## Source Excerpts

### docs/guides/async-uploads.md

Avoid Timeouts with Asynchronous Uploads

Are your requests taking too long and resulting in timeouts? For video, photo, or text post uploads that may require more processing time (file processing, social network publishing queues, etc.), use the async\_upload parameter to make your request asynchronously.

How does it work?

Send your request with async\_upload=true to the appropriate upload endpoint.

The API will immediately respond with a request\_id.

Use this request\_id to check the progress and result at the status endpoint.

Checking Status

The status endpoint supports two different identifier types:

request\_id: Returned by upload endpoints when async\_upload=true

job\_id: Returned when you schedule posts with scheduled\_date

For Async Uploads

For Scheduled Posts

After scheduling a post with scheduled\_date, the API returns a job\_id. Use it to check the status after the scheduled time:

Relevant Endpoints

Text upload: POST /api/upload\_text

Video upload: POST /api/upload

Photo upload: POST /api/upload\_photos

Upload status: GET /api/uploadposts/status?request\_id=\<REQUEST\_ID> or ?job\_id=\<JOB\_ID>

Quick Example: Asynchronous Video Upload

### docs/api/queue-system.md

Queue System

The queue system allows you to automatically schedule posts to predefined time slots. Instead of specifying an exact date/time with scheduled\_date, you can use add\_to\_queue=true to have the system automatically assign your post to the next available slot.

How It Works

The queue is always active with default time slots (9am, 12pm, 5pm Eastern Time). You can customize these slots, timezone, and active days through the Queue Settings endpoints.

When you upload content with add\_to\_queue=true:

The system finds the next available slot based on your queue configuration

Your post is automatically scheduled to that slot

You receive a job\_id to track the scheduled post

Multiple Posts Per Slot

By default, each slot accepts 1 post. You can increase this with the max\_posts\_per\_slot setting to allow multiple posts in the same time slot. This is useful when you want to post to different platforms at the same time (e.g., an Instagram post and a Facebook post both at 9am).

You can also mark individual slots as full to prevent new posts from being added, even if they haven't reached the maximum capacity.

Using the Queue in Uploads

Add the add\_to\_queue parameter to any upload endpoint:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| add\_to\_queue | Boolean | No | If true, automatically schedules the post to your next available queue slot. Cannot be used together with scheduled\_date. |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/rate-limits.md

Rate Limits & Polling Best Practices

Upload-Post applies rate limits at multiple levels to protect the service and the social media platforms. This guide explains each limit and how to design your integration to work within them.

API Rate Limits

Every authenticated API response includes rate limit headers:

| Header | Description |
|--------|-------------|
| X-RateLimit-Limit | Maximum requests allowed in the current window |
| X-RateLimit-Remaining | Requests remaining in the current window |
| X-RateLimit-Reset | Unix timestamp when the window resets |

When you exceed the limit, the API returns HTTP 429 Too Many Requests. Wait until the X-RateLimit-Reset timestamp before retrying.

Upload Status Polling

When using async\_upload=true, you need to poll the Upload Status endpoint to get the result. Here are the recommended intervals:

Polling Strategy

Recommended Polling Intervals

| Scenario | Poll Interval | Max Wait |
|----------|--------------|----------|
| Photo uploads | Every 5–10 seconds | 2 minutes |
| Video uploads (small, < 50 MB) | Every 10 seconds | 5 minutes |
| Video uploads (large, > 50 MB) | Every 15 seconds | 10 minutes |
| Scheduled posts (after scheduled time) | Every 30 seconds | 10 minutes |
| Queued posts | Every 60 seconds | Until next queue slot |

Status Cache TTLs

The status endpoint uses internal caching. Here is how often the status value is refreshed:

| Status | Cache TTL | Meaning |
|--------|-----------|---------|
| queued | 2 seconds | The upload is waiting for a worker |
| pending | 2 seconds | Accepted but not yet started |

[Excerpt truncated in this topic reference; see the source URL for full page context.]
