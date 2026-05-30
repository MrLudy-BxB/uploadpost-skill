---
source_id: llm-section:docs/guides/rate-limits.md
public_url: https://docs.upload-post.com/guides/rate-limits
raw_source: llm.txt
collected: 2026-05-30
---

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
| processing | 3 seconds | At least one platform is actively uploading |
| completed | 5 minutes | All platforms finished successfully |
| failed | 5 minutes | All platforms failed |

Key takeaway: Polling faster than every 5 seconds for non-terminal states is unnecessary — the cache refreshes every 2–3 seconds. For terminal states (completed/failed), the result is cached for 5 minutes, so subsequent polls are fast.

Alternative: Use Webhooks

Instead of polling, configure a webhook to receive a POST notification when the upload completes. This is more efficient for high-volume integrations:

The webhook fires once per platform per upload, so you get immediate notification without polling.

Upload Rate Limits

Per-Request Duplicate Protection

The API prevents duplicate uploads using idempotency keys and rate limiting:

Same content to same platform: If you submit an identical upload (same user, platform, and content hash) within a short window, the API returns the existing request instead of creating a duplicate.

Idempotency-Key header: Send a unique Idempotency-Key or X-Idempotency-Key header to ensure exactly-once processing, even across retries.

Daily Upload Caps (per platform per account)

Each social account has a daily hard cap based on platform limits. Exceeding these returns HTTP 429:

| Platform | Max posts / 24h |
|----------|----------------|
| Instagram | 50 |
| TikTok | 15 |
| LinkedIn | 150 |
| YouTube | 10 |
| Facebook | 25 |
| X (Twitter) | 50 |
| Threads | 50 |
| Pinterest | 20 |
| Reddit | 40 |
| Bluesky | 50 |

These are rolling 24-hour windows per social account, not per API key.

Monthly API Usage

Your plan determines how many API upload calls you can make per month:

| Plan | Monthly uploads |
|------|----------------|
| Free | 10 |
| Paid plans | See pricing |

Check your current usage with GET /api/uploadposts/me — the response includes api\_usage.count.

API Key Brute-Force Protection

Failed authentication attempts (invalid API keys) are rate-limited per IP:

After 10 consecutive failed attempts with the same invalid key, the IP is blocked for 5 minutes.

The API returns HTTP 429 during the block period.

Best Practices

Use webhooks instead of polling when possible — they're instant and don't consume rate limit budget.

Respect X-RateLimit-Remaining — stop sending requests when it reaches 0.

Use exponential backoff on 429 responses — don't hammer the API after hitting a limit.

Set async\_upload=true for all uploads — synchronous uploads timeout after 59 seconds anyway.

Send Idempotency-Key for all upload requests — this protects you from duplicate posts if your HTTP client retries on timeout.

Poll at the recommended intervals — faster polling just returns cached results and wastes your rate limit budget.
